---
title: "[Java | JSP]액션 태그"
categories: [Language, Java]
tags: [Java, 자바, JSP, Action tag, 액션 태그]
image:
  path: /assets/img/posts/language/java/01-java-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Java
---

# 액션 태그

## 액션 태그란?

- JSP에서 미리 정의된 작업을 수행하기 위한 특수 태그
- JSP 페이지에서 Java 코드를 사용하지 않고 특정 작업을 수행할 수 있게 해줌
  + 자바빈(JavaBean)을 사용할 때, 포함 파일을 처리할 때, 포워딩을 할 때 사용됨
- 표준 태그 라이브러리(JSTL)와는 다르며, JSP 사양의 일부로 포함되어 있음

## \<jsp:useBean>

```jsp
<jsp:useBean id="beanName" class="com.example.BeanClass" scope="page | request | session | application"/>
```

- 자바빈을 선언하고, 인스턴스화하며, 스코프 내에서 사용하도록 설정
- `id`: 인스턴스 이름
- `class`: 클래스 경로
- `scope`: 생성한 자바빈 객체의 생존 범위
  + deafult page
  + [[Java \| JSP]내장 객체](https://drj9812.github.io/posts/implicit-objects/){: target="_blank" } 참조

## \<jsp:setProperty>

```jsp
<jsp:setProperty name="beanName" property="propertyName" value="value"/>
```

- 자바빈의 프로퍼티를 설정

```jsp
<jsp:setProperty name="beanName" property="*"/>
```

- 또는 모든 프로퍼티를 설정할 수도 있음

## \<jsp:getProperty>

```jsp
<jsp:getProperty name="beanName" property="propertyName"/>
```

- 자바빈의 프로퍼티 값을 JSP 출력으로 가져옴

## \<jsp:include>

```jsp
<jsp:include page="includedPage.jsp"/>
```

- 다른 JSP 파일을 현재 페이지에 포함
- 정적인 포함(inlcude 디렉티브)과는 다르게 동적으로 실행
- 참조
  + [[Java \| JSP]include 디렉티브와 include 액션 태그의 차이](https://drj9812.github.io/posts/the-difference-between-include-directive-and-include-action-tag/){: target="_blank" }

## \<jsp:param>

```jsp
<jsp:include page="included.jsp">
  <jsp:param name="paramName" value="paramValue" />
</jsp:include>
```

- **다른 JSP 페이지나 JSP 커스텀 태그에 파라미터를 전달**할 때 사용
- 주로 `<jsp:include>`나 `<jsp:forward>` 태그와 함께 사용됨

## \<jsp:forward>

```jsp
<jsp:forward page="nextPage.jsp"/>
```

- 현재 요청을 다른 JSP 페이지, 서블릿 또는 HTML 파일로 포워딩

## \<jsp:plugin>

```jsp
<jsp:plugin type="applet" code="AppletClass" codebase="classes/" width="300" height="300">
  <jsp:params>
    <jsp:param name="param1" value="value1"/>
  </jsp:params>
</jsp:plugin>
```

- 브라우저에 플러그인을 사용하여 Java Applet 또는 JavaBean을 삽입

## 예시

```jsp
<%@ page import="com.itwill.jsp1.model.Contact" %>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8" trimDirectiveWhitespaces="true" %>
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>JSP</title>
  </head>
  <body>
    <jsp:include page="header.jsp"></jsp:include>

    <h1>JSP Action Tag</h1>

    <jsp:useBean id="c1" class="com.itwill.jsp1.model.Contact">
      <jsp:setProperty name="c1" property="id" value="1" />
      <jsp:setProperty name="c1" property="name" value="drj9812" />
      <jsp:setProperty name="c1" property="phone" value="010-1234-5678" />
      <jsp:setProperty name="c1" property="email" value="drj9812@gmail.com" />
    </jsp:useBean>

    <p>
      id: <jsp:getProperty property="id" name="c1"/>
      <br/>
      name: <jsp:getProperty property="name" name="c1"/>
      <br/>
      phone: <jsp:getProperty property="phone" name="c1"/>
      <br/>
    </p>
  </body>
</html>
```

![01-result-ex](/assets/img/posts/language/java/jsp/action-tags/01-result-ex.jpg)
*결과*