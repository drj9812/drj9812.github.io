---
title: "[Spring]컨테이너(Container)와 컨텍스트(Context)"
categories: [Framework, Spring]
tags: [Framework, Java, 자바, Spring, 스프링, Container, 컨테이너, Context, 컨텍스트]
image:
  path: /assets/img/posts/framework/spring/01-spring-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Spring
---

# 컨테이너(Container)와 컨텍스트(Context)

## 컨테이너(Container)란?

- 스프링 프레임워크의 핵심
- **빈(Bean) 객체의 라이프사이클을 관리**하고 **의존성을 주입**해주는 컴포넌트

## 컨테이너의 기능

### 빈 관리(Bean Management)

- 개발자가 정의한 빈 객체의 생명주기(생성, 초기화, 소멸)를 관리

### 의존성 주입(Dependency Injection)

- 빈 객체 간의 의존성을 주입하는 데 사용
	- **빈 객체 간의 결합도를 낮추고 유연성과 테스트 용이성을 증가**시킴

### AOP(Aspect Oriented Programming) 지원

- 관점 지향 프로그래밍(AOP, Aspect Oriented Programming)을 지원하여 핵심 로직과 횡단 관심사(Cross-cutting Concerns)를 분리하고, 관심사를 모듈화

### 트랜잭션 관리(Transaction Management)

- 선언적 트랜잭션 관리를 제공하여 개발자가 트랜잭션 설정에 대한 세부 사항을 명시하지 않고도 트랜잭션을 관리할 수 있음

> **스프링 컨테이너는 빈(Bean)을 기본적으로 싱글톤으로 관리**한다. 이를 통해 여러 개의 빈이 있더라도 하나의 인스턴스만을 생성하여 사용함으로써 메모리를 효율적으로 관리하고, 객체 간의 의존성을 관리할 수 있다. 이는 기존의 싱글톤 패턴의 단점들을 해결하면서 객체를 싱글톤으로 유지할 수 있도록 해준다. 따라서 스프링 컨테이너는 싱글톤 패턴을 직접 적용하지 않아도 되며, 싱글톤 객체의 생성과 관리를 담당하는 싱글톤 레지스트리 역할을 수행한다.
{: .prompt-info }

## 컨텍스트(Context)란?

- 스프링 컨테이너를 나타내는 인터페이스
- 스프링 애플리케이션의 구성 정보를 로드하고, 빈 객체를 인스턴스화하며, 의존성을 주입하는 등의 작업을 수행
- 여러 스프링 컨텍스트 구현체가 있으며, 각각의 컨텍스트는 특정한 용도나 설정 방식에 따라 다르게 사용됨

### 컨텍스트의 유형

