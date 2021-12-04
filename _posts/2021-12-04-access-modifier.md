---
layout: post
title: <JAVA> 접근 제어자 (Access Modifier)
categories: 자바 
tags: [JAVA]
---
> 접근의 범위를 지정해주는 제어자

- 접근 제어자는 외부에서 마음대로 접근하지 못하도록 제한하는 역할을 한다.
- 접근 제어자는 생략 가능하며 생략했을 때에는 자동으로 `default` 로 설정되게 된다.

## 🗿 접근 제어자를 사용할 수 있는 곳 

클래스, 멤버변수, 메서드, 생성자 에 접근 제어자를 사용할 수 있다. 

```java
public class Test {		    // 접근제어자 : public (클래스)
    private int var1;		    // 접근제어자 : private (멤버변수)
    int var2;			   // 접근제어자 : default (멤버변수), 접근제어자가 명시되어 있지 않지만 (default) int var2 라고 생각하면 된다 
  
    public Test(int var1, int var2) {	// 접근제어자 : public (생성자)
        this.var1 = var1;
        this.var2 = var2;
    }
  
    protected int method1() {	// 접근제어자 : protected (메서드)
        SyStem.out.println("var1 : "+var1+" / var2 : "+var2);
    }
}
```


대상별 사용가능한 접근 제어자는 다음과 같다. 

| 대상     | 사용가능한 접근 제어자                |
| -------- | ------------------------------------- |
| 클래스   | public, (default)                     |
| 메서드   | public, protected, (default), private |
| 멤버변수 | public, protected, (default), private |
| 지역변수 | 없음                                  |

**클래스** - public, (default)

프로젝트 폴더 내에서 클래스가 분류되는 방식이 패키지 단위이라 다른 패키지에서 접근이 가능한지 가능하지 않은지만 명시해주면 되기 때문에 접근제한자가 두개만 존재한다. 

class 에 private 혹은 protected 접근제어자를 써주면 `Modifier 'private or protected' not allowed here` 오류가 발생한다. 

