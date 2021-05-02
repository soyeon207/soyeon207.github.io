---
layout: post
title: 인터프리터 패턴 (Interpreter Pattern)
categories: 디자인패턴
tags: [디자인패턴, 행동패턴]
---

### 인터프리터 패턴 (Interpreter Pattern)
- 문장을 해석할 떄 사용하는 패턴 , 즉 간이언어를 만들기 위한 패턴 
- 문법 규칙을 클래스화 한 구조로써 일련의 규칙으로 정의된 언어를 해석하는 패턴
- 문법 규칙이 많아지면 복잡해지고 무거워지기 때문에 그때에는 파서 / 컴파일러를 쓰는게 좋다 
- 언어 분석기라고 생각하면 되며, 스크립트나 컴파일러 문법 등이 있을수 있다 
- 예로 들어서 SQL 구문이나 Shell 커맨드 해석기 , 통신 프로토콜 등이 있다 
- EX) 윈도우 운영체제 운영 프롬포트 : 명령어를 입력하면 여러 작업을 수행할 수 있게 해준다. 명령어는 명령 프롬포트에서 약속된 문법이다 명령어와 같이 약속된 문법을 해석하게 할 수 있는 해석자 패턴을 사용하면 클라이언트가 전달하는 구문을 해석하여 이에 대응되는 처리를 수행할 수 있도록 해 준다.

### 언제 사용할까?
- 정의할 언어의 문법이 간단할 때 (문법이 복잡하면 관리할 클래스가 많아져 복잡해짐)
- 성능이 중요한 문제가 되지 않을 때

### 구조
![https://user-images.githubusercontent.com/39696812/116789691-f9814580-aaea-11eb-9ef0-3702c4c47927.png](https://user-images.githubusercontent.com/39696812/116789691-f9814580-aaea-11eb-9ef0-3702c4c47927.png)
- Interpreter Context : 문장을 저장하고 관리하는 클래스, String 표현식이어야 하며 인터프리터에 보내는 정보, 해당 context 들을 해석함 
- Abstract Expression : 문장을 해석하기 위한 해석자 인터페이스 클래스, interpret()를 정의 
- Terminal Expression(Concrete Expression) : 문장의 종료를 나타내는 해석자 클래스 구현체, interpret()를 구현, 문법 해석의 마지막으로 문자 또는 숫자로 치환되지 않는 최종적인 기호 
- Non-Terminal Expression(Concrete Expression) : 문장의 비종료를 나타내는 해석자 클래스 구현체, Non-Terminal의 interpret()를 구현
- Client : interpreter() 를 호출 

#### Non-Terminal Expression - AbstractExpression
- Aggregation (Shared Aggregation, 집합)
- 전체(비어있는 다이아몬드)와 부분의 관계를 가진다 
- EX) 전체 : user, 부분 : address

#### Terminal vs Non-Terminal
- Terminal(상수) : id, +, -, *, / , 정의된 언어의 알파벳이나 기호로서 영문자의 소문자나 아라비아 숫자, 연산자 기호 등
- Non-Terminal(함수) : `<expression>`, `<term>`, `<factor>`, 언어에서 문자열을 생성하는 데 사용되는 중간과정의 기호

```
<expression> ::= <expression> + <term>
<expression> ::= <expression> - <term>
<term> ::= <term> * <factor>
<term> ::= <term> / <factor>
<term> ::= <factor>
<factor> ::= id
```

5 + 4 - 3 + 7 - 2
![dsd](https://miro.medium.com/max/978/1*bbknPwwsOMcU7vLJK1fSdQ.png)


### 장점, 단점 
#### 장점
- 문법의 추가 및 수정, 구현이 쉬워진다 

#### 단점
- 복잡한 문법의 경우 관리 및 유지가 어렵다
- 성능이 좋지 않을 수 있다 

### 실제 사용 
java.util.regex.Pattern : 정규표현식

java.text.Normalizer : 텍스트 정규화 (미리 정의된 규칙에 맞추어 텍스트를 변환하는 작업 )

### 레퍼런스
[https://booolean.tistory.com/471](https://booolean.tistory.com/471)

[https://blackinkgj.github.io/4_Grammar/](https://blackinkgj.github.io/4_Grammar/)

[https://medium.com/design-patterns-in-python/interpreter-pattern-ed521735906b](https://medium.com/design-patterns-in-python/interpreter-pattern-ed521735906b)

[https://copynull.tistory.com/147](https://copynull.tistory.com/147)

[https://beomseok95.tistory.com/288](https://beomseok95.tistory.com/288)
