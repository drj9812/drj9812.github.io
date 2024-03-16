---
title: "[Spring Boot]스프링 부트와 JPA 활용"
categories: [Study, Spring Boot]
tags: [Java, 자바, Spring Boot, 스프링 부트, JPA, Thymeleaf, H2 Database, 인프런, Inflearn, 김영한]
---

# 스프링 부트와 JPA 활용

## 개발 환경

- IDE: Eclipse STS4
- 템플릿 엔진: Thymeleaf
- ORM 프레임워크: JPA
- DB: H2 Database

## 프로젝트 환경설정

### 프로젝트 생성

![01-create-project(1)](/assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/01-create-project(1).jpg)
*`Project Explorer` 창 내에서 우클릭 > `New` > `Project...`*

![02-create-project(2)](/assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/02-create-project(2).jpg)
*`Spring Boot/Spring Starter Project` > `Next >`*

![03-create-project(3)](/assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/03-create-project(3).jpg)
*설정 > `Next >`

- `Group`
	+ 프로젝트를 생성하는 조직의 고유한 식별자 정보
	+ com.google처럼 일반적으로 회사의 도메인 명을 거꾸로 사용
- `Artifact`
	+ 진행하는 프로젝트의 이름
	+ 같은 `Group` 내의 프로젝트를 구분하는데 사용
	+ 소문자로만 작성하고 특수문자를 사용하지 않음
	+ 버전 정보를 생략한 `jar` 파일의 이름이 됨
- `Pacakge`
	+ 소스 코드의 패키지 이름
	+ `groupId.artifactId` 형식 권장

![04-create-project(4)](/assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/04-create-project(4).jpg)
*라이브러리 검색 후 추가 > `Next >`*

- `Spring Web`
	+ RESTful, Spring MVC, Apache Tomcat을 기본적으로 내장
	+ 웹 애플리케이션을 개발할 때 필수적인 라이브러리
- `Lombok`
	+ 반복적이고 번거로운 코드를 줄이고, 코드의 가독성을 향상시키기 위한 목적으로 만들어진 플러그인 형태의 라이브러리
- `Thymeleaf`
	+ 서버 사이드 템플릿 엔진
- `Spring Data JPA`
	+ 스프링 프레임워크의 일부로서, JPA를 사용하여 데이터 액세스 계층을 구축하고 관리하는 데 도움을 주는 기술
	+ JPA를 보다 쉽게 사용하고 더 높은 수준의 추상화를 제공하여 개발자가 데이터베이스에 접근하고 상호 작용할 때 발생하는 반복적인 작업을 줄여줌
- `Spring Boot Devtools`
	+ 서버를 재시작하지 않아도 템플릿 엔진과 같은 동적인 리소스의 코드 변경사항이 자동으로 브라우저에 반영되도록 도와주는 기능을 제공
		* 정적 리소스는 서버를 재시작하지 않아도 스프링 부트에 의해 자동으로 감지되고 처리됨
- `H2 Database`
	+ 웹 콘솔 환경을 제공하는 DB
	+ 애플리케이션을 실행할 때 메모리 내장 모드로 실행 가능
		* 이 모드에서는 데이터베이스가 메모리에 생성되며, 애플리케이션이 종료될 때 데이터베이스가 소멸
		* 주로 테스트 목적이나 간단한 애플리케이션에서 사용되며, 데이터베이스 파일을 별도로 관리하지 않아도 되어 편리

> 스프링 부트 3.0 버전 이상을 사용할 때는 Java는 17 버전 이상, H2 Database는 2.1.214 버전 이상을 사용해야 하며, 오라클과 자바 라이센스 문제로 `javax` 패키지 이름을 `jakarta`로 변경해야 한다.
{: .prompt-info } 

![05-create-project(5)](/assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/05-create-project(5).jpg)
*`Finish`*

