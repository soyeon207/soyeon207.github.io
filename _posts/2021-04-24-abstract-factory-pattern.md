---
layout: post
title: 추상화 팩토리 패턴 (Abstract Factory Pattern)
categories: 디자인패턴
tags: [디자인패턴, 팩토리패턴, 생성패턴]
---

**[< 팩토리 메소드 패턴 보려면?](https://soyeon207.github.io/%EB%94%94%EC%9E%90%EC%9D%B8%ED%8C%A8%ED%84%B4/2021/04/22/factory-method-pattern.html)**

### 추상화 팩토리 패턴 (Abstract Factory Pattern)
- 추상화 팩토리 패턴은 디자인패턴에서 **생성패턴**에 속한다 
- **[GOF]** 상세화된 서브클래스를 정의하지 않고도 서로 관련성이 있거나 독립적인 여러 객체의 군을 생성하기 위한 인터페이스를 제공합니다.
- 서로 관련이 있는 객체들을 통째로 묶어서 팩토리 클래스로 만들고, 이들 팩토리를 조건에 따라 생성하도록 다시 팩토리를 만들어서 객체를 생성하는 패턴
- 팩토리 패턴을 더 캡슐화 한 방식 
- 다양한 구성 요소 별로 **객체의 집합**을 생성해야 할 때 유용 

![추상화 팩토리 패턴 그림](https://upload.wikimedia.org/wikipedia/commons/thumb/9/9d/Abstract_factory_UML.svg/1354px-Abstract_factory_UML.svg.png)

- AbstractFactory: 개념적 제품에 대한 객체를 생성하는 연산으로 인터페이스 정의, 팩토리 클래스의 공통 인터페이스 
- ConcreteFactory: 구체적인 제품에 대한 객체를 생성하는 연산 구현, 구체적인 팩토리 클래스 
- AbstractProduct: 개념적 제품 객체에 대한 인터페이스 정의, 제품의 공통 인터페이스 
- ConcreteProduct: 구체적으로 팩토리가 생성할 객체를 정의하고, AbstractProduct가 정의하는 인터페이스 구현, 구체적인 제품 
- Client: AbstractFactory와 AbstractProduct 클래스에 선언된 인터페이스를 사용한다.

### 팩토리 메소드의 문제점 
새로운게 추가되었을 때 factory 클래스에 분기를 추가해줘야함

**인스턴스를 타입에 따라 생성해야 하기 때문에 분기문이 들어가고 의존성이 생기게 됨 !!**

추가하는 건 어렵지 않지만 엄청 많이 늘어났다고 했을 때 해당 factory 클래스의 메소드는 엄청나게 커질 수 밖에 없음 

**인스턴스를 바깥에서 만들고 만든다는 행위만 정의하면 분기문을 제거할 수 있지 않을까? => 추상화 팩토리 메소드**

### 예제 
**AbstractProduct: 개념적 제품 객체에 대한 인터페이스 정의, 제품의 공통 인터페이스**
```JAVA
public interface Movie {
    void getCast();
    void getDirector();
}

```

**ConcreteProduct: 구체적으로 팩토리가 생성할 객체를 정의하고, AbstractProduct가 정의하는 인터페이스 구현, 구체적인 제품**
```JAVA
public class Minari implements Movie {

    @Override
    public void getCast() {
        System.out.println("미나리 배우 : 윤여정, 스티븐");
    }

    @Override
    public void getDirector() {
        System.out.println("미나리 감독 : 정이삭");
    }
}

public class LaLaLand implements Movie {

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

**AbstractFactory: 개념적 제품에 대한 객체를 생성하는 연산으로 인터페이스 정의, 팩토리 클래스의 공통 인터페이스**
```JAVA
public interface MovieAbstractFactory {
    Movie getMovie();
}

```

**ConcreteFactory: 구체적인 제품에 대한 객체를 생성하는 연산 구현, 구체적인 팩토리 클래스**
```JAVA
public class MinariFactory implements MovieAbstractFactory{
    @Override
    public Movie getMovie() {
        return new Minari();
    }
}

public class LaLaLandFactory implements MovieAbstractFactory{
    @Override
    public Movie getMovie() {
        return new LaLaLand();
    }
}
```

```JAVA
public class MovieFactory {
    public static Movie getMovie(MovieAbstractFactory factory) {
        return factory.getMovie();
    }
}


public class Main {
  public static void main(String args[]) {

    System.out.println("라라랜드 관련 정보 >>> ");
    // 의존성 주입
    Movie LaLaLand = MovieFactory.getMovie(new LaLaLandFactory());
    LaLaLand.getCast();
    LaLaLand.getDirector();
    System.out.println(">>>>>>>>>>>>>>>>>.");

    System.out.println("미나리 관련 정보 >>> ");
    Movie Minari = MovieFactory.getMovie(new MinariFactory());
    Minari.getCast();
    Minari.getDirector();
    System.out.println(">>>>>>>>>>>>>>>>>.");

  }
}

```



### 장점 vs 단점
**장점**
1. 객체의 자료형이 하위 클래스에 의해서 정해지기 때문에 확장에 용이
2. 팩토리 메소드 패턴의 if-else 혹은 switch 에서 벗어나게 해준다
3. 구체 클래스와 클라이언트 와의 결합도가 낮아지기 때문에 유지보수 용이

**단점**
1. 계속해서 하위 클래스를 생성해야 하기 때문에 불필요하게 많은 클래스를 정의


### 팩토리 메소드 패턴 vs 추상화 팩토리 패턴
팩토리 메소드 패턴 같은 경우에는 **if-else** 나 **switch** 를 통해 서브클래스를 리턴해줬지만 

추상화 팩토리 패턴에서는 서브 클래스에 대한 식별 데이터를 받는게 아니라 또 하나의 팩토리 메소드를 받는다 

#### Factory 클래스에서 객체에 대한 생성을 지원하는 범위
- 팩토리 메소드 패턴
  - 한개의 메소드로 여러개의 객체를 만듬 
- 추상 팩토리 패턴
  - 구상 클래스에 의존하지 않고 여러 개의 관련된 객체를 하나의 팩토리로 묶음

#### 팩토리 메소드에서 만드는 객체의 종류 
- 팩토리 메소드 패턴
  - 인자에 따라서 객체의 종류가 결정 됨 
- 추상 팩토리 패턴
  - 인자에 따라 팩토리의 종류가 결정 됨 

#### 결합도를 낮추는 대상
- 팩토리 메소드 패턴
  - ConcreteProduct와  Client 간의 결합도를 낮출때 사용
  - 하지만 Factory 와 Client 간의 결합도는 높아질 수 있음 (Factory 에서 분기로 나눠주기 때문에)
- 추상 팩토리 패턴
  - ConcreteFactory와 Client간의 결합도를 낮출 때 사용

#### 포커스
- 팩토리 메서드 패턴
   - 메서드(Factory Method) 레벨에서 포커스를 맞춤
  - 클라이언트의 ConcreteProduct 인스턴스 생성 및 구성에 대한 책임을 덜어줌 (ConcreateFactory 에서 분기로 만들어줌)
- 추상 팩토리 패턴
  - 클래스(Abstract Factory) 레벨에서 포커스를 맞춤
  - 각 Product들이 다른 클래스와 함께 사용될 때의 제약사항을 강제할 수 있다.
  - 단, 새로운 ConcreteFactory를 추가할 때 많은 작업이 필요하다. 

#### Inhritance(상속), Composition(구성)
- 팩토리 메소드 패턴
  - 상속을 사용하여 객체의 인스턴스 생성에 대해서는 서브클래스에 의존
  - 지역 레퍼런스 없이 바로 하위 클래스의 함수를 호출하여 객체를 만듦
- 추상 팩토리 패턴
  - 지역 레퍼런스를 두어, 외부로부터 Factory 객체를 DI(의존성주입) 받아서 위임. (외부에서 Factory 인스턴스를 넘겨줘야함)

#### 팩토리 메소드 vs 추상 팩토리 
- 팩토리 메소드 패턴
  - 상속을 통해 객체를 만든다.
  - 팩토리 메소드 패턴을 이용해 객체를 만들 때는 수퍼 클래스를 확장하고 팩토리 메소드를 오버라이드 해야 한다.
  - 객체 생성을 담당하는 팩토리가 서브 클래스에 구현되어 있다.
  - 일반적으로 한 가지 객체를 생성할 때 사용한다.

- 추상 팩토리 패턴
  - 객체 구성을 통해 만든다.
  - 연관된 객체들의 집합을 만들기 위한 추상 형식을 제공한다.
  - 제품이 생상되는 방법은 추상 형식의 서브 클래스에 정의된다.
  - 여러 객체를 하나의 응집화된 군을 만들 때 사용한다.


### 레퍼런스
- [https://johngrib.github.io/wiki/abstract-factory-pattern/](https://johngrib.github.io/wiki/abstract-factory-pattern/)
- [https://ko.wikipedia.org/wiki/%EC%B6%94%EC%83%81_%ED%8C%A9%ED%86%A0%EB%A6%AC_%ED%8C%A8%ED%84%B4](https://ko.wikipedia.org/wiki/%EC%B6%94%EC%83%81_%ED%8C%A9%ED%86%A0%EB%A6%AC_%ED%8C%A8%ED%84%B4)
- [https://gmlwjd9405.github.io/2018/08/08/abstract-factory-pattern.html](https://gmlwjd9405.github.io/2018/08/08/abstract-factory-pattern.html)
- [https://im-yeobi.io/posts/design-pattern/factory-pattern-2/](https://im-yeobi.io/posts/design-pattern/factory-pattern-2/)
- [https://beomseok95.tistory.com/246](https://beomseok95.tistory.com/246)
- [https://beomseok95.tistory.com/243?category=1066005](https://beomseok95.tistory.com/243?category=1066005)