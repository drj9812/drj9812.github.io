---
title: "[Web]JSP와 Servlet을 사용해서 게시판 구현하기"
categories: [Programming, Web]
tags: [Programming, Java, 자바, JSP, Servlet, BBS]
---

# JSP와 Servlet을 사용해서 게시판 구현하기

## 기능

- 회원가입
- 로그인
- 글 작성
- 글 목록

## 개발환경

- IDE: Eclipse STS4
- DB: Oracle

## 프로젝트 환경 설정

### web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="https://jakarta.ee/xml/ns/jakartaee" xmlns:web="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee https://jakarta.ee/xml/ns/jakartaee/web-app_5_0.xsd http://xmlns.jcp.org/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" id="WebApp_ID" version="5.0">
  <display-name>jsp2</display-name>
  <filter>
    <display-name>encodingFilter</display-name>
    <filter-name>encodingFilter</filter-name>
    <filter-class>com.itwill.post.filter.EncodingFilter</filter-class>
  </filter>
  <filter-mapping>
    <filter-name>encodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>
</web-app>
```
{: file="web.xml" }

### pom.xml

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.itwill</groupId>
  <artifactId>post</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <packaging>war</packaging>
  <description>JSP MVC example</description>

  <dependencies>
    <!-- Tomcat 10.1 WAS에서 JSTL을 사용하기 위해서. -->
    <dependency>
      <groupId>jakarta.servlet.jsp.jstl</groupId>
      <artifactId>jakarta.servlet.jsp.jstl-api</artifactId>
      <version version>3.0.0</version>
    </dependency>
    <dependency>
      <groupId>org.glassfish.web</groupId>
      <artifactId>jakarta.servlet.jsp.jstl</artifactId>
      <version>3.0.1</version>
    </dependency>

    <!-- Oracle JDBC Library -->
    <dependency>
      <groupId>com.oracle.database.jdbc</groupId>
      <artifactId>ojdbc11</artifactId>
      <version>23.2.0.0</version>
    </dependency>

    <!-- HiKariCp - Connection Pool -->
    <dependency>
      <groupId>com.zaxxer</groupId>
      <artifactId>HikariCP</artifactId>
      <version>5.0.1</version>
    </dependency>

    <!-- Log4j - 로그 출력 라이브러리 -->
    <dependency>
      <groupId>org.apache.logging.log4j</groupId>
      <artifactId>log4j-core</artifactId>
      <version>2.20.0</version>
    </dependency>
    <dependency>
      <groupId>org.apache.logging.log4j</groupId>
      <artifactId>log4j-api</artifactId>
    <version>2.20.0</version>
    </dependency>
    <dependency>
      <groupId>org.apache.logging.log4j</groupId>
      <artifactId>log4j-slf4j-impl</artifactId>
      <version>2.20.0</version>
    </dependency>
    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-api</artifactId>
      <version>1.7.36</version>
    </dependency>

    <!-- log4j- slf4j-impl과 slf4j-api이 서로 호환이 되야함 그래서 slf4j-api버전의 변경해준 것-->

    <!-- JUnit Test -->
    <!-- 
    웹 서비스를 위한 라이브러리가 아닌 개발 단계의 테스트 용도로만 사용 ->웹서버 반영x(scope 지정)
    -->
    <dependency> 
      <groupId>org.junit.jupiter</groupId>
      <artifactId>junit-jupiter-engine</artifactId>
      <version>5.9.3</version>
      <scope>test</scope> <!-- 따로 추가한 코드 -->
    </dependency>

    <dependency>
      <groupId>org.junit.jupiter</groupId>
      <artifactId>junit-jupiter-api</artifactId>
      <version>5.9.3</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
  <build>
    <plugins>
      <plugin>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.8.1</version>
        <configuration>
          <release>17</release>
        </configuration>
      </plugin>
      <plugin>
        <artifactId>maven-war-plugin</artifactId> <!--war 파일을 만들기 위해 maven-war-plugin을 사용하겠다. -->
        <version>3.2.3</version>
      </plugin>
    </plugins>
  </build>
</project>
```
{: file="pom.xml" }

## Presentation Layer

### View