```groovy
plugins {
    id 'java'
    id 'org.springframework.boot' version '3.2.3'
    id 'io.spring.dependency-management' version '1.1.4'
}

group = 'jpashop'
version = '0.0.1-SNAPSHOT'

java {
    sourceCompatibility = '17'
}

configurations {
    compileOnly {
        extendsFrom annotationProcessor
	}
}

repositories {
    mavenCentral()
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
    implementation 'org.springframework.boot:spring-boot-starter-validation'
    implementation 'org.springframework.boot:spring-boot-starter-web'
	
    compileOnly 'org.projectlombok:lombok'
    annotationProcessor 'org.projectlombok:lombok'
	
    developmentOnly 'org.springframework.boot:spring-boot-devtools'
	
    runtimeOnly 'com.h2database:h2'
	
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}

tasks.named('test') {
    useJUnitPlatform()
}
```

`build.gradle` 파일에 들어가서 프로젝트 빌드 설정이 정상적으로 됐는지 확인한다.

> `Maven`은 프로젝트 빌드 설정을 `XML` 형식으로 작성한다.
{: .prompt-info }

> 스프링 부트 2.2 버전부터는 기본적으로 `JUnit 5`를 지원하기 때문에 `JUnit 4`를 사용하려면 `dependencies` 에 `testImplementation("org.junit.vintage:junit-vintage-engine") { exclude group: "org.hamcrest", module: "hamcrest-core" }` 코드를 추가한다.
{: .prompt-info }

![06-create-project(6)](/assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/06-create-project(6).jpg)
*애플리케이션 실행 > 로그창에 나오는 포트 번호 확인*

> `main` 메서드는 `Spring Starter Project`로 프로젝트를 생성할 때 같이 생성된다.
{: .prompt-info }

![07-create-project(7)](/assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/07-create-project(7).jpg)
*주소창에 `localhost:포트 번호` 또는 `http://127.0.0.1:포트 번호/`입력*

위와 같은 페이지가 나타나면 성공이다.

![08-create-project(8)](/assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/08-create-project(8).jpg)

스프링 부트는 기본적으로 `JUnit`을 내장하고 있는데, 기본 테스트 클래스를 실행하여 정상적으로 작동되는지 확인한다.

### 라이브러리 살펴보기

```console
$ cd 프로젝트 루트 디렉토리
$ gradlew dependencies --configuration compileClasspath
```

강의에서 사용하는 인텔리제이와 달리 이클립스에서는 `gradle` 기반의 프로젝트의 의존성 계층 구조를 보여주는 기능을 제공하지 않아 위와 같이 명령어를 사용해 cmd 창에서 직접 확인해야 한다.

![09-create-project(9)](/assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/09-create-project(9).jpg)

- lombok: Java 개발에서 반복적인 코드 작성을 줄여주는 라이브러리
- spring-boot-starter-data-jpa
	+ spring-boot-starter-aop
		* spring-boot-starter
			- spring-boot
				- spring-boot-core
				- spring-boot-context
			- spring-boot-starter-logging
				- logback-classic: 로깅을 처리하기 위한 라이브러리
				- log4j-to-slf4j
				- jul-to-slf4j
		* spring-aop
		* aspectjweaver
	+ sprint-boot-starter-jdbc
		* spring-boot-starter
		* HikariCP: 커넥션 풀(스프링 부트 2.x 버전부터는 HikariCP가 기본적으로 내장)
		* spring-jdbc
	+ **hibernate-core**
		* persistence-api
		* transaction-api
	+ **spring-data-jpa**
	+ spring-aspects
- spring-boot-starter-thymeleaf: 템플릿 엔진
	+ spring-boot-starter
	+ thymleaf-spring6
- spring-boot-starter-validation: 데이터 입력이나 유효성 검사를 처리하기 위한 라이브러리 및 구성을 제공
	+ spring-boot-starter
	+ tomcat-embed-el
	+ hibernate-validator
- spring-boot-starter-web: 웹 애플리케이션을 위한 다양한 라이브러리 및 구성을 포함
	+ spring-boot-starter
	+ spring-boot-starter-json
	+ spring-boot-starter-tomcat
	+ spring-web
	+ **spring-webmvc**
