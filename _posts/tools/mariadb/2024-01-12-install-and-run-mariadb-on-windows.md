---
title: "[MariaDB]MariaDB 설치 및 실행(Windows)"
categories: [Tools, MariaDB]
tags: [설치, MariaDB, MariaDB 설치]
image:
  path: /assets/img/posts/tools/mariadb/01-mariadb-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: MariaDB
---

# MariaDB 설치 및 실행(Windows)

## 개발 환경
- OS: Winodws 11 Home

## MaraiDB 다운로드

![01-mariadb-homepage](/assets/img/posts/tools/mariadb/install-and-run-mariadb-on-windows/01-mariadb-homepage.png)
*<https://mariadb.org/>{: target="_blank" } > `Download`*

![02-select-option](/assets/img/posts/tools/mariadb/install-and-run-mariadb-on-windows/02-select-option.png)
*이후 이동한 페이지에서 원하는 옵션을 선택 > `Downlad`*

> 2024.01.12 기준 5년 동안 유지 관리되는 최신 장기 릴리스 10.11.6 버전이다.
{: .prompt-tip }

## MariaDB 설치 파일 실행

![03-install-mariadb(1)](/assets/img/posts/tools/mariadb/install-and-run-mariadb-on-windows/03-install-mariadb(1).png)
*다운로드된 파일 실행*

![04-install-mariadb(2)](/assets/img/posts/tools/mariadb/install-and-run-mariadb-on-windows/04-install-mariadb(2).png)
*`Next`*

![05-install-mariadb(3)](/assets/img/posts/tools/mariadb/install-and-run-mariadb-on-windows/05-install-mariadb(3).png)
*`I accept the terms in the License Agreement` 체크 > `Next`*

![06-install-mariadb(4)](/assets/img/posts/tools/mariadb/install-and-run-mariadb-on-windows/06-install-mariadb(4).png)
*`Next`*

<a id="anchor1"></a>

![07-install-mariadb(5)](/assets/img/posts/tools/mariadb/install-and-run-mariadb-on-windows/07-install-mariadb(5).png)
*패스워드 설정 > `Next`*

- Modify password for database user 'root'
	+ 패스워드 설정
- Enable access from remote machines for 'root' user
	+ 'root' 계정에 대해 원격 접속을 허용
- User UTF8 as default server's character set
	+ UTF8을 기본 서버의 캐릭터셋으로 설정

![08-install-mariadb(6)](/assets/img/posts/tools/mariadb/install-and-run-mariadb-on-windows/08-install-mariadb(6).png)
*설정 > `Next`*

- Install as servie
	+ 윈도우 서비스로 등록하여 부팅 시 자동으로 MariaDB 시작
	+ Service Name
		* 윈도우에 등록할 서비스 이름 입력
- Enable networking
	+ TCP 리스너 활성화 여부
	+ TCP port
		* TCP 접속에 사용될 포트 지정

![09-install-mariadb(7)](/assets/img/posts/tools/mariadb/install-and-run-mariadb-on-windows/09-install-mariadb(7).png)
*`Install`*

![10-install-mariadb(8)](/assets/img/posts/tools/mariadb/install-and-run-mariadb-on-windows/10-install-mariadb(8).png)
*`Finish`*

## HeidiSQL 실행

![11-run-heidisql](/assets/img/posts/tools/mariadb/install-and-run-mariadb-on-windows/11-run-heidisql(1).png)
*GUI 툴인 HeidiSQL 실행*

> Mac은 지원하지 않는다.
{: .prompt-tip }

![12-run-heidisql(2)](/assets/img/posts/tools/mariadb/install-and-run-mariadb-on-windows/12-run-heidisql(2).png)
*`신규` > [설치 시 입력했던 패스워드 입력](#anchor1) > `열기`*

![13-success-heidisql-execution](/assets/img/posts/tools/mariadb/install-and-run-mariadb-on-windows/13-success-heidisql-execution.png)
*성공*