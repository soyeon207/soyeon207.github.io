---
layout: post
title: <Spring> IOC 란?
categories: 스프링
tags: [SPRING]
---

## IOC (Inversion of Control, 제어의 역전) 란 ?
> 제어권이 역전된 것

- 말 그대로 **메소드나 객체의 호출작업을 개발자가 결정하는 것이 아니라, 외부에서 결정**되는 것을 의미

### IOC 적용 전
1. 객체 생성
2. 의존성 객체 생성<br>
   ➡ 클래스 내부에서 생성
3. 의존성 객체 메소드 호출

```java
class OrderController {
	private OrderRepository repository = new OrderRepository();
}
```

- 의존성에 대한 제어권을 개발자가 만들어서 자기가 가지고 있음



### IOC 적용 후
1. 객체 생성
2. 의존성 객체 생성<br>
   ➡ 스스로가 만드는것이 아니라 제어권을 **스프링에게 위임하여 스프링이 만들어놓은 객체를 주입**
3. 의존성 객체 메소드 호출

```java
class OrderController {
	private final OrderRepository repo;

	public OrderController(OrderRepository repo) {
		this.repo = repo;
	}
}
```

- ️OrderController 가 제어권을 만들어서 사용하는게 아니라 외부에서 주입받아서 사용함 -> 사용을 하지만 만들지는 않음
-  여기서 의존성을 주입해주는 일을 DI (Dependency Injection, 의존성 주입) 이라고 하는데 의존성 주입도 일종의 제어권 역전이기 때문에 IOC 에 속함
-  스프링의 IOC 컨테이너가 빈으로 등록을 해주고 의존성을 관리해준다



## IOC 컨테이너
> 객체를 생성하고, 객체간의 의존성을 이어주는 역할

Bean Factory, Application Context (Bean Factory 상속)

| 이름                    | 설명                                                         |
| ----------------------- | ------------------------------------------------------------ |
| **Bean Factory**        | IOC 컨테이너의 기능을 정의하고 있는 인터페이스 <br />Bean의 생성 및 의존성 주입, 생명주기(lifecycle) 관리 등의 기능을 제공 |
| **Application Context** | BeanFactory 상속 <br />BeanFactory가 제공하는 기능 외에 AOP, 메세지처리, 이벤트 처리 등의 기능을 제공 |

- 모든 객체가 빈으로 등록되어 있는 건 아님
- 빈으로 등록되면 일단 스프링IDE 에서 앞에 콩아이콘으로 표시 으로 나타내주고 여기서 빈들의 의존성 주입을 IOC 컨테이너가 해줌

```java
@Controller
class OwnerController {
	private final OwnerRepository owner;

    // IOC 컨테이너가 ownerRepository 를 찾아서 넣어줌
   public OwnerController(OwnerRepository ownerRepository) { 
     this.owner = ownerRepository;
   }

}
```

- 의존성 주입은 빈끼리민 가능
- 즉 스프링 IOC 안에 들어있는 객체들끼리만 의존성 주입을 해줌



```java
@Autowired
ApplicationContext applicationContext;

@Test
public void getBean() {
    // applicationContext 에 들어있는 모든 bean 이름 리턴
    applicationContext.getBeanDefinitionNames();
    
    // 파라미터로 bean class 넘기면 됨
    applicationContext.getBean(OrdersController.class);  
}

```



### 🙇🏻‍♀️ 레퍼런스

- [[Spring] DI, IoC 정리](https://velog.io/@gillog/Spring-DIDependency-Injection)
- [[Spring] Spring IoC와 DI](https://gangnam-americano.tistory.com/60)


