---
title: "[Website | 강의]김영한, \"스프링 부트와 JPA 활용1 - 상품 도메인 개발\""
categories: [Programming, Website]
tags: [Programming, Java, 자바, Spring Boot, 스프링 부트, JPA, 인프런, Inflearn, 김영한]
image:
  path: /assets/img/posts/programming/website/lecture/using-spring-boot-and-jpa-1/01-using-spring-boot-and-jpa-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: 김영한, "실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발"
---

# 김영한, "스프링 부트와 JPA 활용1 - 상품 도메인 개발"

## 커리큘럼

1. [[Spring Boot]스프링 부트와 JPA 활용1 - 프로젝트 환경 설정](https://drj9812.github.io/posts/project-configuration/){: target="_blank" }
2. [[Spring Boot]스프링 부트와 JPA 활용1 - 도메인 분석 설계](https://drj9812.github.io/posts/domain-analysis-design/){: target="_blank" }
3. [[Spring Boot]스프링 부트와 JPA 활용1 - 회원 도메인 개발](https://drj9812.github.io/posts/member-domain-development){: target="_blank" }
4. [**[Spring Boot]스프링 부트와 JPA 활용1 - 상품 도메인 개발**](https://drj9812.github.io/posts/item-domain-development){: target="_blank" }
5. [[Spring Boot]스프링 부트와 JPA 활용1 - 주문 도메인 개발](https://drj9812.github.io/posts/order-domain-development){: target="_blank" }
6. [[Spring Boot]스프링 부트와 JPA 활용1 - 웹 계층 개발](https://drj9812.github.io/posts/web-layer-development){: target="_blank" }

## 상품 Entity 개발(비즈니스 로직 추가)

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
{: file="domain.MemberItem.java" }

> 일반적으로 도메인이 자체적으로 처리할 수 있는 기능이 있다면, 해당 기능을 도메인 모델 내에 구현하는 것이 좋다. 이것은 도메인 주도 설계(Domain-Driven Design, DDD)의 핵심 개념 중 하나다.
{: .prompt-tip }

### exception.NotEnoughStockException.java

```java
package jpabook.jpashop.exception;

public class NotEnoughStockException extends RuntimeException {
	
    public NotEnoughStockException() {}
	
    public NotEnoughStockException(String message) {
        super(message);
    }
	
    public NotEnoughStockException(String message, Throwable cause) {
        super(message, cause);
    }
	
    public NotEnoughStockException(Throwable cause) {
        super(cause);
    }
}
```
{: file="exception.NotEnoughStockException.java" }

## 상품 Repository 개발

### repository.ItemRepository.java

```java
package jpabook.jpashop.repository;

import java.util.List;

import org.springframework.stereotype.Repository;

import jakarta.persistence.EntityManager;
import jpabook.jpashop.domain.item.Item;
import lombok.RequiredArgsConstructor;

@Repository
@RequiredArgsConstructor
public class ItemRepository {

    private final EntityManager em;
	
    public void save(Item item) {
        if (item.getId() == null) {
	    em.persist(item);
			
	} else {
	    em.merge(item);
	}
    }
	
    public Item findOne(Long id) {
        return em.find(Item.class, id);
    }
	
    public List<Item> findAll() {
        return em.createQuery("SELECT i FROM Item AS i", Item.class)
	             .getResultList();
    }
}
```
{: file="repository.ItemRepository.java" }

## 상품 Service 개발

### service.ItemService.java

```java
package jpabook.jpashop.service;

import java.util.List;

import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import jpabook.jpashop.domain.item.Item;
import jpabook.jpashop.repository.ItemRepository;
import lombok.RequiredArgsConstructor;

@Service
@Transactional(readOnly = true)
@RequiredArgsConstructor
public class ItemService {

    private final ItemRepository itemRepository;
	
    @Transactional
    public void saveItem(Item item) {
        itemRepository.save(item);
    }
	
    public List<Item> findItems() {
        return itemRepository.findAll();
    }

    public Item findOne(Long itemId) {
        return itemRepository.findOne(itemId);
    }
}
```
{: file="service.ItemService.java" }

## 다음 글

- [[Spring Boot]스프링 부트와 JPA 활용1 - 주문 도메인 개발](https://drj9812.github.io/posts/order-domain-development){: target="_blank" }

## 참고자료

- [김영한, "실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발", Inflearn, Date unknown](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-%ED%99%9C%EC%9A%A9-1){: target="_blank" }