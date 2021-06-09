---
layout: post
title: 자바 object 매핑 (modelmapper, mapstruct)
categories: 자바
tags: [자바, 매핑, modelmapper, mapstruct]
---
### Entity DTO 매핑 필요성 
Entity -> DTO 로 변환하거나 DTO 에서 Entity 로 변환하는 경우는 굉장히 많다<br>
하지만 이때마다 개발자가 일일히 코드로 수정한다고 하면
1. 실수 하기 쉽고 <br>
2. 코드량이 많아지고 복잡해질 수 밖에 없다 <br>
3. 귀찮다 .. 😩

예시를 들어보자 
Student(학생)가 있고, Student 안에 TestResult(시험성적) 이 포함되어 있다고 가정해보자
```JAVA
@Builder
@Getter
public class Student {
    String name;
    int code; 
    String phoneNumber;
}

@Builder
@Getter
public class StudentDto {
    String name;
    int code;
    String phoneNumber;

    public StudentDto toEntity() {
        return Student.builder()
                        .name(this.name)
                        .code(this.code)
                        .phoneNumber(this.phoneNumber)
                        .builder();
    }

    public StudentDto toDto(Student student) {
        return StudentDto.builder()
                        .name(student.getName())
                        .code(student.getCode())
                        .phoneNumber(student.getPhoneNumber())
                        .builder();
    }
}
```
모든 필드에 대해서 일일히 매핑 해줘야 한다 지금 예시 같은 경우엔 3개 밖에 매핑 할게 없었지만<br>
10개가 넘는다고 하면 하나하나 매핑하면서 빼먹는 필드가 생길 수도 있고 필드가 변경 되는 경우 모두 수정해줘야 한다 

해당 문제 관련해 obejct mapping 하는 라이브러리가 많이 있는데 오늘은 **ModelMapper** 랑 **MapStruct**에 대해서 알아보자

### ModelMapper
> 단순하고 지능적인 객체 매핑

