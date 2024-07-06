---
title: "[Java | JSP]내장 객체(Implicit Objects)"
categories: [Language, Java]
tags: [Java, 자바, JSP, 내장 객체]
image:
  path: /assets/img/posts/language/java/01-java-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Java
---

# 내장 객체(Implicit Objects)

## 내장 객체란?

|           분류          |   내장 객체 |                 반환 타입                |                                    설명                                   |
|:-----------------------:|:-----------:|:---------------------------------------:|:------------------------------------------------------------------------:|
|     입출력 관련  객체    |   request   | jakarta.servlet.http.httpServletRequest |                웹 브라우저의 요청 정보를 저장하고 있는 객체                 |
|                         |   response  |  javax.servlet.http.httpServletResponse |             웹 브라우저의 요청에 대한 응답 정보를 저장하는 객체             |
|                         |      out    |       jakarta.servlet.jsp.JspWriter     |            JSP 페이지의 출력할 내용을 가지고 있는 출력 스트림 객체          |
| 외부 환경 정보 제공 객체 |    session  |     jakarta.servlet.http.HttpSession    | 하나의 웹 브라우저 내에서 정보를 유지하기 위한 세션 정보를 저장하고 있는 객체 |
|                         | application |      jakarta.servlet.ServletContext     |               웹 애플리케이션 Context의 정보를 담고 있는 객체               |
|                         | pageContext	|     jakarta.servlet.jsp.PageContext     |                 JSP 페이지에 대한 정보를 저장하고 있는 객체                 |
|     Servlet 관련 객체    |     page    |             java.lang.Object            |                    JSP 페이지를 구현한 자바 클래스 객체                    |
|                         |    config   |       jakarta.servlet.ServletConfig     |                 JSP 페이지에 대한 설정 정보를 담고 있는 객체                |
|       예외 관련 객체     |  exception  |            java.lang.Throwable          |               JSP 페이지에서 예외가 발생한 경우 사용하는 객체               |


- JSP에서 내장 객체는 개발자가 별도의 선언 없이 사용할 수 있도록 JSP가 자동으로 제공하는 객체
- 자주 사용되는 기능을 손쉽게 접근하고 활용할 수 있게 하여 JSP 페이지 작성 시 편리함을 제공
- 내장 객체는 JSP 페이지가 컴파일될 때 Servlet 코드로 변환되며, Serlvet 컨테이너가 자동으로 이 객체들을 생성하고 초기화

## request

```jsp
<%-- request 객체를 사용하여 클라이언트가 보낸 파라미터를 출력--%>
<%
String username = request.getParameter("username");
out.println("Welcome, " + username);
%>
```

- 클라이언트의 요청을 나타내는 객체로, HTTP 요청에 대한 정보를 포함

### 메서드

#### 클라이언트의 요청에 대한 메타데이터 및 연결 정보를 제공하는 메서드

| 반환 타입 |           메서드         |                                설명                         |
|:--------:|:------------------------:|:-----------------------------------------------------------:|
|  String  |      `getRemoteAddr()`   |             웹 서버에 연결한 클라이언트의 주소를 반환         |
|   long   |    `getContentLength()`  |             클라이어트가 전송한 요청정보의 길이를 반환        |
|  String  | `getCharacterEncoding()` | 클라이언트가 요청 정보를 전송할 때 사용한 Character Set을 반환 |
|  String  |     `getContentType()`   |  클라이언트가 요청 정보를 전송할 때 사용한 content 타입을 반환 |
|  String  |      `getProtocol()`     |                클라이언트가 요청한 프로토콜을 반환            |
|  String  |       `getMethod()`      |                       데이터 전송 방식을 반환                |
|  String  |     `getContextPath()`   |       JSP 페이지가 속한 웹어플리케이션의 컨텍스트패스 반환     |
|  String  |     `getServerName()`    |                 연결할 때 사용한 서버 이름을 반환             |
|    int   |     `getServerPort()`    |                      사용중인 프로토콜을 반환                |

#### 클라이언트가 요청과 함께 전송한 파라미터 데이터를 제공하는 메서드

|  반환 타입  |                메서드              |                                           설명                                          |
|:-----------:|:---------------------------------:|:---------------------------------------------------------------------------------------:|
|    String   |     `getParameter(String name)`   |                지정한 이름의 파라미터가 있을 경우 첫 번째 파라미터의 값을 구함              |
|   String[]  | `getParameterValues(String name)` | 지정한 이름의 파라미터가 있을 경우 지정한 이름을 가진 파라미터의 모든 값을 String[]으로 구함 |
|    Object   |     `getAttribute(String name)`   |                클라이언트가 요청 정보를 전송할 때 사용한 Character Set을 반환              |
| Enumeration |        `getParameterNames()`      |                                 모든 파라미터의 이름을 구함                               |
|     Map     |        `getParameterMap()`        |                              전송한 파라미터를 맵 형식으로 구함                            |
|   Cookie[]  |          `getCookies()`           |                                      쿠키 배열을 반환                                     |
| HttpSession |          `getSession()`           |                                      현재 세션을 반환                                     |

## response

```jsp
<%-- response 객체를 사용하여 클라이언트에게 메시지 보내기 --%>
<%
response.setContentType("text/html");
PrintWriter out = response.getWriter();
out.println("<h1>Hello World!</h1>");
%>
```

