---
title: "[Sourcetree]Sourcetree 설치 및 실행(Windows)"
categories: [Tool, Sourcetree]
tags: [설치, Sourcetree, Sourcetree 설치]
image:
  path: /assets/img/posts/tool/sourcetree/01-sourcetree-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Sourcetree
---

# Sourcetree 설치 및 실행(Windows)

## 개발 환경
- OS: Winodws 11 Home

## Sourcetree 다운로드

![01-sourcetree-hompage](/assets/img/posts/tool/sourcetree/install-and-run-sourcetree-on-windows/01-sourcetree-hompage.jpg)
*<https://www.sourcetreeapp.com/>{: target="_blank" } > `Download for Windows` > 체크박스 체크 > `Download`*

## Sourcetree 설치 파일 실행

![02-install-sourcetree(1)](/assets/img/posts/tool/sourcetree/install-and-run-sourcetree-on-windows/02-install-sourcetree(1).jpg)
*다운로드된 파일 실행*

![03-install-sourcetree(2)](/assets/img/posts/tool/sourcetree/install-and-run-sourcetree-on-windows/03-install-sourcetree(2).jpg)
*`건너뛰기` 또는 옵션 선택 > `다음`*

GitHub를 사용하고 있어서 Bitbucket을 사용하지 않는 경우 건너뛴다.

- `Bitbucket`
	+ Git 저장소 호스팅 서비스
	+ 개인 및 팀이 코드를 호스팅하고 버전 관리할 수 있음
- `Bitbukcet Server`
	+ Bitbucket의 기업용 버전으로, 자체 데이터 센터나 클라우드에 설치하여 사용할 수 있음

![04-install-sourcetree(3)](/assets/img/posts/tool/sourcetree/install-and-run-sourcetree-on-windows/04-install-sourcetree(3).jpg)
*옵션 선택 > `다음`*


- `Git`
	+ 이미 설치되어 있는 경우 따로 설치 안 해도 됨
- `Mercurial`
	+ 분산형 버전 관리 시스템(DVCS)
	+ Git과 유사한 기능을 제공하지만 독자적인 설계와 특징을 가지고 있음
- `고급 옵션`
	+ `기본적으로 줄 끝을 자동으로 처리하도록 설정 (권장)`
		* Git이 코드를 체크아웃할 때 줄 바꿈 문자(Line Ending)를 자동으로 변환하는 기능을 설정
		* Unix 및 Unix 계열 운영 체제에서 주로 사용되는 LF 줄 바꿈 문자를 Windows 운영 체제에서 사용되는 CRLF로 자동으로 변환하여 Windows에서도 읽기 쉽도록 함
		* Windows 환경에서 작업하는 경우에는 이 옵션을 활성화하는 것이 좋음
	+ `Configure Global Ignore`
		* Git과 Mercurial(HG)에서 전역으로 적용되는 무시 파일(Global Ignore File)을 설정하는 기능
		* 이 설정을 활성화하면 Git 및 Mercurial은 무시 파일을 사용하여 특정 파일이나 패턴을 무시하도록 지시할 수 있음
		* Visual Studio에서 생성된 파일이나 Windows에서 생성된 썸네일 데이터베이스와 같은 특정 파일을 무시하기 위한 규칙이 미리 설정된 전역 무시 파일을 사용하도록 설정

![05-install-sourcetree(4)](/assets/img/posts/tool/sourcetree/install-and-run-sourcetree-on-windows/05-install-sourcetree(4).jpg)
*사용할 ID, Email 입력 > `다음`*

![06-install-sourcetree(5)](/assets/img/posts/tool/sourcetree/install-and-run-sourcetree-on-windows/06-install-sourcetree(5).jpg)
*SSH 키를 보유하고 있다면 `예`, 아니면 `아니오`*

![07-install-sourcetree(6)](/assets/img/posts/tool/sourcetree/install-and-run-sourcetree-on-windows/07-install-sourcetree(6).jpg)
*설치 완료*