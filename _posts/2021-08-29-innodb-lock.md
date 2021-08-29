---
layout: post
title: InnoDB 의 Lock
categories: DB
tags: [DB]
---
## InnoDB 의 Lock

### Lock 이란 ?
> 트랜잭션 처리의 순차성을 보장하기 위한 방법

- DB 가 처리하는 가장 작은 단위 
- 트랜잭션이 완벽하게 끝날 때까지 다른 요청을 막아줌 


### Lock 의 종류
**락 적용 요소에 따른 분류**
- Shared Lock(`S`)
- Exclusive Lock(`E`)
- Intention Lock

**락이 적용되는 상황에 따른 분류**
- Row-level Lock (default)
- Record Lock
- Gap Lock
- Next-key Lock
- Insert Intention Lock
- Auto-INC Lock

innoDB storage engine 은 기본적으로 Row-level Lock 을 사용 


### 락 적용 요소에 따른 분류
#### Shared Lock (`S`)
: Row-level Lock<br>
: `read` 에 대한 Lock<br>
: 데이터를 읽을 때 사용되어지는 Lock<br>
: S Lock 을 사용하는 쿼리 끼리는 같은 row 에 접근 가능하다 <br>
: `SELECT ... FOR SHARE` 등 일부 `SELECT` 쿼리는 read 작업을 수행할 때 InnoDB가 각 row에 S lock 

#### Exclusive Lock(`X`)
: Row-level Lock

: `write` 에 대한 Lock

: 데이터를 변경하고자 할 때 사용되어지는 Lock

: 트랜잭션이 완료 될 때까지 유지되며, Lock이 해제될 때까지 다른 트랜잭션(읽기 포함)은 해당 리소스에 접근 X

: `SELECT ... FOR UPDATE`나 `UPDATE`, `DELETE` 등의 수정 쿼리를 날릴 때 각 row에 걸리는 lock


**DB 에 X-Lock 걸기**
```mysql
SET autocommit=0;  -- 각 SQL 문장 실행시에 즉시 반영 안되고 COMMIT, ROLLBACK 명령시에 영구반영됨
START TRANSACTION;
UPDATE test SET name = '테스트' where id = 1;
-- 후 Commit 하지 않고 계속 기다리면 됨 
```

#### **S Lock, X Lock 거는 규칙** 
- 여러 트랜잭션이 한 row 에 `S Lock` 을 걸 수 있다 => 여러 트랜잭션이 한번에 여러 row 를 읽을 수 있다 

- `S Lock`이 걸려 있는 row 에 다른 트랜잭션이 `X Lock`을 걸 수 없다 => 다른 트랜잭션이 읽고 있는 row 를 수정/삭제 불가능

- `X Lock` 이 걸려 있는 row 에 다른 트랜잭션이 `S Lock`,` X Lock` 을 걸 수 없다 => 다른 트랜잭션이 수정/삭제 하고 있는 row 는 읽기, 수정 , 삭제 모두 불가능 


**Lock 에 따른 경쟁 여부 관계** 
|        | S-Lock | X-Lock |
| ------ | ------ | ------ |
| S-Lock | X      | O      |
| X-Lock | O      | O      |


#### Intention Lock
: Table-level Lock

: 테이블 안의 **row에 대해서 나중에 어떤 row-level락을 걸 것** 이라는 의도를 알려주기 위해 미리 table-level에 걸어두는 lock

: **SELECT ... LOCK IN SHARE MODE** 이 실행되면 

	1. Intention Shared Lock (`IS`) 이 테이블에 걸림
	2. row-level 에 S-Lock 이 걸림 

: **SELECT ... FOR UPDATE**, **INSERT**, **DELETE**, **UPDATE** 이 실행되면

	1. intention exclusive lock (`IX`) 이 테이블에 걸림
	2. row-level 에 X-Lock 이 걸림

: `IS`, `IX` 락은 여러 트랜잭션에서 동시에 접근 가능하지만, row-level 의 실제 락인 S, X 락에서 접근 제어를 하게 됨 

: **LOCK TABLES**, **ALTER TABLE**, **DROP TABLE** 이 실행될 때는 `IS`, `IX` 를 모두 block하는 table-level 락이 걸린다. 즉 `IS`, `IX` lock 을 획득 하려는 트랜잭션은 대기상태로 빠짐


#### row-level 및 Table-level 에서 두번 Lock 하는 이유 ?
- A 트랜잭션에서 이미 테이블에 대해 락이 걸려있는데, B 트랜잭션에서 해당 테이블의 특정 row에 lock을 거는것을 원천적으로 방지 할 수 있다. (반대의 경우도 마찬가지)

  EX) row-level의 write이 일어나고 있을때 테이블 스키마가 변경되서는 안된다. write query의 경우 이미 `IX` 락을 획득한 상태이기 때문에 해당 테이블의 스키마가 변경되는것을 막을 수 있다.

