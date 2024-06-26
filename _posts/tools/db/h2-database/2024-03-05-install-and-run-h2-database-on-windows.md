---
title: "[H2 Database]H2 Database 설치 및 실행(Windows)"
categories: [Tools, DB]
tags: [설치, H2 Database, H2 Database 설치]
image:
  path: /assets/img/posts/tools/db/h2-database/01-h2-database-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: H2 Database
---

# H2 Database 설치 및 실행(Windows)

## 환경
- OS: Winodws 11 Home

## H2 Database 다운로드

![01-h2-database-hompage](/assets/img/posts/tools/db/h2-database/install-and-run-h2-database-on-windows/01-h2-database-hompage.png)
*<https://www.h2database.com>{: target="_blank" } > 알맞는 버전 다운로드*

스프링 부트 2.x를 사용하면 1.4.200 버전을 사용하고, 스프링 부트 3.x를 사용하면 2.1.214 버전 이상을 사용해야 한다.

## h2.bat 파일 실행

![02-run-h2.bat(1)](/assets/img/posts/tools/db/h2-database/install-and-run-h2-database-on-windows/02-run-h2.bat(1).png)
*`h2.bat` 파일 실행*

Windows에서는 `h2.bat`{: .filepath }을 실행하고, Mac은 `h2.sh`{: .filepath }을 실행한다. `h2.bat`{: .filepath } 또는 `h2.sh`{: .filepath } 실행하는 것은 내장 모드로 H2를 실행하겠다는 의미다.

![03-run-h2.bat(2)](/assets/img/posts/tools/db/h2-database/install-and-run-h2-database-on-windows/03-run-h2.bat(2).png)
*IP 주소를 localhost로 바꾸고 변경 > `저장한 설정`의 `Generic H2(Server)` 선택*

> `h2.bat`{: .filepath }을 실행했을 때 나타나는 cmd 창을 종료하면 H2 서버도 종료된다.
{: .prompt-warning }

## H2 접속 재원 설정

![04-config-h2-access-resource(1)](/assets/img/posts/tools/db/h2-database/install-and-run-h2-database-on-windows/04-config-h2-access-resource(1).png)
*`JDBC URL` 칸에 `jdbc:h2:~/DB이름` 입력(파일 모드로 실행) > `연결`* 

- `jdbc:h2:`
	+ H2 접속 프로토콜
- `~`
	+ 사용자의 홈 디렉토리
- `/DB 이름`
	+ 사용자가 사용할 DB 이름

<a id="anchor1"></a>

> 이 과정은 최소 한번은 실행되어야 한다.
{: .prompt-warning }

![05-check-file](/assets/img/posts/tools/db/h2-database/install-and-run-h2-database-on-windows/05-check-file.png)
*사용자의 홈 디렉토리에 생성된 `DB이름.mv.db`{: .filepath } 확인*

![06-config-h2-access-resource(2)](/assets/img/posts/tools/db/h2-database/install-and-run-h2-database-on-windows/06-config-h2-access-resource(2).png)
*왼쪽 상단의 연결 끊기 버튼 클릭*

![07-config-h2-access-resource(3)](/assets/img/posts/tools/db/h2-database/install-and-run-h2-database-on-windows/07-config-h2-access-resource(3).jpg)
*`JDBC URL` 칸에 `jdbc:h2:tcp://localhost/~/DB이름` 입력(네트워크 모드) > `연결`*

[파일이 생성](#anchor1)된 걸 확인했으니, 이후부터는 `jdbc:h2:tcp://localhost/~/DB이름`(네트워크 모드) 주소로 접속하여 접근한다.

![08-success-h2-execution](/assets/img/posts/tools/db/h2-database/install-and-run-h2-database-on-windows/08-success-h2-execution.jpg)
*성공*

## 참조

- [김영한, "실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발", Inflearn, Date unknown](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-%ED%99%9C%EC%9A%A9-1){: target="_blank" }
