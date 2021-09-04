---
layout: post
title: <DB> 인덱스(index) 란 ?
categories: DB
tags: [DB, index]
---

### [DB] 인덱스(index) 란 ?

### 개념 

> **인덱스**([영어](https://ko.wikipedia.org/wiki/영어): index)는 [데이터베이스](https://ko.wikipedia.org/wiki/데이터베이스) 분야에 있어서 [테이블](https://ko.wikipedia.org/wiki/테이블_(데이터베이스))에 대한 동작의 속도를 높여주는 [자료 구조](https://ko.wikipedia.org/wiki/자료_구조)를 일컫는다. 인덱스는 테이블 내의 1개의 컬럼, 혹은 여러 개의 컬럼을 이용하여 생성될 수 있다. 고속의 검색 동작뿐만 아니라 레코드 접근과 관련 효율적인 순서 매김 동작에 대한 기초를 제공한다. 인덱스를 저장하는 데 필요한 디스크 공간은 보통 테이블을 저장하는 데 필요한 디스크 공간보다 작다. (왜냐하면 보통 인덱스는 키-필드만 갖고 있고, 테이블의 다른 세부 항목들은 갖고 있지 않기 때문이다.) [관계형 데이터베이스](https://ko.wikipedia.org/wiki/관계형_데이터베이스)에서는 인덱스는 테이블 부분에 대한 하나의 사본이다. 인덱스는 고유 제약 조건을 실현하기 위해서도 사용된다. 고유 인덱스는 중복된 항목이 등록되는 것을 금지하기 때문에 인덱스의 대상인 테이블에서 고유성이 보장된다. - 위키백과

- INDEX = 색인 <br>
EX) 만쪽이 넘는 책에서 내가 원하는 부분을 찾을려고 할 때 한장한장 넘겨가면서 찾으면 백만년 걸림 <br>
=> 색인이나 목차 **한번에** 찾을 수 있음 

- DB 의 인덱스도 DB 에 대한 색인이나 목차라고 생각하면 된다. 없는 경우 테이블의 **처음부터 끝(table full scan)**까지 모두 뒤져야 하지만 인덱스가 걸려 있다면 더욱 빠르게 찾을 수 있다 

- 인덱스로 들어온 칼럼값들은 값을 정렬하여 순차적으로 저장됨 <br>
=> 새로운 데이터 값이 들어오는 경우 이를 순차적으로 찾아 정리하기 때문에 새로운 데이터를 insert, delete 하는 것은 시간이 오래 걸리지만 search 는 빨리 됨 

### 인덱스 사용 이유 
- WHERE 구문과 일치하는 열을 **빨리 찾기 위해.**
- 특정 열을 고려 대상에서 **빨리 없애 버리기 위해.**
- **조인 (join)을 실행**할 때 **다른 테이블에서 열을 추출하기 위해.**
- **특정하게 인덱스된 컬럼**을 위한 **MIN() 또는 MAX() 값을 찾기 위해.**
- 사용할 수 있는 키의 최 좌측 접두사(leftmost prefix)를 가지고 **정렬 및 그룹화를 하기 위해.**
- 데이터 열을 참조하지 않는 상태로 값을 추출하기 위해서 **쿼리를 최적화 하는 경우.**

### Index 구조 
- Index 는 논리적/물리적으로 테이블과 독립적
- 테이블은 컬럼에 데이터가 정렬되지 않고 입력된 순서대로 들어가지만, Index는 KEY 컬럼과 ROWID 컬럼 두개로 이루어져 있고 오름차순, 내림차순으로 정렬이 가능
- MySQL에서 테이블 생성 시, 아래와 같은 3가지 파일이 생성된다.
```
FRM : 테이블 구조 저장 파일
MYD : 실제 데이터 파일
MYI : Index 정보 파일 (Index 사용 시 생성)
```
사용자가 쿼리를 통해 Index를 사용하는 칼럼을 검색하게 되면, 이때 MYI 파일의 내용을 활용

### 인덱스 작동 원리 

```sql
select *
from users
where user_id = 55555
```
- 저장된 user 가 10만명일 때 위 SQL 을 실행할 경우 
  1. 서버 프로세스가 파싱 과정을 마친 후 **`DB buffer cache`에 user_id 가 55555인 정보가 있는지 확인**한다.
  2. 정보가 없으면 **`하드 디스크 파일`에서 55555 정보를 가진 블록을 복사**해서 `DB buffer cache`로 가져온 후 **55555 정보만 골라내서 사용자에게 보여줌**

- **Index 없는 경우** : 55555정보가 어떤 블록에 들어 있는지 모르므로 **10만개 전부 `db buffer cache`로 복사한 후 하나하나 찾는다.**
- **Index 있는 경우** : **where 절의 컬럼이 index가 만들어져 있는지 확인** 후, **인덱스에 먼저 가서 55555정보가 어떤 `ROWID`를 가지고 있는지 확인**한 후 **해당 `ROWID`에 있는 블록만 찾아가서 `db buffer cache`에 복사**함.

### Index를 남발하지 말아야 하는 이유
데이터베이스 서버에 성능문제가 발생하면 가장 빨리 생각하는 해결책이 인덱스 추가 생성이다.
문제가 발생할때마다 인덱스를 생성하면서 인덱스가 쌓여가는 것은 하나의 쿼리문을 빠르게는 만들 수 있지만 전체적인 데이터베이스의 성능 부하를 초래한다.

조회 성능을 극대화하려 만든 객체인데 많은 인덱스가 쌓여서 Insert, Delete, Update시에 부하가 발생해 전체적인 데이터베이스 성능을 저하한다.

그렇기에 인덱스를 생성하는것 보다는 SQL문을 좀 더 효율적으로 짜는 방향으로 나가야한다.
인덱스 생성은 마지막 수단으로 강구해야 할 문제이다.

### 인덱스의 종류 
#### B(Balanced)-tree Index
- 가장 보편적으로 사용되는 인덱스 
- 사각형으로 표시된 한개 한개의 데이터를 `노드` 라고 한다 
- 가장 상단의 노드를 `루트 노드`, 중간 노드들을 `브랜치 노드`, 가장 아래 노드들을 `리프 노드` 라고 함 
- 노드 하나에 여러 데이터가 저장될 수 있다 
- 각 노드내 데이터들은 항상 정렬된 상태이며, 데이터와 데이터 사이의 범위를 이용해서 자식 노드를 가진다 
- 항상 좌, 우 자식 노드 개수의 밸런스를 유지하기 때문에 최악의 경우에도 탐색 시간이 ` O(logN)`
- 등호 (=) 나 부등호 (<, >) 를 사용하기에도 적합하다 
- 각 노드는 최대 2개의 key, 최대 3개의 child 를 가질 수 있다 
- 각 노드마다 레코드 포인터가 필요

![https://user-images.githubusercontent.com/39696812/124501489-9e2a4700-ddfc-11eb-8627-d9a2ac9a7777.png](https://user-images.githubusercontent.com/39696812/124501489-9e2a4700-ddfc-11eb-8627-d9a2ac9a7777.png)


#### B+Tree Index
- InnoDB 는 B+tree Index 로 이뤄져 있음 
- B-tree 의 확장 개념 
- 자식 노드가 2개 이상인 B-Tree 를 개선시킨 자료구조 
- 리프노드만 레코드 포인터, 인덱스와 데이터를 가지고 있고 나머지 노드들은 데이터를 위한 인덱스만 가지고 있음 
- 리프 노드들은 LinkedList 로 연결 

![https://media.vlpt.us/images/emplam27/post/bcbce100-d475-4cda-aebe-946d1813949c/B%ED%94%8C%EB%9F%AC%EC%8A%A4%20%ED%8A%B8%EB%A6%AC%20%EA%B8%B0%EB%B3%B8%20%ED%98%95%ED%83%9C.jpg](https://media.vlpt.us/images/emplam27/post/bcbce100-d475-4cda-aebe-946d1813949c/B%ED%94%8C%EB%9F%AC%EC%8A%A4%20%ED%8A%B8%EB%A6%AC%20%EA%B8%B0%EB%B3%B8%20%ED%98%95%ED%83%9C.jpg)


#### Bitmap Index
- 컴퓨터에서 사용하는 최소단위인 비트를 이용하여 컬럼값을 저장하고, ROWID를 자동으로 생성하는 인덱스의 한 방법


#### IOT Index
- Random 액세스가 발생하지 않도록 테이블을 아예 인덱스 구조로 생성되어 있는 Table
- 테이블을 찾아가기 위한 rowid를 갖는 일반 인덱스와는 달리 IOT는 모든 행 데이터를 리프블록에 저장


### clustered vs secondary INDEX
#### SQL 서버의 데이터 관리
- 책이 page 로 구성되어 있는 것처럼 SQL 서버도 Page로 구성되어 있음 
- 가장 기본적인 단위로 실제 SQL 서버에서 데이터에 접근시에도 Page 단위로 접근함

#### 인덱스가 없는 데이터구조
![https://blog.kakaocdn.net/dn/cVdPtt/btqzxDrcU0t/QqnI54VNjRNyNRwUYRE3i0/img.png](https://blog.kakaocdn.net/dn/cVdPtt/btqzxDrcU0t/QqnI54VNjRNyNRwUYRE3i0/img.png)
- 데이터페이지 자체가 실제 데이터가 저장되어 있는 곳이라고 생각하면 됨 


#### Clustered Index 

> 페이지를 알고 있어서 해당 페이지를 바로 펼침 

- 루트 페이지와 리프페이지로 구성되며, 리프페이지는 데이터 그 자체 (= index 자체에 데이터가 포함)
- 물리적으로 정렬되어 있기 때문에 검색속도가 빠름
- 테이블 당 **하나만** 생성 가능 <br>
=> MySQL 에선 Primary Key 가 있는 경우 Primary Key , 없으면 UNIQUE 하면서 NOT NULL 인 컬럼 , 그것도 없다면 임의로 보이지 않는 컬럼을 만들어서 Clustered Index 로 지정 
- 생성 시에는 데이터 페이지 전체가 다시 정렬 
- 영어 사전처럼 책의 내용 자체가 순서대로 정렬되어 있어 인덱스 자체가 책의 내용과 같음 
- 보조 인덱스 보다 검색 속도는 더 빠르다. 하지만 입력/수정/삭제는 더 느리다.
- 데이터 검색 순서 : `루트페이지 -> 리프 페이지(=데이터 페이지)`
- 리프 페이지가 꽉차 있는 경우 **페이지 분할 발생** <br>
=> select 성능은 좋지만 insert, update, delete 성능은 떨어짐 

**클러스터 인덱스 구조**
![https://blog.kakaocdn.net/dn/rft7X/btqzAigzqEW/PNKLLFlvmZTSWXik1Ml7C1/img.png](https://blog.kakaocdn.net/dn/rft7X/btqzAigzqEW/PNKLLFlvmZTSWXik1Ml7C1/img.png)

**클러스터 인덱스 검색**
```
SELECT * FROM TestTbl WHERE UserInitial = 'FFF'
```
![https://blog.kakaocdn.net/dn/mh898/btqzAjs06s0/Kpg4XuaMSWh5vlKAhw5seK/img.png](https://blog.kakaocdn.net/dn/mh898/btqzAjs06s0/Kpg4XuaMSWh5vlKAhw5seK/img.png)
1. 인덱스의 루트페이지를 접근해서 찾는 값이 어떤 리프 페이지(데이터 페이지)에 있는지 확인
2. 리프 페이지로 이동 후에 페이지의 내부 행들을 검색해서 해당 데이터를 찾음
=> 총 2개의 페이지(루트 페이지 + 리프 페이지)를 참조

**클러스터 인덱스 데이터 삽입**
![https://blog.kakaocdn.net/dn/bqVdGF/btqzC3BMJyP/s4vwIK5a4yYQuf4C4X7KuK/img.png](https://blog.kakaocdn.net/dn/bqVdGF/btqzC3BMJyP/s4vwIK5a4yYQuf4C4X7KuK/img.png)
- 인덱스를 통해서 삽입 위치를 찾고, 만약 해당 페이지에 공간이 없는 경우 페이지 분할 작업이 발생 


#### Secondary Index (Non Clustered Index)
> 목차에서 찾고자 하는 내용의 페이지를 찾고 나서 해당 페이지로 이동

- 테이블 당 **여러개** 생성 가능 
- 생성시에 데이터 페이지는 그냥 둔 상태에서 별도의 페이지에 인덱스를 구성.
- 인덱스 자체의 리프 페이지는 데이터가 아니라 데이터가 위치하는 주소값(RID)
- 클러스터형 보다 검색 속도는 더 느리지만 데이터의 입력/수정/삭제는 덜 느리다.
- 보조 인덱스는 여러 개 생성할 수 있다. 함부로 사용할 경우에는 오히려 성능을 떨어뜨릴 수 있다 
- 데이터 검색 순서 : `루트페이지 -> 리프 페이지 -> 데이터 페이지`
- 리프 페이지가 모두 차있어도 **페이지 분할이 발생하지 않음** <br>
  => insert, update, delete 성능은 좋지만 select 성능은 떨어짐 
- 리프 노드에서 데이터 주소를 가지고 있고 물리적으로 데이터를 **재배열하지 않음**

**비클러스터 인덱스 구조**
![https://blog.kakaocdn.net/dn/b4HpbY/btqzCNeS4Bc/nrY08Dgy25DaTIU857bX2K/img.png](https://blog.kakaocdn.net/dn/b4HpbY/btqzCNeS4Bc/nrY08Dgy25DaTIU857bX2K/img.png)

**비클러스터 인덱스 검색**
```
SELECT * FROM TestTbl WHERE UserInitial = 'FFF'
```
![https://blog.kakaocdn.net/dn/bWNzQh/btqzAjzLGqQ/XaKhsDHzn7oNpwVWgprFl1/img.png](https://blog.kakaocdn.net/dn/bWNzQh/btqzAjzLGqQ/XaKhsDHzn7oNpwVWgprFl1/img.png)

**비클러스터 인덱스 삽입**
![https://blog.kakaocdn.net/dn/An4lh/btqzDsg1H88/sIaSyG5FxSZNmNiWn3NkZk/img.png](https://blog.kakaocdn.net/dn/An4lh/btqzDsg1H88/sIaSyG5FxSZNmNiWn3NkZk/img.png)


### 인덱스 사용시 extra 실행계획 
|제목|내용|설명|
|----|---|---|
|Extra|Using index|커버링 인덱스 (쿼리의 모든 항목이 인덱스 컬럼으로 이루어진 상태)|
|Extra|Using index condition|인덱스 컨디션 푸시다운 인덱스|
|type|index|인덱스 풀 스캔 (range 스캔이 아님)|

### 커버링 인덱스
- 원하는 데이터를 인덱스에서만 추출할 수 있는 인덱스를 의미 
- 실행계획을 봤을 때 key 항목에 선택된 인덱스가 있고 Extra 에 Using Index 가 나오면 커버링 인덱스가 사용되었다고 생각하면 된다 

**WHERE + GROUP BY** (index : a,b,c)
- GROUP BY 절에 명시된 컬럼이 인덱스 컬럼의 순서와 같아야 한다.
  group by b / group by b,a 는 적용 안됨 
- 인덱스 컬럼 중 뒤에 있는 컬럼이 GROUP BY 절에 명시되지 않아도 인덱스는 사용할 수 있다.
  group by a 적용 됨 
- 반대로 인덱스 컬럼 중 앞에 있는 컬럼이 GROUP BY 절에 명시되지 않으면 인덱스를 사용할 수 없다
  group by c 는 적용 안됨 
- 인덱스에 없는 컬럼이 GROUP BY 절에 포함되어 있으면 인덱스가 적용되지 않는다.
  group by a,b,c,d 는 적용 안됨 
- WHERE 조건과 GROUP BY가 함께 사용되면 WHERE 조건이 동등 비교일 경우 GROUP BY 절에 해당 컬럼은 없어도 인덱스가 적용된다 
  where a = 1 group by b, c 는 적용됨 

### 인덱스 장점 vs 단점 
#### 장점 
- 테이블을 조회하는 속도와 그에 따른 성능을 향상시킬 수 있다.
- 전반적인 시스템의 부하를 줄일 수 있다.

#### 단점 
- 인덱스를 관리하기 위해 DB의 약 10%에 해당하는 저장공간이 필요하다.
- 인덱스를 관리하기 위해 추가 작업이 필요하다.
- 인덱스를 잘못 사용할 경우 오히려 성능이 저하되는 역효과가 발생할 수 있다.<br>
  => INSERT, UPDATE, DELETE 등의 액션이 발생했을 때 인덱스를 최신의 정렬된 상태로 유지하기 위해서 추가적인 연산 필요 

### 레퍼런스

[[데이터베이스] B-Tree 인덱스 구조](https://beelee.tistory.com/37)<br>
[[Database] DB 인덱싱(Indexing)이란?](https://velog.io/@bsjp400/Database-DB-%EC%9D%B8%EB%8D%B1%EC%8B%B1Indexing%EC%9D%B4%EB%9E%80)<br>
[[SQL] Index(인덱스)](https://velog.io/@gillog/SQL-Index%EC%9D%B8%EB%8D%B1%EC%8A%A4)<br>
[[SQL] Clustered Index & Non-Clustered Index](https://velog.io/@gillog/SQL-Clustered-Index-Non-Clustered-Index)<br>
[[자료구조] 그림으로 알아보는 B+Tree](https://velog.io/@emplam27/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%EA%B7%B8%EB%A6%BC%EC%9C%BC%EB%A1%9C-%EC%95%8C%EC%95%84%EB%B3%B4%EB%8A%94-B-Plus-Tree)<br>
[1. 커버링 인덱스 (기본 지식 / WHERE / GROUP BY)](https://jojoldu.tistory.com/476)