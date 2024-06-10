---
title: "[Eclipse | Error]The server does not support version 5.0 of the J2EE Web module specification"
categories: [Tools, IDE]
tags: [IDE, Eclipse, 이클립스, Tomcat, 톰캣, Error, 에러]
image:
  path: /assets/img/posts/tools/ide/eclipse/01-eclipse-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Eclipse
---

# The server does not support version 5.0 of the J2EE Web module specification

## 개요

![01-the-server-does-not-support-version-5.0-of-the-j2ee-web-module-specification(1)](/assets/img/posts/tools/ide/eclipse/error/the-server-does-not-support-version-5.0-of-the-j2ee-web-module-specification/01-the-server-does-not-support-version-5.0-of-the-j2ee-web-module-specification(1).jpg)
*The server does not support version 5.0 of the J2EE Web module specification*

톰캣 버전을 낮춰서 실행하려고 하니 `The server does not support version 5.0 of the J2EE Web module specification` 라는 경고 문구가 나타났다.

![02-the-selection-canot-be-run-on-any-server](/assets/img/posts/tools/ide/eclipse/error/the-server-does-not-support-version-5.0-of-the-j2ee-web-module-specification/02-the-selection-cannot-be-run-on-any-server.jpg)

실제로 서버를 실행하면 `The selection cannot be run on any server`라는 메시지가 나타나면서 실행되지 않는다.

## 원인 분석

`The server does not support version 5.0 of the J2EE Web module specification` 에러는 나의 **톰캣(서버) 버전에서 실행하려는 웹 애플리케이션이 J2EE(Java EE)의 Web module 5.0 사양을 지원하지 않는다는 것을 의미**한다. J2EE의 Web module 5.0은 특정 버전의 서블릿과 JSP 사양을 의미하며, 이는 톰캣 9.0에서 지원하는 사양이 아니다. 여기서 말하는 **'J2EE의 Web module'은 이클립스의 다이나믹 웹 프로젝트(Dynamic Web Project)에서 설정된 웹 모듈 사양 버전을 의미**한다. 다이나믹 웹 프로젝트는 Java EE 사양을 따르는 웹 애플리케이션을 개발하는 프로젝트 유형이다.

해결 방법 중 하나는 **프로젝트를 해당 버전의 사양에 맞게 업그레이드**하는 것이다. 또는 **톰캣 9.0에서 지원하는 더 낮은 버전의 J2EE의 Web Module 사양을 사용**하는 것도 가능하다. 

## 해결 방법

### 버전 일치

![03-project-properties](/assets/img/posts/tools/ide/eclipse/error/the-server-does-not-support-version-5.0-of-the-j2ee-web-module-specification/03-project-properties.jpg)
*프로젝트 우클릭 > `Properties`*

![04-project-facets](/assets/img/posts/tools/ide/eclipse/error/the-server-does-not-support-version-5.0-of-the-j2ee-web-module-specification/04-project-facets.jpg)
*`project facets` > Dynamic Web Module 버전을 4.0 미만으로 변경 > `Apply and Close`*

톰캣 9.0은 다이나믹 웹 모듈 4.0 버전(Java EE 8)을 완벽하게 지원하며, 하위 호환성 덕분에 4.0 이전 버전의사양을 기반으로 하는 웹 애플리케이션도 지원한다.

![05-server-at-localhost-failed-to-start](/assets/img/posts/tools/ide/eclipse/error/the-server-does-not-support-version-5.0-of-the-j2ee-web-module-specification/05-server-at-localhost-failed-to-start.jpg)
*서버 실행*

전과 다르게 서버는 서버는 실행됐지만 실행에 실패했다.

### 서버 삭제 후 다시 추가

![06-preferences](/assets/img/posts/tools/ide/eclipse/error/the-server-does-not-support-version-5.0-of-the-j2ee-web-module-specification/06-preferences.jpg)
*`Windows` > `Preferences`*

![07-remove-tomcat-on-server(1)](/assets/img/posts/tools/ide/eclipse/error/the-server-does-not-support-version-5.0-of-the-j2ee-web-module-specification/07-remove-tomcat-on-server(1).jpg)
*`Server` > `Runtime Environments` > 삭제하려는 서버 선택 > `Remove`*

![08-remove-tomcat-on-server(2)](/assets/img/posts/tools/ide/eclipse/error/the-server-does-not-support-version-5.0-of-the-j2ee-web-module-specification/08-remove-tomcat-on-server(2).jpg)
*`OK`*

![09-add-tomcat-on-server(1)](/assets/img/posts/tools/ide/eclipse/error/the-server-does-not-support-version-5.0-of-the-j2ee-web-module-specification/09-add-tomcat-on-server(1).jpg)
*`Add...` > 삭제한 서버 다시 선택 > `Next >`*

![10-add-tomcat-on-server(2)](/assets/img/posts/tools/ide/eclipse/error/the-server-does-not-support-version-5.0-of-the-j2ee-web-module-specification/10-add-tomcat-on-server(2).jpg)
*`Finish`*

![11-new-server(1)](/assets/img/posts/tools/ide/eclipse/error/the-server-does-not-support-version-5.0-of-the-j2ee-web-module-specification/11-new-server(1).jpg)
*`Server` 창 내에서 우클릭 > `New` > `Server`*

![12-new-server(2).jpg](/assets/img/posts/tools/ide/eclipse/error/the-server-does-not-support-version-5.0-of-the-j2ee-web-module-specification/12-new-server(2).jpg)
*삭제한 서버 다시 선택 > `Next >`*

![13-new-server(3)](/assets/img/posts/tools/ide/eclipse/error/the-server-does-not-support-version-5.0-of-the-j2ee-web-module-specification/13-new-server(3).jpg)
*실행할 프로젝트 추가*

![14-succeed](/assets/img/posts/tools/ide/eclipse/error/the-server-does-not-support-version-5.0-of-the-j2ee-web-module-specification/14-succeed.jpg)
*서버 실행 성공*