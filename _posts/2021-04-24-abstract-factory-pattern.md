---
layout: post
title: 추상화 팩토리 패턴 (Abstract Factory Pattern)
categories: 디자인패턴
tags: [디자인패턴, 팩토리패턴, 생성패턴]
---

[< 팩토리 메소드 패턴 보려면?](https://soyeon207.github.io/%EB%94%94%EC%9E%90%EC%9D%B8%ED%8C%A8%ED%84%B4/2021/04/22/factory-method-pattern.html)

### 추상화 팩토리 패턴 (Abstract Factory Pattern)
- 팩토리 메소드 패턴은 디자인패턴에서 **생성패턴**에 속한다 
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

### 예제 
추상화 팩토리 메소드 사용 
```JAVA
// AbstractFactory
interface Factory {
  Wall makeWall();
  Room makeRoom();
  Door makeDoor();
}

// AbstractProduct
interface Game {
  void createMap(Factory factory);
  void show();
}

public class Main {
  public static void main(String[] args) {
    // 두 개의 ConcreteFactory
    Factory bombFactory = new BombedMazeFactory();
    Factory monsterFactory = new MonsterFactory();

    // 폭탄 테마의 미로 게임 맵을 만든다
    Game bomberMan = new MazeGame();
    bomberMan.createMap(bombFactory);

    // 괴물 테마의 미로 게임 맵을 만든다
    Game diablo = new MazeGame();
    diablo.createMap(monsterFactory);

    // 괴물 테마의 슈팅 게임 맵을 만든다
    Game doom = new ShootingGame();
    doom.createMap(monsterFactory);
  }
}

```

팩토리 메소드 + 추상화 팩토리 메소드 
```JAVA
private static Game createGame(String type) {
  Game game;
  switch(type) {
    case "bomberMan":
      game = new MazeGame();
      game.createMap(new BombedMazeFactory());
      return game;
    case "doom":
      game = new ShootingGame();
      game.createMap(new MonsterFactory());
      return game;
    case "diablo":
      game = new MazeGame();
      game.createMap(new MonsterFactory());
      return game;
    default:
      throw new IllegalArgumentException("unknown " + type);
    }
  }
}

public static void main(String[] args) {
  createGame("bomberMan").show();
  createGame("diablo").show();
  createGame("doom").show();
}
```

### 팩토리 메소드 패턴 vs 추상화 팩토리 패턴
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