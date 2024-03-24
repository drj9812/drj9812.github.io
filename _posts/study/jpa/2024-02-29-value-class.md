---
title: "[JPA]Value 클래스"
categories: [Study, JPA]
tags: [JPA, Value 클래스, 밸류 클래스, "@Embeddable", "@Embedded"]
---

# Value 클래스

## 참조

- [[JPA]JPA](https://drj9812.github.io/posts/jpa/){: target="_blank" }

## Value의 의미

![01-meaning-of-value](/assets/img/posts/study/jpa/value-class/01-meaning-of-value.jpg)

- 데이터 객체의 설계가 복잡해질 수록 컬럼이 길어져 각 컬럼이 의미하는 것을 파악하기 어려움
- 이때 동일한 의미를 가지고 있는 컬럼들을 구분하여 하나의 객체로 표현한 것을 밸류라고 부름

## 밸류 클래스 구현

- 밸류 클래스는 개념적으로 하나의 의미를 가질 수 있는 변수들을 포함
- 별도의 식별자를 갖지 않음
- 밸류 타입으로 설정되는 클래스는 `@Embeddable` 어노테이션을 통해 명시
- 밸류 클래스를 가져다 쓰는 클래스는 밸류 타입과 동일한 타입의 필드에 `@Embedded` 어노테이션을 명시

## @Embeddable, @Embedded

- `@Embeddable` 어노테이션은 해당 클래스가 다른 `Entity`의 일부로 사용될 수 있게 설정
- `@Embeddable` 어노테이션은 기본적으로 `@Embedded` 어노테이션에 따른 접근 타입을 사용
	+ 필드 접근 타입
		* Reflection API를 사용하여 `Entity`의 속성에 직접 접근
		* 권장
	+ 프로퍼티 접근 타입
		* JPA에서 getter/setter 메서드를 호출하여 `Entity`의 속성에 접근
		* 권장 X

> [[JPA]Reflection API](https://drj9812.github.io/posts/reflection-api/){: target="_blank" } 참조
{: .prompt-tip }

## 접근 방식 설정

### 필드 접근 방식

```java
@Entity
public class Provider {
	
    // ...
    @Embedded
    private Address address;
    // ...
}
```

### 프로퍼티 접근 방식

```java
@Entity
public class Provider {
	
    // ...
    @Embedded
    public Address getAddress() {
        return address;
    }
    // ...
}
```

> `@Embedded`로 매핑된 `Entity`의 접근 방식과 상관없이 `@Embeddable` 클래스에서 접근 방식을 설정하기 위해서는 `@Access(AccessType.FIELD)`, `@Access(AccessType.PROPERTY)` 어노테이션을 사용하면 접근 타입을 고정할 수 있다.
{: .prompt-info }

## 밸류 클래스의 라이프 사이클

- 밸류 클래스는 해당 객체를 매핑한 `Entity`와 라이프 사이클이 동일
- `Entity`에 대한 저장, 수정, 삭제 작업을 수행하면 밸류 클래스도 동일하게 진행

## 실행 예시

### 구조 및 코드

![02-value-project-structure](/assets/img/posts/study/jpa/value-class/02-value-project-structure.jpg)

#### Address.java

```java
package studio.aroundhub.value.entity;

import java.util.Objects;
import javax.persistence.Access;
import javax.persistence.AccessType;
import javax.persistence.Embeddable;

@Embeddable
@Access(AccessType.FIELD)
public class Address {
	
    private String zipCode;
    private String street;
    private String city;
    private String state;

    public Address() {}

    public Address(String zipCode, String street, String city, String state) {
        this.zipCode = zipCode;
        this.street = street;
        this.city = city;
        this.state = state;
    }

    public String getZipCode() {
        return zipCode;
    }

    public void setZipCode(String zipCode) {
        this.zipCode = zipCode;
    }

    public String getStreet() {
        return street;
    }

    public void setStreet(String street) {
        this.street = street;
    }

    public String getCity() {
        return city;
    }

    public void setCity(String city) {
        this.city = city;
    }

    public String getState() {
        return state;
    }

    public void setState(String state) {
        this.state = state;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) {
            return true;
        }
        if (o == null || getClass() != o.getClass()) {
            return false;
        }
        Address address = (Address) o;
        return Objects.equals(zipCode, address.zipCode) &&
               Objects.equals(street, address.street) &&
               Objects.equals(city, address.city) &&
               Objects.equals(state, address.state);
    }

    @Override
    public int hashCode() {
        return Objects.hash(zipCode, street, city, state);
    }
}
```

#### OriginProvider.java

```java
package studio.aroundhub.value.entity;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.Table;

@Entity
@Table(name = "original_provider")
public class OriginProvider {
	
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String code;
    private String name;
    private String zipCode;
    private String street;
    private String city;
    private String state;

    public OriginProvider() {}

    public OriginProvider(String code, String name,
    			          String zipCode, String street, String city, String state) {
        this.code = code;
        this.name = name;
        this.zipCode = zipCode;
        this.street = street;
        this.city = city;
        this.state = state;
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getCode() {
        return code;
    }

    public void setCode(String code) {
        this.code = code;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getZipCode() {
        return zipCode;
    }

    public void setZipCode(String zipCode) {
        this.zipCode = zipCode;
    }

    public String getStreet() {
        return street;
    }

    public void setStreet(String street) {
        this.street = street;
    }

    public String getCity() {
        return city;
    }

    public void setCity(String city) {
        this.city = city;
    }

    public String getState() {
        return state;
    }

    public void setState(String state) {
        this.state = state;
    }
}
```

#### Provider.java

```java
package studio.aroundhub.value.entity;

import javax.persistence.Embedded;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

@Entity
public class Provider {
	
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String code;
    private String name;

    @Embedded
    private Address address;

    public Provider() {}

    public Provider(String code, String name, Address address) {
        this.code = code;
        this.name = name;
        this.address = address;
    }
    
    public Long getId() {
        return id;
    }

    public Address getAddress() {
        return address;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getCode() {
        return code;
    }

    public void setCode(String code) {
        this.code = code;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void setAddress(Address address) {
        this.address = address;
    }
}
```

#### CEntityManagerFactory.java

```java
package studio.aroundhub.value.factory;

import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.Persistence;

public class CEntityManagerFactory {
    private static EntityManagerFactory entityManagerFactory;

    /**
     * EntityManagerFactory는 EntityManager를 생성하기 위한 팩토리 인터페이스로 persistence 단위별로 팩토리를 생성
     */
    public static void initialization(){
        entityManagerFactory = Persistence.createEntityManagerFactory("value");
    }

    /**
     * EntityManager 객체를 생성
     * EntityManager 는 Persistence Context와 Entity를 관리
     *
     * @return EntityManager 객체
     */
    public static EntityManager createEntityManger(){
        return entityManagerFactory.createEntityManager();
    }

    /**
     * EntityManagerFactory 객체 종료
     */
    public static void close(){
        entityManagerFactory.close();
    }
}
```

#### ValueService.java

```java
package studio.aroundhub.value.service;

import javax.persistence.EntityManager;
import studio.aroundhub.value.entity.Address;
import studio.aroundhub.value.entity.OriginProvider;
import studio.aroundhub.value.entity.Provider;
import studio.aroundhub.value.factory.CEntityManagerFactory;

public class ValueService {

    public void insertOriginalProvider() {
    	
        EntityManager entityManager = CEntityManagerFactory.createEntityManger();

        // EntityTransaction으로 객체를 생성해서 트랜잭션 관리를 해도 되지만 EntityManager만으로도 사용할 수 있음
        entityManager.getTransaction().begin();

        try {
            System.out.println("CheckPoint 1");

            OriginProvider originProvider = new OriginProvider("aroundhub123", "어라운드허브", "01234",
                                                               "어라운드로", "강남구", "서울특별시");

            System.out.println("CheckPoint 2");
            
            // Persistence Context에 객체 추가
            entityManager.persist(originProvider); // 이 단계에서 Id 값이 영속성 컨텍스트에 반영됨

            System.out.println("CheckPoint 3");

            // 실제 DB 적용
            entityManager.getTransaction().commit();

            System.out.println("CheckPoint 4");
        } catch (Exception e) {
            entityManager.getTransaction().rollback();
            e.printStackTrace();

        } finally {
            entityManager.close();
        }
    }

    public void insertProvider() {
        EntityManager entityManager = CEntityManagerFactory.createEntityManger();

        // EntityTransaction으로 객체를 생성해서 트랜잭션 관리를 해도 되지만 EntityManager만으로도 사용할 수 있음
        entityManager.getTransaction().begin();

        try {
            System.out.println("CheckPoint 1");

            Provider provider = new Provider("aroundhub123", "어라운드허브",
                                             new Address("01234", "어라운드로", "강남구", "서울특별시"));

            System.out.println("CheckPoint 2");
            // Persistence Context에 객체 추가
            entityManager.persist(provider); // 이 단계에서 Id 값이 영속성 컨텍스트에 반영됨

            System.out.println("CheckPoint 3");

            // 실제 DB 적용
            entityManager.getTransaction().commit();

            System.out.println("CheckPoint 4");
        } catch (Exception e) {
            entityManager.getTransaction().rollback();
            e.printStackTrace();

        } finally {
            entityManager.close();
        }
    }
}
```

#### ValueApplication.java

```java
package studio.aroundhub.value;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import studio.aroundhub.value.factory.CEntityManagerFactory;
import studio.aroundhub.value.service.ValueService;

public class ValueApplication {

    public static void main(String[] args) throws IOException {
    	
        CEntityManagerFactory.initialization();
        ValueService valueService = new ValueService();

        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        while (true) {
            System.out.println("Input your Command // [command] [name]");
            String commandLine = br.readLine();
            String[] splitCommand = commandLine.split(" ");

            // 별도 값 검증하는 로직은 추가하지 않음
            if (splitCommand[0].equalsIgnoreCase("exit")) {
                System.out.println("System closed");
                
                break;

            } else if (splitCommand[0].equalsIgnoreCase("originalProvider")) {
                valueService.insertOriginalProvider();

            } else if (splitCommand[0].equalsIgnoreCase("provider")) {
                valueService.insertProvider();
            }
        }
    }
}
```

### 실행 결과

![03-insert-originalprovider](/assets/img/posts/study/jpa/value-class/03-insert-originalprovider.jpg)
*originalProvider 입력*

`persist()` 메서드가 호출될 때 쿼리 실행

![04-insert-originalprovider-result](/assets/img/posts/study/jpa/value-class/04-insert-originalprovider-result.jpg)
*orginalProvider 입력 결과*

![05-insert-provider](/assets/img/posts/study/jpa/value-class/05-insert-provider.jpg)
*provider 입력*

`persist()` 메서드가 호출될 때 쿼리 실행

![06-insert-provider-result](/assets/img/posts/study/jpa/value-class/06-insert-provider-result.jpg)
*provider 입력 결과*

## 참고자료

- [어라운드 허브 스튜디오 - Around Hub Studio, "밸류 클래스 활용하기 (@Embeddable, @Embedded) [ JPA (Java Persistence API) ]", 2022-01-06](https://www.youtube.com/watch?v=VekLVb6msuU&list=PLlTylS8uB2fAnOge-kDI0ZQxgj78bdTOO&index=6){: target="_blnak" }