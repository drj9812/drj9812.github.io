---
title: "[Tomcat | Error]Address already in use"
categories: [Tools, WAS]
tags: [was, 서버, Tomcat, 톰캣, Error, 에러]
image:
  path: /assets/img/posts/tools/was/tomcat/01-tomcat-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Apache Tomcat
---

# Address already in use

## 개요

![01-address-already-in-use-error](/assets/img/posts/tools/was/tomcat/error/address-already-in-use/01-address-already-in-use-error.jpg)

서버를 실행하니 `java.net.BindException: Address already in use: bind` 에러가 발생했다. 

## 원인 분석

이 에러는 해당 포트를 이미 다른 프로세스가 사용 중이거나 이미 해당 포트를 사용 중인 경우 발생한다. 주로 이미 실행 중인 톰캣 서버나 다른 웹 서버가 해당 포트를 사용 중인 경우에 발생하는 문제다.

## 해결 방법

### 포트 강제 종료

```console
$ netstat -ano | findstr :8090
$ taskkill /f /pid pid_입력
```

### 서버 삭제 후 다시 추가

![02-preferences](/assets/img/posts/tools/was/tomcat/error/address-already-in-use/02-preferences.jpg)
*`Windows` > `Prefrences`*

![03-remove-tomcat-on-server](/assets/img/posts/tools/was/tomcat/error/address-already-in-use/03-remove-tomcat-on-server.jpg)
*`Server` > `Runtime Environments` > 삭제하려는 서버 선택 > `Remove` > `OK`*

![04-add-tomcat-on-server](/assets/img/posts/tools/was/tomcat/error/address-already-in-use/04-add-tomcat-on-server.jpg)
*`Add` > 삭제한 서버 다시 선택 > `Create a new local server` 체크 > `Finish`*

![05-server-run-successfully](/assets/img/posts/tools/was/tomcat/error/address-already-in-use/05-server-run-successfully.jpg)
*서버 실행 성공*