---
title: "[Spring]Spring MVC"
categories: [Framework, Spring]
tags: [Framework, Java, 자바, Spring, 스프링, Spring MVC, 스프링 MVC]
image:
  path: /assets/img/posts/framework/spring/01-spring-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Spring
---

# Spring MVC

## 참조

- [[Design Pattern]MVC](https://drj9812.github.io/posts/mvc/){: target="_blank" }

## Spring MVC란?

- 스프링 프레임워크에서 제공하는 웹 애플리케이션 개발을 위한 모듈 중 하나
- Spring MVC는 **MVC2 모델에 기반**하며, Spring의 IoC(Inversion of Control) 컨테이너와 AOP(Aspect-Oriented Programming) 기능을 통합하여 웹 애플리케이션을 빠르게 개발할 수 있도록 지원
- Spring MVC는 강력한 기능과 유연성을 제공하여 웹 애플리케이션의 요구 사항을 다양하게 처리할 수 있음

## 동작 과정

![02-spring-mvc-structure](/assets/img/posts/framework/spring/spring-mvc/02-spring-mvc-structure.jpg)

1. 웹 애플리케이션 배포
	- 웹 애플리케이션이 서블릿 컨테이너(예: Tomcat)에 배포됨
2. 서블릿 컨테이너 시작
3. 배치 서술자(Deployment Descriptor) 로드
	- 웹 애플리케이션의 구성 정보가 저장되어 있는 `web.xml` 파일이 배치 서술자 역할을 함
	- `web.xml` 파일에는 서블릿, 필터, 리스너 등의 구성 요소들에 대한 설정이 포함되어 있음
4. `ContextLoaderListener` 생성 및 실행
	- `web.xml` 파일의 `<listener>` 태그에 정의된 `ContextLoaderListener` 클래스 실행
	- `ContextLoaderListener`는 `web.xml` 파일에 명시된 컨텍스트를 로딩하는 역할
5. 컨텍스트 로딩
	- `web.xml` 파일에 명시된 컨텍스트를 `ContextLoaderListener`가 로딩
		+ 일반적으로 웹 애플리케이션에서는 두 가지 컨텍스트(Application Context, Servlet Context)를 사용

### 예시

#### 1. 배치 서술자(Deployment Descriptor) 로드

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns="https://jakarta.ee/xml/ns/jakartaee"
	       xmlns:web="http://xmlns.jcp.org/xml/ns/javaee"
	       xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee https://jakarta.ee/xml/ns/jakartaee/web-app_5_0.xsd"
	       id="WebApp_ID" version="5.0">
	
    <display-name>spring1</display-name>

    <!-- Listener 설정 -->
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
    <!-- Listener 설정 끝 -->

    <!-- Application Context(Spring Context) 설정 -->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>/WEB-INF/application-context.xml</param-value>
    </context-param>
    <!-- Application Context(Spring Context) 설정 끝-->


    <!-- Servlet 설정 -->
    <servlet>
        <!-- Spring은 모든 요청을 dspatcherServlet이 처리 -->
        <servlet-name>dispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>

        <!-- String contextConfigLocation = "/WEB-INF/servlet-context.xml"; -->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>/WEB-INF/servlet-context.xml</param-value> 
        </init-param>

        <!-- 메모리에 로드되는 Servlet 순서 -->
        <load-on-startup>1</load-on-startup><!-- 해당 Servlet이 가장 먼저 로드되도록 설정 -->
    </servlet>
    <!-- Servlet 설정 끝 -->

    <!-- Servlet이 처리할 URL 매핑 설정 -->
    <servlet-mapping>
        <servlet-name>dispatcherServlet</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
    <!-- Servlet이 처리할 URL 매핑 설정 끝-->

    <!-- Filter 설정 -->
    <filter>
        <!-- Spring Framework에서 제공하는 Filter 사용 -->
        <filter-name>encodingFilter</filter-name> <!-- Filter 이름 -->
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>

        <!-- String encoding = "UTF-8"; -->
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
    </filter>
    <!-- Filter 설정 끝 -->

    <!-- Filter가 처리할 URL 매핑 설정 -->
    <filter-mapping>
        <filter-name>encodingFilter</filter-name> <!-- 사용할 Filter의 이름-->
        <url-pattern>/*</url-pattern>
    </filter-mapping>
    <!-- Filter가 처리할 URL 매핑 설정 끝-->
</web-app>
```

#### 2. ContextLoaderListener 생성 및 실행

```xml
<listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
```

- 배포 서술자에 명시된 컨텍스트를 생성하고 초기화하기 위해 `ContextLoaderListener` 클래스를 생성하고 실행

#### 3. 컨텍스트 로딩

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	   xmlns:context="http://www.springframework.org/schema/context"
	   xmlns:mybatis="http://mybatis.org/schema/mybatis-spring"
	   xsi:schemaLocation="http://www.springframework.org/schema/beans
					http://www.springframework.org/schema/beans/spring-beans.xsd
        				http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
        				http://mybatis.org/schema/mybatis-spring http://mybatis.org/schema/mybatis-spring.xsd">

	<!-- bean definitions here -->

</beans>
```

- `ContextLoaderListener`가 Application Context(`root-context.xml`)을 로딩
- Application Context(`root-context.xml`)
	+ 모든 서블릿이 공유할 수 있는 빈 객체들의 공간

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	   xmlns:context="http://www.springframework.org/schema/context"
	   xmlns:mvc="http://www.springframework.org/schema/mvc"
	   xsi:schemaLocation="http://www.springframework.org/schema/beans
					http://www.springframework.org/schema/beans/spring-beans.xsd
    				        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
        				http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!-- Dispatcher Servlet이 어노테이션을 사용하여 작성된 컨트롤러를 인식하도록 설정 -->
    <!-- Handler Mapping -->
    <mvc:annotation-driven />

    <!-- 스프링 컨텍스트가 해당 패키지 또는 하위 패키지에서 컴포넌트를 검색하도록 지시 -->
    <!-- 어노테이션이 지정된 클래스들은 스프링에서 관리되는 빈 객체로 등록 -->
    <context:component-scan base-package="패키지 이름" />

    <!-- Dispatcher Servlet이 처리하지 않는 정적인 요청에 필요한 파일들의 폴더 위치 -->
    <mvc:resources location="/static" mapping="/**" />

    <!-- View Resolver 설정 -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/views/" />
        <property name="suffix" value=".jsp" />
    </bean>
    <!-- View Resolver 설정 끝-->

</beans>
```

- `ContextLoaderListener`가 Servlet Context(`servlet.context.xml`)를 로딩
- Servlet Context(`servlet.context.xml`)
	+ 서블릿 각자의 빈 객체들의 공간
	+ 웹, 앱마다 한 개씩 존재하므로 웹, 앱 그자체를 의미
	+ Servlet Context 내의 빈 객체들은 서로 공유될 수 없음
	+ `<mvc:annotation-driven />`
		* Dispatcher Servlet이 어노테이션을 사용하여 작성된 컨트롤러를 인식하도록 설정
		* Handler Mapping 기능 활성화
	+ `<context:component-scan base-package="패키지 이름" />`
		* 스프링 컨텍스트가 해당 패키지 또는 하위 패키지에서 컴포넌트를 검색하도록 지시
		* 어노테이션이 지정된 클래스들은 스프링에서 관리되는 빈으로 등록
	+ `<mvc:resources location="/static" mapping="/**" />`
		* Dispatcher Servlet이 처리하지 않는 정적인 요청에 필요한 파일들의 폴더 위치
	+ `<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">`
		* `InternalResourceViewResolver` 빈 객체 설정
		* JSP 파일을 뷰로 사용할 때 해당 JSP 파일의 경로를 지정해주는 역할
		* 컨트롤러가 View 이름을 반환할 때 설정된 `prefix`와 `suffix`를 사용하여 경로를 결정하고 반환

> 일반적으로 웹 애플리케이션에서는 두 가지 컨텍스트(Application Context, Servlet Context)를 사용한다
{: .prompt-info }

## 참고자료

- [오잎 클로버, "[Spring] Spring Context 설명", WorkShop, 2022-02-13](https://workshop-6349.tistory.com/entry/Spring-Spring-Context-%EC%84%A4%EB%AA%85){: target="_blank" }