---
layout: post
title: <JAVA> 자바 빌드 관리 도구 (Ant, Maven, Gradle)
categories: JAVA
tags: [JAVA]
---

## 자바 빌드 관리 도구 (Ant, Maven, Gradle)

자바 빌드 도구로 Maven 과 Gradle 을 사용했었는데 Maven 에서 Gradle 로 넘어갈 때 조금 더 간결하고 가독성이 좋다 이정도만 알고 넘어갔어서 이번에 차이점 및 성능 비교까지 해보려고 한다 

### 빌드 관리 도구란 ?
> 우리가 프로그램에서 작성한 java 코드 및 xml, properties, jar 파일들을 JVM 이나 WAS 가 인식할 수 있도록 패키징 해주는 빌드 과정 
>
> => **빌드 자동화 도구** 

**등장한 이유**
1.  빠른 기간 동안에 계속해서 늘어나는 라이브러리 추가 
2. 프로젝트를 진행하며 라이브러리의 버전 동기화가 어렵기 때문 

**종류**
Ant, Maven, Gradle 등 
- 과거에는 `Ant` 를 많이 사용했으나 현재는 단순히 빌드 도구로만 이용되고 , `Maven` 에 빌드 + 자동 라이브러리 관리 기능이 추가되면서 `Ant` 에서 `Maven` 으로 많이 옮겨왔고 현재는` Gradle` 을 많이 이용한다 


### Ant (Apache Ant)
> 자바 기반 빌드 도구 

Ant, Maven, Gradle 중 가장 오래됨 

**특징**
- XML을 기반으로 빌드 스크립트 작성 
- 자유롭게 빌드 단위를 지정할 수 있다 
- 생명주기를 갖지 않기 때문에 각각의 target에 대한 의존관계와 일련의 작업을 정의해 주어야 함

**단점**
- XML, Remote Repository를 가져올 수 없었음 (IVY 도입)
- 유연하지만 빌드 단위나 스크립트를 직접 작성해줘야 하기 때문에 프로젝트가 커지는 경우 스크립트 관리나 빌드 과정이 복잡해진다 

### Maven
> XML 기반 상속 구조를 가지고 있는 빌드 도구 

2004년 출시 , Ant 를 사용하던 개발자의 불편함 해소 + 부가기능 추가 

```java
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
    <version>2.2.4.RELEASE</version>
</dependency>
```

**특징** 
- XML 기반 작성 
- 빌드 중인 프로젝트 , 빌드 순서 , 다양한 외부 라이브러리의 종속성 관계를 `pom.xml` 파일에 명시 
- 필요 라이브러리를 pom.xml 에 정의해 놓으면 내가 사용할 라이브러리 뿐만 아니라 해당 라이브러리가 작동하는데 필요한 다른 라이브러리까지 관리하여 네트워크를 통해 자동으로 다운 받아줌 
- 생명 주기를 가지고 있다 
- 상속 구조 (특정 설정을 소수의 모듈에서 공유하기 위해선 비어있는 프로젝트를 만들어서 공통 설정을 넣고 그걸 상속하게 해야함 => 쓸데 없이 설정이 길어지고 중복이 발생하여 가독성이 매우 떨어짐)


### Gradle
> Groovy 기반의 Ant 와 Maven 단점을 보완해 만들어진 빌드 도구 
오픈소스기반의 build 자동화 시스템으로 Groovy 기반 DSL(Domain-Specific Language)로 작성된 빌드 도구 
Ant 와 Maven 의 단점을 보완하고 장점을 취합해 만들어짐 
안드로이드 공식 빌드 시스템 

```java
implementation group: 'org.springframework.boot', name: 'spring-boot-starter-data-jpa', version: '2.2.4.RELEASE'
```

- 설정 주입 방식 (필요한 프로젝트에 바로 주입하는 방식이라 공통적으로 필요한 설정은 공통으로 주입하고 모듈별로 필요한 설정들은 모듈별로 설정해두면 됨) => 멀티 모듈 프로젝트에서 용이 
- Xml 의 구조적인 틀에서 벗어나 간결한 정의 가능 
- Maven 과 Ivy 완전 지원 
- 스크립트 언어로 구성되어 있기 때문에 XML과 달리 간단한 로직구현이 가능 



