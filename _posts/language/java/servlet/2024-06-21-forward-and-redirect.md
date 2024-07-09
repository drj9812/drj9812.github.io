---
title: "[Java | Servlet]Forwrad와 Redirect"
categories: [Language, Java]
tags: [Java, 자바, Servlet, Forward, Redirect]
image:
  path: /assets/img/posts/language/java/01-java-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Java
---

# Forwrad와 Redirect

## Forward

![01-forward](/assets/img/posts/language/java/servlet/forward-and-redirect/01-forward.jpg)
*Forward*

- 서버 측에서 발생하는 이동
  + 클라이언트에게는 보이지 않고 서버 내부에서만 이루어짐
- 단일 요청-응답 사이클 내에서 처리
  + 클라이언트가 요청한 리소스를 다른 리소스로 전달할 때 사용
    * **서버는 새로운 요청을 생성하지 않고, 현재의 요청을 다른 Serlvet이나 JSP 등으로 전달**
- **URL이 변경되지 않음**
  + 클라이언트는 최초의 요청 URL을 유지
    * 클라이언트는 전달된 것이 아니라 최초의 요청을 하고 있는 것처럼 보임
- **같은 웹 애플리케이션 내에서만 가능**
  + 같은 Serlvet 컨텍스트(같은 웹 애플리케이션 프로젝트) 내에서만 페이지 이동이 가능

### 예시

```java
import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import java.io.IOException;


@WebServlet(name = "forwardServlet", urlPatterns = { "/ex3" })
public class FowardServlet extends HttpServlet {

    private static final long serialVersionUID = 1L;

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        request.getRequestDispatcher("example.jsp").forward(request, response);
    }
}
```

![02-ex-forward](/assets/img/posts/language/java/servlet/forward-and-redirect/02-ex-forward.jpg)
*`http://localhost:8090/jsp1/ex3` 접속 결과*

## Redirect

![03-redirect](/assets/img/posts/language/java/servlet/forward-and-redirect/03-redirect.jpg)
*Redirect*

- 클라이언트 측에서 발생하는 이동
  + Redirect는 **클라이언트에게 새로운 URL로 이동하도록 요청**
  + HTTP 상태 코드 3xx를 사용하여 구현
    * 301 Moved Permanently, 302 Found
- **새로운 요청이 발생**
  + 클라이언트는 서버로 새로운 요청을 전송
    * 이 과정에서 URL이 변경될 수 있음
- HTTP 헤더를 통한 전달
  + 서버는 클라이언트에게 새로운 URL로 이동해야 함을 알리기 위해 HTTP 헤더를 사용
- **다른 웹 애플리케이션이나 다른 도메인으로도 이동할 수 있음**
  + 서버 외부의 다른 리소스로 사용자를 리다이렉션할 수 있음

### 예시

![04-ex-redirect](/assets/img/posts/language/java/servlet/forward-and-redirect/04-ex-redirect.jpg)
*`http://localhost:8090/jsp1/ex4` 접속 결과*

```java
import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet(name = "redirectServlet", urlPatterns = { "/ex4" })
public class RedirectServlet extends HttpServlet {

    private static final long serialVersionUID = 1L;

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.sendRedirect("example.jsp");
    }
}
```

## 참고자료

- [Mangkyu, "[Web] Forward와 Redirect 차이", 망나니개발자, 2019-09-10](https://mangkyu.tistory.com/51){: target="_blank" }