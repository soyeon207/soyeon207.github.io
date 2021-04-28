---
layout: post
title: 플라이 웨이트 패턴 (Flyweight pattern)
categories: 디자인패턴
tags: [디자인패턴, 플라이웨이트패턴, 구조패턴]
---

### 플라이웨이트 패턴 (flyweight pattern)
- 동일하거나 유사한 객체들 사이에 가능한 많은 데이터를 서로 공유하여 사용하도록 하여 메모리 사용량을 최소화하는 소프트웨어 디자인 패턴
- 객체를 직접 만드는게 아니라 없다면 만들고, 있으면 객체를 공유
- 많은 수의 객체를 생성해야 할 떄 주로 쓰임 
- 팩토리 메소드 안 객체를 새로 생성 -> 생성하는 객체가 내부적으로 참조하는 객체에 대해, 기존에 있는 객체를 참조만 하는 식으로 객체를 구성

### 구조
![https://user-images.githubusercontent.com/39696812/116422072-588c5380-a87a-11eb-9f36-ea0fff259b6e.png](https://user-images.githubusercontent.com/39696812/116422072-588c5380-a87a-11eb-9f36-ea0fff259b6e.png)
- Flyweight : 공유할 클래스 인터페이스 선언
- ConcreteFlyweight : 클래스 정의 
- FlyweightFactory : Flyweight의 인스턴스 생성 혹은 공유 
- Client : 사용자 

### 예제 
#### 첫번째
**Flyweight : 공유할 클래스 인터페이스 선언**
```JAVA
public interface Building {
  public void make(int x, int y, Color color);
}
```

**ConcreteFlyweight : 클래스 정의**
```JAVA
public class Company implements Building {

  public Company() {
    System.out.println("회사건물 !!");
  }

  @Override
  public void make(int x, int y, Color color) {
    System.out.println("회사건물 세우기");
  }
}

public class Store implements Building {
  public Store() {
    System.out.println("가게 !!");
  }

  @Override
  public void make(int x, int y, Color color) {
    System.out.println("가게건물 세우기");
  }
}
```

**FlyweightFactory : Flyweight의 인스턴스 생성 혹은 공유**
```JAVA
public class Factory {
  private static final HashMap<BuildingType, Building> buildings = new HashMap<BuildingType, Building>();

  public static Building getBuilding(BuildingType type) {
    Building building = buildings.get(type);

    if(building == null) {
      if (type.equals(BuildingType.STORE))) {
        building = new Store();
      } else if(type.equals(BuildingType.COMPANY)) {
        building = new Company();
      } else {
        return null;
      }
      buildings.put(type, building);
    } 
    return building;
  }

  public static enum BuildingType {
    STORE, COMPANY
  }
}
```

#### 두번째
```JAVA
public class BuildingModel {
  Height height;
  Width width;

  public BuildingModel() {
    height = new Hieght(300);
    width = new Width(300);
  }
}

public class Building {
  BuildingModel buildingModel;
  Location location;
  Color color;
}

public class BuildingFactory {
  private static final BuildingModel buildingModel = new BuildingModel();

  static public Building create(Location location, Color color) {
    Building building = new Building();
    building.setBuildingModel(buildingModel);
    building.setLocation(location);
    building.setColor(color);
    return building;
  }
}

```

### 패턴 활용 예시
#### JAVA String pool
```JAVA
class Main{
  public static void Main() {
    String s1 = "String";
    String s2 = "String";

    System.out.println(s1 == s2); // true
  }
}
```
- 메모리에 존재하는지 확인 후, 메모리에 존재하면 해당 메모리 값을 참조

#### JAVA valueOf()
```
public static Integer valueOf(int i) {
        assert IntegerCache.high >= 127;
        if (i >= IntegerCache.low && i <= IntegerCache.high)
             return IntegerCache.cache[i + (-IntegerCache.low)];
         return new Integer(i);
}
```
- 요청시에 새로 만드는게 아니라 IntegerCache에 이미 존재하는 참조를 반환

### 레퍼런스
- [ko.wikipedia.org/wiki](https://ko.wikipedia.org/wiki/%ED%94%8C%EB%9D%BC%EC%9D%B4%EC%9B%A8%EC%9D%B4%ED%8A%B8_%ED%8C%A8%ED%84%B4)
- [https://readystory.tistory.com/137](https://readystory.tistory.com/137)
- [https://m.blog.naver.com/2feelus/220669069127](https://m.blog.naver.com/2feelus/220669069127)
- [https://lee1535.tistory.com/106](https://lee1535.tistory.com/106)