### Maven vs Gradle

Maven 엔 Gradle 비교 문서가 없지만 Gradle 엔 비교 문서가 있을 정도로 Gradle 은 비교에 자신감 있는 모습
Gradle 자체가 Ant + Maven 의 단점을 보완하고 장점을 취합해서 만들어진 빌드 도구라 더 좋은 스펙을 가지고 있다 


**XML vs Groovy** 

- Build 라는 동적인 요소를 XML 로만 정의하기엔 설정 내용이 길어지고 가독성이 떨어진다 
- Groovy 같은 경우 코드로만 봤을 때도 더욱 간단하게 사용 가능 , 또한 동적인 빌드는 스크립트로 직접 코드를 구현해서 확장 가능 

=> Gradle 의 Groovy 를 사용했을 때 코드를 더 효율적으로 사용 가능 



**상속 구조 vs 설정 주입 방식** 

- Maven 의 상속 구조인 경우 특정 설정을 소수의 모듈에서 공유하기 위해선 비어있는 프로젝트를 만들어서 공통 설정을 넣고 그걸 상속하게 해야함 => 쓸데 없이 설정이 길어지고 복이 발생하여 가독성이 매우 떨어짐
- Gradle 의 설정 주입 방식의 경우 필요한 프로젝트에 바로 주입하는 방식이라 공통적으로 필요한 설정은 공통으로 주입하고 모듈별로 필요한 설정들은 모듈별로 설정해두면 됨

=> 멀티 모듈 프로젝트를 진행한다고 할 땐 Gradle 의 설정 주입 방식이 더 효율적이다 



**성능**

Gradle 이 Maven 보다 최대 100배 빠름  

- **증분성(Incrementality)** : Gradle은 가능한 경우 변경된 파일만 작업해 중복 작업을 피한다. → 증분 빌드

- **Build cache** : 동일한 입력에 대해서 gradle 빌드를 재사용 한다.
- **Gradle 데몬** : 빌드 정보를 메모리에 유지하는 프로세스를 구동한다.



#### 소규모 다중 프로젝트 빌드

 10개 모듈 다중 프로젝트 빌드의 일반적인 작업에 대한 결과 (각 하위 프로젝트들은 50개의 소스 파일과 50개의 테스트 소스 파일)

**클린 빌드의 경우 2~3배, 증분 변경의 경우 약 7배, Gradle 작업 출력이 캐시될 때 최대 14배 빠름 !!**

![https://user-images.githubusercontent.com/39696812/126539261-665b1eca-0cde-4883-b51d-519ef89864db.png](https://user-images.githubusercontent.com/39696812/126539261-665b1eca-0cde-4883-b51d-519ef89864db.png)



#### 대규모 다중 프로젝트 빌드 

500개 모듈 다중 프로젝트 빌드에 대한 일반적인 작업의 결과 (각 하위 프로젝트에는 100개의 소스 파일과 100개의 테스트 파일)

**클린 빌드의 경우 3~10배, 증분 변경의 경우 약 85배, Gradle 작업 출력이 캐시될 때 최대 13배 빠름 !!**

![https://user-images.githubusercontent.com/39696812/126539585-93321485-25fb-45e2-94aa-479cd7238d81.png](https://user-images.githubusercontent.com/39696812/126539585-93321485-25fb-45e2-94aa-479cd7238d81.png)



### 레퍼런스

[자바 빌드 도구](https://velog.io/@dnstlr2933/%EC%9E%90%EB%B0%94-%EB%B9%8C%EB%93%9C-%EB%8F%84%EA%B5%AC)<br>
[Java 빌드 도구 - Ant, Maven, Gradle](https://happydeveloperslife.tistory.com/entry/Java-%EB%B9%8C%EB%93%9C-%EB%8F%84%EA%B5%AC-Ant-Maven-Gradle)<br>
[Ant, Maven, Gradle 이 뭘까?](https://sugerent.tistory.com/571)<br>
[Gradle vs Maven: Performance Comparison](https://gradle.org/gradle-vs-maven-performance/)<br>
[Gradle vs Maven Comparison](https://gradle.org/maven-vs-gradle/)<br>