#### WEB-INF/main.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
pageEncoding="UTF-8" trimDirectiveWhitespaces="true"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>Insert title here</title>
  </head>
  <body>
    <header>
      <h1>메인 페이지</h1>
    </header>
    <nav>
      <ul>
        <c:if test ="${ not empty signedInUser}">
          <li>
            <span>${ signedInUser }</span>
           <c:url var="signOut" value="/user/signout"></c:url>
            <a href="${ signOut }">로그아웃</a>
          </li>
        </c:if>
        <c:if test= "${ empty signedInUser }">
          <li>
            <c:url var="signInPage" value="/user/signin"></c:url>
            <a href="${ signInPage }">로그인</a>
         </li>
        </c:if>
        <li>
          <c:url var="postList" value="/post"></c:url>
          <a href="${ postList }">포스트 목록 페이지</a>
        </li>
      </ul>
    </nav>
  </body>
</html>
```
{: file="WEB-INF/main.jsp" } 

#### WEB-INF/post/create.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
pageEncoding="UTF-8" trimDirectiveWhitespaces="true"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>Post</title>
  </head>
  <body>
    <header>
      <h1>새 포스트 작성 페이지</h1>
    </header>
    <nav>
      <ul>
        <li>
          <c:url var="mainPage" value="/"></c:url>
            <a href="${ mainPage }">메인 페이지</a>
        </li>
        <li>
          <c:url var="postList" value="/post"></c:url>
            <a href="${ postList }">포스트 목록 페이지</a>
        </li>
      </ul>
    </nav>
    <main>
      <c:url value="/post/create" var="postCreate" />
        <form action="${ postCreate }" method="post">
          <div>
            <input type="text" name="title" placeholder="제목 입력" required autofocus />
          </div>
          <div>
            <textarea row="5" cols="80" name="content" placeholder="내용입력" required></textarea>
          </div>
          <div>
            <input type="text" name="author" placeholder="아이디 입력" required />
          </div>
          <div>
            <input type="submit" value="작성 완료" />
          </div>
        </form>
    </main>
  </body>
</html>
```
{: file="WEB-INF/post/create.jsp" }

#### WEB-INF/post/delete.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
pageEncoding="UTF-8" trimDirectiveWhitespaces="true"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>Post</title>
  </head>
  <body>
    <header>
      <h1>새 포스트 작성 페이지</h1>
    </header>

    <nav>
      <ul>
        <li>
          <c:url var="mainPage" value="/"></c:url>
          <a href="${ mainPage }">메인 페이지</a>
        </li>
        <li>
          <c:url var="postList" value="/post"></c:url>
          <a href="${ postList }">포스트 목록 페이지</a>
        </li>
        <li>
          <c:url var="postModify" value="/post/modify">
          <c:param name="id" value="${ post.id }"></c:param>
          </c:url>
          <a href="${ postModify }">포스트 수정</a>
        </li>
      </ul>
    </nav>
  </body>
</html>
```
{: file="WEB-INF/post/delete.jsp" }

#### WEB-INF/post/list.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
pageEncoding="UTF-8" trimDirectiveWhitespaces="true"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>JSP</title>
  </head>
  <body>
    <h1>포스트</h1>
    <nav>
      <ul>
        <li>
          <c:url var="mainPage" value="/"></c:url>
          <a href="${ mainPage }">메인 페이지</a>
        </li>
        <li>
          <c:url var="postCreate" value="/post/create"></c:url>
          <a href="${ postCreate }">새 포스트 작성</a>
        </li>
      </ul>
    </nav>

    <main>
      <table>
        <thead>
          <tr>
            <th>글 번호</th>
            <th>제목</th>
            <th>작성자 ID</th>
            <th>수정 시간</th>
          </tr>
        </thead>
        <tbody>
          <c:forEach items="${ posts }" var="post">
          <tr>
            <td>${ post.id }</td>
            <td>
              <c:url value="/post/detail" var="postDetail">
              <c:param name="id" value="${ post.id }"></c:param>
              </c:url>
              <a href="${ postDetail }">${ post.title }</a>
            </td>
            <td>${ post.author }</td>
            <td>${ post.modifiedTime }</td>
          </tr>
          </c:forEach>
        </tbody>
      </table>
      <c:url value="/post/search" var="searchPage"></c:url>
      <form action="${ searchPage }">
        <select name="category">
          <option value="t">제목</option>
          <option value="c">내용</option>
          <option value="tc">제목 + 내용</option>
          <option value="a">작성자</option>
        </select>
        <input type="text" name="keyword" placeholder="검색어" required autofocus />
        <input type="submit" value="검색"/>
      </form>
    </main>
  </body>
</html>
```
{: file="WEB-INF/post/list.jsp" }

