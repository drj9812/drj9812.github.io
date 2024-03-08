---
title: "[Spring Boot]스프링 부트와 JPA 활용"
categories: [Study, Spring Boot]
tags: [Java, 자바, Spring Boot, 스프링 부트, JPA, 인프런, Inflearn, 김영한]
---

# 스프링 부트와 JPA 활용

## 프로젝트 환경설정

### 프로젝트 생성

> 강의에선 인텔리제이를 사용했지만, 나는 이클립스가 더 편하기에 이클립스를 사용했다.

![[섹션 1]프로젝트 생성(1)](/assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/%5B섹션%5F1%5D프로젝트%5F생성%281%29.png)

프로젝트 우클릭 > `New` > `Project` 클릭

![[섹션 1]프로젝트 생성(2)](/assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/%5B섹션%5F1%5D프로젝트%5F생성%282%29.png)

`Spring Boot>Spring Starter Project` 선택 > `Next >` 클릭

![[섹션 1]프로젝트 생성(3)](/assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/%5B섹션%5F1%5D프로젝트%5F생성%283%29.png)

설정 후 `Next >` 클릭

- groupId
	+ com.google처럼 일반적으로 회사의 도메인 명을 거꾸로 사용
- artifactId
	+ 진행하는 프로젝트의 이름을 사용하며, 소문자로만 작성하고 특수문자를 사용하지 않음
	+ 버전 정보를 생략한 jar 파일의 이름이 됨
- pacakge
	+ groupId와 artifactId를 합치는 것을 권장

![[섹션 1]프로젝트 생성(5)](/assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/%5B섹션%5F1%5D프로젝트%5F생성%284%29.png)

라이브러리를 검색하여 선택한 후 추가 > `Next >` 클릭

- Spring Web
	+ RESTful, Spring MVC, Apache Tomcat을 기본적으로 내장
- Thymeleaf
	+ 서버 사이드 템플릿 엔진
	+ 요즘은 JSP를 거의 안 쓰는 추세
	+ 스프링 부트에서도 JSP를 권장하지 않음
- JPA
	+ Java Persist Api
	+ Spring Data와 Hibernate를 가지고 사용함
- H2
	+ DB(DataBase)
	+ 웹 콘솔 환경을 제공
	+ 애플리케이션을 실행할 때 메모리 내장 모드로 실행 가능
		* 이 모드에서는 데이터베이스가 메모리에 생성되며, 애플리케이션이 종료될 때 데이터베이스가 소멸
		* 주로 테스트 목적이나 간단한 애플리케이션에서 사용되며, 데이터베이스 파일을 별도로 관리하지 않아도 되어 편리
- Lombok
	+ 반복적이고 번거로운 코드를 줄이고, 코드의 가독성을 향상시키기 위한 목적으로 만들어진 플러그인 형태의 라이브러리

![[섹션 1]프로젝트 생성(5)](/assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/%5B섹션%5F1%5D프로젝트%5F생성%285%29.png)

`Finish` 클릭

![[섹션 1]프로젝트 생성(6)](/assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/%5B섹션%5F1%5D프로젝트%5F생성%286%29.png)

```groovy
plugins {
	id 'java'
	id 'org.springframework.boot' version '3.2.1'
	id 'io.spring.dependency-management' version '1.1.4'
}

group = 'jpabook'
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
	implementation 'org.springframework.boot:spring-boot-starter-web'
	compileOnly 'org.projectlombok:lombok'
	runtimeOnly 'com.h2database:h2'
	annotationProcessor 'org.projectlombok:lombok'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
}

tasks.named('test') {
	useJUnitPlatform()
}
```

프로젝트가 정상적으로 생성됐다면 `build.gradle` 파일에 들어가서 프로젝트 빌등 설정이 정상적으로 됐는지 확인한다.

> Maven은 프로젝트 빌드 설정을 XML 형식으로 작성한다.
{: .prompt-tip }

> 스프링 부트 2.x 버전부터는 기본적으로 `JUnit 5`를 지원하기 때문에 `JUnit 4`를 사용하려면 `dependencies` 에 `testImplementation("org.junit.vintage:junit-vintage-engine") { exclude group: "org.hamcrest", module: "hamcrest-core" }` 코드를 추가한다.
{: .prompt-tip }