|      | X    | IX   | S    | IS   |
| :--- | :--- | :--- | :--- | ---- |
| X    | O    | O    | O    | O    |
| IX   | O    | X    | O    | X    |
| S    | O    | O    | X    | X    |
| IS   | O    | X    | X    | X    |


### 락이 적용되는 상황에 따른 분류
#### Row-level Lock
> 테이블 row 마다 걸리는 Lock

- S-Lock , X-Lock


#### Record Lock
> DB index record에 걸리는 Lock

- 개별 인덱스 레코드에 S-Lock 혹은 X-Lock 을 설정 
- primary key, unique key 로 조회해서 하나의 인덱스 레코드(=row)에만 lock 을 거는 것 


#### Gap Lock
> DB index record 의 gap 에 걸리는 Lock

- gap : index 중에 DB 에 실제 record 가 없는 부분 

  EX ) id column 만 있는 테이블인데 인덱스가 걸려 있고, id = 3 인 row 와 id = 7 인 row 가 있을 때 

  ```
      Index table               Database
  -------------------          ---------
  | id  | row addr  |          |  id   |
  -------------------          ---------
  |  3  | addr to 3 |--------->|   3   |
  |  7  | addr to 7 |--------->|   7   |
  -------------------          ---------
  ```

  - `id <= 2` , `3< id < 7`, `8 <= id` 에 해당하는 부분에는 index record 가 없음 
  - 이 부분이 index record 의 gap 

- 레코드 간의 gap 에 새로운 Insert 가 되는 현상을 방지하는 lock 

- 최초 레코드의 이전, 마지막 레코드의 이후의 gap 에도 lock 을 설정 

- record lock 은 **이미 존재하는 row가 변경 되지 않게 보호** 하는 반면, gap lock 은 조건에 해당하는 **새로운 row 가 추가 되는 걸 방지** 

- id column 만 있는 테이블인데 인덱스가 걸려 있고, id = 3 인 row 와 id = 5, id = 7인 row 가 있을 때

```mysql
SELECT * FROM tbl WHERE id BETWEEN 5 AND 5 FOR UPDATE;
## 3 < id < 5 (최초레코드 이전 gap) [gap x-lock]
## 5 < id < 7 (마지막 레코드 이후 gap) [gap x-lock]
```

#### Next-key Lock
> record lock 과 gap lock 을 함께 사용하는 Lock

- REPEATABLE READ 에서 phatom read를 막기위해 탐색시 next-key lock을 사용
- id column 만 있는 테이블인데 인덱스가 걸려 있고, id = 3 인 row 와 id = 5, id = 7인 row 가 있을 때 

```mysql
SELECT * FROM tbl WHERE id BETWEEN 3 AND 5 FOR UPDATE;
## id = 3, 5 [record x-lock]
## id < 3 (최초레코드 이전 gap) [gap x-lock]
## 3 < id < 5 [gap x-lock]
## 5 < id < 7 (마지막 레코드 이후 gap) [gap x-lock]
```

#### Insert Intention Lock
> insert 구문 실행시 획득하는 특수한 형태의 gap lock

- INSERT될 row에 대해서 exclusive lock을 걸기 전에 먼저 insert intention lock을 건다
- EX) pk=3, pk=6의 레코드가 존재하는 테이블이 존재
  - A 트랜잭션에서 pk=5에 INSERT, B 트랜잭션에서 pk=4에 INSERT시도
  - 만약 일반적인 gap lock 사용한다면:
    - A트랜잭션이 pk=5를 INSERT하는 과정에서 pk=3~5에는 gap lock 걸림
    - B트랜잭션이 pk=4에 INSERT 시도시 pk=3~5에 gap lock이 걸려있기때문에 A가 트랜잭션이 완전히 종료될 때 까지 기다려야 한다.
    - 대기시간 존재!
  - Insert Intention Lock 사용시:
    - A트랜잭션이 pk=5를 INSERT하는 과정에서 pk=3~5에는 insert intention lock 걸림
    - B트랜잭션이 pk=4에 INSERT 시도시 pk=3~5에 insert intention lock이 걸려있더라도 pk가 겹치지 않기때문에 바로 진행 가능
    - 대기시간 없음!
    - 실제 InnoDB의 동작 방식

#### Auto-INC Lock
> 여러 트랜잭션이 동시에 실행될때 AUTO_INCREMENT 컬럼의 값을 일관성 있게 증가시키기 위해 필요한 lock

### Lock 이 해제되는 타이밍

transaction 이 모두 `commit` 되거나 `rollback` 될 때 함께 unlock

### 레퍼런스
[Lock으로 이해하는 Transaction의 Isolation Level](https://suhwan.dev/2019/06/09/transaction-isolation-level-and-lock/)

[Mysql innoDB Lock, isolation level과 Lock 경쟁](https://taes-k.github.io/2020/05/17/mysql-transaction-lock/)

[[MySQL]MySQL 벼락치기(5) - 갭락(Gap Lock)과 넥스트 키 락(Next-Key Lock)](https://idea-sketch.tistory.com/46)

