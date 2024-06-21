---
title: "[Java | JSP]include 디렉티브와 include 액션 태그의 차이"
categories: [Language, Java]
tags: [Java, 자바, JSP, incldue, Action tag, 액션 태그]
image:
  path: /assets/img/posts/language/java/01-java-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Java
---

# include 디렉티브와 액션 태그 include의 차이

|    분류    |                           include 디렉티브                             |                    include 액션 태그                       |
|:----------:|:---------------------------------------------------------------------:|:----------------------------------------------------------:|
|   포함시점  |                          컴파일 시점에 포함                            |                     런타임 시점에 포함                      |
|  변경 반영  | 포함된 파일이 변경되면 포함하는 파일을 다시 컴파일해야 변경 내용이 반영됨 |               포함된 파일이 변경되면 즉시 반영               |
|    성능     |               컴파일 시점에 병합되므로 런타임 오버헤드가 없음           |    런타임 시점에 포함되므로 약간의 오버헤드가 발생할 수 있음   |
|  사용 용도  |               페이지의 일부분이 고정적이고 변경이 적을 때 사용          |  페이지의 일부분이 동적으로 변경되거나 자주 업데이트될 때 사용  |
|  파일 경로  |                    포함할 파일의 경로를 상대 경로로 지정                | 포함할 파일의 경로를 상대 경로 또는 절대 경로로 지정할 수 있음 |
| 파일 확장자 |                         JSP, JSPF, HTML, 텍스트                       |                             JSP                             |

## include 디렉티브

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
         pageEncoding="UTF-8" trimDirectiveWhitespaces="true" %>

<%-- JSP Fragment --%>
<%! String str = "include.jspf"; %>
<% out.println(str + "<br/>"); %>
```
{: file="include.jspf" }

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
         pageEncoding="UTF-8" trimDirectiveWhitespaces="true" %>
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>Insert title here</title>
  </head>
  <body>
    <%@ include file="include.jspf" %>
    <% 
    out.println(str + "<br/>"); 
    str = "main.jsp";
    out.println(str + "<br/>"); 
    %>
  </body>
</html>
```
{: file="main.jsp" }

![01-include-directive-result(1)](/assets/img/posts/language/java/jsp/the-difference-between-include-directive-and-include-action-tag/01-include-directive-result(1).jpg)
*`main.jsp`{: .filepath }*

include 디렉티브를 사용하면 JSP 페이지(`main.jsp`{: .filepath })가 컴파일될 때 포함된 파일(`include.jspf`{: .filepath })의 내용이 **통합된 하나의 Servlet 파일이 생성**되기 때문에 포함된 파일(`include.jspf`{: .filepath })의 자원을 사용할 수 있다. 위 사진을 보면 `main.jsp`{: .filepath }에는 `str` 변수가 없음에도 불구하고, `str` 변수를 출력하고 있는 것을 확인할 수 있다.

![02-include-directive-result(2)](/assets/img/posts/language/java/jsp/the-difference-between-include-directive-and-include-action-tag/02-include-directive-result(2).jpg)
*`main.jsp`{: .filepath } 새로고침*

하지만 `main.jsp`{: .filepath }을 새로고침하면 다른 결과가 나오는데, 이는 **Servlet이 한번 컴파일되어 메모리에 올라간 이후에는 Servlet 객체가 재활용**되기 때문에 클래스 수준에서 한 번만 실행되는 클래스 변수(`str`)가 Servlet 객체의 수명 동안 변경된 상태로 남아 있게 되어 처음 요청 시와 새로고침 후의 출력 결과가 다르게 나타나는 것이다.

이와 같은 이유로 개발자는 Serlvet 객체의 재활용 원리를 염두에 두고 코딩을 해야 하며, 특히 JSP와 같이 상태를 유지할 수 있는 선언문을 사용할 때는 변수의 초기화와 상태 관리에 신경 써야 한다.

> **inlcude 디렉티브를 사용하여 포함된 파일의 내용은 상호 간에 변수를 공유하거나 교류하지 않아 단순히 코드 조각이 포함되는 것과 같다.** 따라서 include 디렉티브는 정적 페이지나 변하지 않는 로직을 모듈화하여 재사용하는 데 적합하며, 페이지 간에 변수를 공유하거나 동적으로 변경할 필요가 없는 경우에 사용된다. 예를 들어, 고정된 메시지 출력, 공통 header나 footer, 고정된 메서드 호출 등을 포함하는 데 유용하다.
{: .prompt-info }

## include 액션 태그

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
         pageEncoding="UTF-8" trimDirectiveWhitespaces="true" %>

<%!
String str = "include.jsp";
%>

<% 
str = request.getParameter("str");
out.println(str + "<br/>");
%>
```
{: file="include.jsp" }

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
         pageEncoding="UTF-8" trimDirectiveWhitespaces="true" %>
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>Insert title here</title>
  </head>
  <body>
    <jsp:include page="include.jsp" flush="false">
      <jsp:param name="str" value="main.jsp"/>
    </jsp:include>	
    
    <p>main.jsp 페이지입니다.</p>
  </body>
</html>
```
{: file="main.jsp" }

![03-incldue-action-tag-result](/assets/img/posts/language/java/jsp/the-difference-between-include-directive-and-include-action-tag/03-incldue-action-tag-result.jpg)
*`main.sjp`{: .filepath }* 

include 액션 태그는 페이지가 실행될 때 포함된 파일(`include.jsp`{: .filepath })로 이동하여 로직을 수행한 뒤, 다시 원래 페이지(`main.jsp`{: .filepath })로 돌아와 나머지 로직을 수행하기 때문에 **포함된 파일(`include.jsp`{: .filepath })의 자원이 공유되지 않는다.** 즉, 포함된 파일(`include.jsp`{: .filepath })에서 선언된 변수(`str`)는 포함하는 파일(`main.jsp`{: .filepath })에서 직접 사용할 수 없다.

대신에 **`<jsp:param>` 태그를 사용하여 포함된 파일(`include.jsp`{: .filepath })에 파라미터를 전달**할 수 있는데, 이러한 특성으로 인해 반복되는 페이지의 일부분이 동적으로 변경되거나 자주 업데이트될 때 사용된다.


> include 액션 태그의 `flush` 속성을 `true`로 설정하면 포함된 파일을 실행하기 전에 출력 버퍼가 클라이언트로 전송되어 클라이언트는 빠르게 이전 콘텐츠를 볼 수 있다.
{: .prompt-info }

> `name` 속성값은 직접 명시해야 하지만, `value` 속성값은 표현식(`%= ... %>`)을 사용할 수 있다.
{: .prompt-info }

> include 액션 태그는 JSPF 파일을 포함할 수 없다.
{: .prompt-warning }

## 참고자료

- [코데방, "JSP 태그의 종류와 사용법", 절차대로 생각하고 객체로 코딩하기, 2020-02-14](https://codevang.tistory.com/197){: target="_blank" }