![https://user-images.githubusercontent.com/39696812/144704691-18e04474-deda-4f3d-83fb-1b439a628292.png](https://user-images.githubusercontent.com/39696812/144704691-18e04474-deda-4f3d-83fb-1b439a628292.png)



또한, 하나의 java파일에는 반드시 하나의 public 클래스만 존재해야 한다. 이유는 가독성 및 유지보수 때문인데, 하나의 파일 내에 public class 가 너무 많은 경우 클래스를 찾기도 힘들고 유지보수도 힘들어 지기 때문이다. 

![https://user-images.githubusercontent.com/39696812/144704784-515fbbdb-6b9b-4def-afdc-50852985c6ee.png](https://user-images.githubusercontent.com/39696812/144704784-515fbbdb-6b9b-4def-afdc-50852985c6ee.png)

![https://user-images.githubusercontent.com/39696812/144704791-9bc23bd4-77c7-495c-bff6-3fa770fa100e.png](https://user-images.githubusercontent.com/39696812/144704791-9bc23bd4-77c7-495c-bff6-3fa770fa100e.png)

실제 예제를 통해 보자면 Main.java 에 Test innerClass 를 만드는 경우 public 으로 하게 되면 Test 클래스를 public 접근제어자를 지우라고 한다.


**메서드 , 멤버변수** - public, protected, (default), private

메서드와 멤버변수는 클래스 안에 존재하기 때문에 상속관계가 존재할 수 있고, 클래스 내부 사용에 대해서도 제한이 필요하기 때문에 모든 접근 제한자를 사용할 수 있다. 


**지역변수** - 없음 

지역변수라는 개념 자체가 한 메소드 또는 반복문, 특정 모듈 안에서 사용되고 사라지는 개념이기 때문에 접근 제한자를 설정하지 않는다. 


## 🗂 자바 접근 제어자의 종류 

자바의 접근 제어자는 

1. private :: 같은 클래스 내에서만 접근 가능 
2. default :: 같은 패키지 내에서만 접근 가능 
3. protected :: 같은 패키지, 또는 해당 클래스를 상속받은 외부 패키지의 클래스에서 접근이 가능
4. public :: 접근 제한이 없음 

총 4가지가 있고, 접근 권한을 표로 정리했을 때 다음과 같으며, `private -> default -> protected -> public` 순으로 많은 접근을 허용한다.

| 제어자    | 같은 클래스 | 같은 패키지 | 상속 | 전체 |
| --------- | ----------- | ----------- | ---- | ---- |
| private   | O           | X           | X    | X    |
| default   | O           | O           | X    | X    |
| protected | O           | O           | O    | X    |
| public    | O           | O           | O    | O    |



### ❌ private 

- 접근 제어자가 private 라면 해당 클래스내에서만 접근 가능


```java
import java.lang.*;

class TestClass {
    private int var1;

    public int getVar1() {
        return var1;
    }
}

public class Main {
    public static void main(String args[]) {
        TestClass testClass = new TestClass();

        System.out.println("var1 : "+testClass.getVar1());
        System.out.println("var1 : "+testClass.var1);
    }
}
```

TestClass 라는 클래스에 var1 변수가 private 로 선언되어 있다고 했을 때 같은 Class 인 getVar1 메소드에서는 var1 에 접근 할 수 있지만 

Main 클래스에서 testClass 의 변수인 var1 에 접근할려고 하는 경우 아래 사진과 같은 에러가 발생하면서 접근 할 수 없다. 

![https://user-images.githubusercontent.com/39696812/144703303-3d27432e-6085-43a0-9c60-2c00e117d860.png](https://user-images.githubusercontent.com/39696812/144703303-3d27432e-6085-43a0-9c60-2c00e117d860.png)


### 🏘 default 

- 접근제어자가 default 인 경우 default 접근제어자가 있는 패키지내에서만 접근 가능
- 접근제어자를 별도로 설정하지 않는 경우 default 로 설정된다 

```
package test1;
public class TestClass {
    int var1;
}


package test1;
import java.lang.*;
public class Main {
    public static void main(String args[]) {
        TestClass testClass = new TestClass();
        System.out.println("var1 : "+testClass.var1);
    }
}
```

test1 package 에 있는 TestClass 에 var1 이라는 변수가 있을 때 

같은 패키지에 위치해 있는 Main 클래스에서는 접근이 가능하지만 

```
package test2;
import test1.TestClass;

public class Test2Class {
    public void Test2Method() {
        TestClass testClass = new TestClass();
        testClass.var1;			// 오류 발생 
    }
}
```

다른 패키지인 test2 에서는 var1 에 접근하려고 할 때 아래와 같은 오류가 발생하며 접근할 수 없다

![https://user-images.githubusercontent.com/39696812/144703630-cd932a1f-3f75-4e8f-a7e8-a3301b2c9063.png](https://user-images.githubusercontent.com/39696812/144703630-cd932a1f-3f75-4e8f-a7e8-a3301b2c9063.png)



### 👼 protected

- 같은 패키지 또는 해당 클래스를 상속받은 외부 패키지의 클래스에서 접근이 가능

```java
package test1;
public class TestClass {
    protected int var1;
}

package test1;
import java.lang.*;

public class Main {
    public static void main(String args[]) {
        TestClass testClass = new TestClass();
        System.out.println("var1 : "+testClass.var1);
    }
}
```

default 예제와 동일하게 test1 패키지에 TestClass 가 있고 protected 로 접근제어자가 설정된 var1 변수가 있을 때 같은 패키지인 Main 클래스에서 접근 가능하다 

```java
package test2;
import test1.TestClass;
public class Test2ExtendClass extends TestClass {
    public void Test2ExtendClassMethod() {
        System.out.println(var1);
    }
}

package test2;
import test1.TestClass;
public class Test2Class {
    public void Test2Method() {
        TestClass testClass = new TestClass();
        System.out.println(testClass.var1);		 // 에러 발생 
    }
}

```

test2 패키지에 TestClass 를 상속받은 Test2ExtendClass 에선 var1 변수에 접근 가능하지만 

상속받지 않은 Test2Class 에서는 var1 변수에 접근이 불가능하다. 

![https://user-images.githubusercontent.com/39696812/144703954-5f71527b-239f-4011-ba82-524cc1c5f147.png](https://user-images.githubusercontent.com/39696812/144703954-5f71527b-239f-4011-ba82-524cc1c5f147.png)



### 👌 public 
- 접근 제한이 없음
- public 으로 설정한 경우 위 예제 모든 곳에서 접근 가능하다.



## 💊 접근 제어자를 이용한 캡슐화

캡슐화란 
- 비밀번호와 같은 데이터를 외부에서 함부로 변경하지 못하도록 하기 위해, 실제 구현내용 일부를 외부에 감추어 은닉하는 것 

```
public class Member {
	private String password;
	private int age;
	
	public String getPassword() {
		return password;
	}
	
	public int getAget() {
		return age;
	}
	
	public void setPassword(String password) {
		this.password = password;
	}
	
	public void setAget(int age) {
		this.age = age;
	}

}
```

➡️ 변수를 숨기기 위해 private 접근 제어자를 사용하고 get/set 메소드를 통해 변수에 접근 할 수 있도록 캡슐화 

### 🙇🏻‍♀️ 레퍼런스
[[JAVA] 접근제어자 (Access Modifier) by shaking shaking님](https://88240.tistory.com/448)<br>
[07-2 접근제어자 (Access Modifier) by 점프투자바](https://wikidocs.net/232)<br>
[[Java]접근 제어자(Access Modifier) by heyhyo님](https://hyoje420.tistory.com/13)<br>
[접근제어자와 캡슐화 by 록씨님](https://roxxy.tistory.com/entry/%EC%A0%91%EA%B7%BC%EC%A0%9C%EC%96%B4%EC%9E%90%EC%99%80-%EC%BA%A1%EC%8A%90%ED%99%94)<br>


