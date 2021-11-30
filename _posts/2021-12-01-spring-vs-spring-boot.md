---
layout: post
title: Spring vs Spring boot
categories: Spring 
tags: [Spring, SpringBoot]
---
# Spring vs Spring boot
사용하고 있는 자바 프레임 워크가 Spring 인가요 Spring boot 인가요 라고 물어봤을 때 둘 간의 차이점이 뭐지 라고 고민했던 적이 있었다. 이번 포스팅에선 Spring 과 Spring boot 가 뭔지에 대해서 알아보고 더 나아가서 두개의 차이점까지 알아보자

##  Spring Framework
> JAVA 기술들을 더 쉽게 사용할 수 있게 해주는 오픈소스 프레임 워크

- 가장 대중적이고 대표적인 java 프레임워크

### 🤔 이름이 Spring이 된 이유
'개발자들의 겨울은 끝났다, 이 프레임워크로 인해서 봄이왔다'라고 해서 이름이 Spring 이 되었다고 한다.

### ✏️ 스프링이 해결하고자 하는 것
- 의존성 주입으로 인한 객체간의 결합도를 낮춤으로써 코드 재사용성 향상 및 단위테스트 용이 
- 중복된 코드 제거 
- 다른 프레임워크와의 통합 

### 🥸 특징
1. 의존성 주입 (DI, Dependency Injection)
2. 제어의 역전 (IOC, Inversion Of Control)
3. 관점 지향 프로그래밍 (AOP, Aspect-Oriented Programming)

의 총 세가지 특성으로 결합도를 낮추는 방식으로 어플리케이션을 개발 

### 😡 단점
1. 설정할 게 너무 많다 <br>
   설정파일이 너무 길고, 한눈에 알아 볼 수 없다 

2. 의존성 관리가 힘들다 <br>
    스프링 버전이 계속 올라감에 따라 호환이 되는 의존성과 그렇지 않은 의존성을 반드시 구분해서 의존성을 추가해야 함 

3. 배포가 어렵다 <br>
   내장 톰캣이 없다 보니 스프링 프로젝트에서 .war 파일을 추출 후 WAS 환경에서 실행했어야 했다. WAS 없이 독립적으론 실행이 불가능 

➡️ 해당 단점들을 해결하고자 나온게 **Spring boot** 

## Spring Boot
> Spring Boot는 최소한의 기능으로 스프링 기반의 프로덕션 급 응용 프로그램 및 서비스를 쉽게 만들 수 있도록하는 것을 목표 로 함 

### ✏️ 스프링 부트가 해결하고자 하는 것
- Auto Configuration 자동 설정 
- 쉬운 의존성 관리
- 내장 서버

#### Auto Configuration 자동 설정
1. 스프링 기능을 위한 자동 설정 지원<br>
    Spring Boot 프로젝트를 생성하여 main에 가게 되면 **@SpringBootApplication** 어노테이션이 붙게 된다 

    **@SpringBootApplication** 내부 어노테이션 
    - `@ComponentScan` 
        - basePackages 프로퍼티 값에 별도의 경로를 설정하지 않으면 해당 어노테이션이 위치한 패키지가 루트 경로가 설정
        -  component-scan 을 통해 component 를 찾고 bean 생성을 진행하면서 우리가 설정한 bean 들이 생성
    - `@EnableAutoConfiguration` 
        - 자동 설정의 핵심 어노테이션 
        - 추가적인 Bean 들을 읽어서 등록 
        - 해당 어노테이션으로 spring.factories 안에 들어있는 수많은 자동 설정들이 조건에 따라 적용이 되어 수 많은 Bean들이 생성되고, 스프링 부트 어플리케이션이 실행
    - `@SpringBootConfiguration`
        - Spring boot의 **설정을 나타내는 어노테이션**
        - Spring의 @Configuration을 대체

    ➡️ Spring Boot는 Component scan을 통해서 모은 component들의 정보와 spring.factories 파일에 사전 정의한 auto-configuration 내용에 의해 bean 자동 생성을 진행


2. starter 의존성을 통해 간단하게 설정<br>
    starter란 ? 의존성과 설정을 자동화해주는 모듈<br>
    예를 들어 Spring-boot-starter-jpa를 의존성 추가했을 때 Spring Boot는 아래와 같은 일들을 해준다 
    - spring-aop, spring-jdbc 등의 의존성을 걸어준다. *easy dependency*
    - classpath를 뒤져서 어떤 database를 사용하는지 파악하고, 자동으로 entityManager를 구성해준다. *auto-configuration*
    - 해당 모듈들 설정에 필요한 properties 설정을 제공한다. *auto-configuration*

    **🌿 AS-IS 스프링 사용하는 경우** <br>
    Spring MVC를 추가하고 웹과 관련된 여러 설정 파일들(xml 파일들)을 직접 생성 및 관리

    **🌳 TO-BE 스프링 부트를 사용하는 경우**<br>
    spring-boot-starter-web 의존성을 추가해주면 관련 모듈(jar) 들이 자동으로 추가됨  

#### 쉬운 의존성 관리
1. Spring-boot-starter<br>
   웹 어플리케이션을 개발할 때, **spring-web**, **spring-webmvc** , **jackson-databind** 등과 같은 의존성 필요

   **🌿 AS-IS 스프링 사용하는 경우**<br>
   의존성을 하나하나씩 찾고, 호환되는 버전에 맞춰 의존성을 추가해줘야 함 

   **🌳 TO-BE 스프링 부트를 사용하는 경우**<br>
   의존성에 spring-boot-starter-web 한 줄만 적어주면 web 과 관련된 의존성(spring-web, spring-webmvc, jackson-databind 등) 추가 가능 

   ```java
   compile group: 'org.springframework.boot', name: 'spring-boot-starter-web', version: '2.3.1.RELEASE'
   ```

2. io.spring.dependency-management
   - 스프링부트의 의존성을 관리해주는 플러그인
   - dependency manager가 관리하는 프로젝트들은 알아서 버전관리 즉, build 설정에 dependency에 대한 버전을 명시할 필요가 없음
   - Spring Boot 자체를 업그레이드 하면 하위 종속성들도 일관된 방식으로 업그레이드

   ```java
   plugins {
     id "io.spring.dependency-management" version "1.0.9.RELEASE"
   }
   ```

   ➡️ Spring Boot의 dependency-management를 이용하여 **단 한줄의 의존성으로 수 많은 프로젝트들의 버전을 충돌 없이 관리**할 수 있음 

#### 내장 서버<br>
- Spring Boot는 Servlet Contaioner를 내장하고 있음 

**🌿 AS-IS 스프링 사용하는 경우** 
- war (Web Application Archive) 파일로 배포 가능 
- war파일은 웹 어플리케이션을 압축해 저장해놓은 파일로, tomcat과 같은 was에서 돌아갈 수 있는 구조를 담고 있음 
- 따라서 Spring으로 개발한 프로젝트를 배포하기 위해서는 (웹 어플리케이션이 압축된)war파일과 (프로그램을 실행시킬)WAS 가 필요 

**🌳 TO-BE 스프링 부트를 사용하는 경우**
- Tomcat이나 Jetty같은 내장 서버를 가지고 있기 때문에 jar파일로 배포 가능 

## 레퍼런스
[Spring vs Spring Boot](https://limjunho.github.io/2021/08/15/Spring-vs-SpringBoot.html)
[Spring🌱 vs SpringBoot🌼](https://ssoco.tistory.com/66)