##### WEB-INF/post/modify.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
pageEncoding="UTF-8" trimDirectiveWhitespaces="true"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>Post</title>
  </head>
  <body>
    <header>
      <h1>새 포스트 작성 페이지</h1>
    </header>

    <nav>
      <ul>
        <li>
          <c:url var="mainPage" value="/"></c:url>
          <a href="${ mainPage }">메인 페이지</a>
        </li>
        <li>
          <c:url var="postList" value="/post"></c:url>
          <a href="${ postList }">포스트 목록 페이지</a>
        </li>
        <li>
          <c:url var="postDetail" value="/post/detail">
          <c:param name="id" value="${ post.id }"></c:param>
          </c:url>
          <a href="${ postDetail }">포스트 상세보기</a>
        </li>
      </ul>
    </nav>

    <main>
      <form id="postModifyForm">
        <div>
          <input id="id" name="id" type="number" value="${ post.id }" readonly />
        </div>
        <div>
          <input id="title" name="title" type="text" value="${ post.title }" autofocus />
        </div>
        <div>
          <textarea id="content" name="content" rows="5" cols="80" >${ post.content }</textarea>
        </div>
        <div>
          <button id="btnUpdate">수정완료</button>
          <button id="btnDelete">삭제</button> 
        </div>
      </form>
    </main>
    <script src="../js/post-modify.js"></script>
  </body>
</html>
```
{: file="WEB-INF/post/modify.jsp" }

#### WEB-INF/user/signin.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
pageEncoding="UTF-8" trimDirectiveWhitespaces="true"%>
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>Post</title>
  </head>
  <body>
    <header>
      <h1>로그인</h1>
    </header>

    <main>
      <form method="post">
        <div>
          <input type="text" name="username" placeholder="사용자 아이디 입력" required autofocus />
        </div>
        <div>
          <input type="password" name="password" placeholder="비밀번호 입력" required /> 
        </div>
        <div>
          <input type="submit" value="로그인" />
        </div>
      </form>
    </main>
  </body>
</html>
```
{: file="WEB-INF/user/signin.jsp" }

### Filter

#### EncodingFilter.java

```java
package com.itwill.post.filter;

import jakarta.servlet.Filter;
import jakarta.servlet.FilterChain;
import jakarta.servlet.FilterConfig;
import jakarta.servlet.ServletException;
import jakarta.servlet.ServletRequest;
import jakarta.servlet.ServletResponse;
import jakarta.servlet.annotation.WebFilter;
import jakarta.servlet.http.HttpFilter;
import jakarta.servlet.http.HttpServletRequest;

import java.io.IOException;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

/**
* Servlet Filter implementation class EncodingFilter
*/
@WebFilter()
public class EncodingFilter extends HttpFilter implements Filter {

    //Slf4j 로깅 기능:
    private static final Logger log = LoggerFactory.getLogger(EncodingFilter.class);

    // 생성자
    public EncodingFilter () {
        log.info("EncodingFilter 생성자 호출");
    }

    /**
    * @see Filter#init(FilterConfig)
    */
    public void init(FilterConfig fConfig) throws ServletException {
        // 필더 객체가 생성된 후 초기화(initialization)작업이 필요할 때 WAS가 호출하는 생명주기 메서드.
        log.info("init() 호출");
    }

    /**
    * @see Filter#doFilter(ServletRequest, ServletResponse, FilterChain)
    */
    void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
    throws IOException, ServletException {
        log.info("doFilter() : chain.doFilter() 호출 전");

       // 클라이언트에서 온 요청을 컨트롤러(서블릿)에게 전달하기 전에 실행할 코드들을 작성.
       // 요청(request)의 인코딩 타입을 "UTF-8"로 설정: 톰캣 버전 9.0 ?이전부터는 무조건 해줘야함 그 이상 버전이라도 해주는 게 좋음
       request.setCharacterEncoding("UTF-8");

        // 요청을 필터체인의 그 다음 단계로 전달 -> 컨트롤러 메서드가 호출됨.
        chain.doFilter(request, response);
        //n번째 필터가 요청을 n + 1 번째 필터로 넘기겠다.
        //두 번째 피

       log.info("doFilter() : chain.doFilter() 호출 후");
    }

    /**
    * @see Filter#destroy()
    */
    @Override
    public void destroy() {
        // Filter 객체를 메모리에서 제거할 때 WAS가 호출하는 메서드
        // Filter 객체가 소멸될 때 리소스 해제(close)와 같은 작업이 필요하면 해당 작업들을 수행함.
        // 웹 서버를 셧다운 되지 않는 이상 filter는 셧다운 되지 않음

        log.info("destroy() 호출"); //서버가 죽기전 까지는 호출되지 않음
    }
}
```
{: file="EncodingFilter.java" }