- spring-boot-starter-test(testCompile)
	+ spring-boot-starter
	+ spring-boot-test
	+ spinrg-boot-test-autoconfigure
	+ json-path
	+ jakarta.xml.bind-api
	+ json-smart
	+ assertj-core: 코드에서 예상한 값이 예상한 것과 같은지를 검증하는 데 사용되는 Java 언어를 기반으로 하는 단언(assertion) 라이브러리
	+ awaitility
	+ hamcrest
	+ junit-jupiter: 단위 테스트를 위한 Java 프레임워크(JUnit 4는 `junit-vintage-engine`)
	+ mockito-core: 단위 테스트를 작성하고 실행하기 위한 목 객체(Mock objects)를 생성하는 도구를 제공하는 라이브러리
	+ mockito-junit-jupiter
	+ jsonassert
	+ spring-core
	+ spring-test: 스프링 프레임워크에서 제공하는 테스트 지원 라이브러리
	+ xmlunit-core
- h2(Runtime)

> DB가 포함된 의존성 계층 구조를 확인하려면 `gradlew dependencies --configuration runtimeClasspath` 명령어를, test가 포함된 의존성 계층 구조를 확인하려면 `gradlew dependencies --configuration testCompileClasspath` 명령어를 입력하면 된다.
{: .prompt-info }

### View 환경설정

#### Thymeleaf

- 순수 HTML을 기반으로 하기 때문에 Natural templates으로도 불림
- 서버를 구동하지 않을 때는 정적 HTML(View)을 생성하며, 서버를 구동했을 때는 동적 HTML을 생성하게 됨
	+ 서버 상에서 동작하지 않아도 되기 때문에 서버 동작없이 화면을 확인할 수 있어, 더미데이터를 넣고 화면 디자인을 테스트하기 용이함
- Spring에서는 공식적으로 Thymeleaf 사용을 권장하고 있음

> 클라이언트(브라우저)는 JSP 코드를 해석할 수 없기 때문에, 서버(톰캣 등)는 JSP 파일을 서블릿 코드(Java)로 변환하고, 생성된 서블릿 코드는 Java 컴파일러에 의해 컴파일되어 Java 바이트코드로 변환된다. 이 과정에서 .class 파일이 생성된다. JSP로 작성된 코드는 이 과정이 끝난 후에야 클라이언트(브라우저)에서 해당 JSP 페이지의 내용이 화면에 표시되는 것이다.
{: .prompt-info }

#### Thymeleaf 예시

```java
package jpabook.jpashop;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class HelloController {
	
    @GetMapping("hello")
    public String hello(Model model) {
        model.addAttribute("data", "Hello!!");
		
            return "hello";
	};
}
```

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymleaf.org">
<head>
	<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
	<title>Hello</title>
</head>
<body>
	<p th:text="'안녕하세요. ' + ${data}">안녕하세요. 손님</p>
</body>
</html>
```
- `xmlns:th`
	+ 타임리프의 th 속성을 사용하기 위해 선언된 네임스페이스(namespace)
	+ 순수 HTML 코드로만 이루어진 페이지인 경우 선언하지 않아도 됨
- `th:text`
	+ 속성값이 해당 태그의 content를 대체함

<a id="anchor1"></a>

![10-create-project(10)](/assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/10-create-project(10).jpg)

`<p>` 태그의 content인 "안녕하세요. 손님"이라는 문자열은 렌더링되어 "안녕하세요. Hello!!"로 대체되었다. 또한, 페이지 소스코드에서 확인할 수 있듯이 브라우저는 타임리프의 `th:` 속성을 알지 못한다. 

![11-create-project(11)](/assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/11-create-project(11).jpg)

[서버에서 동적으로 HTML 코드를 생성하여 렌더링된 페이지의 출력 결과](#anchor1)와 다르게 hello.html 파일을 직접 열 경우 순수 HTML 코드가 출력되어 `<p>` 태그의 content인 "안녕하세요. 손님"이라는 문자열이 그대로 출력되었다는 것을 알 수 있다.

### H2 데이터베이스 설치

[[H2 Database]H2 Database 설치하기(Windows)](https://drj9812.github.io/posts/install-h2-database-on-windows/){: target="_blank" } 참조

### JPA와 DB 설정, 동작 확인

![12-create-project(12)](/assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/12-create-project(12).jpg)

```yaml
spring:
  application:
    name: jpashop
   
  datasource:
    driver-class-name: org.h2.Driver
    url: jdbc:h2:tcp://localhost/~/jpashop
    username: sa
    password:
  
  jpa:
    hibernate:
      ddl-auto: create
    properties:
      hibernate:
