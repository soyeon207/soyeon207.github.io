---
layout: post
title: 플라이 웨이트 패턴 (Flyweight pattern)
categories: 디자인패턴
tags: [디자인패턴, 플라이웨이트패턴, 구조패턴]
---

### 플라이웨이트 패턴 (flyweight pattern)
- 동일하거나 유사한 객체들 사이에 가능한 많은 데이터를 서로 공유하여 사용하도록 하여 메모리 사용량을 최소화하는 소프트웨어 디자인 패턴
- 객체를 직접 만드는게 아니라 없다면 만들고, 있으면 객체를 공유
- 많은 수의 객체를 생성해야 할 떄 주로 쓰임 
- 팩토리 메소드 안 객체를 새로 생성 -> 생성하는 객체가 내부적으로 참조하는 객체에 대해, 기존에 있는 객체를 참조만 하는 식으로 객체를 구성

### 구조
![https://user-images.githubusercontent.com/39696812/116422072-588c5380-a87a-11eb-9f36-ea0fff259b6e.png](https://user-images.githubusercontent.com/39696812/116422072-588c5380-a87a-11eb-9f36-ea0fff259b6e.png)
- Flyweight : 공유할 클래스 인터페이스 선언
- ConcreteFlyweight : 클래스 정의 
- FlyweightFactory : Flyweight의 인스턴스 생성 혹은 공유 
- Client : 사용자 

### 예제 
**Flyweight : 공유할 클래스 인터페이스 선언**
```JAVA
public interface Food {
  public void eat();
}
```

**ConcreteFlyweight : 클래스 정의**
```JAVA
public class pizza implements Food {

  public Pizza() {
    System.out.println("피자 !!");
  }

  @Override
  public void eat() {
    System.out.println("먹기 !! ");
  }
}

public class Hamburger implements Food {
  public Hamburger() {
    System.out.println("햄버거 !!");
  }

  @Override
  public void eat() {
    System.out.println("먹기 !!");
  }
}
```



**FlyweightFactory : Flyweight의 인스턴스 생성 혹은 공유**

**Client : 사용자**


- 자바 Stirng pool
  - 컴파일 할 때 스트링 객체로 선언되어 있는 것들 jvm heap 에 들어가고 같은 내용이 선언되면 기존의 객체 참조

### 레퍼런스
- [https://ko.wikipedia.org/wiki/%ED%94%8C%EB%9D%BC%EC%9D%B4%EC%9B%A8%EC%9D%B4%ED%8A%B8_%ED%8C%A8%ED%84%B4](https://ko.wikipedia.org/wiki/%ED%94%8C%EB%9D%BC%EC%9D%B4%EC%9B%A8%EC%9D%B4%ED%8A%B8_%ED%8C%A8%ED%84%B4)
- 
