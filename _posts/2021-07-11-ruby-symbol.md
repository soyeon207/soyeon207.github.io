---
layout: post
title: <Ruby> 심볼이란?
categories: Ruby
tags: [Ruby]
---

> 문자열과 거의 같은 오브젝트 , 문자열 같이 생긴 정수

루비에서 해시를 만들 때 아래 코드처럼 두가지 방법을 혼용해서 만드는 경우가 많은데 사용할 때 <br>
두가지의 정확한 차이점을 이해하지 않고 그냥 사용했던 경우가 많이 있었다 

```ruby
a = {'name': 'soyeon'}
b = {'name'=> 'soyeon'}

puts a[:name] # 3
puts b['name'] # 3
```

a 처럼 표현하는 방식이 해시를 만들 때 심볼로 만드는 방식이라고 해서 심볼에 대해서 알아 보려고 한다 


### 심볼이란 ?

이름을 직접 문자열로 처리하는 것보다 속도면에서 유리하기 때문에 루비의 내부구조에서는 메소드명 , 변수명 등등의 이름이 정수로 관리 되고 있다 

그리고 그 정수를 Ruby 의 코드상에서 표현한 것이 심볼

:test 로 쓰면 이게 "test" 라고 생각하되, 문자열과 다르게 방식한다고 생각하면 될 거 같다 

- 심볼은 변경이 `불가능(immutable)`한 객체
- 값이 한번 지정되고 나면 값을 변경하는 것은 불가능하지만, 덮어 쓰는 건 가능 
- 같은 값인 경우 반드시 동일 
- 문자열이 아니라 `데이터의 위치값을 가진 포인트값`을 참조한다 

### 심볼 사용하는 방법 

큰 따옴 (`""`) 대신 콜론 기호(`:`) 를 사용하면 된다 

```ruby
s = "test"
s1 = :test

puts s.class  ## String
puts s1.class ## Symbol
```

### 심볼의 특징 
#### 메모리 소비가 낮다 

포인터처럼 한번 만들어진 심볼은 여러번 사용할 때에도 같은 영역을 참조한다 

**문자열** 

```ruby
t = "test"
t1 = "test"

puts t.object_id    ## 14072300
puts t1.object_id   ## 14072200
puts t == t1        ## true
puts t.equal?(t1)   ## false
```

**심볼**

```ruby
t = :test
t1 = :test

puts t.object_id    ## 376988
puts t1.object_id   ## 376988
puts t == t1        ## true
puts t.equal?(t1)   ## true 
```

-> 문자열의 경우에는 두개의 object id 가 모두 다르지만, 심볼의 경우에는 두개의 object id 와 가르키는 메모리 공간도 동일한 걸 알 수 있다 

-> 같은 메모리를 가르키기 때문에 심볼의 경우 메모리 소비가 낮기 때문에 `속도도 빨라지게` 된다 


### 심볼을 해시키로 사용하는 이유

**문자열을 해시키로 사용하는 경우** 

Ruby 는 문자열을 평가하고 내용을 보고 (해시 함수 계산) 해시에 이미 저장된 키의 값과 결과를 비교해야 한다 


**심볼을 해시키로 사용하는 경우** 

해당 키가 불변이라는 게 암시적으로 정해져 있기 때문에 훨씬 빨리 키를 비교할 수 있다 


### 레퍼런스

[ruby 심볼이란](https://negabaro.github.io/archive/ruby-hash-symbol)

[Ruby language에서의 Symbol(심볼)이란?](https://www.hahwul.com/2018/03/28/ruby-language-symbol/)

[ruby - Ruby에서 기호를 해시 키로 사용하는 이유는 무엇입니까?](https://pythonq.com/so/ruby/20426)

