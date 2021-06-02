---
layout: post
title: 반복자 패턴 (이터레이터 패턴, iterator pattern)
categories: 디자인패턴
tags: [디자인패턴, 행동패턴]
---
### 반복자 패턴 (이터레이터 패턴, iterator pattern)
> 컬렉션 구현 방법을 노출시키지 않으면서도 그 집합체안에 들어있는 모든 항목에 접근할 수 있게 해 주는 방법을 제공해 주는 패턴.

자바를 예로 들었을 때 List 와 Map 을 동일한 방법으로 접근 하는 거라고 생각하면 됨 

### 필요한 이유  
자바의 컬렉션(Collection) 은 Set, List, Queue, Map 이 있음
4가지 모두 데이터를 저장하는 방식이 다르다 보니 안에 있는 모든 데이터를 가져온다고 할 때 모두 다른 쿼리로 값을 가져와야함 
```JAVA
List<Long> testList = new ArrayList<>();
testList.add(1L);
for (int i = 0; i < testList.size(); i++) {
System.out.println(testList.get(i));
}

HashMap<Long, Long> testMap = new HashMap<>();
testMap.put(1L, 1L);
for (Long key : testMap.keySet()) {
System.out.println(testMap.get(key));
}
```
이렇게 같은 값을 가져오는 거지만 로직이 달라짐 

캡슐화 되지 않은 반복에 대해서 사용자가 각기 다른 구조들에 대해서 모두 파악하고 있어야 함 

반복문을 돌아서 가져오는 게 동일하다고 하면 반복기능만 통일해서 사용할 수 있지 않을까 ? => 이터레이터 패턴 

### 구조
![https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile26.uf.tistory.com%2Fimage%2F99E9603359C75E353351CF](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile26.uf.tistory.com%2Fimage%2F99E9603359C75E353351CF)
- Iterator : 집합체의 요소들을 순서대로 검색하기 위한 인터페이스 정의
- ConcreateIterator : Iterator 인터페이스를 구현함
- Aggregate : 여러 요소들로 이루어져 있는 집합체
- ConcreateAggregate : Aggreagate 인터페이스를 구현하는 클래스

### 예제 
```JAVA
public interface Snack {
    void make();
}

public class Candy implements Snack {
    @Override
    public void make() {
        System.out.printf("사탕 만들기");
    }
}

public class Chocolate implements Snack {
    @Override
    public void make() {
        System.out.printf("초콜릿 만들기");
    }
}

public class CandyFactory {
    public static final int MAX_CAPA = 100;
    public static final List<Candy> CANDY_LIST = new ArrayList<>();

    public void make() {
        for(int i=0; i<MAX_CAPA; i++) {
            CANDY_LIST.add(new Candy());
        }
    }

    public Iterator<Candy> createIterator() {
        return CANDY_LIST.stream().iterator();
    }

}

public class ChocolateFactory {
    public static final int MAX_CAPA = 100;
    public static final HashMap<Integer, Chocolate> CHOCOLATE = new HashMap<>();

    public void make() {
        for(int i=0; i<MAX_CAPA; i++) {
            CHOCOLATE.put(i, new Chocolate());
        }
    }

    public Iterator<Chocolate> createIterator() {
        return CHOCOLATE.values().iterator();
    }

}

public class Main {
    public static void main(String[] args) {

        ChocolateFactory chocolateFactory = new ChocolateFactory();
        CandyFactory candyFactory = new CandyFactory();

        chocolateFactory.make();;
        candyFactory.make();

        Iterator<Candy> candyIterator = candyFactory.createIterator();
        Iterator<Chocolate> chocolateIterator = chocolateFactory.createIterator();

        while(candyIterator.hasNext()) {
            Candy candy = candyIterator.next();
            candy.make();
        }

        while(chocolateIterator.hasNext()) {
            Chocolate chocolate = chocolateIterator.next();
            chocolate.make();
        }
    }
}
```
=> CandyFactory 는 Candy 를 List 로 가지고 있고, ChocolateFactory 는 Chocolate 를 Map 으로 가지고 있지만 
이터레이터 패턴을 사용했을 때에는 Main 에선 같은 반복문의 형태로 값을 가져오는 걸 알 수 있음 

혹, 집합체에 이터레이터를 만드는 메소드가 제공되지 않는다면 직접 구현해주는 것도 방법임 

### Iterator 패턴 vs for 문 

1) Iterator 패턴을 사용했을 때
```JAVA
while (it.hasNext()) {
Chocolate chocolate = (Chocolate) it.next();
System.out.println(chocolate.getNum());
}
```

2) for문을 사용했을 때
```JAVA
for (int i = 0; i < ChocolateFactory.getLength(); i++) {
System.out.println(ChocolateFactory.getChocolate(i).getNum());
}
```

- ChocolateFactory의 구현에 의존하지 않는다.
- ChocolateFactory에 무언가 수정사항이 생기더라도 hocolateFactory가 올바른 Iterator를 반환하기만 한다면 while문이 사용되는 Main은 수정할 필요가 없다. 
  하나의 클래스를 수정하더라도 다른 클래스에 큰 영향 없이 작은 수정 만으로도 끝낼 수 있음

### Collection iterator
**Collections.java (interface)**

자바 모든 Collection 들은 해당 파일을 상속 받기 때문에 iterator() 메소드 사용 가능 
![https://user-images.githubusercontent.com/39696812/120523563-00181b00-c411-11eb-9258-470e7520fad1.png](https://user-images.githubusercontent.com/39696812/120523563-00181b00-c411-11eb-9258-470e7520fad1.png)

**HashMap.java**

values() 같은 경우에도 Collection<?> 로 리턴해주기 때문에 iterator() 메소드 사용 가능 
![https://user-images.githubusercontent.com/39696812/120523757-42d9f300-c411-11eb-8f3e-13ee60566b68.png](https://user-images.githubusercontent.com/39696812/120523757-42d9f300-c411-11eb-8f3e-13ee60566b68.png)


### 레퍼런스
- [https://gdtbgl93.tistory.com/143](https://gdtbgl93.tistory.com/143)
- [https://jusungpark.tistory.com/25](https://jusungpark.tistory.com/25)
- [https://zion830.tistory.com/33](https://zion830.tistory.com/33)
