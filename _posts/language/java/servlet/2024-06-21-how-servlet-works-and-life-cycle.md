---
title: "[Java | Servlet]Servlet의 동작 과정과 생명 주기"
categories: [Language, Java]
tags: [Java, 자바, Servlet, Life Cycle, 생명주기]
image:
  path: /assets/img/posts/language/java/01-java-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Java
---

# Servlet의 동작 과정과 생명 주기

```java
package com.itwill.jsp1;

import java.io.IOException;

import jakarta.servlet.ServletConfig;
import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

@WebServlet(name = "lifeCycleServlet", urlPatterns = {"/lifecycle"})
public class lifeCycleServlet extends HttpServlet {

    @Override
    public void init(ServletConfig config) throws ServletException {
        System.out.println("init() 메서드 실행");
        super.init(config);
    }

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("service() 메서드 실행");
        super.service(request, response);
    }

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("doGet() 메서드 실행");
    }


    @Override
    public void destroy() {
        System.out.println("destroy() 메서드 실행");
        super.destroy();
    }
}
```
{: file="lifeCycleServlet.java" }

## 클라이언트 요청 수신

![01-client-request](/assets/img/posts/language/java/servlet/how-servlet-works-and-life-cycle/01-client-request.jpg)

- 클라이언트가 Servlet에게 HTTP 요청을 보냄
  + 요청은 HTTP 프로토콜을 통해 서버로 전송

## 웹 서버의 요청 전달

![02-mapping.jpg](/assets/img/posts/language/java/servlet/how-servlet-works-and-life-cycle/02-mapping.jpg)

- 웹 서버는 요청을 받고, 해당 요청을 처리할 Servlet 컨테이너(Servlet 엔진)로 전달
- Servlet 컨테이너는 요청된 Servlet의 URL 패턴을 기반(`web.xml`{: .filepath } 또는 `@WebServlet` 어노테이션)으로 적절한 Servlet 인스턴스를 찾음

## Servlet 인스턴스 생성 또는 재사용

- 요청에 해당하는 **Servlet 인스턴스가 메모리에 존재하지 않으면**, Servlet 컨테이너는 해당 Servlet 클래스를 메모리에 로드하고 **인스턴스를 생성**
- **이미 인스턴스가 존재할 경우, 기존 인스턴스를 재사용**

## Servlet 초기화

![03-init()](/assets/img/posts/language/java/servlet/how-servlet-works-and-life-cycle/03-init().jpg)

- Servlet 인스턴스가 생성될 때, `init()` 메서드가 호출되어 초기화 작업을 수행
- 이 단계에서는 주로 설정 로딩, 데이터베이스 연결 등 초기화 작업을 수행

## service() 메서드가 클라이언트의 요청을 처리

![04-service()](/assets/img/posts/language/java/servlet/how-servlet-works-and-life-cycle/04-service().jpg)

- **요청의 HTTP 메서드(GET, POST 등)에 따라 적절한 메서드(`doGet()`, `doPost()` 등)가 호출됨**
- 각 메서드는 요청을 처리하고, 필요에 따라 데이터를 조회하거나 변경하고, 적절한 응답을 생성

## 응답 생성

![05-create-response](/assets/img/posts/language/java/servlet/how-servlet-works-and-life-cycle/05-create-response.jpg)

- Servlet은 HTML, JSON, XML 등의 형식으로 응답을 생성
- DB 조회 결과나 다른 자원으로부터 받은 데이터를 사용하여 동적인 내용을 생성할 수 있음

> Content-Type은 `PrintWriter` 객체를 얻기 전에 설정하는 것이 좋다. 그렇지 않으면 응답의 Content-Type이 올바르게 설정되지 않을 수 있다.
{: .prompt-warning }

## 응답 전송

![06-send-response](/assets/img/posts/language/java/servlet/how-servlet-works-and-life-cycle/06-send-response.jpg)

- 생성된 응답은 `HttpServletResponse` 객체를 통해 클라이언트에게 전송
- `response.getWriter().write()` 또는 `response.getOutputStream().write()` 메서드를 사용하여 데이터를 클라이언트로 보냄

## Servlet 종료

![07-destroy()](/assets/img/posts/language/java/servlet/how-servlet-works-and-life-cycle/07-destroy().jpg)

- **Servlet이 메모리에서 제거될 때 `destroy()` 메서드를 호출**하여 자원을 정리하고 인스턴스를 제거
  + 서버 종료
  + Servlet 재배포
  + Servlet 언로드
  + Servlet 컨테이너의 재설정
- 이 단계에서는 주로 DB 연결 해제, 기타 리소스 정리 등의 종료 작업을 수행