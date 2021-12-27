---
layout: post
title: <Spring> AOP 란?
categories: 스프링
tags: [SPRING]
---
> 어떤 로직을 기준으로 **핵심적인 관점, 부가적인 관점**으로 나누어 보고 그 관점을 기준으로 각각 **모듈화**하여 개발하는 프로그래밍

`핵심적인 관점` : 시스템의 목적에 해당하는 주요 로직 / 비즈니스 로직<br>
`부가적인 관점` : 시스템의 여러 부분에 걸쳐 공통적이고 반복적으로 필요로 하는 처리내용 / 데이터베이스 연결, 로깅, 파일 입출력, 트랜잭션 등<br>
- AOP는 시스템의 여러 영역에 걸쳐 공통적이고 반복적으로 적용된 부가적인 관점을 분리하여 별도의 모듈에서 설계, 구현,운영하는 프로그래밍 기법
- 클래스 혹은 메소드마다 흩어져 있는 코드들을 모듈화하고 분리하여 재사용


### 🥺 AOP 가 필요한 경우 

아래 예제에서 `코드AAAAAAAAA`, `코드BBBBBBBBB` 는 class A 의 method a(), b() 및 class B 의 method c() 에서 동시에 사용되고 있다.<br> 
이렇게 코드가 필요한 곳마다 하드코딩 되어 있다고 했을 때 코드 중 한 군데라도 변경되면 사용하는 모든 곳을 변경해줘야 한다. 

```java
class A {
	method a() {
		코드AAAAAAAAA
		System.out.println("a 메소드입니다");
		코드BBBBBBBBB
	}
	
	method b() {
		코드AAAAAAAAA
		System.out.println("b 메소드입니다");
		코드BBBBBBBBB
	}
}

class B {
	method c() {
		코드AAAAAAAAA
		System.out.println("c 메소드입니다");
		코드BBBBBBBBB
	}
}
```


그렇기 때문에 밑의 예제처럼 중복되는 코드를 별도의 클래스 메소드로 빼서 실행될 수 있도록 하는 걸 **AOP** 라고 한다. 

```java
class A {
	method a() {
		System.out.println("a 메소드입니다");
	}
	
	method b() {
		System.out.println("b 메소드입니다");
	}
}

class B {
	method c() {
		System.out.println("c 메소드입니다");
	}
}

class AAAAAAAAABBBBBBBBB {
	method aaaaaabbbbbb(JoinPoint point) {
		코드AAAAAAAAA
		point.execute();
		코드BBBBBBBBB
	}
}
```



### 다양한 AOP 구현 방법 

| 방법            | 설명                                                         |
| --------------- | ------------------------------------------------------------ |
| 컴파일          | 컴파일러가 .java 파일을 .class 로 바꿀 때 코드를 넣어주는 방법<br>A.java -- (AOP) --> A.class [**AspectJ**] |
| 바이트코드 조작 | 클래스로더에서 메모리로 올라갈 때 코드를 넣어주는 방법<br>A.java ---> A.class -- (AOP, 클래스로더) --> 메모리 [**AspectJ**] |
| 프록시 패턴     | 스프링 AOP 사용 방법<br>A 클래스의 Bean을 만들 때 A 타입의 Proxy Bean을 만들어 Aspect 코드를 추가하여 동작 |



### 💨 프록시 패턴이란?

> 기존 코드 건드리지 않고 새로운 기능 추가하기

- 스프링의 경우 `프록시 기반의 AOP` 를 지원 
- 런타임 시점 (스프링 컨테니어 로딩 시점) 에 자동 프록시 생성기에서 주요 Target 클래스 빈에 대해 부가 기능을 실행해줄 프록시 빈을 만들어준다.

```java
// Subject
public interface Payment {
  void pay(int amount);
}

// Client
public class Store { 
  Payment payment;
  
  public Store(Payment payment) {
    this.payment = payment;
  }
  
  public void buySomething() {
    payment.pay(100);
  }
}

// RealSubject
public class Card implements Payment {
  @Override
  void pay(int amount) {
    System.out.println(amount + "현금 결제");
  }
}

// Proxy
public class ProxyCard implements Payment {
  Payment cash = new Cash();
  
  @Override
  void pay(int amount) {
    if (amount > 100) {
      System.out.println(amount + "신용 카드");
    } else {
      cash.pay(amount);
    }
  }
}

public class StoreTest {
  @Test
  public void cardTest() {
    Payment proxyCard = new ProxyCard();
    Store store = new Store(proxyCard);
    store.buySomething();
  }
}

```

- Subject, Client, RealSubject 의 코드는 변경하지 않고 Proxy 코드를 만들어서 필요한 코드를 추가 
- Proxy 코드가 스프링에서 자동으로 빈이 등록될 때 만들어진다고 생각하면 됨 


### 스프링 AOP

- 프록시 기반의 AOP 구현체 
- 스프링 빈에만 AOP 적용 가능 


#### ⏰ @LogExecutionTime 으로 메소드 처리 시간 로깅하기

@LogExecutionTime 어노테이션 (어디에 적용할지 표시 해두는 용도)

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface LogExecutionTime {
}
```



실제 Aspect (@LogExecutionTime 애노테이션 달린곳에 적용)

```java
@Component // bean 등록
@Aspect
public class LogAspect {

   Logger logger = LoggerFactory.getLogger(LogAspect.class);

   @Around("@annotation(LogExecutionTime)")  
   public Object logExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
       StopWatch stopWatch = new StopWatch();
       stopWatch.start();

       Object proceed = joinPoint.proceed(); // joinPoint : 어노테이션이 붙어 있는 메소드 , proceed : 실행
     
       stopWatch.stop();
       logger.info(stopWatch.prettyPrint());
       return proceed;
   }
}
```
➡️ 스프링이 제공해주는 어노테이션 기반의 AOP 


#### 🔖 스프링 어노테이션  

| 이름        | 설명                                                         |
| ----------- | ------------------------------------------------------------ |
| @Aspect     | 흩어져있는 관심사인 부가 기능들이 모듈화한 것                |
| @Target     | 모듈화된 부가 기능(Aspect)을 수행해줘야 하는 주요 비지니스 클래스 or 메서드 등을 의미 |
| @Advice     | Target 에 종속되지 않고 순수하게 부가 기능만 실제로 구현된 부분, 부가 기능 구현체 |
| @JointPoint | 구현된 부가 기능(Advice)을 실제 수행할 지점                  |
| @PointCut   | JointPoint의 상세한 스펙을 정의한 것. 부가기능을 수행할 지점(Advice)에 대한 스펙 정의서 |



### 🙇🏻‍♀️ 레퍼런스
- [예제로 배우는 스프링 입문 (개정판) - 스프링 AOP](https://www.inflearn.com/course/spring_revised_edition/lecture/19742?tab=curriculum&volume=0.37&quality=auto)<br>
- [[스프링] Spring AOP 기초](https://velog.io/@dusdn2424/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%8A%A4%ED%94%84%EB%A7%81-AOP-%EC%97%90-%EB%8C%80%ED%95%98%EC%97%AC#%EB%AC%B8%ED%97%8C)<br>
- [[Spring] 스프링 AOP 무작정 사용하기](https://velog.io/@ksh030506/Spring-%EC%8A%A4%ED%94%84%EB%A7%81-AOP-%EB%AC%B4%EC%9E%91%EC%A0%95-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0)<br>
- [[Spring] 스프링 AOP 개념 이해와 적용 방법 (+Proxy 프록시)](https://creamilk88.tistory.com/148)

  