![[섹션 1]프로젝트 생성(7)](/assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/%5B섹션%5F1%5D프로젝트%5F생성%287%29.png)
![[섹션 1]프로젝트 생성(8)](/assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/%5B섹션%5F1%5D프로젝트%5F생성%288%29.png)

`JpashopApplication` 클래스를 실행하고 로그창에 나오는 주소로 들어갔을 때 위 페이지가 나타나면 성공

![[섹션 1]프로젝트 생성(9)](/assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/%5B섹션%5F1%5D프로젝트%5F생성%289%29.png)

스프링 부트는 기본적으로 JUnit을 내장하고 있는데, 기본 테스트 클래스를 실행하여 정상적으로 작동되는지 확인한다.

### 라이브러리 살펴보기

> 강의에서 사용하는 인텔리제이에서는 `gradle` 빌드 도구를 사용한 프로젝트의 의존성 정보를 확인할 수 있지만, 이클립스에서는 확인하는 법을 찾지 못해 `gradlew dependencies --configuration compileClasspath` 명령어를 사용하여 아래와 같이 확인하였다. 이때 디렉토리는 생성한 프로젝트이다.

![[섹션 1]라이브러리 살펴보기(9)](/assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/%5B섹션%5F1%5D라이브러리%5F살펴보기%281%29.png)

의존관계를 간단히 정리하자면 아래와 같다.

- spring-boot-starter-web
	+ spring-boot-starter-tomcat: 톰캣 (웹서버)
	+ spring-webmvc: 스프링 웹 MVC
- spring-boot-starter-thymeleaf: 타임리프 템플릿 엔진(View)
- spring-boot-starter-data-jpa
	+ spring-boot-starter-aop
	+ spring-boot-starter-jdbc
		* HikariCP 커넥션 풀 (부트 2.0 기본)
	+ hibernate + JPA: 하이버네이트 + JPA
	+ spring-data-jpa: 스프링 데이터 JPA
- spring-boot-starter(공통): 스프링 부트 + 스프링 코어 + 로깅
	+ spring-boot
		* spring-core
	+ spring-boot-starter-logging
		* logback, slf4j
- spring-boot-starter-test
	+ junit: 테스트 프레임워크
	+ mockito: 목 라이브러리
	+ assertj: 테스트 코드를 좀 더 편하게 작성하게 도와주는 라이브러리
	+ spring-test: 스프링 통합 테스트 지원

### View 환경 설정

#### Thymeleaf
- 순수 HTML을 기반으로 하기 때문에 Natural templates으로도 불림
- 서버를 구동하지 않을 때는 정적 HTML(View)을 생성하며, 서버를 구동했을 때는 동적 HTML을 생성하게 됨
	+ 서버 상에서 동작하지 않아도 되기 때문에 서버 동작없이 화면을 확인할 수 있어, 더미데이터를 넣고 화면 디자인을 테스트하기 용이함
- Spring에서는 공식적으로 Thymeleaf 사용을 권장하고 있음

> 클라이언트(브라우저)는 JSP 코드를 해석할 수 없기 때문에, 서버(톰캣 등)는 JSP 파일을 서블릿 코드(Java)로 변환하고, 생성된 서블릿 코드는 Java 컴파일러에 의해 컴파일되어 Java 바이트코드로 변환된다. 이 과정에서 .class 파일이 생성된다. JSP로 작성된 코드는 이 과정이 끝난 후에야 클라이언트(브라우저)에서 해당 JSP 페이지의 내용이 화면에 표시되는 것이다.
{: .prompt-tip }

#### Thymleaf 사용해보기

jpabook.jpashop.HelloController

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
hello.html

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
	+ 타임리프의 th 속성을 사용하기 위해 선언된 네임스페이스
	+ 순수 HTML 코드로만 이루어진 페이지인 경우 선언하지 않아도 됨
- `th:text`
	+ 속성값이 해당 태그의 content를 대체함

##### <a id="anchor1">실행 결과</a>

![[섹션 1]View 환경 설정(1)](/assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/%5B섹션%5F1%5DView%5F환경%5F설정%281%29.png)