#      "[show_sql]": true
        "[format_sql]": true
      
logging:
  level:
    org:
      hibernate:
        SQL: debug
        orm:
          jdbc:
            bind: trace
```

> `yml` 파일은 콜론(:)과 들여쓰기(indentation)를 이용하여 계층 구조를 표현한다. 이때 들여쓰기는 공백 두 개를 사용하는 것이 일반적이다.
{: .prompt-info }

- `spring.application`
	+ `name`
		* 스프링 애플리케이션의 이름
		* 필수 속성은 아니지만, 명시적으로 이름을 설정함으로써 애플리케이션의 구성을 명확하게 할 수 있음
		* 여러 개의 스프링 애플리케이션을 운영하거나 모니터링할 때 유용
- `spring.datasource`
	+ `driver-class-name`:  사용하는 DB의 JDBC 드라이버 클래스의 이름
	+ `url`: 데이터베이스에 연결하기 위한 접속 주소
	+ `username`: 사용자 이름
	+ `password`: 비밀번호

- `spring.jpa`
	+ `hibernate.ddl-auto`: `Entity`로 등록된(`@Entity` 어노테이션이 붙은) 클래스에 DDL과 관련된 작업이 이뤄졌을 경우 매핑된 DB의 테이블에 자동 반영
		* `create`
			* 애플리케이션을 시작할 때마다 기존 스키마를 삭제하고 새로운 스키마를 생성
			* 이미 존재하는 데이터베이스를 완전히 재생성
		* `create-drop`
			* 애플리케이션을 시작할 때마다 데이터베이스를 새롭게 생성하고 애플리케이션이 종료될 때 해당 데이터베이스를 삭제
			* 주로 개발 환경에서 테스트를 위해 사용
		* `update`
			* 애플리케이션을 시작할 때마다 변경된 부분만 업데이트
			* 엔티티로 등록된 클래스와 매핑되는 테이블이 없으면 새로 생성하는 것은 `create`와 동일하지만 기존 테이블이 존재한다면 위의 두 경우와 달리 테이블의 컬럼을 변경
			* 기존에 존재하는 컬럼의 속성(크기, 데이터 타입 등)은 건드리지 않고, 새로운 컬럼이 추가되는 변경사항만 반영
		* `validate`
			* 애플리케이션을 시작할 때마다 데이터베이스 스키마를 유효성 검사
			* 변경사항은 적용되지 않고, 데이터베이스 스키마가 애플리케이션의 엔티티와 일치하는지만 확인
			* 만약 테이블이 아예 존재하지 않거나, 테이블에 엔티티의 필드에 매핑되는 컬럼이 존재하지 않으면 예외를 발생시키면서 애플리케이션을 종료
		* `none`
			* Hibernate가 자동으로 데이터베이스 스키마를 조작하지 않도록 함
			* 개발자가 직접 스키마를 관리
	+ `properties.hibernate."[format_sql]"`: 생성된 SQL을 보기 좋게 포맷팅

> 개발 초기 단계 및 테스트 환경에서는 `create` 또는 `update`, 테스트 서버에서는 `update` 또는 `validate`, 스테이징 및 운영 서버에서는 `validate` 또는 `none` 옵션을 사용하는 것이 좋다.
{: .prompt-tip }

> 스키마가 완성되어 `validate` 또는 `none` 옵션 등을 사용하는 환경에서 `create` 옵션을 사용하게 된다면 그 즉시 DB의 데이터가 모두 날라가므로 주의해야 한다.
{: .prompt-warning }

> `jpa.properties.hibernate.show_sql: true` 옵션은 Hibernate가 실행하는 SQL 문장이 로그로 출력되어 일반적으로 `logging.level.org.hibernate.SQL: debug` 옵션과 동일한 효과를 가지지만, `jpa.properties.hibernate.show_sql: true`의 경우 `System.out`에 출력하는 반면 `logging.level.org.hibernate.SQL: debug`의 경우 logger를 통해 출력을 하는 차이점이 존재한다. 운영 환경에서의 모든 로그 출력은 가급적 logger를 사용하는 것이 권장되므로, `logging.level.org.hibernate.SQL: debug`를 사용한다.
{: .prompt-tip }

- `logging.level`
	+ `org.hibernate`
		* `SQL`
			* `debug`: Hibernate가 생성하는 SQL 쿼리에 대한 로깅 레벨을 debug로 설정
		* `orm.jdbc.bind`
			* `trace`: Hibernate가 JDBC 파라미터를 바인딩할 때의 동작을 자세히 로깅

> 스프링 부트 2.x 버전을 사용한다면 Hibernate 5의 설정인 `logging.level.org.hibernate.type.descriptor: trace`를, 3.x 버전을 사용한다면 Hibernate 6의 설정인 `logging.level.org.hibernate.orm.jdbc.bind: trace`를 사용한다.
{: .prompt-info }

#### 동작 확인

```java
package jpabook.jpashop;

