---
layout: post
title: 팩토리 메소드 패턴 (Factory Method Pattern)
categories: 디자인패턴
tags: [디자인패턴, 생성패턴]
---

**[< 추상화 팩토리 패턴 보려면?](https://soyeon207.github.io/%EB%94%94%EC%9E%90%EC%9D%B8%ED%8C%A8%ED%84%B4/2021/04/24/abstract-factory-pattern.html)**

### 팩토리 메소드 패턴이란?
> **객체 생성 처리**를 서브 클래스로 분리해서 처리하도록 캡슐화 하는 패턴

- 팩토리 메소드 패턴은 디자인패턴에서 **생성패턴**에 속한다 
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
**Product : 팩토리 메소드로 생성될 객체의 공통 인터페이스**
```java
public interface Movie {
    void getCast();
    void getDirector();
}

```
**ConcreteCreator : 팩토리 메소드 구현하는 클래스**
```java
// 실제 로직 구현 팩토리 
public class Minari implements Movie{

    @Override
    public void getCast() {
        System.out.println("미나리 배우 : 윤여정, 스티븐");
    }

    @Override
    public void getDirector() {
        System.out.println("미나리 감독 : 정이삭");
    }
}

public class LaLaLand implements Movie{

    @Override
    public void getCast() {
        System.out.println("라라랜드 배우 : 라이언고슬링, 엠마스톤 등");
    }

    @Override
    public void getDirector() {
        System.out.println("라라랜드 감독 : 데이미언 셔젤");
    }

}

```

**Creator : 팩토리 메소드를 갖는 클래스**
```java
// 팩토리 클래스
public class MovieFactory {
    public static Movie getMovie(MovieName movieName) {
        if(movieName == MovieName.LALALAND) {
            return new LaLaLand();
        } else if(movieName == MovieName.MINARI) {
            return new Minari();
        } else {
            System.out.printf("해당하는 영화가 없습니다.");
            return null;
        }
    }
}

public enum MovieName {
    LALALAND, MINARI
}
```

```java
public class Main {
    public static void main(String args[]) {

        System.out.println("라라랜드 관련 정보 >>> ");
        Movie LaLaLand = MovieFactory.getMovie(MovieName.LALALAND);
        LaLaLand.getCast();
        LaLaLand.getDirector();
        System.out.println(">>>>>>>>>>>>>>>>>.");

        System.out.println("미나리 관련 정보 >>> ");
        Movie Minari = MovieFactory.getMovie(MovieName.MINARI);
        Minari.getCast();
        Minari.getDirector();
        System.out.println(">>>>>>>>>>>>>>>>>.");

    }
}

```
- 팩토리메소드인 watchMovie 에서 객체를 생성 후 반환해주기 때문에 FactoryMain 에서는 객체에 대한 정보를 가지고 있지 않아도 된다 

### 장점 vs 단점 
**장점**
1. 객체의 자료형이 하위 클래스에 의해서 정해지기 때문에 확장에 용이 
2. 구체 클래스와 클라이언트 와의 결합도가 낮아지기 때문에 유지보수 용이

**단점**
1. 계속해서 하위 클래스를 생성해야 하기 때문에 불필요하게 많은 클래스를 정의

### 레퍼런스
- [https://gmlwjd9405.github.io/2018/08/07/factory-method-pattern.html](https://gmlwjd9405.github.io/2018/08/07/factory-method-pattern.html)
- [https://johngrib.github.io/wiki/factory-method-pattern/](https://johngrib.github.io/wiki/factory-method-pattern/)
- [https://velog.io/@ha0kim/Design-Pattern-%EC%83%9D%EC%84%B1-%ED%8C%A8%ED%84%B4Creational-Patterns#3-%ED%8C%A9%ED%86%A0%EB%A6%AC-%EB%A9%94%EC%84%9C%EB%93%9C-%ED%8C%A8%ED%84%B4factory-method-pattern](https://velog.io/@ha0kim/Design-Pattern-%EC%83%9D%EC%84%B1-%ED%8C%A8%ED%84%B4Creational-Patterns#3-%ED%8C%A9%ED%86%A0%EB%A6%AC-%EB%A9%94%EC%84%9C%EB%93%9C-%ED%8C%A8%ED%84%B4factory-method-pattern)
- [https://niceman.tistory.com/143](https://niceman.tistory.com/143)
- [https://gdtbgl93.tistory.com/19](https://gdtbgl93.tistory.com/19)
- [https://scorpio-mercury.tistory.com/18](https://scorpio-mercury.tistory.com/18)