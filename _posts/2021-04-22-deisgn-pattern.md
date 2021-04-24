---
layout: post
title: 디자인 패턴이란?
categories: 디자인패턴
tags: [디자인패턴, 팩토리패턴]
---

### 디자인 패턴이란?
[위키백과] 소프트웨어 공학에서 소프트웨어 디자인에서 특정 문맥에서 공통적으로 발생하는 문제에 대해 재사용 가능한 해결책이다 

즉 , 객체 지향 프로그래밍 설계할 때 발생하는 문제들을 방지 하기 위해서 사용되는 패턴 

개발 하기 전 설계할 때 올바른 설계를 할 수 있도록 도와주고 다른 사람들이랑 협업시에도 더욱 빠르게 의사소통 할 수 있게 해준다 

EX ) 빌더 패턴 쓰는건 어떤가요 ? 처럼 이런이런 식으로 설계 해보자 보다 명확하게 커뮤니케이션 할 수 있음 


### 디자인 패턴의 종류 
- GOF 디자인 패턴엔 23가지의 디자인 패턴이 존재하고 3가지(생성, 구조, 행위)로 구분 
- 범위에 따라 클래스, 객체로 나눌 수 있음 

#### 생성(Creational) 패턴
- 시스템으로부터 객체 생성/합성 을 분리해내기 위함
- 객체가 변경되어도 프로그램 구조에 영향이 없도록 유연성 제공 

싱글턴 패턴 (Singleton Pattern), 프로토타입 패턴 (Prototype Pattern), 팩토리 메소드 패턴 (Factory Method Pattern), 빌더 패턴 (Builder Pattern), 추상 팩토리 패턴 (Abstract Factory Pattern)
#### 구조(Structural) 패턴
- 클래스나 객체를 조합해 더 큰 구조를 만드는 패턴 

어댑터 패턴(Adapter Pattern), 브리지 패턴(Bridge Pattern), 합성 패턴(Composite pattern), 데코레이터 패턴(Decorator Pattern), 퍼사드 패턴(Facade Pattern), 플라이웨이트 패턴(Flyweight Pattern), 프록시 패턴(Proxy Pattern)
#### 행위(Behavioral) 패턴
- 클래스나 객체간 교류 방법, 책임 분배에 대한 패턴

책임연쇄 패턴(Chain of responsibility), 커맨드 패턴(Command Pattern), 인터프리터 패턴(Interpreter Pattern), 이터레이터 패턴 (Iterator Pattern), 옵저버 패턴(Observer Pattern), 전략 패턴 (Strategy Pattern), 템플릿 메서드 패턴 (Template method pattern), 방문자 패턴 (visitor Pattern), 중재자 패턴 (Mediator Pattern), 상태 패턴 (State Pattern), 기념품 패턴 (Memento Pattern)




### 레퍼런스
- [https://readystory.tistory.com/114](https://readystory.tistory.com/114)
- [https://tech-people.github.io/2020/01/08/java-design-pattern-creational/](https://tech-people.github.io/2020/01/08/java-design-pattern-creational/)
