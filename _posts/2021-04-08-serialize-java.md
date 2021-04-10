---
layout: post
title: 직렬화 vs 마샬링 vs 역직렬화
categories: 개발
tags: [직렬화]
---

# 직렬화란(Serialize)란 ?

### 직렬화(Serialize)

object 나 data 를 저장하거나 나중에 재구성할 수 있는 포맷으로 변환 하는 과정

-> 바이트 스트림으로 바꿔서 데이터를 스트림에 쓰기 위해 연속적인 데이터로 변환 하는 것

object 를 직렬화 하는 걸 **마샬링**이라고도 한다

#### 마샬링 vs 직렬화 ??

마샬링과 직렬화는 비슷한 개념이지만 직렬화는 **바이트 스트림으로 변환** 하는 것, 마샬링은 **변환하는 과정**을 뜻한다

직렬화는 객체가 대상이지만 마샬링은 변환자체에 목적이 있음 그렇기 때문에 서로 다른 언어 간의 데이터 전송은 직렬화 X , 마샬링이라고 함

루비에서 캐시로 저장할 때 루비만의 방식으로 데이터를 저장하는데 그걸 자바 캐시에서 읽는 작업을 할 때 마샬링, 언마샬링이라는 단어를 써서 무슨 용어인지 궁금했었는데 루비와 자바 간의 객체가 다르고 서로 다른 언어 간의  

데이터 전송이니깐 마샬링이라고 하는 듯

### 역직렬화(Deserialize)

바이트 스트림으로 바뀐 데이터를 object 나 data 로 변환하는 과정

-> 객체가 저장되었던 그 상태로 변환하는 것

![https://www.geeksforgeeks.org/serialization-in-java/](https://media.geeksforgeeks.org/wp-content/cdn-uploads/gq/2016/01/serialize-deserialize-java.png)


### 참고 문서

https://hyesun03.github.io/2019/09/08/marshalling-vs-serialization/
