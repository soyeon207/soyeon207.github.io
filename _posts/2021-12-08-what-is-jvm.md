---
layout: post
title: <JAVA> 자바의 구동원리와 JVM
categories: 자바 
tags: [JAVA]
---

## 🕹 자바 프로그램 구동 원리

![https://user-images.githubusercontent.com/39696812/144739834-6c27a86d-1c75-43fc-8564-1fc6f128cff3.jpg](https://user-images.githubusercontent.com/39696812/144739834-6c27a86d-1c75-43fc-8564-1fc6f128cff3.jpg)

1. 소스코드를 작성한다. (.java 확장자의 소스 파일) ➡️ **Test.java**
2. 컴파일러 (Javac.exe) 가 자바 소스 코드(.java)를 바이트 코드(.class)로 변환한다 (.class 확장자의  클래스 파일) ➡️ **Test.class**
3. 런처(java.exe) 로 자바 가상 머신을 구동시킨다. 
4. 자바 가상 머신 (JVM) 이 바이트 코드를 해석하여 자바 프로그램이 실행된다. 

### ❓ .java 파일이란 ? 

👉 java 규칙에 맞게 작성한 모든 소스코드 파일 <br>
👉 사람이 읽을 수 있는 text 로 구성 

### ❓ .class 파일이란 ? 

👉 컴파일러에 의해 생성된 bytecode 로 구성된 파일<br>
👉 JVM 을 위한 코드 <br>
👉 자바를 실행할 수 있는 모든 장치에서 실행 가능 

### 💭 예제
src > main > java > test1 에 `Main.java` 와 `TestClass.java` 가 있을 때 컴파일러를 실행한 경우 

![https://user-images.githubusercontent.com/39696812/144740421-ccf9cb63-dbd1-41d9-a606-428d8f65f6d3.png](https://user-images.githubusercontent.com/39696812/144740421-ccf9cb63-dbd1-41d9-a606-428d8f65f6d3.png)

build > classes > java > main 에 `Main.class` , `TestClass.class` 가 만들어 진 걸 확인 할 수 있다. 

![https://user-images.githubusercontent.com/39696812/144740440-ea83fe1e-8af4-4420-8d94-b464372cc623.png](https://user-images.githubusercontent.com/39696812/144740440-ea83fe1e-8af4-4420-8d94-b464372cc623.png)


## 🤖 자바 가상머신 (JVM) 이란?

### 🧑‍🍳 JVM 이 만들어진 이유 

- 기존의 언어 (C, C++) 인 경우 컴파일 플랫폼과 타겟 플랫폼이 다른 경우 프로그램이 동작하지 않음<br>
  EX) 리눅스에서 컴파일된 프로그램은 플랫폼이 윈도우로 바뀌게 되면 정상적으로 동작하지 않음 

- 그렇기 때문에 크로스 컴파일 (타겟 플랫폼에 맞춰 컴파일) 로 해당 문제를 해결했지만 자바에서는 JVM 을 통해 플랫폼이 달라도 실행되도록 


### ✏️ JVM 을 알아야 하는 이유 

- 동일한 기능의 프로그램이라고 하더라도 메모리 관리가 되지 않는 다면 속도 저하 현상이나 튕김 현상 등이 일어날 수 밖에 없고, 메모리 관리가 효율적으로 잘 된다면 성능이 좋아지기 때문 

- 한정된 메모리를 효율적으로 사용하여 최고의 성능을 내기 위함 

### 🗂 JVM 구성 

![https://user-images.githubusercontent.com/39696812/144742938-19b542bf-407b-43cf-8d9e-824c93583cc9.jpg](https://user-images.githubusercontent.com/39696812/144742938-19b542bf-407b-43cf-8d9e-824c93583cc9.jpg)



#### ⛓ 클래스 로더 (Class Loader)

![https://user-images.githubusercontent.com/39696812/145208079-928a85d2-0cd2-4bca-854c-9cb1cdea6afb.png](https://user-images.githubusercontent.com/39696812/145208079-928a85d2-0cd2-4bca-854c-9cb1cdea6afb.png)

##### 로딩 (loading)

>  .class 파일을 읽어 내용에 따라 적절한 바이너리 데이터를 생성하고, 메소드 영역에 저장 

**Bootstrap**

1. 원시 클래스 로더 
2. 최상위 클래스로더로 Java 가 아니라 네이티브 코드로 구현되어 있음 
3. JVM 이 실행될 때 같이 메모리에 올라감 
4. Java 클래스는 Java.lang.ClassLoader 인스턴스에 의해 로드됨. 여기서 ClassLoader 를 로드하는 것이 Bootstrap 클래스 로더
5. JAVA_HOME/jre/lib 디렉토리에 존재하는 핵심 java API 들을 로드
6. 모든 클래스 로더의 부모 

**Extension**

1. 부트 스트랩 클래스 로더의 자식 
2. JAVA_HOME/jre/lib/ext 혹은 java.ext.dirs 시스템 속성으로 명시된 다른 특정 디렉토리인 확장 디렉토리 안에 존재하는 클래스들을 로드

**Application**

1. 확장 클래스 로더의 자식 
2. 모든 어플리케이션 레벨의 클래스 로드 
3. 우리가 작성한 클래스는 이 Application 클래스 로더에 의해 로드 


##### 링크 (linking)

> 로딩 단계에서 생성된 바이너리 데이터를 JVM 런타임 데이터로 합침 

**Verify**

1. 클래스(.class) 파일의 정확성을 보장하기 위한 단계 
2. 읽은 클래스의 바이너리 데이터가 유효한 것인지 확인 

**Prepare**

1. 클래스의 static 변수와 기본값에 필요한 메모리 공간을 준비 
2. 클래스 및 인터페이스의 static 필드를 생성하고 기본값으로 초기화한다.

**Resolve**

1. 심볼릭 메모리 레퍼런스를 메모리 영역에 존재하는 실제 레퍼런스로 교체<br>
   EX) Test test = new Test() 에서 new Test() 부분은 실제 레퍼런스를 가리키지 않음. 그렇기 때문에 실제 힙에 들어있는 인스턴스를 가리키는 작업을 Resolve 시점에 해줌 

##### 초기화 (intialization)

> 링크 단계의 preapre 에서 확보한 메모리 영역에 클래스의 static 값들을 할당

#### 🏃‍♂️ Runtime Data Area

![https://user-images.githubusercontent.com/39696812/145206269-5788893b-f066-40a7-b0ce-6a7a2baf64dc.png](https://user-images.githubusercontent.com/39696812/145206269-5788893b-f066-40a7-b0ce-6a7a2baf64dc.png)


##### Method Area

> 클래스 정보를 처음 메모리 공간에 올릴 때 초기화되는 대상을 저장하기 위한 메모리 공간 

1. 클래스 정보, 변수 정보, static 으로 선언된 변수가 저장
2. 모든 스레드가 공유하는 메모리 영역 
3. JVM 이 시작될 때 생성 

**Runtime Constant Pool**

> 말그대로 `상수` 정보가 저장되는 공간 

- 클래스와 인터페이스 상수, 메소드와 필드에 대한 모든 레퍼런스를 저장
- JVM은 런타임 상수 풀을 통해 해당 메소드나 필드의 실제 메모리 상 주소를 찾아 참조 

##### Heap Area

> 객체를 저장하는 가상 메모리 공간 

1. 런타임 시 동적으로 할당하여 사용하는 공간 
2. 동적으로 New 연산자를 통해 생성된 객체 또는 배열을 저장하는 영역 
3. 참조하는 변수나 필드가 없다면 의미 없는 객체가 되어 가비지 컬렉터 (GC) 의 대상이 되어 회수됨 

➡️ Eden, Survivor1, Survivor2, Old, Permanent 로 이루어져 있음 


##### Java Stack Area

> 임시로 할당되었다가 메소드를 빠져나가면 바로 소멸되는 특성의 데이터를 저장하기 위한 영역

1. 각 스레드별로 하나씩 존재하며, 스레드가 시작될 때 할당 
2. 메소드를 호출할 때마다 프레임(Frame) 을 추가(push) 하고, 메소드가 종료되면 해당 프레임을 제거(pop) 하는 동작을 수행 
3. 선입후출(FILO) 방식 
4. 메소드 정보, 지역변수, 매개변수, 연산 중 발생하는 임시 데이터 저장
5. 메소드 호출 시 생성되는 스레드 수행정보를 기록하는 Frame을 저장
6. 기본(원시)타입 변수는 스택 영역에 직접 값을 가진다.
   참조타임 변수는 힙 영역이나 메소드 영역의 객체 주소를 가진다.

##### PC Register

> 스레드가 어떤 부분을 어떤 명령어로 실행해야할지 기록하는 영역 

1. 스레드가 생성될 떄마다 그 스레드가 어떠한 명령을 실행하게 될지에 대한 부분을 기록하는 메모리 공간 
2. 현재 수행중인 JVM 의 명령어 주소를 저장 
3. 실행되고 있는 스레드에서 몇번째 줄을 실행해야하는지 나타내는 역할 

##### Native Method Stack

> 바이트 코드가 아닌 실제 실행할 수 있는 기계어로 작성된 프로그램을 실행시키는 영역

1. 자바외 언어로 작성된 네이티브 코드를 위한 메모리 영역
2.  JNI 를 통해 호출하는 코드를 수행하기 위한 공간

#### 🔩 실행 엔진 (Execution)

> 클래스를 실행시키는 역할 <br>
> 바이트 코드를 실제로 JVM 내부에서 기계가 실행할 수 있는 형태로 변경 

##### 인터프리터

1. 실행 엔진은 자바 바이트 코드를 명령어 단위로 하나하나씩 읽어서 해석하고 실행 
2. 한줄 씩 수행하기 떄문에 느리다는 단점이 있음 


##### JIT (Just-In Time)

1. 인터프리터 방식으로 실행하다가 적절할 시점에 바이트코드 전체를 컴파일하여 네이티브 코드로 변경하고 이후에는 네이티브 코드로 직접 실행 
2. 네이티브 코드는 캐시에 보관하기 때문에 한 번 컴파일된 코드는 계속 빠르게 수행 
3. 하지만 컴파일하는 과정이 바이트 코드를 인터프리팅하는 것보다 훨씬 오래걸리기 때문에 한번만 실행되는 코드면 인터프리팅하는게 유리 


##### 가비지 콜렉터

### 🙇🏻‍♀️ 레퍼런스

[JAVA의 기본 - 1. 자바 파일(.java)와 .class 파일과의 관계](https://droptable.tistory.com/42)<br>
[자바의 구동 원리와 JVM(Java Virtual Machine) by 개발새발!님](https://gbsb.tistory.com/2?category=674290#java-memory)<br>
[JVM 어떻게 동작하는가? :: 마이구미](https://mygumi.tistory.com/115)<br>
[클래스 로더(Class Loader) by goodGid](https://goodgid.github.io/Java-Class-Loader/)<br>
[JVM - 3 (Execution Engine, 실행 엔진)](https://jjjayyy.tistory.com/24)<br>
[메모리 영역(Runtime Data Area, JVM Memory)](https://beststar-1.tistory.com/14)<br>
[JAVA\] JVM이란? 개념 및 구조 (JDK, JRE, JIT, 가비지 콜렉터...)](https://doozi0316.tistory.com/entry/1주차-JVM은-무엇이며-자바-코드는-어떻게-실행하는-것인가)<br>
[마로의 Java(자바) 정리 - 8. 자바 메모리 구조](https://hoonmaro.tistory.com/19)