import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.Id;
import lombok.Getter;
import lombok.Setter;

@Entity
@Getter @Setter
public class Member {
	
    @Id
    @GeneratedValue
    private Long id;

    private String username;
}
```

- `@Entity`
	+ DB 테이블과 매핑되는 클래스
- `@Id`
	+ DB 테이블의 Primary Key 컬럼과 매핑
- `@GeneratedValue`
	+ JPA에서 엔터티의 기본 키 값을 어떻게 생성할지 지정하는데 사용
	+ strategy 속성을 명시하지 않으면, JPA는 기본적으로 AUTO 전략을 사용
		* 데이터베이스가 자동 증가(Auto Increment)를 지원한다면, IDENTITY 전략을 사용(MySQL, MariaDB, PostgreSQL 등)
		* 데이터베이스가 시퀀스(Sequence)를 지원한다면, SEQUENCE 전략을 사용(Oracle, PostgreSQl, DB2, H2 등)
		* 두 가지 다 지원하지 않는 경우, TABLE 전략을 사용(모든 DB)

> [[JPA]기본 키(Primary Key) 생성 전략](https://drj9812.github.io/posts/primary-key-generation-strategy/){: target="_blank" } 참조
{: .prompt-tip }

```java
package jpabook.jpashop;

import org.springframework.stereotype.Repository;

import jakarta.persistence.EntityManager;
import jakarta.persistence.PersistenceContext;

@Repository
public class MemberRepository {
	
    @PersistenceContext
    private EntityManager em;
	
    public Long save(Member member) {
        em.persist(member);
		
        return member.getId();
	}
	
    public Member find(Long id) {
        return em.find(Member.class, id);
	}
}
```

- `@Repository`
	+ 스프링 프레임워크의 컴포넌트 스캔의 대상이 되는 어노테이션
	+ 데이터 액세스 계층의 빈으로 등록되는 클래스를 표시
	+ DAO(Data Access Object) 패턴을 구현하는 클래스에 지정
	+ Spring Data JPA에서 제공하는 `Reposiotry` 인터페이스를 상속한다면 명시하지 않아도 됨
-  `@PersistenceContext`
	+ 컨테이너의 관리 대상이되는 컴포넌트에서 사용
	+ 컨테이너에 의해 생성된 `EntityManagerFactory`를 통해 `EntityManager` 생성 후 주입
		* `EntityManager`가 생성되면 영속성 컨텍스트(Persistence Context)가 생성됨

> `@Configuration` 어노테이션이 붙은 클래스 또는 `XML` 파일을 통해 `LocalContainerEntityManagerFactoryBean`을 직접 설정할 수 있다. ["[토비의 스프링 - Vol.2] 2장 - 2.4 JPA", 기억나는 노트, 2022-01-17](https://milenote.tistory.com/171#LocalEntityManagerFactoryBean){: target="_blank" } 참조
{: .prompt-info }

> `@Autowired`
{: .prompt-info }

## 요구사항 분석

## 도메인과 테이블 설계

## 아키텍처 구성

## 핵심 비즈니스 로직 개발(회원, 상품, 주문)

## 테스트

## 웹 계층 개발

## 참고 자료

- [김영한, "실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발", Inflearn, Date unknown](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-%ED%99%9C%EC%9A%A9-1){: target="_blank" }