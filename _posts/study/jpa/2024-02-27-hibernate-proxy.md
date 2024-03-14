---
title: "[JPA]Hibernate 프록시(Proxy)"
categories: [Study, JPA]
tags: [JPA, Hibernate, Proxy, 프록시]
---

# Hibernate 프록시(Proxy)

## 참조

- [[JPA]JPA](https://drj9812.github.io/posts/jpa/){: target="_blank" }
- [[JPA]Entity에 기본 생성자가 필요한 이유](https://drj9812.github.io/posts/why-jpa-entity-needs-no-args-constructor/){: target="_blank" }

## 프록시란?

JPA의 구현체 중 하나인 Hibernate에서 프록시는 다른 객체를 대신하여 그 역할을 수행하는 객체를 말한다. **프록시는 클라이언트와 실제 객체 사이에 위치하여 클라이언트의 요청을 대신 처리하거나, 필요한 경우에만 실제 객체를 생성하고 동작시킨다.** 주로 지연 로딩(Lazy Loading)을 구현하기 위해 등장하였다.

### 예시

```java
import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.Table;

@Entity
@Table(name = "user")
public class UserEntity {
	
    @Id
    private String id;

    private String name;

    public UserEntity() {}

    public UserEntity(String id, String name) {
    	this.id = id;
    	this.name = name;
    }
    
    public String getId() {
        return id;
    }

    public String getName() {
        return name;
    }
}
```

```java
import javax.persistence.EntityManager;
import javax.persistence.EntityTransaction;
import studio.aroundhub.entity_manager_factory.entity.UserEntity;
import studio.aroundhub.entity_manager_factory.factory.CEntityManagerFactory;

public class EntityManagerFactoryApplication {

    public static void main(String[] args) {

        CEntityManagerFactory.initialization();
        EntityManager entityManager = CEntityManagerFactory.createEntityManger();

        EntityTransaction entityTransaction = entityManager.getTransaction();

        try {
            entityTransaction.begin();

            UserEntity findUser = entityManager.find(UserEntity.class, "drj9812");
            
            System.out.println("getClass(): " + findUser.getClass());
            System.out.println("getId(): " + findUser.getId());
            System.out.println("getName(): " + findUser.getName());
            
            entityTransaction.commit();

        } catch (Exception e) {
            e.printStackTrace();
            entityTransaction.rollback();

        } finally {
            entityManager.close();
        }
        CEntityManagerFactory.close();
    }
}
```

```console
find() 전
Hibernate: 
    select
        userentity0_.id as id1_0_0_,
        userentity0_.created_at as created_2_0_0_,
        userentity0_.name as name3_0_0_,
        userentity0_.updated_at as updated_4_0_0_ 
    from
        user userentity0_ 
    where
        userentity0_.id=?
find() 후
getClass(): class studio.aroundhub.entity_manager_factory.entity.UserEntity
getId(): drj9812
getName(): 스밀라
```

위 코드의 실행 결과를 보면, `find()` 메서드를 호출할 때 즉시 쿼리가 실행되면서 지정된 `Entity` 의 실제 객체를 DB에서 로드한다는 것을 알 수 있다. 즉, 클래스의 정보와 필드에 접근하는 것과 무관하게 쿼리가 실행되었다는 뜻이다.

```java
import javax.persistence.EntityManager;
import javax.persistence.EntityTransaction;
import studio.aroundhub.entity_manager_factory.entity.UserEntity;
import studio.aroundhub.entity_manager_factory.factory.CEntityManagerFactory;

public class EntityManagerFactoryApplication {

    public static void main(String[] args) {

        CEntityManagerFactory.initialization();
        EntityManager entityManager = CEntityManagerFactory.createEntityManger();

        EntityTransaction entityTransaction = entityManager.getTransaction();

        try {
            entityTransaction.begin();

	    System.out.println("getReference() 전");
            UserEntity refUser = entityManager.getReference(UserEntity.class, "drj9812");
            System.out.println("getReference() 후");

            System.out.println("getClass(): " + refUser.getClass());
            System.out.println("getId(): " + refUser.getId());
            System.out.println("getName(): " + refUser.getName());

	    entityTransaction.commit();

        } catch (Exception e) {
            e.printStackTrace();
            entityTransaction.rollback();

        } finally {
            entityManager.close();
        }
        CEntityManagerFactory.close();
    }
}
```

```console
getReference() 전
getReference() 후
getClass(): class studio.aroundhub.entity_manager_factory.entity.UserEntity$HibernateProxy$uZfvsnk8
getId(): drj9812
Hibernate: 
    select
        userentity0_.id as id1_0_0_,
        userentity0_.created_at as created_2_0_0_,
        userentity0_.name as name3_0_0_,
        userentity0_.updated_at as updated_4_0_0_ 
    from
        user userentity0_ 
    where
        userentity0_.id=?
getName(): 스밀라
```

반면에 위 코드의 실행 결과에서는 `find()` 메서드를 호출할 때와 달리, `getReference()` 메서드를 호출할 때 즉시 쿼리가 실행되지 않았다는 것을 알 수 있다.

`getReference()` 메서드는 프록시 객체를 생성하고 반환한다(이때 프록시 객체는 영속화된다). 실제로 `getClass()` 메서드를 출력한 값을 보면, 원본 `Entity` 클래스명 뒤에 Hibernate에서 자동으로 생성한 내부 클래스명이 추가되었다.

생성된 **프록시 객체는 `Entity`의 클래스 정보와 식별자(`@Id`)만을 가지고 있다.** 프록시 객체는 이미 식별자인 `id` 값을 가지고 있었기 때문에, 쿼리를 실행하지 않고 `getId()` 메서드를 호출하여 출력할 수 있었던 것이다. 하지만  `Entity`의 `name` 값은 프록시 객체가 가지고 있지 않기 때문에, `getName()` 메서드를 호출하려고 할 때 그제서야 쿼리를 실행한다. 실제로 **필요한 경우에만 DB에 접근**한 것이다.

따라서, `getNmae()` 메서드를 통해 가져온 name 값은 프록시 객체에서 가져온 것이 아닌 실제 `Entity` 객체에 위임하여 가져온 것이다.

![01-schematic-proxy-process](/assets/img/posts/study/jpa/hibernate-proxy/01-schematic-proxy-process.jpg)
*위 그림은 예시 코드의 과정을 간단하게 도식화한 것이다.*

## 프록시 객체의 생성과 초기화

프록시 객체의 생성과 초기화는 다른 개념이다. 프록시 객체가 "초기화된다"는 말은 프록시 객체가 실제 `Entity`로 대체되는 것, 즉, 실제 `Entity` 객체의 참조값을 갖게 된다는 것을 의미한다. 프록시 객체는 처음에는 실제 `Entity`의 참조를 가지고 있지 않고, 실제 `Entity`의 데이터가 필요한 시점에 해당 실제 `Entity`의 대리자로 동작하며 대체한다.

### 프록시 객체의 생성

- `getReference()` 메서드 호출
- 객체 그래프에서 연관된 `Entity`를 로드할 때, 연관된 `Entity`를 대신하는 프록시 객체가 생성
- 객체 그래프에서 연관된 `Entity`를 로드할 때, `FetchType`이 `LAZY`로 설정된 경우

> 프록시 객체는 생성되면 영속성 컨텍스트에 저장된다.
{: .prompt-info }

### 프록시 객체의 초기화

- 프록시 객체의 메서드 중에서 실제 `Entity`의 데이터가 필요한 경우
- 프록시 객체의 필드에 직접 접근하는 경우
- 영속성 컨텍스트 내에서 프록시 객체를 명시적으로 초기화하는 경우(`Hibernate.initialize()`)

> 프록시 객체는 한 번 초기화되면, 그 이후에는 다시 초기화되지 않는다.
{: .prompt-info }

## 특징

- **생성된 프록시 객체는 `Entity` 객체를 상속**
	+ 프록시 객체는 Reflection API를 통해 생성됨
- **`Entity` 객체에 대한 참조를 보관(프록시 객체 초기화)**
	+ 클래스 정보나 식별자가 아닌 프록시 객체의 멤버를 호출하면, 참조를 통해 멤버 호출을 위임하고 `Entity` 객체의 멤버를 호출함
- **동일성 보장**
	+ 영속성 컨텍스트에 `Entity`가 저장되어있다면, 프록시 객체가 생성되지 않음
	+ 반대로 영속성 컨텍스트에 프록시 객체가 저장되어있다면, `Entity`가 생성되지 않음
- 준영속, 영속 삭제 상태의 프록시를 초기화하는 경우 `LazyInitializationException` 에러가 발생
- OSIV 옵션이 `false`일 때, 트랜젝션 바깥에서 프록시를 초기화하는 경우 `LazyInitializationException` 에러가 발생
- 식별자에 대한 getter 메서드 이름이 자바 빈 규약을 만족시키지 못할 경우, 식별자 값에 접근하려할 때 프록시 객체가 초기화될 수 있음
	+ 메서드 이름이 `getId()`가 아닌 `findId()`인 경우
- 프록시 객체가 가진 필드 값들은 모두 `null`
	+ 식별자 값은 프록시가 아닌 프록시 내부의 인터셉터에 들어있음

## equals() 메서드 오버라이드

```java
@Override
public boolean equals(Object obj) {
    if (this == obj) {
        return true;			
    }

    if (obj == null) {
        return false;			
    }

    if (getClass() != obj.getClass()) {
        return false;
    }

    Order order = (Order) obj;

    return Objects.equals(id, order.id);
}
```

프록시 객체를 사용할 때 위와 같이 IDE의 자동 생성 기능을 활용하여 생성된 오버라이드된 `equals()` 메서드를 사용하면 문제가 생길 수 있다.

```java
Order order = userEntity.getOrder();
Order sameOrder = userEntity.getOrder();
assertThat(order).isEqualTo(sameOrder);
```

위 코드에서 변수 `order`와 `sameOrder`는 식별자로 접근하지 않았으므로 둘 다 프록시 객체다. `sameOrder`를 인자로 하여 `order`의 `equals()` 메서드를 호출하면, 정의한 `equals()` 메서드 내부의 `if (getClass() != obj.getClass()) {return false)` 코드가 실행된다. 이때 `getClass()` 메서드가 반환하는 객체는 원본 객체이고, 인자로 전달받은 `sameOrder`가 반환하는 객체는 프록시 객체이기 때문에 서로 주소값이 같음에도 불구하고 `equals()` 메서드는 `true`가 아닌 `false`를 반환한다.

```java
@Override
public boolean equals(Object obj) {
    if (this == obj) {
        return true;			
    }

    if (obj == null) {
        return false;			
    }

    if (!(obj instanceof Order) {
        return false;
    }

    Order order = (Order) obj;

    return Objects.equals(id, order.getId());
}
```

따라서 보편적인 의도대로 `equals()` 메서드를 오버라이드하기 위해서는, 위와 같이 `equals()` 메서드를 오버라이드할 때 일반적으로 사용되는 `getClass()` 메서드를 사용하지 않고 `instanceof` 연산자를 사용해야 한다. 프록시 객체는 원본 객체를 상속받았기 때문이다.

또한, 프록시의 모든 필드 값들은 `null`이기 때문에 필드 접근 방식 대신 프로퍼티 접근 방식을 사용해야 프록시 객체의 필드에 접근할 수 있다.

## 로딩 전략

### 즉시 로딩(Eager Loading)

```java
@Entity
@Table(name = "user")
public class UserEntity {
	
    // ...
    @ManyToOne(fetch = fetchType.EAGER)
    @JoinColumn(name = "order_id")
    private Order orderId
    // ...
}
```

- `Entity`를 조회할 때, `Entity`와 연관된 모든 테이블을 로딩
- 연관된 `Entity`를 조회하는 시점에 연관된 모든 테이블을 로딩하므로, 애플리케이션의 성능을 저하시킬 수 있음

### 지연 로딩(Lazy Loading)

```java
@Entity
@Table(name = "user")
public class UserEntity {
	
    // ...
    @ManyToOne(fetch = fetchType.LAZY)
    @JoinColumn(name = "order_id")
    private Order orderId
    // ...
}
```

- `Entity`가 실제로 필요한 시점에만 로딩
- DB와의 상호 작용을 최적화하고 애플리케이션의 성능을 향상시키는 데 도움을 줌

## 참고자료

- [4기_오찌, "JPA Hibernate 프록시 제대로 알고 쓰기", Tecoble, 2022-10-17](https://tecoble.techcourse.co.kr/post/2022-10-17-jpa-hibernate-proxy/){: target="_blank" }