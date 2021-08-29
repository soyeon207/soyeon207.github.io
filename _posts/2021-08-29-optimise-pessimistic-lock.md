---
layout: post
title: 낙관적 락(optimistic Lock)과 비관적 락(pessimistic Lock)
categories: DB
tags: [DB]
---

## 낙관적 락(optimistic Lock)과 비관적 락(pessimistic Lock)

### 낙관적 락 (optimistic Lock)

: 충돌이 발생하지 않는다고 낙관적으로 가정

: DB 가 제공하는 락 기능이 아니라 appllication 에서 제공하는 버전 관리 기능을 사용

: version 등의 구분 컬럼으로 충돌을 예방

: 트랜잭션을 커밋하는 시점에 충돌을 알 수 있음

: 최종 업데이트 과정에서만 락을 점유하기 때문에 락 점유 시간을 최소화하여 동시성을 높일 수 있다

UPDATE DUAL SET VERSION = VERSION + 1 WHERE VERSION = 1

1. 두 트랜잭션(A, B)이 거의 동시에 어떤 ROW를 변경하려고 하는 상황
2. A가 살짝 먼저 접근하고 바로 뒤이어 B가 접근한다.
3. A가 해당 ROW 와 version 를 UPDATE (선수치기)
4. B가 커밋 시점에 해당 ROW를 업데이트 하려고 version 을 체크해 보니.. 처음과 다른경우
5. 어플리케이션은 `ObjectOptimisticLockingFailureException` 을 발생시키고 첫번째 A의 커밋만 적용하여 정합성을 유지한다.
6. 실패된 커밋은 Application level 에서 후처리



 **💎 루비에서 낙관적 락 (optimistic Lock) 사용하기**

- 테이블에 lock_version 필드 추가 

  ```ruby
  t.integer :lock_version, default: 0
  ```

  ➡ 기본값은 0 으로 설정해줘야 함 

  ➡ ActiveRecord::StaleObjectError => e 예외 발생 



**☕️ JPA에서 낙관적 락 (optimistic Lock) 사용하기**

``` 
@Version
```



### 비관적 락 (perssimistic Lock)

: Repeatable Read, Serializableable 정도의 격리성에서 가능 

: 트랜잭션이 시작될 때 Shared Lock 또는 Exclusive Lock을 걸고 시작하는 방법

: DB 가 제공하는 락 사용

: 데이터 수정 즉시 트랜잭션 충돌을 알 수 있음

: 교착 상태와 같은 문제 야기

###  

### 낙관적 락 vs 비관적 락 

낙관적 락 (optimistic lock) 은 트랜잭션을 필요로 하지 않음 

1. 클라이언트가 서버에 정보를 요청
2. 서버에서는 정보를 반환
3. 클라이언트에서 이 정보를 이용하여 수정 요청
4. 서버에서는 수정 적용 ( 충돌 감지 가능 )

의 경우에도 충돌 감지를 할 수 있음. 비관적 락 (perssimistic lock) 같은 경우에는 2번에서 해당 row 에 S-Lock 이 걸리고, 3번에서는 해당 row 에 X-Lock 이 날라가기 때문에 트랜잭션을 유지 할 수 없음 

낙관적 락이 성능적으로도 좋음 

=> 충돌이 많이 일어나지 않을 것이라고 보여지는 곳에 사용하면 좋은 성능 기대 가능 

하지만 낙관적 락의 최대 단점은 롤백임. 만약 **충돌이 났다고 한다면 이를 해결하려면 개발자가 수동으로 롤백처리를 한땀한땀 해줘야합니다.** 비관적 락이라면 트랜잭션을 롤백하면 끝나는 작업이지만 낙관적 락은 그렇지 않음. 