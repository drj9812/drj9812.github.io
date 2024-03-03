---
title: "[JPA]Entity에 기본 생성자가 필요한 이유"
categories: [공부, JPA]
tags: [JPA, Reflection API, Proxy, 프록시]
---

# Entity에 기본 생성자가 필요한 이유

## 참조

- [[JPA]JPA](https://drj9812.github.io/posts/jpa/){: target="_blank" }
- [[JPA]Reflection API](https://drj9812.github.io/posts/reflection-api/){: target="_blank" }
- [[JPA]Hibernate Proxy(프록시)](https://drj9812.github.io/posts/hibernate-proxy/){: target="_blank" }

![01-no-args-constructor-compile-error](/assets/img/posts/study/jpa/why-jpa-entity-needs-no-args-constructor/01-no-args-constructor-compile-error.jpg)

`@Entity` 어노테이션이 붙은 `Entity` 클래스에 기본 생성자를 정의하지 않으면 위 사진과 같이 컴파일 에러가 발생한다.

## Reflection API

JPA는 객체를 생성할 때 내부적으로 Reflection API를 사용하는데, 이때 기본 생성자가 사용된다고 한다.

## JPA, Hibernate 스펙

> ***The entity class must have a public or protected constructor with no parameters**, which is called by the persistence provider runtime to instantiate the entity. The entity class may have additional constructors for use by the application.*
>
> ["Jakarta Persistence 3.2 Specification Document", Jakarta EE, 2023-11-23](https://jakarta.ee/specifications/persistence/3.2/jakarta-persistence-spec-3.2-m1){: target="_blank" }

> ***The entity class must have a no-argument constructor, which may be public, protected or package visibility**. It may define additional constructors as well.*
>
> ["User Guide", Hibernate ORM Documentation - 6.4, 2024-02-08](https://docs.jboss.org/hibernate/orm/6.4/userguide/html_single/Hibernate_User_Guide.html){: target="_blank" }

JPA와 JPA를 구현하는 Hibernate 스펙에서 `Entity` 클래스는 각각, `protected`, package visibility 이상의 기본 생성자를 가져야한다고 명시하고 있다.

### 왜 public, protected인가?

![02-private-no-args-constructor-comile-error](/assets/img/posts/study/jpa/why-jpa-entity-needs-no-args-constructor/02-private-no-args-constructor-comile-error.jpg)

JPA를 구현하는 Hibernate는 지연 로딩 전략을 사용할 때 클라이언트의 요청을 위임하기 위해 프록시 객체를 생성한다. 이 프록시 객체는 `Entity` 객체를 상속받는데, 만약 생성자의 접근 제어자가 `private`으로 선언되어 있다면, 하위 클래스인 프록시 객체가 해당 생성자를 상속받지 못하게 될 것이다. 

그럼에도 불구하고, 필드의 접근 제어자가 `private`으로 선언되는 것은 가능하다. 왜냐하면 프록시 객체는 `Entity` 클래스의 패키지 범위 이상의 getter/setter를 상속받아 필드에 우회적으로 접근할 수 있기 때문이다.

> `Entity` 클래스를 `final`로 선언할 수 없는 이유도 프록시 객체와 관련이 있다. `final`로 선언된 클래스는 상속받을 수 없기 때문에 프록시 객체가 상속받아야 할 `Entity` 클래스는 `final`로 선언될 수 없다.
{: .prompt-info }

## 참고자료

- [Mike Nakis, "Why does Hibernate require no argument constructor?", Stack Overflow, 2015-05-05](https://stackoverflow.com/questions/2935826/why-does-hibernate-require-no-argument-constructor){: target="_blank" }
- [Uzma Khan, "Need for Default Constructor in JPA Entities", Baeldung, 2024-01-08](https://www.baeldung.com/jpa-no-argument-constructor-entity-class){: target="_blank" }