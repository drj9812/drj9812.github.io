---
title: "[H2 Database]In-Memory 모드로 테스트 실행하기"
categories: [Tools, H2 Database]
tags: [설치, H2 Database, In-Memory]
image:
  path: /assets/img/posts/tools/h2-database/01-h2-database-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: H2 Database
---

# In-Memory 모드로 테스트 실행하기

## In-Mmemory 모드란?

- DB가 메모리에만 저장되고 디스크에 저장되지 않는 모드
	+ DB가 메모리에만 저장되므로 디스크 I/O의 부담이 없어서 매우 빠른 속도로 데이터를 읽고 쓸 수 있음
- 애플리케이션이 실행될 때 동시에 H2 Database가 실행됨
	+ 애플리케이션이 실행 주체

## 애플리케이션 설정

![01-h2-homepage](/assets/img/posts/tools/h2-database/run-a-test-in-in-memory-mode/01-h2-homepage.jpg)
*<https://www.h2database.com/html/main.html>{: target="_blank" } > `Cheat Sheet`*

![02-method-of-in-memory-mode](/assets/img/posts/tools/h2-database/run-a-test-in-in-memory-mode/02-method-of-in-memory-mode.jpg)
*사용하고자 하는 In-Memory 모드 방식의 URL 복사*

- `jdbc:h2:mem:test`
	+ **모든 연결이 닫히면 DB가 제거되는 방식**
		* DB는 애플리케이션의 실행 중에만 유지되고 애플리케이션이 종료되면 DB가 삭제됨
	+ 각 테스트 시에 새로운 DB가 생성되어 독립적인 테스트 환경을 유지하는 데 사용될 수 있음
- `jdbc:h2:mem:test;DB_CLOSE_DELAY=-1`
	+ **모든 연결이 닫혀도 DB가 유지되는 방식**
		* DB를 재사용하거나 여러 번 연결하여 데이터를 공유할 수 있음
	+ DB가 메모리에 계속 남아 있기 때문에 메모리 누수가 발생할 수 있음
- `jdbc:h2:mem: unnamed private`
	+ 이름이 없는 프라이빗 DB
	+ **한 번의 연결만 허용되며, 연결이 닫히면 DB가 삭제되는 방식**
	+ 단일 연결의 휘발성 DB를 생성할 때 사용될 수 있음

![03-configure-application](/assets/img/posts/tools/h2-database/run-a-test-in-in-memory-mode/03-configure-application.jpg)
*`프로젝트/src/test/resources/config`{: .filepath } 폴더의 애플리케이션 설정 파일에 복사한 URL 붙여넣기*

일반적으로 프로젝트의 소스 코드는 `src/main/resources/`{: .filepath } 디렉토리에 위치하며, 이 디렉토리의 리소스가 운영 환경에서 사용된다. 그러나 테스트를 위한 리소스는 `src/test/resources/`{: .filepath } 디렉토리에 위치한다. 이 디렉토리의 리소스는 테스트 환경에서 사용되며, 테스트 코드가 실행될 때 클래스패스에 추가된다.

따라서 테스트 환경에서는 우선적으로 `src/test/resources/`{: .filepath } 디렉토리에 있는 리소스를 참조한다. 이는 테스트 중에 프로덕션 환경의 설정을 오염시키지 않고 테스트에 필요한 리소스를 관리할 수 있도록 도와준다. 이때 **이클립스에서 애플리케이션 설정 파일은 `config`{: .filepath } 폴더 내에 위치해야 인식하므로 `config`{: .filepath } 폴더를 추가로 생성**해야 한다.

> 인텔리제이는 애플리케이션 설정 파일이 `config`{: .filepath } 폴더 내에 위치하지 않아도 인식한다.
{: .prompt-info }

![04-succeed-test](/assets/img/posts/tools/h2-database/run-a-test-in-in-memory-mode/04-succeed-test.jpg)
*테스트 성공*

## 참고자료

- [hyeri.lim, "src/test/resources 동작 문제", Inflearn, 2022-01-24](https://www.inflearn.com/course/lecture?courseSlug=%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-%ED%99%9C%EC%9A%A9-1&unitId=24291&category=questionDetail&tab=community&q=44954){: target="_blank" }
