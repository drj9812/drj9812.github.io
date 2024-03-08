---
title: "[Eclipse]프로젝트 import 에러 해결하기"
categories: [Tool, Eclipse]
tags: [Eclipse, 이클립스, import]
---

# 프로젝트 import 에러 해결하기

## 개요

![01-error-importing-project-to-eclipse](/assets/img/posts/tool/eclipse/error-importing-project-to-eclipse/01-error-importing-project-to-eclipse.jpg)

import한 프로젝트에 컴파일 에러가 발생했다.

## 원인 분석

![02-go-to-problems-view(1)](/assets/img/posts/tool/eclipse/error-importing-project-to-eclipse/02-go-to-problems-view(1).jpg)
*`Windows` > `Show View` > `Other...`*

![03-go-to-problems-view(2).jpg](/assets/img/posts/tool/eclipse/error-importing-project-to-eclipse/03-go-to-problems-view(2).jpg)
*`General` > `Problems` > `Open`

![04-check-cause](/assets/img/posts/tool/eclipse/error-importing-project-to-eclipse/04-check-cause.jpg)
*`Problems` > `Errors`*

- `The project was not built since its build path is incomplete. Cannot find the class file for java.lang.Object. Fix the build path then try building this project`
- `The type java.lang.Object cannot be resolved. it is indirectly referenced from required .class files`

프로젝트를 빌드하는 데 문제가 생겨 `java.lang.Object`의 클래스 파일을 찾을 수 없다는 말이다. import한 프로젝트의 JDK 버전과 현재 사용중인 JDK 버전이 일치하지 않을 경우 발생하는 문제다.

## 해결 방법

![05-fixing-error(1)](/assets/img/posts/tool/eclipse/error-importing-project-to-eclipse/05-fixing-error(1).jpg)
*프로젝트 우클릭 > `Build Path` > `Configure Build Path...`*

![06-fixing-error(2)](/assets/img/posts/tool/eclipse/error-importing-project-to-eclipse/06-fixing-error(2).jpg)
*`Java Build Path` > `Libraries` > `Add Library...`*

![07-fixing-error(3)](/assets/img/posts/tool/eclipse/error-importing-project-to-eclipse/07-fixing-error(3).jpg)
*`JRE System Library` > `Next >`*

![08-fixing-error(4)](/assets/img/posts/tool/eclipse/error-importing-project-to-eclipse/08-fixing-error(4).jpg)
*`Workspace default JRE` > `Finish`*

![09-fixing-error(5)](/assets/img/posts/tool/eclipse/error-importing-project-to-eclipse/09-fixing-error(5).jpg)
*추가한 JRE 선택 > `Apply and Close`*

![10-fix-error](/assets/img/posts/tool/eclipse/error-importing-project-to-eclipse/10-fix-error.jpg)
*해결 완료*
