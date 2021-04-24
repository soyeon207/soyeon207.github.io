---
layout: post
title: 브리지 패턴 (Bridge pattern)
categories: 디자인패턴
tags: [디자인패턴, 브리지패턴, 구조패턴]
---

### 브리지 패턴 (Bridge pattern)
- 구현부에서 추상층을 분리하여 각자 독립적으로 변형 & 확장이 가능, 즉 기능과 구현에 대해서 두 개를 별도의 클래스로 구현
- 기능 클래스 계층과 구현 클래스 사이에 다리를 놓는다 

### 구조
![https://user-images.githubusercontent.com/39696812/115954120-39b05900-a52a-11eb-8c16-965bf78972a8.png](https://user-images.githubusercontent.com/39696812/115954120-39b05900-a52a-11eb-8c16-965bf78972a8.png)
- Implementor : 기능 **구현**을 위한 인터페이스
- ConcreteImplementor : 실제 기능 **구현**
- Abstraction : **기능** 계층의 최상위 클래스
- RefindAbstraction : **기능** 확장

### 예제 
![https://user-images.githubusercontent.com/39696812/115954722-7762b100-a52d-11eb-81c7-293d0cae6feb.png](https://user-images.githubusercontent.com/39696812/115954722-7762b100-a52d-11eb-81c7-293d0cae6feb.png)

**Implementor : 기능 구현을 위한 인터페이스**
```Java
public interface MakeHandler() {
    public void prepareIngredient();
    public void stuffingIngredient();
}
```

**ConcreteImplementor : 실제 기능 구현**
```Java
public class PotatoHotdogMethod() implements MakeHandler {
    public void prepareIngredient() {
        System.out.println("감자 준비");
    }

    public void stuffingIngredient() {
        System.out.println("감자를 꼬치에 꽂는다");
    }
}

public class CheeseHotdogMethod() implements MakeHandler {
    public void prepareIngredient() {
        System.out.println("치즈 준비");
    }

    public void stuffingIngredient() {
        System.out.println("치즈를 꼬치에 꽂는다");
    }
}
```

**Abstraction : 기능 계층의 최상위 클래스**
```Java
public abstract class HotDog() {
    private MakeHandler makeHandler;

    public HotDog(MakeHandler makeHandler) {
        this.makeHandler = makeHandler;
    }

    public void prepareIngredient() {
        makeHandler.prepareIngredient;
    }

    public void stuffingIngredient() {
        makeHandler.stuffingIngredient;
    }

    public abstract void fry();
}
```

**RefindAbstraction : 기능 확장**
```Java
public class PotatoHotDog extends HotDog {
    public PotatoHotDog(PotatoHotdogMethod potatoHotDogMethod) {
        super(potatoHotDogMethod);
    }

    @Override
    public void fry() {
        System.out.println("감자핫도그 튀기기 전 준비");
    }
}

public class CheeseHotDog extends HotDog {
    public CheeseHotDog(CheeseHotdogMethod cheeseHotdogMethod) {
        super(cheeseHotdogMethod);
    }

    @Override 
    public void fry() {
        System.out.println("치즈핫도그 튀기기 전 준비");
    }
}

```

```Java
public class Main {
    public static void Main(String args[]) {
        HotDog cheeseHotDog = new CheeseHotDog(new CheeseHotdogMethod());
        HotDog potatoHotDog = new PotatoHotDog(new PotatoHotDogMethod());
        cheeseHotDog.fry();
        cheeseHotDog.prepareIngredient();
        cheeseHotDog.stuffingIngredient();
    }
}
```


### 레퍼런스
- [ko.wikipedia.org](https://ko.wikipedia.org/wiki/%EB%B8%8C%EB%A6%AC%EC%A7%80_%ED%8C%A8%ED%84%B4)
- [https://brownbears.tistory.com/498](https://brownbears.tistory.com/498)