---
title: "[Java | JSP]스크립팅 요소"
categories: [Language, Java]
tags: [Java, 자바, JSP]
image:
  path: /assets/img/posts/language/java/01-java-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Java
---

# 스크립팅 요소

|       요소 종류	     |   태그 형식	|                설명	             |
|:--------------------:|:-----------:|:--------------------------------:|
| 스크립트릿(Scriptlet)	|  <% ... %>  | Java 코드를 JSP 페이지에 포함시킴	|
|   표현식(Expression)	|  <%= ... %> |     Java 표현식의 결과를 출력     |
|  선언문(Declaration)	|  <%! ... %> |        변수나 메서드를 선언       |
|  디렉티브(Directive)	|  <%@ ... %> |      JSP 페이지의 설정을 정의     |
|     주석(Comment)	    | <%-- ... %> |                주석              |

## 스크립트릿(Scriptlet)

```jsp
<%
ArrayList<Contact> data = new ArrayList<>();

his.startFor();

for (int i = 0; i < 10; i++) {
    Contact c = new Contact(i, "이름_" + i, "전화번호_" + i, "eamil_" + i);
    data.add(c);
}
%>
```
{: file="scriptlet.jsp" }

```java
public void _jspService(final jakarta.servlet.http.HttpServletRequest request,
                        final jakarta.servlet.http.HttpServletResponse response) throws java.io.IOException,
                                                                                        jakarta.servlet.ServletException {
    // ...

    try {
        ArrayList<Contact> data = new ArrayList<>();

        this.startFor();

        for (int i = 0; i < 10; i++) {
            Contact c = new Contact(i, "이름_" + i, "전화번호_" + i, "eamil_" + i);
            data.add(c);
        }
    }

    //...
}
```
{: file="scriptlet_jsp.java }

- JSP 페이지에 Java 코드를 포함시키는 데 사용
- JSP가 Servlet으로 변환될 때 `_jspService()` 메서드 내에 포함됨

## 표현식(Expression)

```jsp
<%= "데이터 리스트의 크기: " + data.size() %>
```
{: file="scriptlet.jsp" }

```java
public void _jspService(final jakarta.servlet.http.HttpServletRequest request,
                        final jakarta.servlet.http.HttpServletResponse response) throws java.io.IOException,
                                                                                        jakarta.servlet.ServletException {
    // ...

    try {
        // ...
        out.print( "데이터 리스트의 크기: " + data.size() );
    }

    //...
}
```
{: file="scriptlet_jsp.java" }

- Java 표현식의 결과를 JSP 페이지에 출력하는 데 사용
- `_jspService()` 메서드 내의 `out.print()` 문으로 변환
- 표현식은 값을 간단히 출력하는 데 유용하며, 표현식 안에 복잡한 로직을 포함시키는 것은 피하는 것이 좋음
  + 변수 값, 계산 결과, 또는 메서드 호출 결과를 출력

## 선언문(Declaration)

```jsp
<%!
public void startFor() {
    System.out.println("반복문이 시작됩니다. 이것은 HTML이 아닌 콘솔 출력");
}
%>
```
{: file="scriptlet.jsp" }

```java
public final class scriptlet_jsp extends org.apache.jasper.runtime.HttpJspBase
                                 implements org.apache.jasper.runtime.JspSourceDependent,
                                            org.apache.jasper.runtime.JspSourceImports,
                                            org.apache.jasper.runtime.JspSourceDirectives {

    public void startFor() {
        System.out.println("반복문이 시작됩니다. 이것은 HTML이 아닌 콘솔 출력");
    }
}
```

- JSP 페이지에 변수나 메서드를 선언하는 데 사용
- Servlet 클래스의 멤버 변수나 메서드로 포함됨

## 디렉티브(Directive)

| 디렉티브 종류	|                  형식                |                     설명                    |
|:------------:|:------------------------------------:|:-------------------------------------------:|
|     page	   |            <%@ page ... %>           |        JSP 페이지의 전역 속성을 설정         |
|    include	 |       <%@ include file="..." %>      |             다른 JSP 파일을 포함             |
|    taglib	   | <%@ taglib uri="..." prefix="..." %>	| 커스텀 태그 라이브러리를 사용할 수 있도록 설정 |

### page

```jsp
<%@ page import="com.itwill.jsp1.model.Contact" %>
<%@ page import="java.util.ArrayList"%>
<%@ page language="java"
        contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8"
        trimDirectiveWhitespaces="true"%>
```
{: file="scriptlet.jsp" }

```java
import com.itwill.jsp1.model.Contact;
import java.util.ArrayList;
// ...

public final class scriptlet_jsp extends org.apache.jasper.runtime.HttpJspBase
                                 implements org.apache.jasper.runtime.JspSourceDependent,
                                            org.apache.jasper.runtime.JspSourceImports,
                                            org.apache.jasper.runtime.JspSourceDirectives {

    public void _jspService(final jakarta.servlet.http.HttpServletRequest request,
                            final jakarta.servlet.http.HttpServletResponse response) throws java.io.IOException,
                                                                                            jakarta.servlet.ServletException {
        // ...

        try {
            // ...
            response.setContentType("text/html; charset=UTF-8");
            // ...
        }

        //...
    }
}
```
{: file="scriptlet_jsp.java" }

- JSP 페이지의 전역 속성을 설정
- 주요 속성
  + `language`
    * JSP 페이지에서 사용할 언어를 지정
    * Default Java
  + `extends`
    * 생성된 서블릿이 상속할 클래스를 지정
  + `import`
     * JSP 페이지에서 사용할 Java 클래스를 Import
     * 여러 클래스를 콤마로 구분하여 지정할 수 있음
  + `session`
    * 현재 페이지에서 세션을 사용할지 여부를 지정
    * Default true
  + `buffer`
    * 초기 버퍼 크기를 지정
    * Default 8kb
  + `autoFlush`
    * Buffer가 가득 찼을 때 자동으로 flush할지 여부를 지정
  + `isThreadSafe`
    * JSP 페이지가 멀티스레드 환경에서 안전한지 여부를 지정
    * Default true
  + `errorPage`
    * 오류가 발생했을 때 이동할 페이지를 지정
  + `isErrorPage`
    * 현재 페이지가 오류 페이지인지 여부를 지정
  + `contentType`
    * 클라이언트에게 전송되는 문서의 MIME 타입과 문자 인코딩을 설정
  + `pageEncoding`
    * JSP 페이지의 문자 인코딩을 지정
  + `isELIgnored`
    * JSP 페이지에서 EL(Expression Language)을 무시할지 여부를 지정

### include

### taglib

## 주석(Comment)

| 주석 종류 |	         형식          |                              설명                              |
|:---------:|:---------------------:|:--------------------------------------------------------------:|
|  JSP 주석 |     <%-- ... --%>     |         클라이언트에게는 보이지 않으며, JSP 파일에만 포함         |
| HTML 주석 |	     <!-- ... -->     |         클라이언트에게 보이며, 브라우저에서 해석되지 않음	         |
| Java 주석 | /* ... */ 또는 // ...	| 스크립트릿이나 선언문 내부에서 사용되며, 클라이언트에게 보이지 않음 |

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>JSP 주석 예제</title>
  </head>
  <body>
    <!-- 이것은 HTML 주석입니다. 클라이언트에게 보입니다 -->
    <h1>JSP 주석 예제</h1>

    <%-- 이것은 JSP 주석입니다. 클라이언트에게 보이지 않습니다 --%>
    <%
    // 이것은 Java 주석입니다. 클라이언트에게 보이지 않습니다
    int x = 10; 

    /* 이것은 
      여러 줄 Java 주석입니다 */
    int y = 20;
    %>

    <p>이것은 일반 HTML입니다. x의 값은 <%= x %> 입니다.</p>
  </body>
</html>
```
{: file="scriptlet.jsp" }

## 참고자료

- ["Jakarta Server Pages Specification, Version 4.0", Jakarta EE, 2024-01-21](https://jakarta.ee/specifications/pages/4.0/jakarta-server-pages-spec-4.0#the-page-directive){: target="_blank" }