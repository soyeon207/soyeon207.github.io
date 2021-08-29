---
layout: post
title: 분산락 (Distributed lock)
categories: DB
tags: [DB]
---

## 분산락 (Distributed lock)
> 서버가 여러 대인 상황에서 동일한 데이터에 대한 동기화를 보장하기 위해 사용 


### 필요한 이유

: 서버가 한대가 아니라 여러 대인 경우 여러 서버로 API가 분산 호출 됨 

: 서버들간에 동기화된 처리가 필요하고, 여러 서버에 공통된 락을 적용해야 하기 때문에 redis 를 이용하여 분산락을 이용한다 

: 분산락 같은 경우 db 등 **공통된 데이터 저장소**를 이용해 자원이 사용중인지 확인하기 때문에 전체 서버에 동기화된 처리가 가능 하다 


### Redis 기반 분산락 사용 하기 

**Lettuce**

- 스핀락을 사용하여 구현 가능 함 

  **🌟 스핀락이란 ?**

  : 만약 다른 스레드가 Lock 을 소유하고 있다면 그 lock이 반환될 때까지 계속 확인 하며 기다리는 것 

  : Lock 을 점유하는 시간이 짧을 경우 유용하지만 스레드가 Lock 을 오래 유지하는 경우 CPU 에 부담을 줄 수 있음 

```java
void doProcess() {
    String lockKey = "lock";

    try {
        while (!tryLock(lockKey)) { // (2)
            try {
                Thread.sleep(50);
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
        }
        
        // (3) do process
    } finally {
        unlock(lockKey); // (4)
    }
}

boolean tryLock(String key) {
    return command.setnx(key, "1"); // (1)
}

void unlock(String key) {
    command.del(key);
}
```

출처 : [레디스와 분산 락(1/2) - 레디스를 활용한 분산 락과 안전하고 빠른 락의 구현](https://hyperconnect.github.io/2019/11/15/redis-distributed-lock-1.html)

1. redis `setnx` : 동일한 key가 없을 경우에만 저장
2. 락을 획득 즉, redis 에 key 를 저장한 경우가 될 때까지 `while` 로 계속 락 획득을 시도 
3. 락을 획득 한 후 로직 수행 
4. 로직을 모두 수행 한 후 락 해제 (redis 에서 해당 키 삭제)


**문제점**

1. Lock 의 타임아웃 설정 X 

   ➡ 락을 획득하지 못한 경우 무한 루프를 돌게 됨. 그렇기 때문에 일정 시간이 지나면 락이 만료되도록 해야하는데 `setnx` 명령어는 expire time 을 지정할 수 없음 

2. Redis 에 많은 부하 

    ➡ 스핀락은 지속적으로 락의 획득을 시도하기 때문에 많은 부담을 줄 수 밖에 없음 


**Redisson**

```java
RLock lock = redissonClient.getLock("test");
lock.lock(10, TimeUnit.SECONDS);

try {
  if (voteMapper.selectVoteHistoryCount(userSeq, voteSeq) > 0) {
    log.info("이미 투표했습니다.");
  } else {
    log.info("투표 성공!");
    voteMapper.insertVoteHistory(userSeq, voteSeq);
  }
} finally {
  lock.unlock();
}
```

Lock 획득 프로세스

1. 대기없는 `tryLock` 오퍼레이션을 하여 락 획득에 성공하면 `true`를 반환합니다. 이는 경합이 없을 때 아무런 오버헤드 없이 락을 획득할 수 있도록 해줍니다.
2. pubsub을 이용하여 메세지가 올 때까지 대기하다가 락이 해제되었다는 메세지가 오면 대기를 풀고 다시 락 획득을 시도합니다. 락 획득에 실패하면 다시 락 해제 메세지를 기다립니다. 이 프로세스를 타임아웃시까지 반복합니다.
3. 타임아웃이 지나면 최종적으로 `false`를 반환하고 락 획득에 실패했음을 알립니다. 대기가 풀릴 때 타임아웃 여부를 체크하므로 타임아웃이 발생하는 순간은 파라미터로 넘긴 타임아웃시간과 약간의 차이가 있을 수 있습니다.


특징 

1. Lock에 타임아웃이 구현되어 있음 

   ```sql
   public boolean tryLock(long waitTime, long leaseTime, TimeUnit unit) throws InterruptedException
   ```

   - waitTime : 락을 얻기 위한 대기 시간 
   - leaseTime : 락이 만료되어 사라지는 시간 지정 

2. 스핀 락을 사용하지 않음 

   - Redisson 은 **pubsub** 기능을 사용

     🌟 **pubsub** 

   ​		: 채널을 구독한 subscribe 에게 모든 메세지를 전송하는 것 

   ​		: 구독자들에게 바로 메세지를 전달하기 때문에 메시지를 보관하지는 않음 

   ​		: 어느 채널을 통해서 메시지를 전달해준다고 생각하면 됨 ! 

   ​		: redis-cli pub/sub 사용하기 

   ​			redis-cli -> subscribe test -> publish test "테스트입니다"

   - Lock 이 해제 될 때마다 subscribe 한 클라이언트에게 메시지(Lock 시도 해도됨)을 주어 일일히 레디스에 요청을 보내 확인하지 않아도 됨

3. Lua 스크립트 사용 

   - 락 획득가능 여부, 획득, pubsub 알림은 atomic 해야함 
   - Lua 스크립트 자체가 하나의 큰 명령으로 해석되기 때문에 atomic 하게 처리 됨 



**Redis 원자적 메소드 사용**

내부적으로 redis setnx 사용 

```
boolean success = stringRedisTemplate.opsForValue().setIfAbsent(key, val, 10, TimeUnit.MINUTES);
```

이미 있는 key 인 경우 false 리턴 



### MySQL 기반 분산락 적용

1. ExclusiveLock 테이블 생성 

2. repository

   ```java
   @Lock(LockModeType.PESSIMISTIC_WRITE)
   @QueryHints({@QueryHint(name = "javax.persistence.lock.timeout", value = "300000")})
   @Override
   Optional<ExclusiveLock> findById(Long id);
   
   @Lock(LockModeType.PESSIMISTIC_WRITE)
   @QueryHints({@QueryHint(name = "javax.persistence.lock.timeout", value = "300000")})
   Optional<ExclusiveLock> findByCode(String code);
   ```

3. ExclusiveLock.find 후 update 

   => find 할 때 s-lock 이 걸리 기 때문에 순서 보장 됨 





### 레퍼런스

[레디스와 분산락](https://happyer16.tistory.com/entry/%EB%A0%88%EB%94%94%EC%8A%A4%EC%99%80-%EB%B6%84%EC%82%B0%EB%9D%BD)

[[운영체제] 스핀락(Spin Lock)이란?](http://itnovice1.blogspot.com/2019/09/spin-lock.html)

[레디스와 분산 락(1/2) - 레디스를 활용한 분산 락과 안전하고 빠른 락의 구현](https://hyperconnect.github.io/2019/11/15/redis-distributed-lock-1.html)

[Atomic 처리와 cache stampede 대책을 위해 Redis Lua script를 활용한 이야기](https://engineering.linecorp.com/ko/blog/atomic-cache-stampede-redis-lua-script/)

[[redis] redisson을 통한 분산 락](