---
title: "[Web | 강의]김영한, \"스프링 부트와 JPA 활용1 - 주문 도메인 개발\""
categories: [Programming, Web]
tags: [Programming, Java, 자바, Spring Boot, 스프링 부트, JPA, 인프런, Inflearn, 김영한]
image:
  path: /assets/img/posts/programming/web/lecture/using-spring-boot-and-jpa-1/01-using-spring-boot-and-jpa-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: 김영한, "실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발"
---

# 김영한, "스프링 부트와 JPA 활용1 - 주문 도메인 개발"

## 커리큘럼

1. [[Spring Boot]스프링 부트와 JPA 활용1 - 프로젝트 환경 설정](https://drj9812.github.io/posts/project-configuration/){: target="_blank" }
2. [[Spring Boot]스프링 부트와 JPA 활용1 - 도메인 분석 설계](https://drj9812.github.io/posts/domain-analysis-design/){: target="_blank" }
3. [[Spring Boot]스프링 부트와 JPA 활용1 - 회원 도메인 개발](https://drj9812.github.io/posts/member-domain-development){: target="_blank" }
4. [[Spring Boot]스프링 부트와 JPA 활용1 - 상품 도메인 개발](https://drj9812.github.io/posts/item-domain-development){: target="_blank" }
5. [**[Spring Boot]스프링 부트와 JPA 활용1 - 주문 도메인 개발**](https://drj9812.github.io/posts/order-domain-development){: target="_blank" }
6. [[Spring Boot]스프링 부트와 JPA 활용1 - 웹 계층 개발](https://drj9812.github.io/posts/web-layer-development){: target="_blank" }

## 주문, 주문 상품 Entity 개발

### domain.Order.java

```java
package jpabook.jpashop.domain;

import java.time.LocalDateTime;
import java.util.ArrayList;
import java.util.List;

import jakarta.persistence.CascadeType;
import jakarta.persistence.Column;
import jakarta.persistence.Entity;
import jakarta.persistence.EnumType;
import jakarta.persistence.Enumerated;
import jakarta.persistence.FetchType;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.Id;
import jakarta.persistence.JoinColumn;
import jakarta.persistence.ManyToOne;
import jakarta.persistence.OneToMany;
import jakarta.persistence.OneToOne;
import jakarta.persistence.Table;
import lombok.Getter;
import lombok.Setter;

@Entity
@Table(name = "orders")
@Getter @Setter
@NoArgsConstructor(access = AccessLevel.PROTECTED)
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

//	protected Order() {}

	// == 생성 메서드 == //
	public static Order createOrder(Member member, Delivery delivery, OrderItem... orderItems) {
		Order order = new Order();
		order.setMember(member);
		order.setDelivery(delivery);
		
		for (OrderItem orderItem: orderItems) {
			order.addOrderItem(orderItem);
		}
		
		order.setStatus(OrderStatus.ORDER);
		order.setOrderDate(LocalDateTime.now());
		
		return order;
	}
	
	// == 비즈니스 로직 == //
	/**
	 * 주문 취소
	 */
	public void cancel() {
		if (delivery.getStatus() == DeliveryStatus.COMP) {
			throw new IllegalStateException("이미 배송완료된 상품은 취소가 불가능합니다.");
		}
		
		this.setStatus(OrderStatus.CANCEL);
	
		for (OrderItem orderItem : this.orderItems) {
			orderItem.cancel();
		}
	}
	
	// == 조회 로직 == //
	/**
	 * 전체 주문 가격 조회
	 */
	public int getTotalPrice() {
		int totalPrice = 0;
		
//		for (OrderItem orderItem : this.orderItems) {
//			totalPrice += orderItem.getTotalPrice();
//		}
//		
//		return totalPrice;
		
		return this.orderItems.stream()
				.mapToInt(OrderItem::getTotalPrice  ).sum();
	}
}
```
{: file="domain.Order.java" }

- `protected Order() {}`
    + 생성 메서드를 사용하게 하기 위한 제약
    + [[JPA]Entity에 기본 생성자가 필요한 이유](https://drj9812.github.io/posts/why-jpa-entity-needs-no-args-constructor/){: target="_blank" } 참조

### domain.item.Item.java

```java
package jpabook.jpashop.domain.item;

import java.util.ArrayList;
import java.util.List;

import jakarta.persistence.Column;
import jakarta.persistence.DiscriminatorColumn;
import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.Id;
import jakarta.persistence.Inheritance;
import jakarta.persistence.InheritanceType;
import jakarta.persistence.ManyToMany;
import jpabook.jpashop.domain.Category;
import jpabook.jpashop.exception.NotEnoughStockException;
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
	
	// == 비즈니스 로직 == //
	/*
	 * stock 증가
	 */
	public void addStock(int quantity) {
		this.stockQuantity += quantity;
	}
	
	/*
	 * stock 감소
	 */
	public void removceStock(int quantity) {
		int restStock = this.stockQuantity - quantity;
		
		if (restStock < 0) {
			throw new NotEnoughStockException("need more stock");
		}
		this.stockQuantity = restStock;
	}
}
```
{: file="domain.item.Item.java"}

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
@NoArgsConstructor(access = AccessLevel.PROTECTED)
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
	
//    protected OrderItem() {}

	// == 생성 메서드 == //
	public static OrderItem createOrderItem(Item item, int orderPrice, int count) {
		OrderItem orderItem = new OrderItem();
		orderItem.setItem(item);
		orderItem.setOrderPrice(orderPrice);
		orderItem.setCount(count);
		
		item.removceStock(count);
		
		return orderItem;
	}
	
	// == 비즈니스 로직 == //
	public void cancel() {
		getItem().addStock(count); 
	}

	// == 조회 로직 == //
	/**
	 * 주문상품 전체 가격 조회
	 */
	public int getTotalPrice() {
		return getOrderPrice() * getCount();
	}
}
```
{: name="domain.OrderItem.java" }

- `protected OrderItem() {}`
    + 생성 메서드를 사용하게 하기 위한 제약
    + [[JPA]Entity에 기본 생성자가 필요한 이유](https://drj9812.github.io/posts/why-jpa-entity-needs-no-args-constructor/){: target="_blank" } 참조

## 주문 Repository 개발

### repository.OrderRepository.java

```java
package jpabook.jpashop.repository;

import org.springframework.stereotype.Repository;

import jakarta.persistence.EntityManager;
import jakarta.persistence.criteria.Order;
import lombok.RequiredArgsConstructor;

@Repository
@RequiredArgsConstructor
public class orderRepository {
	
	private final EntityManager em;
	
	public void save(Order order) {
		em.persist(order);
	}
	
	public Order findOne(Long id) {
		return em.find(Order.class, id);
	}
	
//	public List<Order> findAll(OrderSearch orderSearch) {}
}
```
{: name="repository.OrderRepository.java" }

## 주문 Service 개발

### service.OrderServuice.java

```java
package jpabook.jpashop.service;

import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import jpabook.jpashop.domain.Delivery;
import jpabook.jpashop.domain.Member;
import jpabook.jpashop.domain.Order;
import jpabook.jpashop.domain.OrderItem;
import jpabook.jpashop.domain.item.Item;
import jpabook.jpashop.repository.ItemRepository;
import jpabook.jpashop.repository.MemberRepository;
import jpabook.jpashop.repository.OrderRepository;
import lombok.RequiredArgsConstructor;

@Service
@Transactional(readOnly = true)
@RequiredArgsConstructor
public class OrderService {

	private final OrderRepository orderRepository;
	private final MemberRepository memberRepository;
	private final ItemRepository itemRepository;
	
	/**
	 * 주문
	 */
	@Transactional
	public Long order(Long memberId, Long itemId, int count) {
		
		// Entity 조회
		Member member = memberRepository.findOne(memberId);
		Item item = itemRepository.findOne(itemId);
		
		// 배송정보 생성
		Delivery delivery = new Delivery();
		delivery.setAddress(member.getAddress());
		
		// 주문상품 생성
		OrderItem orderItem = OrderItem.createOrderItem(item, item.getPrice(), count);
		
		// 주문 생성
		Order order = Order.createOrder(member, delivery, orderItem);
		
		// 주문 저장
		orderRepository.save(order);

		return order.getId();
	}

	/**
	 * 주문 취소
	 */
	@Transactional
	public void cancelOrder(Long orderId) {
		
		// 주문 Entity 조회
		Order order = orderRepository.findOne(orderId);
		
		// 주문 취소
		order.cancel();
	}
	
	// 검색
//	public List<Order> findOrders(OrderSearch orderSearch) {
//		return orderRepository.findAll(orderSearch);
//	}
}
```
{: name="service.OrderService.java" }

- [[Design Pattern]도메인 모델 패턴(Domain Model Pattern)](https://drj9812.github.io/posts/domain-model-pattern/){: target="_blank" } 참조

## 주문 기능 테스트

```java

```

## 주문 검색 기능 개발

```java

```

## 다음 글

- [[Spring Boot]스프링 부트와 JPA 활용1 - 웹 계층 개발](https://drj9812.github.io/posts/web-layer-development){: target="_blank" }

## 참고자료

- [김영한, "실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발", Inflearn, Date unknown](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-%ED%99%9C%EC%9A%A9-1){: target="_blank" }