보다시피 `<p>`태그의 content인 "안녕하세요. 손님"이라는 문자열은 렌더링되어 "안녕하세요. Hello!!"로 대체되었다. 참고로  페이지 소스코드에서 확인할 수 있듯이 브라우저는 타임리프의 `th:` 속성을 알지 못한다. 

![[섹션 1]View 환경 설정(2)](/assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/%5B섹션%5F1%5DView%5F환경%5F설정%282%29.png)

[서버에서 동적으로 HTML 코드를 생성하여 렌더링된 페이지의 출력 결과](#anchor1)와 다르게 hello.html 파일을 직접 열 경우 순수 HTML 코드가 출력되어 `<p>` 태그의 content인 "안녕하세요. 손님"이라는 문자열이 그대로 출력되었다.

#### spring-boot-devtools 라이브러리 추가하기

![[섹션 1]View 환경 설정(3)](/assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/%5B섹션%5F1%5DView%5F환경%5F설정%283%29.png)

프로젝트 우클릭 > `Spring` > `Add Starters` 클릭

![[섹션 1]View 환경 설정(4)](/assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/%5B섹션%5F1%5DView%5F환경%5F설정%284%29.png)

Spring Boot DevTools 검색 후 추가하기

![[섹션 1]View 환경 설정(5)](/assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/%5B섹션%5F1%5DView%5F환경%5F설정%285%29.png)

`build.gradle` 파일 확인

![[섹션 1]View 환경 설정(6)](/assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/%5B섹션%5F1%5DView%5F환경%5F설정%286%29.png)

`build.gradle` 파일이 확인됐으면 프로젝트 우클릭 > `Gradle` > `Refresh Gradle Project` 클릭

![[섹션 1]View 환경 설정(7)](/assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/%5B섹션%5F1%5DView%5F환경%5F설정%287%29.png)

`Project` > `Build Automatically` 체크

- spring-boot-devtools
	+ 서버를 재시작하지 않아도 템플릿 엔진과 같은 동적인 리소스의 코드 변경사항이 자동으로 브라우저에 반영되도록 도와주는 기능을 제공

> 정적 리소스는 서버를 재시작하지 않아도 Spring Boot에 의해 자동으로 감지되고 처리됨
{: .prompt-tip }

### H2 데이터베이스 설치

#### 다운로드 및 설치
![[섹션 1]H2 데이터베이스 설치(1)](/assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/%5B섹션%5F1%5DH2%5F데이터베이스%5F설치%281%29.png)

<https://www.h2database.com>{:target="_blank"} > 알맞는 버전을 다운로드

스프링 부트 2.x를 사용하면 1.4.200 버전을 사용하고, 스프링 부트 3.x를 사용하면 2.1.214 버전 이상을 사용해야 한다.

#### 데이터베이스 파일 생성
![[섹션 1]H2 데이터베이스 설치(2)](/assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/%5B섹션%5F1%5DH2%5F데이터베이스%5F설치%282%29.png)

Windows는 `h2.bat` 파일을 실행하고, Mac은 `h2.sh` 파일을 실행한다. `h2.bat` 또는 `h2.sh` 실행하는 것은 내장 모드로 H2 데이터베이스를 실행하는 것이다.

![[섹션 1]H2 데이터베이스 설치(3)](/assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/%5B섹션%5F1%5DH2%5F데이터베이스%5F설치%283%29.png)

그럼 cmd창과 함께 위와 같은 페이지가 뜨게 되는데, ip주소를 localhost로 바꾸고 저장한 설정에서 Generic H2(Server)를 선택한다.

> 파일을 실행했을 때, 나타나는 cmd 창을 종료하면 H2 데이터베이스 서버도 종료된다.
{: .prompt-warning }

![[섹션 1]H2 데이터베이스 설치(4)](/assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/%5B섹션%5F1%5DH2%5F데이터베이스%5F설치%284%29.png)

이후, JDBC URL을 'jdbc:h2:~/jpashop'(파일 모드)으로 바꿔준 뒤 연결을 클릭한다. **이 과정은 최소 한번은 실행되어야 한다.**
 
'jdbc:h2:~/jpashop'에서 'jdbc:h2:'는 H2 데이터베이스에 접속하기 위한 프로토콜을 의미하고, '~/jpashop'에서  '~'는 사용자의 홈 디렉토리, 'jpashop'은 데이터베이스의 이름을 의미한다. 즉, 이 설정을 사용하면 H2 데이터베이스가 사용자의 홈 디렉토리에 'jpashop.mv.db' 파일을 사용하여 데이터를 저장한다.

![[섹션 1]H2 데이터베이스 설치(5)](/assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/%5B섹션%5F1%5DH2%5F데이터베이스%5F설치%285%29.png)

<a id="anchor2"></a>

홈 디렉토리에 생성된 H2 데이터베이스 파일 확인

![[섹션 1]H2 데이터베이스 설치(6)](/assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/%5B섹션%5F1%5DH2%5F데이터베이스%5F설치%286%29.png)

연결이 되면 위 페이지로 넘어가게 되는데, 왼쪽 상단의 연결 끊기 버튼을 클릭한다.

![[섹션 1]H2 데이터베이스 설치(7)](/assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/%5B섹션%5F1%5DH2%5F데이터베이스%5F설치%287%29.png)

[파일이 생성](#anchor2)된 걸 확인했으니, 이후부터는 'jdbc:h2:tcp://localhost/~/jpashop'(네트워크 모드) 주소로 접속하여 접근한다.

### JPA와 DB 설정, 동작확인

> 스프링 부트는 기본적으로 `application.properties` 파일을 사용하여 애플리케이션 설정을 관리하지만, 이 강의에선 `yml` 파일을 사용하므로 `application.properties` 파일을 삭제하고 `application.yml` 파일을 새로 생성하여 사용한다.

application.yml

![[섹션 1]JPA와 DB 설정, 동작확인 (7)](/assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/%5B섹션%5F1%5DJPA와%5FDB%5F설정,%5F동작확인%281%29.png)

```yaml
spring:
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

> yml 파일을 사용할 때에는  :(콜론)과 공백(indentation)을 이용하여 계층 구조를 표현하는데, 이때 **공백은 띄어쓰기 2칸을 사용**한다.
{: .prompt-tip }

`spring.datasource`
- `driver-class-name`: 사용할 JDBC 클래스
- `url`: 데이터베이스에 연결하기 위한 접속 주소
- `username`: 사용자 이름
- `password`: 비밀번호

`spring.jpa`
- `hibernate.ddl-auto`: 엔티티로 등록된(`@Entity` 어노테이션이 붙은) 클래스에 DDL과 관련된 작업이 이뤄졌을 경우 매핑된 DB의 테이블에 자동 반영
	+ `create`
		* 애플리케이션을 시작할 때마다 기존 스키마를 삭제하고 새로운 스키마를 생성
		* 이미 존재하는 데이터베이스를 완전히 재생성
	+ `create-drop`
		* 애플리케이션을 시작할 때마다 데이터베이스를 새롭게 생성하고 애플리케이션이 종료될 때 해당 데이터베이스를 삭제
		* 주로 개발 환경에서 테스트를 위해 사용
	+ `update`
		* 애플리케이션을 시작할 때마다 변경된 부분만 업데이트
		* 엔티티로 등록된 클래스와 매핑되는 테이블이 없으면 새로 생성하는 것은 `create`와 동일하지만 기존 테이블이 존재한다면 위의 두 경우와 달리 테이블의 컬럼을 변경
		* 기존에 존재하는 컬럼의 속성(크기, 데이터 타입 등)은 건드리지 않고, 새로운 컬럼이 추가되는 변경사항만 반영
	+ `validate`
		* 애플리케이션을 시작할 때마다 데이터베이스 스키마를 유효성 검사
		* 변경사항은 적용되지 않고, 데이터베이스 스키마가 애플리케이션의 엔티티와 일치하는지만 확인
		* 만약 테이블이 아예 존재하지 않거나, 테이블에 엔티티의 필드에 매핑되는 컬럼이 존재하지 않으면 예외를 발생시키면서 애플리케이션을 종료
	+ `none`
		* Hibernate가 자동으로 데이터베이스 스키마를 조작하지 않도록 함
		* 개발자가 직접 스키마를 관리

> 개발 초기 단계 및 테스트 환경에서는 `create` 또는 `update`, 테스트 서버에서는 `update` 또는 `validate`, 스테이징 및 운영 서버에서는 `validate` 또는 `none` 옵션을 사용하는 것이 좋다.
{: .prompt-tip }

> 스키마가 완성되어 `validate` 또는 `none` 옵션 등을 사용하는 환경에서 `create` 옵션을 사용하게 된다면 그 즉시 DB의 데이터가 모두 날라가므로 주의해야 한다.
{: .prompt-warning }

- `properties`
	+ `format_sql`
		* 생성된 SQL을 보기 좋게 포맷팅

> `jpa.properties.hibernate.show_sql: true` 옵션은 Hibernate가 실행하는 SQL 문장이 로그로 출력되어 일반적으로 `logging.level.org.hibernate.SQL: debug` 옵션과 동일한 효과를 가지지만, `jpa.properties.hibernate.show_sql: true`의 경우 `System.out`에 출력하는 반면 `logging.level.org.hibernate.SQL: debug`의 경우 logger를 통해 출력을 하는 차이점이 존재한다. 운영 환경에서의 모든 로그 출력은 가급적 logger를 통해 남겨야 하는 것이 권장되므로, `logging.level.org.hibernate.SQL: debug`를 사용하였다.
{: .prompt-tip }

`logging.level`
- `org.hibernate.SQL`
	+ `debug`
		* Hibernate가 생성하는 SQL 쿼리에 대한 로깅 레벨을 debug로 설정

- `org.hibernate.orm.jdbc.bind`
	+ `trace`
		* Hibernate의 JDBC 바인딩에 대한 로깅 레벨을 trace로 설정
		* 이 옵션은 Hibernate가 JDBC 파라미터를 바인딩할 때의 동작을 자세히 로깅

> 스프링 부트 2.x 버전을 사용한다면 Hibernate 5의 설정인 `org.hibernate.type: trace`를, 3.x 버전을 사용한다면 Hibernate 6의 설정인 `org.hibernate.orm.jdbc.bind: trace`를 사용한다.
{: .prompt-tip }

#### JPA 동작확인

jpabook.jpashop.Member.java

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
	+ 엔터티 클래스(영속성을 가진 객체)에서 데이터베이스 테이블의 기본 키(primary key)에 해당하는 필드를 표시하는데 사용
- `@GeneratedValue`
	+ JPA(Java Persistence API)에서 엔터티의 기본 키 값을 어떻게 생성할지 지정하는데 사용
	+ strategy 속성을 명시하지 않으면, JPA는 기본적으로 AUTO 전략을 사용
		* 데이터베이스가 자동 증가(Auto Increment)를 지원한다면, IDENTITY 전략을 사용(MySQL, MariaDB, PostgreSQL 등)
		* 데이터베이스가 시퀀스(Sequence)를 지원한다면, SEQUENCE 전략을 사용(Oracle, PostgreSQl, DB2, H2 등)
		* 두 가지 다 지원하지 않는 경우, TABLE 전략을 사용(모든 DB)

jpabook.jpashop.MemberRepository.java

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
	+  Spring Framework에서 데이터 액세스 계층의 빈으로 등록되는 클래스를 표시하는 데 사용되는 어노테이션
	+ 주로 DAO(Data Access Object) 패턴을 구현하는 클래스에 지정되며, 이 어노테이션을 사용하면 해당 클래스는 자동으로 Spring의 컴포넌트 스캔에 의해 빈으로 등록되어 관련된 기능들을 수행할 수 있게 됨
	+  Spring에게 해당 클래스가 데이터 액세스 레이어의 구현체임을 알려줌
	+ `JpaRepsitory<T, ID>` 인터페이스 또는 그와 유사한 Spring Data JPA에서 제공하는 리포지토리 인터페이스( `CrudRepository<T, ID>`, `PagingAndSortingRepository<T, ID>` 등) 를 상속한다면 따로 어노테이션을 명시하지 않아도 됨
-  `@PersistenceContext`

## 참조

- [김영한, "실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발", Inflearn, Date unknown](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-%ED%99%9C%EC%9A%A9-1){: target="_blank" }