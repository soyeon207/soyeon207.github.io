---
layout: post
title: <JAVA> static 이란?
categories: 자바
tags: [JAVA]
---

### 1. static 이란 ?

static 을 번역했을 때 정적 즉 고정된 이란 의미를 가지고 있고, 여러 객체의 메소드들이 동시에 접근, 수정할 수 있는 전역변수같은 느낌이다

static 키워드를 사용한 변수를 클래스 변수라고 한다. 즉 객체가 아닌 클래스의 변수이기 때문에 new로 객체를 안만들었어도 사용이 가능하다.

ex. `System.out.println(Test.count);`


### 2. 메모리 위치

static 변수, 클래스는 JVM Runtime Area 에서 **Method Area** 에 들어간다

Method Area 는 스레드마다 존재하지 않고 모든 스레드가 공유하며, GC 가 관리 하지 않기 때문에 한번 할당하는 경우 프로그램 종료시까지 메모리가 할당된 채로 존재한다.

<div style="text-align:right; font-weight:bold">
<a href="https://soyeon207.github.io/%EC%9E%90%EB%B0%94/2021/12/08/what-is-jvm.html">
👀 &lt;JAVA&gt; 자바의 구동원리와 JVM 보러 가기 ➡️
</a>
</div>


### 3. 사용 될 수 있는 곳

멤버 변수, 메소드 , 초기화 블록


### 4-1. static 변수 (정적 변수)

> 메모리에 고정적으로 할당되어, 프로그램이 종료될 때 해제되는 변수

- 클래스가 메모리에 올라갈 때 이미 생성되기 때문에 객체를 생성하지 않고 접근이 가능하다
- 모든 곳에서 같은 곳의 메모리 주소만 바라보기 때문에 값을 공유 한다

```java
class Test {
    public static String staticVar = "staticVar";
    public String instanceVar = "instanceVar";
}

public class Main {
    public static void main(String args[]) {
        System.out.printf(Test.staticVar);
        System.out.printf(Test.instanceVar); // 에러 발생 
    }
}
```


### 4-2. static 메소드 (정적 메소드)
- 객체의 생성 없이 호출 가능
- 유틸리티 관련 메소드들은 여러번 사용되기 때문에 정적 메소드로 한번 메모리에 올라가서 사용하는게 적합하기 때문에 이때 많이 사용
  <br>EX) Math.max() 등
- static 메소드에서는 인스턴스 변수 사용 불가능
  <br>➡ 인스턴스 변수는 인스턴스가 생성되고 메모리가 할당되어야지만 사용가능하지만 static 메소드는 메모리에 올라갈 때 메모리가 할당되기 때문

```java
class Test {
    public static String staticVar = "staticVar";
    public String instanceVar = "instanceVar";
    
    public static void staticMethod() {
        System.out.printf(staticVar);
        System.out.printf(instanceVar); // 에러 발생 
    }

    public void instanceMethod() {
        System.out.printf(staticVar);
        System.out.printf(instanceVar);
    }
}

public class Main {
    public static void main(String args[]) {
        Test.staticMethod();
        Test.instanceMethod(); // 에러 발생 
    }
}

```


### 4-3. static 초기화 블록
- 클래스가 로딩 될 때 호출되며, 각 클래스당 최초 1회만 실행된다
- 생성자보다 먼저 실행됨

```java
class Test {
    static {
        System.out.println("Test static 초기화 블록");
    }

    { System.out.println("Test 초기화 블록"); }

    public Test() {
        System.out.println("생성자");
    }
}

public class Main {
    public static void main(String args[]) {
        Test t = new Test();
        Test t2 = new Test();
    }
}

/**
 * ####### 결과 #######
 * Test static 초기화 블록
 * Test 초기화 블록
 * 생성자
 * Test 초기화 블록
 * 생성자
 */

```


### 5. static 변수의 문제
1. 메모리 문제<br>
   static 은 어플리케이션이 실행되자 마자 메모리에 올라가기 때문에 호출이 자주 되지 않거나 한번도 되지 않는 경우엔 메모리 낭비로 이어질 수 있다

2. 추적<br>
   전역이라는 것은 어디서든 접근이 가능하다는 의미이기 때문에 내가 의도하지 않은 값이 들어간 경우 추적이 어렵다.


### 레퍼런스
[07-3 정적 변수와 메소드 (static)](https://wikidocs.net/228)<br>
[static 키워드 (메모리 상 위치, 클래스 변수, 클래스 메소드, static 블록)](https://sjh836.tistory.com/125)<br>
[Java\] static변수와 static 메소드](https://mangkyu.tistory.com/47)<br>
[java static키워드 바로알기](https://rockdrumy.tistory.com/214)

