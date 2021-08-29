---
layout: post
title: 트랜잭션 격리 수준 (isolation Level)
categories: DB
tags: [DB]
---

## 트랜잭션 격리 수준 (isolation Level)
> 동시에 여러 트랜잭션이 실행된 경우 어떻게 동작할지를 명시

- Dirty Read (비커밋 읽기)

  : 트랜잭션에서 커밋되기 전 작업 내용을 다른 트랜잭션에서 볼 수 있음 

  EX) A 라는 트랜잭션에서 id : 22, name: 이름을 insert 하고 커밋하지 않은 상태에서 B 라는 트랜잭션이 id: 22 를 select 하는 경우 읽을 수 있음. 이때 A 트랜잭션에서 커밋되지 않고 롤백되는 경우 데이터 정합성에 문제가 생김 

  👉 **가능한 이유**👈

  InnoDB 엔진이 transaction 을 커밋하는 방식 때문에 가능. InnonDB 는 일단 commit 이 되지 않더라도 실행한 모든 쿼리를 DB 에 적용. 즉, 특별히 log를 보고 특정 시점의 snapshot을 복구하는 **consistent read**를 하지 않고 그냥 해당 시점의 DB 를 읽으면 dirty read 

  🌟 **consistent read(일관적 읽기)**

  : select 시 현재 DB 값이 아닌 특정 시점의 DB snapshot 을 읽어 오는 것 

  : 이 snapshot 은 commit 된 변화만이 저장된 것을 의미 

  : InnoDB 엔진은 각 쿼리를 실행할 때마다 실행한 쿼리의 log를 차곡차곡 저장한다. 그리고 나중에 consistent read를 할 때 이 log를 통해 특정 시점의 DB snapshot을 복구하여 가져온다.

  
- Non-Repeatable Read (비반복 읽기)

  : 한 트랜잭션에서 같은 쿼리를 두번 실행시 그 사이 다른 트랜잭션이 값을 수정, 삭제 해서 두 쿼리의 결과가 상이하게 나타남 

- Phantom Read (가상 읽기)

  : 한 트랜잭션 안에서 일정 범위의 레코드를 두번 이상 읽을 때, 첫번째 쿼리에서 없던 레코드가 두번째 쿼리에서 나타나는 현상


격리 수준은 해당 문제가 발생하는지에 따라 분류

| 분리 레벨        | Dirty Read | Non-Repeatable Read | Phantom Read |
| ---------------- | ---------- | ------------------- | ------------ |
| READ UNCOMMITEED | 발생       | 발생                | 발생         |
| READ COMMITTED   | -          | 발생                | 발생         |
| REPEATABLE READ  | -          | -                   | 발생         |
| SERIALIZABLE     | -          | -                   | -            |

1. READ UNCOMMITTED
   - **SELECT 쿼리를 실행 할 때 다른 트랜잭션에서 커밋되지 않은 데이터를 읽을 수 있음**
   - Dirty Read, Non-Repeatable Read, Phantom Read 발생
   - 오라클은 이 레벨 지원 X

2. READ COMMITTED
   - **커밋 완료된 데이터만 읽을 수 있는 격리수준**
   - Non-Repeatable Read, Phantom Read 발생
   - DB2, SQL Server, Sybase의 경우 Shared Lock을 이용해서 구현, 하나의 레코드를 읽을 때 Lock을 설정하고 해당 레코드에서 빠지는 순간 Lock해제 => Select 문으로 읽을 때만 해당 Row 에 대해 S Lock 이 걸림 
   - Oracle은 Lock을 사용하지 않고 쿼리시작 시점의 Undo 데이터를 제공하는 방식으로 구현
   - 매번 read 마다 consistent read(일관적 읽기)를 통한 새로운 snapshot 을 생성
     ➡️ Non-Repeatable Read, Phantom Read 발생 
   - Record lock 만 사용하고 Gap lock 사용 X

3. REPEATABLE READ
   - **하나의 트랜잭션에서 반복되는 SELECT 는 항상 같은 값을 조회**
   - 처음 read 한 시간을 기록하고 그 이후 모든 select 마다 처음 시점을 기준으로 consistent read 수행 
     ➡️ Non-Repeatable Read 발생 X 
   - 트랜잭션이 완료 될 때까지 해당 데이터에 대한 갱신, 삭제가 불가능 (해당 row 에 Shared Lock 이 걸림)
   - InnoDB 엔진에서 사용하는 기본 격리 수준
   - DB2, SQL Server의 경우 트랜잭션 고립화 수준을 Repeatable Read로 변경하면 읽은 데이터에 걸린 공유 Lock을 커밋할 때까지 유지하는 방식으로 구현 => 트랜잭션이 커밋 될 때까지 Select 문으로 읽은 Row 에 S Lock 이 걸림
   - Oracle은 이 레벨을 명시적으로 지원하지 않지만 for update절을 이용해 구현 가능,

4. SERIALIZABLE
   - **트랜잭션이 완료 될 때까지 그 영역에 해당하는 데이터 수정(UPDATE) 및 입력(INSERT) 불가능** 
   - 기본적으로 REPEATABLE READ 와 동일하지만 `SELECT` 쿼리가 전부 `SELECT ... FOR SHARE`로 자동 변경
     -> 모든 select row 에 대해 S Lock 이 걸리고 X Lock (update, delete 등 불가능)<br>
     -> S Lock 걸린 row 에 update, delete 시도 시 deadlock
   - 동시성이 가장 떨어지고 쉽게 Dead Lock 에 걸릴 수 있음
   - 많이 사용하지 않음 

### 레퍼런스

[트랜잭션, 트랜잭션 격리수준(Isolation Level)](https://feco.tistory.com/45)

[트랜잭션 수준 읽기 일관성](http://wiki.gurubee.net/pages/viewpage.action?pageId=3900389)

[트랜잭션의 격리 수준(isolation Level)이란?](https://nesoy.github.io/articles/2019-05/Database-Transaction-isolation)

[[MySQL] 트랜잭션의 격리 수준(isolation level)](https://deveric.tistory.com/85)

