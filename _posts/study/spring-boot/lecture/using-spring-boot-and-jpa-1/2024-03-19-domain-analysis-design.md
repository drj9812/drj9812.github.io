---
title: "[Spring Boot | 강의]김영한, \"스프링 부트와 JPA 활용1 - 도메인 분석 설계\""
categories: [Study, Spring Boot]
tags: [Java, 자바, Spring Boot, 스프링 부트, JPA, 인프런, Inflearn, 김영한]
image:
  path: /assets/img/posts/study/spring-boot/lecture/using-spring-boot-and-jpa-1/01-using-spring-boot-and-jpa-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: 김영한, "실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발"
---

# 김영한, "스프링 부트와 JPA 활용1 - 도메인 분석 설계"

## 커리큘럼

1. [[Spring Boot]스프링 부트와 JPA 활용1 - 프로젝트 환경 설정](https://drj9812.github.io/posts/project-configuration/){: target="_blank" }
2. [**[Spring Boot]스프링 부트와 JPA 활용1 - 도메인 분석 설계**](https://drj9812.github.io/posts/domain-analysis-design/){: target="_blank" }
3. [[Spring Boot]스프링 부트와 JPA 활용1 - 회원 도메인 개발](https://drj9812.github.io/posts/member-domain-development){: target="_blank" }
4. [[Spring Boot]스프링 부트와 JPA 활용1 - 상품 도메인 개발](https://drj9812.github.io/posts/item-domain-development){: target="_blank" }
5. [[Spring Boot]스프링 부트와 JPA 활용1 - 주문 도메인 개발](https://drj9812.github.io/posts/order-domain-development){: target="_blank" }
6. [[Spring Boot]스프링 부트와 JPA 활용1 - 웹 계층 개발](https://drj9812.github.io/posts/web-layer-development){: target="_blank" }

## 참조

- [[JPA]연관 관계 매핑](https://drj9812.github.io/posts/relationship-mapping/){: target="_blank" }

## 개발 환경

- IDE: Eclipse STS4
- 템플릿 엔진: Thymeleaf
- ORM 프레임워크: JPA
- DB: H2 Database

## 요구사항 분석

![01-requirement](/assets/img/posts/study/spring-boot/lecture/using-spring-boot-and-jpa-1/domain-analysis-design/01-requirement.jpg)
*요구사항*

| 요구사항 명 |          기능 ID        |    기능 명   | 필수 데이터 |
|:--------------:|:-----------------------:|:-------------:|:---------------:|
|   회원 기능  | MEMBER_REGISTER |  회원 등록  |   회원 이름  |
|                  |     MEMBER_FIND   |  회원 조회  |   회원 이름  |
|   상품 기능  |    PROD_REGISTER  |  상품 등록  |   상품 이름  |
|                  |    PROD_MODIFY    |  상품 수정  |   상품 이름  |
|                  |      PROD_FIND      |  상품 조회  |   상품 이름  |
|   주문 기능  |      ORDER_PROD   |  상품 주문  |   상품 이름  |
|                  |      ORDER_FIND    |  주문 조회  |   상품 이름  |
|                  |   ORDER_CANCEL   |  주문 취소  |   상품 이름  |

## 도메인 모델과 테이블 설계

![02-domain-model](/assets/img/posts/study/spring-boot/lecture/using-spring-boot-and-jpa-1/domain-analysis-design/02-domain-model.jpg)
*도메인 모델*

- 회원과 주문은 일대다(1:N) 관계
- 주문과 주문상품은 일대다(1:N) 관계
- 주문 상품과 상품은 다대일(N:1) 관계
- 상품의 카테고리와 상품은 다대다(N:N) 관계

> 회원은 여러 상품을 주문할 수 있다. 그리고 한 번 주문할 때 여러 상품을 선택할 수 있으므로 주문과 상품은 다대다 관계다. 하지만 이런 **다대다 관계는 관계형 데이터베이스는 물론이고 Entity에서도 거의 사용하지 않는다.** 따라서 그림처럼 주문 상품이라는 Entity를 추가해서 다대다 관계를 일대다, 다대일 관계로 풀어냈다.
{: .prompt-info }

### Entity 분석

![03-entity](/assets/img/posts/study/spring-boot/lecture/using-spring-boot-and-jpa-1/domain-analysis-design/03-entity.jpg)
*Entity*

#### 회원(Member)

- id: Long
	+ PK
- name: String
- address: Adress
	+ [[JPA]Value 클래스](https://drj9812.github.io/posts/value-class/){: target="_blank" } 참조
- orders: List

> 회원이 주문을 하기 때문에, 회원이 주문리스트를 가지는 것은 얼핏 보면 잘 설계한 것 같지만, 객체 세상은 실제 세계와는 다르다. 실무에서는 회원이 주문을 참조하지 않고, 주문이 회원을 참조하는 것으로 충분하다. 여기서는 일대다, 다대일의 양방향 연관관계를 설명하기 위해서 추가했다.
{: .prompt-tip }

#### 주문(Order)

- id
- member: Member
- orderItems: List
	+ 주문과 주문 상품은 일대다(1:N) 관계
- delivery: Delivery
- orderDate: Date
- status: OrderStatus
	+ `Enum`을 사용해서 주문(Order), 취소(Cancel) 표현

#### 배송(Delivery)

- id
- order: Order
- address: Address
- status: DeliveryStatus
	+ Value 클래스

#### 주문 상품(OrderItem)

- id
- item: Item
- order: Order
- orderPrice
- count
	+ 주문 상품 개수

#### 상품(Item)

- id
- name
- price: int
- stockQuantity
	+ 재고
- categories: List
	+ Album
		* artist
		* etc
	+ Book
		* author
		* isbn
	+ Movie
		* director
		* acgtor

#### 카테고리(Category)

- id
- name
- items: List
- parent: Category
- child: List

> 회원(Member)과 주문(Order)같은 양방향 연관 관계를 지양하고 단방향 연관 관계를 지향해야 한다. 또한, 카테고리(Category)와 상품(Item)같은 다대다(N:N) 관계는 일대다(1:N):다대일(N:1) 관계로 대체하는 것을 권장한다.
{: .prompt-tip }

### 테이블 분석

![04-table](/assets/img/posts/study/spring-boot/lecture/using-spring-boot-and-jpa-1/domain-analysis-design/04-table.jpg)
*Table*

> 상품(ITEM) 테이블은 싱글 테이블 전략을 사용한 것이다.
{: .prompt-info }

#### 연관 관계 매핑 분석

- 회원(MEMBER)과 주문(ORDERS)
	+ 일대다(다대일)의 양방향 관계
	+ `Order.member`, `ORDERS.MEMBER_ID` 외래 키(FK) 매핑
		* **외래 키가 있는 주문을 연관 관계의 주인으로 설계하는 것을 권장**
- 주문 상품(ORDER_ITEM)과 주문(ORDERS)
	+ 다대일의 양방향 관계
	+ `OrderItem.order`, `ORDER_ITEM.ORDER_ID` 외래 키 매핑
		* **외래 키가 있는 주문 상품을 연관 관계의 주인으로 설계하는 것을 권장**
- 주문 상품(ORDER_ITEM)과 상품(ITEM)
	+ 다대일의 단방향 관계
	+ `OrderItem.item`, `ORDER_ITEM.ITEM_ID` 외래 키
- 주문(ORDERS)과 배송(DELIVERY)
	+ 일대일의 양방향 관계
	+ `Order.delivery`, `ORDERS.DELIVERY_ID` 외래 키와 매핑
- 카테고리(CATEGORY)와 상품(ITEM)
	+ 일대다:다대다의 양방향 관계
		* 객체 관계에서는 다대다 관계를 컬렉션을 사용해서 표현할 수 있지만 테이블 관계에서는 다대다 관계를 표현하기 어렵기 때문에 `CATEGORY_ITEM` 테이블을 만들어 다대다 관계를 대체함
	+ `@ManyToMany` 어노테이션을 사용해서 매핑
		* 다대다 관계는 권장되지 않음

> 연관 관계의 주인은 단순히 외래 키를 누가 관리하냐의 문제이지 비즈니스상 우위에 있다고 주인으로 정하면 안된다. 예를 들어서 자동차와 바퀴가 있으면, 일대다 관계에서 항상 다쪽에 외래 키가 있으므로 외래 키가 있는 바퀴를 연관 관계의 주인으로 정하면 된다. 물론 자동차를 연관 관계의 주인으로 정하는 것이 불가능 한 것은 아니지만, 자동차를 연관 관계의 주인으로 정하면 자동차가 관리하지 않는 바퀴 테이블의 외래 키 값이 업데이트되므로 관리
와 유지보수가 어렵고, 추가적으로 별도의 업데이트 쿼리가 발생하는 성능 문제도 있다.
{: .prompt-tip }

## Entity 클래스 개발

> 이론적으로 Getter, Setter 모두 제공하지 않고, 꼭 필요한 별도의 메서드를 제공하는게 가장 이상적이다. 하지만 실무에서 Entity의 데이터는 조회할 일이 너무 많으므로, Getter의 경우 모두 열어두는 것이 편리하다. Getter는 아무리 호출해도 호출 하는 것 만으로 어떤 일이 발생하지는 않는다. 하지만 Setter는 문제가 다르다. Setter를 호출하면 데이터가 변한다. Setter를 막 열어두면 가까운 미래에 Entity가 도대체 왜 변경되는지 추적하기 점점 힘들어진다. 그래서 Entity를 변경할 때는 Setter 대신에 변경 지점이 명확하도록 변경을 위한 비즈니스 메서드를 별도로 제공해야 한다.
{: .prompt-tip }

### domain.Member.java

```java
package jpabook.jpashop.domain;

import java.util.ArrayList;
import java.util.List;

import jakarta.persistence.*;
import lombok.Getter;
import lombok.Setter;

@Entity
@Getter @Setter
public class Member {
	
    @Id @GeneratedValue
    @Column(name ="member_id")
    private Long id;
    
    private String name;
    
    @Embedded
    private Address address;
    
    @OneToMany(mappedBy = "member")
    private List<Order> orders = new ArrayList<>();
}
```
{: file="domain.Member.java" }

> Entity의 식별자는 ID를 사용하고 PK 컬럼명은 `member_id`를 사용했다. Entity는 타입(여기서는 Member )이 있으므로 ID 필드만으로 쉽게 구분할 수 있다. 테이블은 타입이 없으므로 구분이 어렵다. 그리고 테이블은 관례상 테이블명 + ID 를 많이 사용한다. 참고로 객체에서 ID 대신에 `memberId`를 사용해도 된다. 중요한 것은 일관성이다.
{: .prompt-tip }

### domain.Order.java

```java
package jpabook.jpashop.domain;

import java.time.LocalDateTime;
import java.util.ArrayList;
import java.util.List;

import jakarta.persistence.*;
import lombok.Getter;
import lombok.Setter;

@Entity
@Table(name = "orders")
@Getter
@Setter
public class Order {

    @Id
    @GeneratedValue
    @Column(name = "order_id")
    private Long id;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "member_id") // FK
    private Member member; // 주문 회원

    @OneToMany(mappedBy = "order", cascade = CascadeType.ALL)
    private List<OrderItem> orderItems = new ArrayList<>();

    @OneToOne(cascade = CascadeType.ALL, fetch = FetchType.LAZY)
    @JoinColumn(name = "delivery_id")
    private Delivery delivery; // 배송 정보

    private LocalDateTime orderDate; // 주문 시간

    @Enumerated(EnumType.STRING)
    private OrderStatus status; // 주문 상태[Order, Cancel]

    // == 연관 관계 편의 메서드 == //
    public void setMember(Member member) {
        this.member = member;
	member.getOrders().add(this);
    }

    public void addOrderItem(OrderItem orderItem) {
    	orderItems.add(orderItem);
	orderItem.setOrder(this);
    }

    public void setDelivery(Delivery delivery) {
        this.delivery = delivery;
	delivery.setOrder(this);
    }
}
```
{: file="domain.Order.java" }

- `cascade`
	+ 관계형 데이터베이스에서 사용되는 개념으로, 한 테이블의 행에 대한 작업이 해당 행과 관련된 다른 테이블의 행에 영향을 주는 것을 의미
	+ 부모-자식 관계를 갖는 테이블 간의 작업에서 사용
	+ `CascadeType.PERSIST`
		* Entity를 영속화할 때 연관된 Entity도 함께 영속화
	+ `CascadeType.MERGE`
		* Entity를 병합할 때 연관된 Entity도 함께 병합
	+ `CascadeType.REMOVE`
		* Entity를 제거할 때 연관된 Entity들도 함께 제거
	+ `CascadeType.REFRESH`
		* Entity를 새로고침할 때, 연관된 Entity들도 함께 새로고침
	+ `CascadeType.DETACH`
		* Entity를 영속성 컨텍스트로부터 분리하면 연관된 Entity들도 분리
	+ `CascadeType.ALL`
		* 모든 Cascade Type들이 적용

### domain.OrderStatus.java

```java
package jpabook.jpashop.domain;

public enum OrderStatus {
	
    ORDER, CANCEL
}
```
{: file="domain.OrderStatus.java" }

### domain.OrderItem.java

```java
package jpabook.jpashop.domain;

import jakarta.persistence.*;
import jpabook.jpashop.domain.item.Item;
import lombok.Getter;
import lombok.Setter;

@Entity
@Table(name = "order_item")
@Getter @Setter
public class OrderItem {

    @Id @GeneratedValue
    @Column(name = "order_item_id")
    private Long id;
	
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name ="item_id") // FK
    private Item item; // 주문 상품
	
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "order_id") // FK
    private Order order; // 주문

    private int orderPrice; // 주문 가격
    private int count; // 주문 수량
}
```
{: file="domain.OrderItem.java" }

### domain.Delivery.java

```java
package jpabook.jpashop.domain;

import jakarta.persistence.*;
import lombok.Getter;
import lombok.Setter;

@Entity
@Getter @Setter
public class Delivery {

    @Id @GeneratedValue
    @Column(name = "delivery_id")
    private Long id;
	
    @OneToOne(mappedBy = "delivery", fetch = FetchType.LAZY)
    private Order order;
	
    @Embedded
    private Address address;
	
    @Enumerated(EnumType.STRING)
    private DeliveryStatus status; // ENUM [READY(준비), COMP(배송)]
}
```
{: file="domain.Delivery.java" }

### domain.DeliveryStatus.java

```java
package jpabook.jpashop.domain;

public enum DeliveryStatus {

    READY, COMP
}
```
{: file="domain.DeliveryStatus.java" }

### domain.item.Item.java

```java
package jpabook.jpashop.domain.item;

import java.util.ArrayList;
import java.util.List;

import jakarta.persistence.*;
import jpabook.jpashop.domain.Category;
import lombok.Getter;
import lombok.Setter;

@Entity
@Inheritance(strategy = InheritanceType.SINGLE_TABLE)
@DiscriminatorColumn(name = "dtype")
@Getter @Setter
public abstract class Item {

    @Id @GeneratedValue
    @Column(name = "item_id")
    private Long id;
	
    private String name;
    private int price;
    private int stockQuantity;
	
    @ManyToMany(mappedBy = "items")
    private List<Category> categories = new ArrayList<>();
}
```
{: file="domain.Item.java" }

### domain.item.Album.java

```java
package jpabook.jpashop.domain.Item;

import jakarta.persistence.DiscriminatorValue;
import jakarta.persistence.Entity;
import lombok.Getter;
import lombok.Setter;

@Entity
@DiscriminatorValue("A")
@Getter @Setter
public class Album extends Item {
	
    private String artist;
    private String etc;
}
```
{: file="domain.item.Album.java" }

### domain.item.Book.java

```java
package jpabook.jpashop.domain.Item;

import jakarta.persistence.DiscriminatorValue;
import jakarta.persistence.Entity;
import lombok.Getter;
import lombok.Setter;

@Entity
@DiscriminatorValue("B")
@Getter @Setter
public class Book extends Item {

    private String author;
    private String isbn;
}
```
{: file="domain.item.Book.java" }

### domain.item.Movie.java

```java
package jpabook.jpashop.domain.Item;

import jakarta.persistence.DiscriminatorValue;
import jakarta.persistence.Entity;
import lombok.Getter;
import lombok.Setter;

@Entity
@DiscriminatorValue("M")
@Getter @Setter
public class Movie extends Item {

    private String director;
    private String author;
}
```
{: file="domain.item.Movie.java" }


### domain.Category.java

```java
package jpabook.jpashop.domain;

import java.util.ArrayList;
import java.util.List;

import jakarta.persistence.*;
import jpabook.jpashop.domain.item.Item;
import lombok.Getter;
import lombok.Setter;

@Entity
@Getter
@Setter
public class Category {

    @Id
    @GeneratedValue
    @Column(name = "category_id")
    private Long id;

    private String name;

    @ManyToMany
    @JoinTable(name = "category_item",
	             joinColumns = @JoinColumn(name = "category_id"),
		     inverseJoinColumns = @JoinColumn(name = "item_id"))
    private List<Item> items = new ArrayList<>();

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "parent_id")
    private Category parent;

    @OneToMany(mappedBy = "parent")
    private List<Category> child = new ArrayList<>();

    // == 연관 관계 편의 메서드 == //
    public void addChildCategory(Category child) {
        this.child.add(child);
	child.setParent(this);
    }
}
```
{: file="domain.Category.java" }

> 실무에서는 `@ManyToMany` 어노테이션을 사용하지 말자. `@ManyToMany` 어노테이션은 편리한 것 같지만, 중간 테이블(`CATEGORY_ITEM`)에 컬럼을 추가할 수 없고, 세밀하게 쿼리를 실행하기 어렵기 때문에 실무에서 사용하기에는 한계가 있다. 중간 Entity(`CategoryItem`)를 만들고 `@ManyToOne` , `@OneToMany` 어노테이션으로 매핑해서 사용하자. 정리하면 다대다 매핑을 일대다, 다대일 매핑으로 풀어내서 사용하자
{: .prompt-tip }

### domain.Address.java

```java
package jpabook.jpashop.domain;

import jakarta.persistence.*;
import lombok.Getter;

@Embeddable
@Getter
public class Address {

    private String city;
    private String street;
    private String zipcode;
	
    protected Address () {}
	
    public Address (String city, String street, String zipcode) {
        this.city = city;
	this.street = street;
	this.zipcode = zipcode;
	} 
}
```
{: file="domain.Address.java" }

> Value 클래스는 변경 불가능하게 설계해야 한다. `@Setter`를 제거하고, 생성자에서 값을 모두 초기화해서 변경 불가능한 클래스를 만들자. JPA 스펙상 Entity나 임베디드 타입(`@Embeddable`)은 자바 기본 생성자(default constructor)를 `public` 또는 `protected`로 설정해야 한다. `public`으로 두는 것 보다는 `protected`로 설정하는 것이 그나마 더 안전하다. JPA가 이런 제약을 두는 이유는 JPA 구현 라이브러리가 객체를 생성할 때 리플랙션 같은 기술을 사용할 수 있도록 지원해야 하기 때문이다.
{: .prompt-tip }

> FK를 설정하는 건 시스템마다 다르다. 정합성보다는 서비스 자체가 중요하다면 FK를 빼고 인덱스만 잘 잡아주면 되고, 돈과 관련된 데이터가 항상 맞아야 되면 FK를 사용한다.
{: .prompt-tip }

## Entity 설계시 주의점

### Setter 사용 X

- Entity 클래스는 가급적 Setter 사용 X
- 변경 포인트가 너무 많아서, 유지보수가 어려움
- 나중에 리펙토링으로 Setter 제거

### 모든 연관 관계는 지연 로딩으로 설정

- 즉시 로딩(`EAGER`)은 예측이 어렵고, 어떤 SQL이 실행될지 추적하기 어려움
- JPQL을 실행할 때 N+1 문제가 자주 발생
- 실무에서 모든 연관 관계는 지연 로딩(`LAZY`)으로 설정
- 연관된 Entity를 함께 DB에서 조회해야 하면, fetch join 또는 Entity 그래프 기능을 사용한다.
- `@XToOne` 관계는 기본이 즉시 로딩이므로 지연 로딩을 직접 명시해서 사용 

### 컬렉션은 필드 레벨에서 초기화

```java
Member member = new Member();
System.out.println(member.getOrders().getClass()); // class java.util.ArrayList

em.persist(member);
System.out.println(member.getOrders().getClass()); // class org.hibernate.collection.internal.PersistentBag
```

- 컬렉션은 필드 레벨에서 바로 초기화 하는 것이 안전
	+ `null` 문제에서 안전
- 임의의 메서드에서 컬력션을 잘못 생성하면 하이버네이트 내부 메커니즘에 문제가 발생
	+ Hibernate는 Entity를 영속화할 때, 컬렉션을 감싸서 Hibernate가 제공하는 내장 컬렉션으로 변경

### 테이블, 컬럼명 생성 전략

- 스프링 부트에서 Hibernate 기본 매핑 전략을 변경해서 실제 테이블 필드명은 다름
	+ [https://docs.spring.io/spring-boot/docs/2.1.3.RELEASE/reference/htmlsingle/#howtoconfigure-hibernate-naming-strategy](https://docs.spring.io/spring-boot/docs/2.1.3.RELEASE/reference/htmlsingle/#howtoconfigure-hibernate-naming-strategy){: target="_blank" }
	+ [http://docs.jboss.org/hibernate/orm/5.4/userguide/html_single/](http://docs.jboss.org/hibernate/orm/5.4/userguide/html_single/Hibernate_User_Guide.html#naming){: target="_blank" }
- 하이버네이트 기존 구현: Entity의 필드명을 그대로 테이블의 컬럼명으로 사용
- 스프링 부트 신규 설정: `SpringPhysicalNamingStrategy`
1. 카멜 케이스 > 언더스코어(memberPoint → member_point)
2. `.`(점) > `_`(언더스코어)
3. 대문자 > 소문자

#### 적용 2 단계

1. 논리명 생성: 명시적으로 컬럼, 테이블명을 직접 적지 않으면 `ImplicitNamingStrategy` 사용
	- `spring.jpa.hibernate.naming.implicit-strategy`: 테이블이나, 컬럼명을 명시하지 않을 때 논리명 적용
2. 물리명 적용
	- `spring.jpa.hibernate.naming.physical-strategy`: 모든 논리명에 적용됨, 실제 테이블에 적용
	- username, usernm 등으로 회사 룰로 바꿀 수 있음

#### 스프링 부트 기본 설정

`spring.jpa.hibernate.naming.implicit-strategy:
org.springframework.boot.orm.jpa.hibernate.SpringImplicitNamingStrategy`
`spring.jpa.hibernate.naming.physical-strategy:
org.springframework.boot.orm.jpa.hibernate.SpringPhysicalNamingStrategy`


## 다음 글

- [[Spring Boot]스프링 부트와 JPA 활용1 - 회원 도메인 개발](https://drj9812.github.io/posts/member-domain-development){: target="_blank" }

## 참고자료

- [김영한, "실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발", Inflearn, Date unknown](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-%ED%99%9C%EC%9A%A9-1){: target="_blank" }
- [5기_테오, "JPA Cascade는 무엇이고 언제 사용해야 할까?", Tecoble, 2023-10-16](https://tecoble.techcourse.co.kr/post/2023-08-14-JPA-Cascade/){: target="_blank "}