---
title: "[Tomcat]버전 별 요구 스펙"
categories: [Tools, WAS]
tags: [WAS, Tomcat, 톰캣]
image:
  path: /assets/img/posts/tools/was/tomcat/01-tomcat-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Apache Tomcat
---

# 버전 별 요구 스펙

- 2024-06-11 기준

## 현재 지원되는 버전

![01-currently-supported-versions](/assets/img/posts/tools/was/tomcat/requirement-specifications-by-tomcat-version/01-currently-supported-versions.jpg)

- Alpha
    + 사양 및/또는 중요한 버그에 의해 요구되는 테스트되지 않은/누락된 기능이 대량으로 포함될 수 있음
    + 어떤 시간 동안도 안정적으로 실행되지 않을 것으로 예상되는 버전
- Beta
    + 일부 검증되지 않은 기능 및/또는 비교적 사소한 버그가 포함될 수 있음
    + 안정적으로 실행되지 않을 것으로 예상되는 버전
- Stable
    + 상대적으로 경미한 버그가 소수 포함될 수 있음
    + 프로덕션 용도로 사용되며 장기간 안정적으로 실행될 것으로 예상되는 버전

### 11.0.x(Alpha)

- Tomcat 10.1.x 기반으로 구축된 버전
- Jakarta EE 11 스펙 구현
    - Servlet 6.1
    - JSP 4.0
    - EL 6.0
    - WebSocket 2.2 및 Authentication 3.1

### 10.1.x(Stable)

- Tomcat 10.0.x를 기반으로 구축된 버전
- Jakarta EE 10 스펙 구현
    + Servlet 6.0
    + JSP 3.1
    + EL 5.0
    + WebSocket 2.1 및 Authentication 3.0

### 9.x(Stable)

- Tomcat 8.0.x 및 8.5.x를 기반으로 구축된 버전
- Java EE 스펙 구현
    + Servlet 4.0
    + JSP 2.3
    + EL 3.0
    + WebSocket 1.1 및 JASPIC 1.1
- HTTP/2에 대한 지원 추가
    + Java 9(Apache Tomcat 9.0.0.M18 이후)에서 실행하거나 Tomcat 기본 라이브러리가 설치되어 있어야 함
- JSSE 커넥터(NIO 및 NIO2)와 함께 TLS 지원을 위해 OpenSSL 사용에 대한 지원 추가
- TLS 가상 호스팅(SNI)에 대한 지원 추가

## 현재 지원되지 않는 버전

![02-unsupported-versions](/assets/img/posts/tools/was/tomcat/requirement-specifications-by-tomcat-version/02-unsupported-versions.jpg)

- 이러한 Apache Tomcat 버전은 수명이 종료되었으므로 사용자는 지원되는 버전으로 업그레이드하는 것을 권장

## 참고자료
- ["Which version?", Apache Tomcat, Date unknown](https://tomcat.apache.org/whichversion.html){: target="_blank" }