---
layout: post
title: 자바 직렬화
categories: 개발
tags: [자바,직렬화]
---

### 자바 직렬화 조건
직렬화 할 클래스에 java.io.Serializable 인터페이스를 상속
```
public class Test implements Serializable
```

### 자바 직렬화 하는 방법

java.io.ObjectOutputStream 사용

### 자바 역직렬화 하는 방법

java.io.ObjectInputStream 사용

### Serializable를 사용 하는 이유
csv, json 등을 통해 직렬화를 하게되면 다른 시스템간에서도 사용할 수 있다는 장점이 있다.

하지만 자바 직렬화는 자바의 시스템에 맞춰져 있기 때문에 목적에 맞게 써야겠지만 직렬화를 쓰는게 더 좋음

### 직렬화를 사용하는 경우
- 서블릿 세션
- 캐시
- 자바 RMI

### serialVeresionUID
1. Serializable 을 상속하는 Class 의 경우 serialVeresionUID 변수를 사용
2. serialVeresionUID 를 지정하지 않으면 컴파일러가 계산한 값 부여 (컴파일에 따라 값이 다를 수 있음) -> 필수 값 아님

### serialVeresionUID 관련 직렬화 시 오류 발생
1. 컴파일러는 Serializable Class 혹은 Outer Class를 참고하여 만들기 때문에 만약 클래스에 변경이 있으면 serialVersionUID도 변경이 있을 수 있음
2. 저장하는 쪽의 컴파일러와 불러오는 쪽의 컴파일러가 다를 경우 / 저장하는 시기의 클래스 내용과 불러오는 시기의 클래스 내용이 다를 경우 에러 발생

에러가 안나기 위해선

1. Serializable 를 사용할 때 serialVersionUID 지정
    → 멤버 변수 및 메서드 추가엔 문제 발생 X
    → 멤버 변수 제거 및 이름 변경은 오류는 발생하지 않지만 데이터는 누락
    → 멤버 변수 타입 변경시엔 에러 발생
2. Serializable 를 사용하지 않고 다른 포맷 사용

현재 Serializable 로 저장된 클래스를 변경해야하는 경우

→ 기존에 있는 파일을 고치는게 아니라 새로운 파일을 만들서 캐시에 저장

### 참고 자료
https://woowabros.github.io/experience/2017/10/17/java-serialize.html
https://woowabros.github.io/experience/2017/10/17/java-serialize2.html
https://m.blog.naver.com/PostView.nhn?blogId=writer0713&logNo=220922099055&proxyReferer=https:%2F%2Fwww.google.com%2F
https://stackoverflow.com/questions/10378855/java-io-invalidclassexception-local-class-incompatible