![01-spring-container-hierarchy](/assets/img/posts/framework/spring/container-and-context/01-spring-container-hierarchy.jpg)
*qkrrnjswo, "스프링 컨테이너", [rnjswo9578.log](https://velog.io/@rnjswo9578/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88){: target="_blank" }, 2023-07-24*

#### BeanFactory

- 스프링 컨테이너의 최상위 인터페이스로서, 가장 기본적인 컨테이너
- 빈 객체의 라이프사이클 관리, 의존성 주입 등의 기능을 제공
	+ lazy-loading의 개념을 따라 **실제 빈 객체가 필요한 시점에 빈을 생성**
- 주로 자원(리소스) 사용을 최소화하고 싶을 때 사용됨

#### ApplicationContext

- BeanFactory를 확장한 인터페이스
	+ BeanFactory의 모든 기능을 포함하며, 더불어 추가적으로 다양한 기능을 제공
		* 메시지 소스 처리: 다국어 지원을 위해 메시지 번들을 관리하고 필요에 따라 메시지를 가져옴
		* 이벤트 발행: 옵저버 패턴을 사용하여 애플리케이션 내에서 이벤트를 발생시키고 이를 처리
		* 국제화(i8n) 지원
		* 환경 설정: 애플리케이션의 환경 변수, 프로퍼티 설정 등을 관리
		* 프로필(Profile)
		* 스프링 AOP: 핵심 로직과 횡단 관심사(Cross-cutting Concerns)를 분리하고 모듈화
		* 트랜잭션 관리: 애플리케이션에서 필요한 트랜잭션 설정을 간편하게 처리
		* 스프링의 라이프사이클 인터셉팅: 빈의 생성, 초기화, 소멸 등의 단계에서 특정 작업을 수행
		* 빈 후처리: 빈 객체를 생성한 후에 빈 객체에 대한 추가적인 처리
- **빈 객체들을 미리 생성하여 애플리케이션 시작 시점에 모든 빈 객체를 로드**
	+ eager-loading
- 다양한 환경 및 설정에 대한 지원을 제공하기 때문에 BeanFactory보다 일반적으로 사용됨

## 컨테이너의 설정과 컨텍스트 생성

> 빈의 이름(ID)는 유일한 식별자이므로 중복되어서는 안된다.
{: .prompt-warning }

### XML 기반

#### 컨테이너 설정

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
				    http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- 빈(Bean) 객체 정의 -->
    <!-- MyBean myBean = new Mybean(); -->
    <!-- 웹 서버가 실행되면 Mybean() 생성자를 호출하고 생성된 객체를 메모리에 로드 -->
    <bean id="myBean" class="com.example.MyBean">
        <!-- 의존성 주입 -->

        <!-- myBean.setDependency(dependencyBean()); -->
        <property name="dependency" ref="dependencyBean"/>

        <!-- myBean.setName("의존성 주입"); -->
        <property name="name" value="의존성 주입"/>
    </bean>
    <!-- return myBean; -->

    <!-- DependencyBean dependencyBean = new DependencyBean(); -->
    <bean id="dependencyBean" class="com.example.DependencyBean"/>
    <!-- return dependencyBean; -->

</beans>
```

> Java EE 웹 애플리케이션의 배치 서술자(deployment descriptor)인 `web.xml`{: .filepath }(웹 애플리케이션의 설정 및 구성 정보를 정의하는 XML 파일) 내에서 `<context-param>` 태그를 사용하여 XML 기반의 컨테이너 설정 파일의 위치를 명시할 수 있다. 이렇게 함으로써 해당 설정 파일의 위치를 애플리케이션에 알리고, 컨테이너가 해당 설정 파일을 인식하여 적용할 수 있게 된다. 설정 파일의 위치를 명시함으로써 컨테이너가 애플리케이션을 올바르게 구성하고 실행할 수 있도록 한다.
{: .prompt-info }

#### 컨텍스트 생성

```java
ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");

// 컨테이너에서 빈을 가져와서 사용
MyBean myBean = (MyBean) context.getBean("myBean");
myBean.doSomething();
```

- 클래스패스(ClassPath)에 위치한 XML 파일을 기반으로 스프링 컨테이너(`ApplicationContext`)를 생성

```java
// 파일 시스템에서 XML 설정 파일을 로드하여 스프링 컨테이너를 생성
ApplicationContext context = new FileSystemXmlApplicationContext("/path/to/applicationContext.xml");

// 컨테이너에서 빈을 가져와서 사용
MyBean myBean = (MyBean) context.getBean("myBean");
myBean.doSomething();
```

- 파일 시스템 상의 절대 경로를 기반으로 스프링 컨테이너(`ApplicationContext`)를 생성

```java
// XmlWebApplicationContext를 생성
ApplicationContext context = new XmlWebApplicationContext();

// 설정 파일을 지정
context.setConfigLocation("/WEB-INF/applicationContext.xml");

// 생성된 컨텍스트를 초기화
context.refresh();

// 컨테이너에서 빈을 가져와서 사용
MyBean myBean = (MyBean) context.getBean("myBean");
myBean.doSomething();
```

- 스프링 웹 애플리케이션에서 사용되며, XML 파일을 사용하여 설정된 웹 애플리케이션 컨텍스트(`ApplicationContext`)를 생성
- `XmlWebApplicationContext`를 생성하고, `setConfigLocation()` 메서드를 사용하여 웹 애플리케이션의 XML 설정 파일 경로를 설정
- 주로 Spring MVC 프로젝트에서 `DispatcherServlet`에 의해 사용됨
	+ `DispatcherServlet`을 생성하고 생성자에 `XmlWebApplicationContext`를 전달하여 `DispatcherServlet`이 생성된 스프링 컨텍스트를 사용할 수 있도록 함

> 위의 코드는 단독으로 실행되는 Java 애플리케이션이 아니라 스프링 웹 애플리케이션 내부에서 사용되어야 한다. 일반적으로 Spring MVC 프로젝트에서는 `web.xml`{: .filepath }이나 `JavaConfig`를 사용하여 `DispatcherServlet`을 설정하고 등록하게 된다.
{: .prompt-info }

### 어노테이션 기반

#### 컨테이너 설정

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component
public class MyBean {
    private DependencyBean dependency;

    @Autowired
    public void setDependency(DependencyBean dependency) {
        this.dependency = dependency;
    }

    public void doSomething() {
        dependency.doSomething();
    }
}
```

- 스프링 컨테이너의 빈으로 등록할 클래스에 `@Component` 어노테이션을 명시

#### 컨텍스트 생성

```java
// 어노테이션 기반 설정 클래스를 사용하여 Spring 컨테이너 생성
ApplicationContext context = new AnnotationConfigApplicationContext(MyBean.class, DependencyBean.class);

// 컨테이너에서 빈을 가져와서 사용
MyBean myBean = context.getBean(MyBean.class);
myBean.doSomething();
```

- `AnnotationConfigApplicationContext` 클래스를 사용하여 스프링 컨테이너(`ApplicationContext`)를 생성
- 생성자에는 어노테이션 기반 설정 클래스를 전달하면, 스프링 해당 클래스들을 검색하여 빈으로 등록

### 자바 기반

#### 컨테이너 설정

```java
@Configuration
public class AppConfig {

    @Bean
    public MyBean myBean() {
        return new MyBean(dependencyBean());
    }

    @Bean
    public DependencyBean dependencyBean() {
        return new DependencyBean();
    }
}
```

- 스프링의 컨테이너 설정 클래스에 `@Configuration` 어노테이션을 명시
- 스프링 컨테이너에 빈으로 등록되는 멤버에 `@Bean` 어노테이션을 명시

#### 컨텍스트 생성

```java
// 자바 기반 설정 클래스를 사용하여 스프링 컨테이너 생성
ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);

// 컨테이너에서 빈을 가져와서 사용
MyBean myBean = context.getBean(MyBean.class);
myBean.doSomething();
```

- `AnnotationConfigApplicationContext` 클래스를 사용하여 스프링 컨테이너(`ApplicationContext`)를 생성
- 생성자에는 자바 기반 설정 클래스를 인자로 전달
- 스프링은 이 클래스를 검색하여 빈을 생성하고 등록

## 참고자료

- [Composite, "Spring @Component @Bean 알고 쓰기", composite.log, 2020-05-27](https://velog.io/@composite/Spring-Component-Bean-%EC%95%8C%EA%B3%A0-%EC%93%B0%EA%B8%B0){: target="_blank" } 