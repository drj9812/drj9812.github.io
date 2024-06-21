---
title: "[CS]Web Server와 WAS"
categories: [CS]
tags: [CS, Web Server, WAS]
---

# Web Server와 WAS

## 웹 페이지의 분류

![01-static-pages-and-dynamic-pages](/assets/img/posts/cs/web/web-server-and-was/01-static-pages-and-dynamic-pages.jpg)

|    특징   |     정적 페이지      |      동적 페이지    |
|:---------:|:-------------------:|:-------------------:|
|   콘텐츠  |         고정적       |        가변적       |
| 생성 방식	| HTML 파일로 직접 작성 | 서버에서 실시간 생성 |
| 로딩 속도 |          빠름	       |   상대적으로 느림    |
|  유지보수 |     파일 수정 필요    |  코드와 DB 관리 필요 |
| 사용 사례	|    정보 제공 사이트   | 사용자 상호작용 사이 |

### 정적 페이지(Static Pages)

- Web Server가 파일 경로 이름을 받아 경로와 일치하는 file contents를 반환
- 항상 동일한 페이지를 반환
    + 이미지, HTML, CSS, JavaScript 파일과 같이 컴퓨터에 저장되어 있는 파일들
    + 사용자가 요청할 때마다 항상 같은 내용을 제공하기 때문에, 서버에 부담이 적고 빠르게 로드될 수 있음
- 블로그의 홈페이지, 회사 소개 페이지

### 동적 페이지(Dynamic Pages)

- 사용자의 입력이나 기타 매개변수에 따라 변경되는 컨텐츠를 반환
    + 웹 서버에 의해서 실행되는 프로그램을 통해서 만들어진 결과물
        * 개발자는 Servlet에 `doGet()`을 구현
- 서버에서 요청을 받은 후에 해당 요청에 맞게 즉석에서 생성
    + 생성 및 전달에 더 많은 서버 자원이 필요하고, 로딩 시간이 길어질 수 있음
- 온라인 상점의 제품 목록 페이지, 소셜 미디어의 피드

#### 처리 방법

##### 스크립트 방식

- 클라이언트가 서버에 웹 문서를 요청하면 웹 문서에 동적인 요소를 포함하는 방식
- PHP
- ASP.NET Web Forms
  + VBScript 또는 JavaScript 기반
- JSP
  + Java 기반

##### 재생성 방식

- 클라이언트가 서버에 웹 문서를 요청하면 서버가 다른 애플리케이션을 통해 웹 문서를 재생성하여 제공하는 방식
- CGI(Common Gateway Interface)
  + Perl, Python, C 등 다양한 언어로 작성된 외부 프로그램
- Node.js with Express
  + JavaScript와 Express 프레임워크를 사용하는 서버 애플리케이션
- Ruby on Rails
  + Ruby 기반의 서버 애플리케이션 프레임워크
- Servlet
  + Java 기반의 서버 애플리케이션

## Web Server와 WAS의 차이

![02-web-service-architecture(1)](/assets/img/posts/cs/web/web-server-and-was/02-web-service-architecture(1).jpg)

### Web Server

#### Web Server란?

- Web Server가 설치되어 있는 컴퓨터
    + 하드웨어적 개념
- **클라이언트(웹 브라우저 또는 웹 크롤러)로부터 HTTP 요청을 받아 정적인 컨텐츠를 제공**하는 컴퓨터 프로그램
    + 소프트웨어적 개념
    + HTTP라는 프로토콜을 기반으로 동작하므로 HTTP 서버라고도 불림
- Apache Server, Nginx, IIS(Windows 전용 Web Server) 등

#### Web Server의 기능

- 정적인 컨텐츠 제공
    + WAS를 거치지 않고 바로 자원을 제공
- 동적인 컨텐츠 제공을 위한 요청 전달
    + 클라이언트의 요청(Request)을 WAS에 보내고, WAS가 처리한 결과를 클라이언트에게 전달(Response)
- 요청 파일이 없거나 문제가 발생하면 정해진 코드 값으로 응답
- 클라이언트로부터의 요청에 대한 기본 사용자 인증(Basic Authentication) 처리

### WAS(Web Application Server)

#### WAS란?

- DB 조회나 다양한 로직 처리를 요구하는 동적인 컨텐츠를 제공하기 위해 만들어진 Application Server
- HTTP를 통해 컴퓨터나 장치에 애플리케이션을 수행해주는 미들웨어(소프트웨어 엔진)
- 웹 컨테이너(Web Container) 또는 서블릿 컨테이너(Servlet Container)라고도 불림
    + 컨테이너
        * JSP, Servlet을 실행시킬 수 있는 소프트웨어