#### AuthenticationFilter.java

```java
import jakarta.servlet.Filter;
import jakarta.servlet.FilterChain;
import jakarta.servlet.FilterConfig;
import jakarta.servlet.ServletException;
import jakarta.servlet.ServletRequest;
import jakarta.servlet.ServletResponse;
import jakarta.servlet.annotation.WebFilter;
import jakarta.servlet.http.HttpFilter;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import jakarta.servlet.http.HttpSession;

import java.io.IOException;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

/**
* Servlet Filter implementation class AuthenticationFilter
*/
@WebFilter(filterName = "authenticationFilter",
           urlPatterns = {"/post/create", "/post/detail", "/post/modify", "/post/update", "/post/delete"})
// urlPatterns에 설정된 요청 주소들에 대해서,
// 로그인이 되어 있으면 요청을 계속해서 처리(컨트롤러에게 요청을 전달).
// 로그인이 되어 있지 않으면 로그인 페이지로 redirect.
public class AuthenticationFilter extends HttpFilter implements Filter {

    private static final Logger log = LoggerFactory.getLogger(AuthenticationFilter.class);

    public AuthenticationFilter() {
        log.info("AuthenticationFilter 생성자 호출");
    }

    /**
     * @see Filter#init(FilterConfig)
     */
    @Override
    public void init(FilterConfig fConfig) throws ServletException {
        log.info("AuthenticationFilter: doFilter() 호출");
    }

    /**
    * @see Filter#doFilter(ServletRequest, ServletResponse, FilterChain)
    */
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) 
    throws IOException, ServletException {
        log.info("AuthenticationFilter: doFilter() 호출");

        // 로그인이 되어 있는 지를 체크:
        // (1) request(요청 객체)에서 session을 찾음.
        HttpSession session = ((HttpServletRequest) request).getSession();

        // (2) 세션에 로그인 정보(signedInUser)가 있는 지를 확인.
        String username = (String) session.getAttribute("signedInUser");
        log.info("로그인 정보: {}", username);

        if (username != null) { // 로그인 정보가 세션에 저장된 경우,
            // 요청을 필터 체인 순서대로 전달 - 해당 컨트롤러로 전달. 
            chain.doFilter(request, response);

            return;
        }

        // 로그인 정보가 없으면, 로그인 페이지로 redirect
        ((HttpServletResponse) response).sendRedirect("/post/user/signin");
    }

    /**
     * @see Filter#destroy()
     */
      @Override
    public void destroy() {
        log.info("AuthenticationFilter: destroy() 호출");
    }
}
```
{: file="AuthenticationFilter.java" }

### Controller

## Business Layer

### Model

### Service

## Data Access Layer

### Datasource

#### HikariDataSourceUtil.java

```java
package com.itwill.post.datasource;

import com.zaxxer.hikari.HikariConfig;
import com.zaxxer.hikari.HikariDataSource;

public class HikariDataSourceUtil {
    // 싱글톤
    private HikariDataSource ds;
    private static HikariDataSourceUtil instance = null;

    private HikariDataSourceUtil () {
        // HikariCP 환경 설정 객체
        HikariConfig config = new HikariConfig();

        config.setDriverClassName("oracle.jdbc.OracleDriver");
        config.setJdbcUrl("jdbc:oracle:thin:@localhost:1521:xe");
        config.setUsername("scott");
        config.setPassword("tiger");

        // DataSource 객체 생성
        ds = new HikariDataSource(config);
    };

    public static HikariDataSourceUtil getInstance() {
        if (instance == null) {
            instance = new HikariDataSourceUtil();
        }

        return instance;
    }

    public HikariDataSource getDataSource() {
        return ds;
    }
}
```
{: file="HikariDataSourceUtil.java" }

### Repository