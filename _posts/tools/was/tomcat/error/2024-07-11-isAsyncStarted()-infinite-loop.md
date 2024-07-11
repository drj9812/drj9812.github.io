---
title: "[Tomcat | Error]isAsyncStarted() 무한 루프"
categories: [Tools, WAS]
tags: [was, 서버, Tomcat, 톰캣, Error, 에러]
image:
  path: /assets/img/posts/tools/was/tomcat/01-tomcat-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Apache Tomcat
---

# isAsyncStarted() 무한 루프

## 개요

![01-isAsyncStarted()-infinite-loop](/assets/img/posts/tools/was/tomcat/error/isAsyncStarted()-infinite-loop/01-isAsyncStarted()-infinite-loop.jpg)


Servlet에서 처리할 URL 패턴을 `/`에서 `/*`로 바꿨더니 위 사진과 같이 `jakarta.servlet.ServletRequestWrapper.isAsyncStarted(ServletRequestWrapper.java:368)` 메서드가 계속 호출되면서 이클립스가 멈춰버렸다.

## 원인

### Servlet 컨테이너의 과부하

Serlvet에서 처리할 URL을 `/*`로 설정하면 모든 요청을 처리하게 된다. 이로 인해, Servlet 컨테이너가 이를 처리하는 데 과부하가 걸릴 수 있다. 특히, 많은 수의 요청이 Servlet을 통해 순환하면서 서버의 성능이 급격히 저하될 수 있다.

## 해결 방법

![02-fixing-error.jpg](/assets/img/posts/tools/was/tomcat/error/isAsyncStarted()-infinite-loop/02-fixing-error.jpg)

처리할 URL 패턴을 변경하면 해결된다.

### /와 /*의 차이

`/`로 매핑된 Servlet은 Root Servlet으로, **특정한 URL 패턴이 없는 모든 요청을 처리**한다. 기본 Servlet 역할을 하며, 정적 리소스 요청 (예: HTML, CSS, JS 파일) 및 JSP 파일 요청도 포함됩니다. 그러나 **다른 명시적인 Servlet 매핑이 있는 경우에는 해당 서블릿이 우선 처리**된다.

`/*`로 매핑된 Servlet은 모든 경로의 요청을 포괄적으로 처리한다. 이 패턴은 모든 Servlet 요청을 포함하며, 특정 패턴으로 매핑된 Servlet이 있어도 `/*` 패턴이 적용된다. 일반적으로 프론트 컨트롤러 패턴(예: Spring MVC의 DispatcherServlet)에서 사용된다.