- Tomcat, JBoss, Jeus, Web Sphere 등

#### WAS의 역할

- Web Server + Web Container
- Web Server 기능들을 구조적으로 분리하여 처리하고자하는 목적으로 제시됨
    + 분산 트랜잭션, 보안, 메시징, 쓰레드 처리 등의 기능을 처리하는 분산 환경에서 사용됨
    + DB와 같이 수행됨

#### WAS의 주요 기능

- 프로그램 실행 환경과 DB 접속 기능 제공
- 업무를 처리하는 비즈니스 로직 수행
- 엔터프라이즈 환경에서 필요한 트랜잭션 보안, 트래픽 관리, DB 커넥션 풀, 사용자 관리 기능 등

### Web Server와 WAS를 구분하는 이유

#### Web Server가 필요한 이유

클라이언트(웹 브라우저)에 이미지 파일(정적 컨텐츠)을 보내는 과정을 생각해보자. 이미지 파일과 같은 정적인 파일들은 웹 문서(HTML 문서)가 클라이언트로 보내질 때 함께 가는 것이 아니다. 클라이언트는 HTML 문서를 먼저 받고 그에 맞게 필요한 이미지 파일들을 다시 서버로 요청하면 그때서야 이미지 파일을 받아온다. Web Server를 통해 정적인 파일들을 Application Server까지 가지 않고 앞단에서 빠르게 보내줄 수 있다. 따라서 Web Server에서는 정적 컨텐츠만 처리하도록 기능을 분배하여 서버의 부담을 줄일 수 있다.

#### WAS가 필요한 이유

웹 페이지는 정적 컨텐츠와 동적 컨텐츠가 모두 존재한다. 사용자의 요청에 맞게 적절한 동적 컨텐츠를 만들어서 제공해야 한다. 이때, Web Server만을 이용한다면 사용자가 원하는 요청에 대한 결과값을 모두 미리 만들어 놓고 서비스를 해야 한다. 하지만 이렇게 수행하기에는 자원이 절대적으로 부족하다. 따라서 WAS를 통해 요청에 맞는 데이터를 DB에서 가져와서 비즈니스 로직에 맞게 그때 그때 결과를 만들어서 제공함으로써 자원을 효율적으로 사용할 수 있다.

웹 서버에서는 클라이언트로부터의 요청을 다양한 목적으로 서비스하기 위해 HTML뿐만 아니라 다양한 형식의 문서와 웹 애플리케이션을 처리한다. 그런데 여러 웹 클라이언트로부터의 요구를 웹 서버 단독으로 처리하면 서버의 처리량이 많아지고 속도 및 보안 같은 성능에 문제가 생긴다. 또한, 웹과 C/S(Client/Server) 환경을 모두 필요로 할 때는 웹 기반의 요청과 C/S 환경 기반의 요청을 각각 개별적으로 처리하도록 구축하여야 한다.

이러한 여러 가지 이유로 여러 개의 서버를 병렬로 처리하는 방법을 쓰기도 하지만, 웹 서버의 기능을 분리해서 처리하려는 목적으로 WAS를 사용한다. 클라이언트로부터 요청받은 일과 화면에 표현하는 로직(Presentation Logic)까지만 웹 서버에서 담당하고, 다양한 기능을 수행하는 로직(Business Logic)은 컨테이너가 담당하도록 WAS에서 일을 나누어 역할을 분담하는 것이다.

#### WAS가 Web Server의 기능도 모두 수행하면 되지 않을까?

##### **기능을 분리하여 서버 부하 방지**

WAS는 DB 조회나 다양한 로직을 처리하느라 바쁘기 때문에 단순한 정적 컨텐츠는 Web Server에서 빠르게 클라이언트에 제공하는 것이 좋다. WAS는 기본적으로 동적 컨텐츠를 제공하기 위해 존재하는 서버이다. 만약 정적 컨텐츠 요청까지 WAS가 처리한다면 정적 데이터 처리로 인해 부하가 커지게 되고, 동적 컨텐츠의 처리가 지연됨에 따라 수행 속도가 느려진다. 즉, 이로 인해 페이지 노출 시간이 늘어나게 될 것이다.

##### **물리적으로 분리하여 보안 강화**

- SSL에 대한 암복호화 처리에 Web Server를 사용

##### **여러 대의 WAS를 연결 가능**