- 서버가 클라이언트에게 보낼 응답을 나타내는 객체

### 메서드

- `setContentType(String type)`: 응답의 콘텐츠 타입을 설정
- `getWriter()`: 응답에 출력할 수 있는 `PrintWriter` 객체를 반환
- `addCookie(Cookie cookie)`: 응답에 쿠키를 추가
- `sendRedirect(String location)`: 클라이언트를 다른 URL로 리디렉션

## session

```jsp
<%-- session 객체를 사용하여 세션에 사용자 이름을 저장 --%>
<%
session.setAttribute("username", "Alice");
String username = (String) session.getAttribute("username");
out.println("Hello, " + username);
%>
```

- 사용자의 세션을 나타내는 객체로, 각 사용자마다 고유한 세션을 가짐

### 메서드

- `getAttribute(String name)`: 세션 속성 값을 반환
- `setAttribute(String name, Object value)`: 세션 속성을 설정
- `invalidate()`: 세션을 무효화

## application

```jsp
<%-- application 객체를 사용하여 웹 애플리케이션 초기화 매개변수를 가져오기--%>
<%
String adminEmail = application.getInitParameter("adminEmail");
out.println("Admin email: " + adminEmail);
%>
```

- Servlet 컨텍스트를 나타내는 객체로, 애플리케이션 전역에서 접근 가능한 속성을 관리

### 메서드

- `getAttribute(String name)`: 애플리케이션 속성 값을 반환
- `setAttribute(String name, Object value)`: 애플리케이션 속성을 설정
- `getRealPath(String path)`: 가상 경로를 실제 경로로 반환

## out

```jsp
<%-- out 객체를 사용하여 HTML 코드를 출력 --%>
<% out.println("<h2>Hello, JSP!</h2>"); %>
```

- JSP 페이지에 출력을 보내는 객체로, `JspWriter` 클래스를 사용

### 메서드

- `print(String s)`: 문자열을 출력
- `println(String s)`: 문자열을 출력하고 개행
- `clear()`: 출력 버퍼를 지움
- `flush()`: 출력 버퍼를 flush

## config

- 서블릿의 설정 정보를 나타내는 객체

### 메서드

- `getServletName()`: 서블릿의 이름을 반환
- `getServletContext()`: 서블릿 컨텍스트를 반환

## pageContext

- 페이지 속성을 관리하는 객체로, JSP 페이지의 모든 범위(페이지, 요청, 세션, 애플리케이션)에서 속성에 접근할 수 있음

### 메서드

- `getAttribute(String name)`: 속성 값을 반환
- `setAttribute(String name, Object value)`: 속성을 설정
- `include(String path)`: 다른 자원을 포함
- `forward(String path)`: 요청을 다른 자원으로 포워드

## page

- 현재 JSP 페이지 객체를 나타냄
  + JSP 페이지 자신을 참조하는 객체
  + 자신의 스코프(변수 등)에 접근할 때 사용
- 일반적으로 잘 사용되지 않음
- **JSP 페이지 자체의 메서드를 사용할 수 있음**

## exception

- 예외 처리 페이지에서 예외 객체를 나타냄
- JSP 페이지가 예외를 처리할 때 사용
- **예외 객체의 메서드를 사용할 수 있습니다**
  + `getMessage()`, `printStackTrace()` 등

## 내장 객체의 영역(scope)

![01-scope-of-implicit-objects](/assets/img/posts/language/java/jsp/implicit-objects/01-scope-of-implicit-objects.jpg)

### Page 영역

- `PageContext` 객체에 저장되어 **해당 페이지서만 유효**
  + 하나의 페이지 내에서만 공유 가능
- 한 번의 웹 클라이언트 요청에 하나의 JSP 페이지가 호출

### Request 영역

- `HttpServletRequest` 객체에 저장되어 **하나의 HTTP 요청 동안 유효**
- 한 번의 웹 클라이언트 요청에 같은 요청을 공유하는 페이지가 대응
- 브라우저가 결과를 받으면 그 요청과 관련된 Request 내장 객체는 사라짐
- 주로 페이지 모듈화에 사용됨

### Session 영역

- `HttpSession` 객체에 저장되어 **하나의 HTTP 세션 동안 유효**
- **하나의 웹 브라우저당 1개의 Session 객체가 생성됨**
- 웹 브라우저를 닫기 전까지 페이지를 이동하더라도 유지되며 웹 서버에서 제공하는 것
  + 동일한 세션 내의 여러 요청 간에 공유할 수 있음
- 주로 로그인에서 사용

### Application 영역

- `ServletContext` 객체에 저장되어 **웹 애플리케이션 전체에서 유효**
- **하나의 웹 브라우저당 1개의 Application 객체가 생성됨**
- 웹 어플리케이션에 속한 모든 페이지, 페이지에 대한 요청, 세션 모두 하나의 application 영역에 속함
  + 모든 JSP 페이지는 하나의 application 내장 객체를 공유하고 있으며 이 application 내장 객체가 application 영역에 속함
  + 애플리케이션이 종료될 때까지 객체는 유지됨

## 참고자료

- [ImOk, "[JSP] 내장객체(Implicit Object)", ImOk.log, 2021-12-13]([URL](https://velog.io/@imok-_/JSP-%EB%82%B4%EC%9E%A5%EA%B0%9D%EC%B2%B4Implicit-Object)){: target="_blank" }