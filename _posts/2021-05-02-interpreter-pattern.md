---
layout: post
title: 인터프리터 패턴 (Interpreter Pattern)
categories: 디자인패턴
tags: [디자인패턴, 행동패턴]
---

### 인터프리터 패턴 (Interpreter Pattern)
> 미니 언어로 쓰여진 프로그램 -> Interpreter(통역) -> 문제 해결

- 문장을 해석할 떄 사용하는 패턴 , 즉 간이언어를 만들기 위한 패턴 
    - 언어를 해석할 땐 보통 BNF 형태로 나타내는 경우가 많음 -> 인터프리터 패턴도 BNF 구조의 언어를 해석하기 알맞게 디자인 되어있음
- 언어 분석기라고 생각하면 되며, 스크립트나 컴파일러 문법 등이 있을수 있다 
- 예로 들어서 SQL 구문이나 Shell 커맨드 해석기 , 통신 프로토콜 등이 있다 
- EX) 윈도우 운영체제 운영 프롬포트 : 명령어를 입력하면 여러 작업을 수행할 수 있게 해준다. 명령어는 명령 프롬포트에서 약속된 문법이다 명령어와 같이 약속된 문법을 해석하게 할 수 있는 해석자 패턴을 사용하면 클라이언트가 전달하는 구문을 해석하여 이에 대응되는 처리를 수행할 수 있도록 해 준다.

#### BNF란?
- 배커스-나우어 형식(Backus-Naur Form)의 약어
- 컴퓨터 언어에서 언어의 문법을 수학적인 수식으로 나타낼 때 사용하는 언어 도구 

```
<digit> → 0 | 1 | 2 | … | 9
```

=> `<digit>`(구조이름 , LHS, 비단말 기호, Non-Terminal) 가 `0 | 1 | 2 | … | 9`(RHS, 단말 기호, Terminal) 로 정의됨을 의미

EX)
```
<digit> ::= 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9
<hex_letter> ::= A | B | C | D | E | F
<hex> ::= <digit> | <hex_letter>
```

**5 + 4 - 3 + 7 - 2**

![dsd](https://miro.medium.com/max/978/1*bbknPwwsOMcU7vLJK1fSdQ.png)

**무선 조정기로 자동차 움직이기**

자동차에게 내릴 수 있는 명령 
- 앞으로 1미터 전진(go) / 우회전(right) / 좌회전(left)
- 반복(repeat)

```
program go end // 자동차가 앞으로 감 
``` 

```
program repeat 4 go right end end // 자동차가 네모모양으로 돔
```

```
<program> ::= program <command list>
<command list> ::= <command> * end 
<command> ::= <repeat command> | <primitive command>
<repeat command> ::= repeat <number> <command list>
<primitive command> ::= go | right | left
```
- `*` : 앞의 것을 0번 이상 반복한다는 의미
- `|` : 또는 이라는 의미

- terminal expression
  - 문법 규칙에서, 더 이상 전개되지 않는 expression
  - 문법 규칙의 종착점을 의미함
  - go , right , left
- non-terminal expression
  - 문법 규칙에서, 계속해서 다시 전개되는 expression
  - `<program>` , `<command>`

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
- [BNF란? https://blog.shar.kr/969](https://blog.shar.kr/969)
- [BNF 표기법 https://perfectacle.github.io/2018/08/15/bnf/](https://perfectacle.github.io/2018/08/15/bnf/)
- [Java로 배우는 디자인패턴 입문  http://contents.kocw.or.kr/document/lec/2012/DukSung/ChoiSeungHoon/14.pdf](http://contents.kocw.or.kr/document/lec/2012/DukSung/ChoiSeungHoon/14.pdf)
- [https://booolean.tistory.com/471](https://booolean.tistory.com/471)
- [https://blackinkgj.github.io/4_Grammar/](https://blackinkgj.github.io/4_Grammar/)
- [https://medium.com/design-patterns-in-python/interpreter-pattern-ed521735906b](https://medium.com/design-patterns-in-python/interpreter-pattern-ed521735906b)
- [https://copynull.tistory.com/147](https://copynull.tistory.com/147)
- [https://beomseok95.tistory.com/288](https://beomseok95.tistory.com/288)
