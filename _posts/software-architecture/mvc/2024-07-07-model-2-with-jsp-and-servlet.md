---
title: "[Design Pattern | MVC]JSP와 Servlet을 사용한 Model 2"
categories: [Software Architecture, Design Patterns]
tags: [Software Architecture, Design Pattern, 디자인 패턴, MVC, Model 1]
---

# JSP와 Servlet을 사용한 Model 2

## View

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
pageEncoding="UTF-8" trimDirectiveWhitespaces="true"%>
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>JSP</title>
  </head>
  <body>
    <h1>MVC pattern</h1>
    <h2>${ now }</h2>
  </body>
</html>
```
{: file="WEB-INFO/view.jsp" }

View는 `WEB-INFO`{: .filepath } 폴더 내에 위치시킨다. 이렇게 하면 클라이언트의 모든 요청을 Controller가 담당하게 된다.

## Controller

```java
import java.io.IOException;
import java.sql.Timestamp;
import java.time.LocalDateTime;

import jakarta.servlet.ServletConfig;
import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;


@WebServlet(name = "mvcServlet", urlPatterns = {"mvc"})
public class MVCservlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    /**
    * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
    */
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        LocalDateTime now = LocalDateTime.now();
        Timestamp ts = Timestamp.valueOf(now);

        request.setAttribute("now", ts);

        request.getRequestDispatcher("/WEB-INF/view.jsp")
               .forward(request, response);
    }
}
```
{: file="controller/MVCServlet.java" }

Controller는 클라이언트의 요청에 대해 응답한다. 위 Controller는 'http://localhost:8090/jsp1/mvc' 요청에 대해 `/WEB-INF/view.jsp`{: .filepath } 페이지를 포워드 방식으로 응답하고 있다.

> `@WebServlet` 어노테이션 대신 `web.xml`{: .filepath }를 사용해서 매핑을 처리해도 된다.
{: .prompt-info }

![response-result](/assets/img/posts/software-architecture/design-patterns/mvc/model-2-with-jsp-and-servlet/response-result.jpg)
*응답 결과*

클라이언트는 View(`/WEB-INF/view.jsp`{: .filepath })를 응답받는다. Controller에서 포워드 방식으로 응답했기 때문에 요청 주소(http://localhost:8090/jsp1/mvc)는 바뀌지 않았다.