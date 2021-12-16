---
layout: post
title: <JAVA> final 이란?
categories: 자바
tags: [JAVA]
---

## \<JAVA\> final 이란 ?

> 변하지 않는 값

### 1. final 이란 ?

- 일단 초기화 되고 나면 절대 그 값을 변경할 수 없음

```java
final String finalStr = "finalStr";
finalStr = "change finalStr"; 			// ** 오류 발생 **
```


### 2. 사용될 수 있는 곳

| 이름     | 설명                                                         |
| -------- | ------------------------------------------------------------ |
| 클래스   | 변경될 수 없는 클래스, 확장 될 수 없는 클래스                |
| 메서드   | 변경될 수 없는 메서드, 상속으로 **오버라이딩**을 통해 재정의 될 수 없다 |
| 멤버변수 | 상수                                                         |
| 지역변수 | 상수                                                         |



### 3-1. 클래스

- final 이 붙어 있는 클래스는 **상속 불가**
- 다른 클래스에서 상속하여 재정의할 수 없음
- 보안이나 효율성에서 장점이 있음
- EX) Integer 과 같은 랩퍼(Wrapper) 클래스

```java
final class Test {

}

class Test2 extends Test{ // ** 오류 발생 **

}
```

![finalClassError](/assets/images/finalClassError.png)

### 3-2. 메서드

- **override 제한**
- 상속 받은 클래스가 해당 클래스를 수정하지 못하도록 함

```java
class Test {
    void TestMethod() { System.out.printf("TestMethod"); }
    final void testMethod2() { System.out.printf("TestMethod2"); }
}

class Test2 extends Test {
    void TestMethod() { System.out.printf("overriding TestMethod"); }
    void testMethod2() { System.out.printf("overriding TestMethod2"); } // ** 오류 발생 **
}
```

![finalClassError](/assets/images/finalMethodError.png)


### 3-3. 멤버변수 / 지역변수

- 한 번 값을 할당하면 **수정할 수 없음**

- 무조건 값이 할당되어야 함. 아니면 에러 발생



초기값을 주는 방법

1. 변수 선언 시 할당
2. 생성자를 통해 할당

```java
class Test {
    final String finalVar1 = "finalVar1"; // 1. 변수 선언시 할당
    final String finalVar2;
    final String finalVar3; // ** 값이 할당되지 않은 경우 에러 발생 **

    
    public Test() {
        finalVar2 = "finalVar2"; // 2. 생성자를 통해 할당 
    }
}
```




### 3-4. 파라미터

- 파라미터로 넘어온 값 수정 불가능

```java
class Test {
    public String returnStr(final String str) {
        str = "change"; // ** 에러 발생 **
        return str;
    }
}
```

![finalClassError](/assets/images/finalVarError.png)


### 4. static 과 final

- 모든 객체가 공유하는 불변의 값
- 보통 같이 많이 사용함

EX) PI 값 `public static final PI = 3.14;`


### 레퍼런스

[Java - final](https://www.youtube.com/watch?v=61BuIBK7AZk)<br>
[[Java] final 그게 뭔데, 어떻게 쓰는 건데 ](https://makemethink.tistory.com/184)