- Load Balancing을 위해서 Web Server를 사용
- Fail over(장애 극복), Fail back 처리에 유리
- 특히 대용량 웹 어플리케이션의 경우(여러 개의 서버 사용) Web Server와 WAS를 분리하여 무중단 운영을 위한 장애 극복에 쉽게 대응할 수 있음
    + 앞 단의 Web Server에서 오류가 발생한 WAS를 이용하지 못하도록 한 후 WAS를 재시작함으로써 사용자는 오류를 느끼지 못하고 이용할 수 있음

##### **여러 웹 어플리케이션 서비스 가능**

- 하나의 서버에서 PHP Application과 Java Application을 함께 사용하는 경우

##### **기타**

- 접근 허용 IP 관리, 2대 이상의 서버에서의 세션 관리 등도 Web Server에서 처리하면 효율적

즉, 자원 이용의 효율성 및 장애 극복, 배포 및 유지보수의 편의성 을 위해 Web Server와 WAS를 분리한다.
Web Server를 WAS 앞에 두고 필요한 WAS들을 Web Server에 플러그인 형태로 설정하면 더욱 효율적인 분산 처리가 가능하다.

## Web Service Architecture

### 예시

![03-web-service-architecture(2)](/assets/img/posts/cs/web/web-server-and-was/03-web-service-architecture(2).jpg)
*Web Service Architecture 예시: 위 예시 말고 다른 구조가 될 수도 있다.*

1. Web Server는 웹 브라우저 클라이언트로부터 HTTP 요청을 받음
2. Web Server는 클라이언트의 요청(Request)을 WAS에 보냄
3. WAS는 관련된 Servlet을 메모리에 올림
4. WAS는 `web.xml`{: .filpath }을 참조하여 해당 Servlet에 대한 Thread를 생성(Thread Pool 이용)
5. `HttpServletRequest`와 `HttpServletResponse` 객체를 생성하여 Servlet에 전달
    1. Thread는 Servlet의 `service()` 메서드를 호출
    2. `service()` 메서드는 요청에 맞게 `doGet()` 또는 `doPost()` 메서드를 호출
    3. `protected doGet(HttpServletRequest request, HttpServletResponse response)`
6. `doGet()` 또는 `doPost()` 메서드는 인자에 맞게 생성된 적절한 동적 페이지를 Response 객체에 담아 WAS에 전달
7. WAS는 Response 객체를 HttpResponse 형태로 바꾸어 Web Server에 전달
8. 생성된 Thread를 종료하고, `HttpServletRequest`와 `HttpServletResponse` 객체를 제거

> `destroy()` 메서드는 Servlet이 메모리에서 제거될 때(서버 종료, Servlet 재배포, Servlet 언로드, Servlet 컨테이너의 재설정) 호출된다.
{: .prompt-info }

## DBMS와 MiddleWare의 개념

### DBMS(Database Management System)

- 다수의 사용자들이 DB 내의 데이터를 접근할 수 있도록 해주는 소프트웨어
- DBMS는 보통 Server 형태로 서비스를 제공
- MySQL, MariaDB, Oracle, PostgreSQL 등

#### DBMS Server에 직접 접속해서 동작하는 Client Program의 문제점

- 클라이언트에 로직이 많아지고 이에 따라 Client Program의 크기가 커짐
- 로직이 변경될 때마다 매번 배포가 되어야 함
- Client에 대부분의 로직이 포함되어 배포가 되기 때문에 보안에 취약
- 이를 해결하기 위해 아래와 같은 MiddlWare가 등장

### MiddleWare

- Client - MiddleWare Server - DB Server(DBMS)
- 동작 과정
    1. 클라이언트는 단순히 요청만 중앙에 있는 MiddleWare Server에게 전달
    2. MiddleWare Server에서 대부분의 로직이 수행된
    3. 이때, 데이터를 조작할 일이 있으면 DBMS에 부탁
    4. 로직의 결과를 Client에게 전송
    5. Client는 그 결과를 화면에 보여줌
- 비즈니스 로직을 클라이언트와 DBMS 사이의 MiddleWare Server에서 동작하도록 함으로써 클라이언트는 입력과 출력만 담당

## 참고자료

- [heejeong Kwon, "[Web] Web Server와 WAS의 차이와 웹 서비스 구조", Heee's Development Blog, 2018-10-27](https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html){: target="_blank" }
- [melonicedlatte, "웹 서버와 WAS, 컨테이너의 개념 알아보기", Easy is Perfect, 2019-06-23](https://melonicedlatte.com/web/2019/06/23/210300.html){: target="_blank" }
- 오정임, *처음 해보는 Servlet & JSP 웹 프로그래밍*(부천: 루비페이퍼, 2017), 608.