[공식 사이트](http://modelmapper.org)에 따르면 ModelMapper 은 **단순하고 지능적인 객체 매핑**이라고 설명하고 있다<br>
이 말 그대로 이름과 자료형이 같다면 자동으로 매핑해주고, 동일하지 않은 경우 몇가지의 설정을 통해 간단하게 매핑해줄수 있다 <br>
#### 적용하기 
의존성 주입<br>
**gradle** 
```
compile group: 'org.modelmapper', name: 'modelmapper', version: '2.3.0'
```
**maven**
```
<dependency>
  <groupId>org.modelmapper</groupId>
  <artifactId>modelmapper</artifactId>
  <version>2.4.2</version>
</dependency>
```

Spring Bean 등록<br>
```
@Bean
public ModelMapper modelMapper() {
    return new ModelMapper();
}
```
#### 사용하기
Entity -> DTO
```JAVA
modelMapper.map(student, StudentDto.class);
```
DTO -> Entity
```JAVA
modelMapper.map(studentDto, Student.class);
```
이렇게 사용해주면 일치하는 필드들에 대해서 매핑해줄 수 있다 😀 <br>
기본 매핑 외에 일치 하지 않는 필드들에 대해서 매핑 할 수 있는 방법이 [공식문서](http://modelmapper.org/user-manual/property-mapping/) 에도 많이 나와있으니 참고해서 사용하면 될 것 같다 <br>
나도 아직까진 기본 매핑 밖에 사용해보지 못했다


### MapStruct
> Java bean mappings, the easy way!

[공식 사이트](https://mapstruct.org/)에 따르면 MapStruct 는 구성 접근 방식에 대한 규칙을 기반으로 Java Bean 유형 간의 매핑 구현을 크게 단순화하는 코드 생성기 라고 한다 
#### 적용하기
의존성 주입<br>
**gradle(>= 4.6)**
```
implementation 'org.mapstruct:mapstruct:1.4.2.Final'
annotationProcessor 'org.mapstruct:mapstruct-processor:1.4.2.Final'
```
**maven**
```
...
<properties>
    <org.mapstruct.version>1.4.2.Final</org.mapstruct.version>
</properties>
...
<dependencies>
    <dependency>
        <groupId>org.mapstruct</groupId>
        <artifactId>mapstruct</artifactId>
        <version>${org.mapstruct.version}</version>
    </dependency>
</dependencies>
...
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.8.1</version>
            <configuration>
                <source>1.8</source> <!-- depending on your project -->
                <target>1.8</target> <!-- depending on your project -->
                <annotationProcessorPaths>
                    <path>
                        <groupId>org.mapstruct</groupId>
                        <artifactId>mapstruct-processor</artifactId>
                        <version>${org.mapstruct.version}</version>
                    </path>
                    <!-- other annotation processors -->
                </annotationProcessorPaths>
            </configuration>
        </plugin>
    </plugins>
</build>
```
#### 사용하기
modelMapper 와 다른 점은 MapStruct 는 매핑하는 interface 를 만들어준다 자세한 건 코드를 통해 알아보자
```JAVA
@Mapper(componentModel = "spring")
public interface StudentDtoMapper {
    StudenDtoMapper INSTANCE = Mappers.getMapper(StudenDtoMapper.class);

    StudentDto asDto(Student student);
}
```
Student entity 를 StudentDto 로 변환하는 코드이다. 이렇게 파일을 작성해주면 mapStruct 가 StudentDtoMapperImpl 을 작성해준다 ‼‼

**entity 안에 entity 가 있는 경우**
- @Mapping source => 파라미터 중 매핑 해야할 대상 , target => Dto 내 매핑 대상 
```JAVA
@Mapper(componentModel = "spring")
public interface StudentDtoMapper {
    StudenDtoMapper INSTANCE = Mappers.getMapper(StudenDtoMapper.class);

    @Mapping(source = "grade", target = "grade")
    StudentDto asDto(Student student, Grade grade);
}
```

**무시해야할 속성이 있는 경우**
- grade 는 매핑 대상에서 제외
```JAVA
@Mapping(target="grade", ignore=true)
```

**특정 속성에 자바 표현식을 넣고 싶은 경우**
- createdDate 에 지금 시간을 넣어줌 
```JAVA
@Mapping(target="createdDate", expression = "java(java.time.Instant.now())")
```

### ModelMapper vs MapStruct
ModelMapper 은 내부적으로 Runtime 시점에 Reflection API 를 사용해서 매핑해주지만 Reflection API 같은 경운 상대적으로 성능이 좋지 않다<br>
MapStruct 는 컴파일러 시점에 코드를 생성하기 때문에 안전하게 사용 가능하다 

[객체 매핑 관련해서 성능 비교한 글](https://www.baeldung.com/java-performance-mapping-frameworks)을 보면 ModelMapper 와 MapStruct 의 성능 차이를 확실히 알 수 있다 

Reflection API 같은 경운 상대적으로 성능이 좋지 않기 떄문에 MapStruct 를 사용하는게 좋다 

### 레퍼런스
- [Object Mapping 어디까지 해봤니?](https://meetup.toast.com/posts/213)
- [ModelMapper 사용하기](https://blog.deliwind.com/posts/235)
- [ModelMapper와 MapStruct에 대해서 학습하기](https://mangchhe.github.io/spring/2021/01/25/ModelMapperAndMapStruct/)
- [객체 변환하기. 자바 코드 매핑 vs MapStruct vs ModelMapper?](https://lob-dev.tistory.com/entry/%EA%B0%9D%EC%B2%B4-%EB%B3%80%ED%99%98%ED%95%98%EA%B8%B0-%EC%9E%90%EB%B0%94-%EC%BD%94%EB%93%9C-%EB%A7%A4%ED%95%91-vs-MapStruct-vs-ModelMapper)