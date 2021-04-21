---
layout: post
title: 팩토리 메소드 패턴 (Factory Method Pattern)
categories: 디자인패턴
tags: [디자인패턴, 팩토리패턴, 생성패턴]
---

### 팩토리 메소드 패턴이란?
- 팩토리 메소드 패턴은 디자인패턴에서 **생성패턴**에 속한다 
- 객체 생성 처리를 서브 클래스로 분리 해 처리하도록 캡슐화 하는 패턴 
- **[GOF]** 객체를 생성하기 위해 인터페이스를 정의하지만, 어떤 클래스의 인스턴스를 생성할지에 대한 결정은 서브클래스가 내리도록 한다
- 어떤 클래스가 자신이 생성해야하는 객체의 클래스를 예측 할수 없을 때 사용 
=> 객체 생성을 대신 해주는 공장이라고 생각하면 편함 

![팩토리 메소드 패턴 그림](https://upload.wikimedia.org/wikipedia/commons/thumb/a/a3/FactoryMethod.svg/600px-FactoryMethod.svg.png)
- Product : 팩토리 메소드로 생성될 객체의 공통 인터페이스
- ConcreteCreator : 팩토리 메소드 구현하는 클래스 
- Creator : 팩토리 메소드를 갖는 클래스 

### 구현 방법 
1. Creator를 추상 클래스로 정의하고, 팩토리 메소드는 abstract로 선언하는 방법
2. Creator가 구체 클래스이고, 팩토리 메소드의 기본 구현을 제공하는 방법

### 예제 
```java
interface Movie {
   public void ticketing();
}
```


```java
// 실제 로직 구현 팩토리 
class ActionMovie implements Movie {
    @Override
    Movie ticketing() {
        System.out.println("액션 영화를 예매했습니다");
    }
}
```

```java
// 실제 로직 구현 팩토리 
class RomanceMovie implements Movie {
    @Override
    Movie ticketing() {
        System.out.println("로맨스 영화를 예매했습니다");
    }
}
```

```java
// 팩토리 클래스
public class Theater {
    // 팩토리 메소드 - 객체 생성 후 반환 
    public Movie watchMovie(String type) {
        if (type.equals("action")) {
            return new ActionMovie();
        } else if (type.equals("romance")) {
            return new RomanceMovie();
        }

        return theater;
    }
}
```

```java
public class FactoryMain {
	public static void main(String[] args) {
        Theater theater = new Theater();

        Movie actionMovie = theater.watchMovie("action");
    }
}
```
- 팩토리메소드인 watchMovie 에서 객체를 생성 후 반환해주기 때문에 FactoryMain 에서는 객체에 대한 정보를 가지고 있지 않아도 된다 


### 레퍼런스
- [https://gmlwjd9405.github.io/2018/08/07/factory-method-pattern.html](https://gmlwjd9405.github.io/2018/08/07/factory-method-pattern.html)
- [https://johngrib.github.io/wiki/factory-method-pattern/](https://johngrib.github.io/wiki/factory-method-pattern/)
- [https://velog.io/@ha0kim/Design-Pattern-%EC%83%9D%EC%84%B1-%ED%8C%A8%ED%84%B4Creational-Patterns#3-%ED%8C%A9%ED%86%A0%EB%A6%AC-%EB%A9%94%EC%84%9C%EB%93%9C-%ED%8C%A8%ED%84%B4factory-method-pattern](https://velog.io/@ha0kim/Design-Pattern-%EC%83%9D%EC%84%B1-%ED%8C%A8%ED%84%B4Creational-Patterns#3-%ED%8C%A9%ED%86%A0%EB%A6%AC-%EB%A9%94%EC%84%9C%EB%93%9C-%ED%8C%A8%ED%84%B4factory-method-pattern)
- [https://niceman.tistory.com/143](https://niceman.tistory.com/143)
- [https://gdtbgl93.tistory.com/19](https://gdtbgl93.tistory.com/19)