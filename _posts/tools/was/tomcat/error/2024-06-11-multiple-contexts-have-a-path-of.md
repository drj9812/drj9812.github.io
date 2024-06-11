---
title: "[Tomcat | Error]Multiple Contexts have a path of"
categories: [Tools, WAS]
tags: [was, 서버, Tomcat, 톰캣, Error, 에러]
image:
  path: /assets/img/posts/tools/was/tomcat/01-tomcat-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Apache Tomcat
---

# Multiple Contexts have a path of

## 개요

![01-error-multiple-contexts-have-a-path-of](/assets/img/posts/tools/was/tomcat/error/multiple-contexts-have-a-path-of/01-error-multiple-contexts-have-a-path-of.jpg)

서버를 실행하려고하니 에러가 발생했다.

## 원인

톰캣 서버에서 같은 경로를 가지는 여러 개의 컨텍스트(모듈 또는 프로젝트)가 설정되어 있을 때 발생하는 문제다. 나의 경우 서버를 삭제하고 다시 추가하는 과정에서 컨텍스트가 중복 추가된 것 같다.

## 해결 방법

### 모듈 삭제

![02-remove-module](/assets/img/posts/tools/was/tomcat/error/multiple-contexts-have-a-path-of/02-remove-module.jpg)
*`Server` > 문제가 발생한 서버 선택 > `Module` > 경로가 같은 모듈 전부 삭제*

![03-server-run-successfully](/assets/img/posts/tools/was/tomcat/error/multiple-contexts-have-a-path-of/03-server-run-successfully.jpg)
*서버 실행 성공*

## 참고자료

- [carrot62, "[Tomcat 에러]Could not publish server configuration for Tomcat v8.5 Server at localhost.", webservice_basics, 2020-08-13](https://carrot62.tistory.com/72){: target="_blank" }