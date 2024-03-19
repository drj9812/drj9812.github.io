---
title: "[Spring Boot]스프링 부트와 JPA 활용1 - 도메인 분석 설계"
categories: [Study, Spring Boot]
tags: [Java, 자바, Spring Boot, 스프링 부트, JPA, Thymeleaf, H2 Database, 인프런, Inflearn, 김영한]
---

# 스프링 부트와 JPA 활용1 - 도메인 분석 설계

## 커리큘럼

1. [[Spring Boot]스프링 부트와 JPA 활용1 - 프로젝트 환경 설정](https://drj9812.github.io/posts/project-configuration/){: target="_blank" }
2. [**[Spring Boot]스프링 부트와 JPA 활용1 - 도메인 분석 설계**](https://drj9812.github.io/posts/domain-analysis-design/){: target="_blank" }
3. [[Spring Boot]스프링 부트와 JPA 활용1 - 애플리케이션 구현 준비](https://drj9812.github.io/posts/preparing-for-application-implementation/){: target="_blank" }
4. [[Spring Boot]스프링 부트와 JPA 활용1 - 회원 도메인 개발](https://drj9812.github.io/posts/member-domain-development){: target="_blank" }
5. [[Spring Boot]스프링 부트와 JPA 활용1 - 상품 도메인 개발](https://drj9812.github.io/posts/product-domain-development){: target="_blank" }
6. [[Spring Boot]스프링 부트와 JPA 활용1 - 주문 도메인 개발](https://drj9812.github.io/posts/order-domain-development){: target="_blank" }
7. [[Spring Boot]스프링 부트와 JPA 활용1 - 웹 계층 개발](https://drj9812.github.io/posts/web-layer-development){: target="_blank" }

## 개발 환경

- IDE: Eclipse STS4
- 템플릿 엔진: Thymeleaf
- ORM 프레임워크: JPA
- DB: H2 Database

## 요구사항 분석

![01-requirement-analysis](/assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/domain-analysis-design/01-requirement-analysis.jpg)

| 요구사항 명 |          기능 ID        |    기능 명   | 필수 데이터 |
|:--------------:|:-----------------------:|:-------------:|:---------------:|
|   회원 기능  | MEMBER_REGISTER |  회원 등록  |   회원 이름   |
|                  |     MEMBER_FIND   |  회원 조회  |   회원 이름   |
|   상품 기능  |    PROD_REGISTER   |  상품 등록  |   상품 이름  |
|                  |    PROD_MODIFY    |  상품 수정  |   상품 이름  |
|                  |      PROD_FIND      |  상품 조회  |   상품 이름  |
|   주문 기능  |      ORDER_PROD    |  상품 주문  |   상품 이름  |
|                  |      ORDER_FIND     |  주문 조회  |  상품 이름   |
|                  |   ORDER_CANCEL   |  주문 취소  |    상품 이름  |

## 도메인 모델과 테이블 설계

![02-design-domain-and-table](/assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/domain-analysis-design/02-design-domain-and-table(1).jpg)

- 회원과 주문은 일대다(1:N) 관계
- 주문과 주문상품은 일대다(1:N) 관계
- 주문 상품과 상품은 다대일(N:1) 관계
- 상품의 카테고리와 상품은 다대다(N:N) 관계

> 회원은 여러 상품을 주문할 수 있다. 그리고 한 번 주문할 때 여러 상품을 선택할 수 있으므로 주문과 상품은 다대다 관계다. 하지만 이런 **다대다 관계는 관계형 데이터베이스는 물론이고 `Entity`에서도 거의 사용하지 않는다**. 따라서 그림처럼 주문 상품이라는 `Entity`를 추가해서 다대다 관계를 일대다, 다대일 관계로 풀어냈다.
{: .prompt-info }

### Entity 분석

![03-analysis-member-entity](/assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/domain-analysis-design/03-analysis-member-entity.jpg)

#### 회원(Member)

- id: Long
	+ PK
- name: String
- address: Adress
	+ Value 클래스
		* [[JPA]Value 클래스](https://drj9812.github.io/posts/value-class/){: target="_blank" }참조
- orders: List

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

> 회원(Member)과 주문(Order)같은 양방향 연관 관계를 지양하고 단방향 연관 관계를 지양해야 한다.
{: .prompt-tip }

### 테이블 분석

![04-analysis-member-table](/assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/domain-analysis-design/04-analysis-member-table.jpg)

- 회원과 주문
	+ 일대다(1:N) , 다대일(N:1)의 양방향 관계
	+ `Order.member`, `ORDERS.MEMBER_ID` 외래 키(FK) 매핑
		* **외래 키가 있는 주문을 연관 관계의 주인으로 설계하는 것을 권장**

- 주문 상품과 주문
	+ 다대일(N:1) 양방향 관계
	+ `OrderItem.order`, `ORDER_ITEM.ORDER_ID` 외래 키 매핑
		* **외래 키가 있는 주문 상품을 연관 관계의 주인으로 설계하는 것을 권장**

- 주문 상품과 상품
	+ 다대일(N:1) 단방향 관계
	+ `OrderItem.item`, `ORDER_ITEM.ITEM_ID` 외래 키

- 주문과 배송
	+ 일대일(1:1) 양방향 관계
	+ `Order.delivery`, `ORDERS.DELIVERY_ID` 외래 키와 매핑

- 카테고리와 상품
	+ 다대다(N:N) 양방향 관계
	+ `@ManyToMany` 어노테이션을 사용해서 매핑
		* 다대다 관계는 권장되지 않음

> 연관 관계의 주인은 단순히 외래 키를 누가 관리하냐의 문제이지 비즈니스상 우위에 있다고 주인으로 정하면 안된
다. 예를 들어서 자동차와 바퀴가 있으면, 일대다 관계에서 항상 다쪽에 외래 키가 있으므로 외래 키가 있는 바퀴
를 연관 관계의 주인으로 정하면 된다. 물론 자동차를 연관 관계의 주인으로 정하는 것이 불가능 한 것은 아니지만, 
자동차를 연관 관계의 주인으로 정하면 자동차가 관리하지 않는 바퀴 테이블의 외래 키 값이 업데이트되므로 관리
와 유지보수가 어렵고, 추가적으로 별도의 업데이트 쿼리가 발생하는 성능 문제도 있다.
{: .prompt-tip }

## Entity 클래스 개발1

## Entity 클래스 개발2

## Entity 설계시 주의점

## 다음 글

- [[Spring Boot]스프링 부트와 JPA 활용1 - 애플리케이션 구현 준비](https://drj9812.github.io/posts/preparing-for-application-implementation/){: target="_blank" }

## 참고 자료

- [김영한, "실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발", Inflearn, Date unknown](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-%ED%99%9C%EC%9A%A9-1){: target="_blank" }