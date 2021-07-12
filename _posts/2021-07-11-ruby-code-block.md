---
layout: post
title: <Ruby> 코드 블록(code block)이란 ?
categories: Ruby
tags: [Ruby]
---

> 매개 변수처럼 메서드 호출과 결합 할 수 있는 코드 

코드 블록은 `이름이 없는 함수`라고 생각하면 되고, 코드블록을 어떤 함수나 메소드에 매개변수로 전달 할 수 있다 


### 코드 블록 사용하는 방법 
첫번째, **중괄호**로 코드 블록 표시하기 

```ruby
{ puts "hello" }
```

두번째, **do/end** 로 코드 블록 표시하기 

```ruby
do 
  puts "hello"
end
```

- 두가지로 나뉘는 이유는 상황에 따라 더 자연스럽게 어울리는 방식이 있기 때문이다 
- 중괄호와 do/end 중에선 중괄호로 쓰는게 더 우선순위가 높다 


### 코드블록과 yield

```ruby
def code_block
  puts ">>>>>>>>"
  yield
  yield
  puts ">>>>>>>>"
end

code_block { puts "code_block" }

## >>>>>>>>
## code_block
## code_block
## >>>>>>>>
```

- 메소드에서는 루비의 yield 문을 이용해서 결합된 코드 블록을 여러 차례 실행 할 수 있다 
- 코드블록을 사용해서 넘긴 puts 가 yield 가 불릴 때마다 한 번씩 실행되어 총 두번 실행된다 
- yield 에 인자를 적으면 코드 블록에 이 값이 매개 변수로 전달 된다 

  ```ruby
  def test
    yield("soyeon", "hello")
  end
  
  test {|person, say| puts "#{person} 가 #{say} 라고 말함"}
  ```

### 사용 예시 

```ruby
words = %w( w o r d)

words.each do |word|
  puts word
end

words.each {|word| puts word}
```

- 코드 블록을 반복자 구현에 사용하기도 한다 

### 레퍼런스 

[[Ruby/루비] 코드 블록과 yield](https://junstar92.tistory.com/11)