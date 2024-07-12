---
title: "[Java]자바의 종류"
categories: [Language, Java]
tags: [Java, 자바, Java SE, Java EE, Java ME]
image:
  path: /assets/img/posts/language/java/01-java-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Java
---

# 자바의 종류

## 자바의 종류

### Java SE(Standard Edition)

![01-components-of-java-se-8](/assets/img/posts/language/java/types-of-java/01-components-of-java-se-8.jpg)
*Java SE 8의 구성 요소*

- 데스크톱, 서버, 임베디드 시스템 개발을 위한 표준 Java 플랫폼
- 애플리케이션 개발에 사용됨
- **Java의 기본 라이브러리 및 개발 도구를 포함**
  + JDK(Java Development Kit)
    * Java 개발 환경으로서 Java Virtual Machine(JMV)과 컴파일러, 디버거 및 애플리케이션 개발을 위한 도구들이 포함
  + JRE(Java Runtime Environment)
    * Java 애플리케이션 개발 도구인 JDK의 일부
    * Java 애플리케이션이 실행되는 데 필요한 최소한의 요건을 제공
    * JVM과 핵심적인 클래스들 그리고 각종 지원 파일들로 구성

### Java EE(Enterprise Edition)

- Java의 기본적인 기능을 정의한 **Java SE에 웹 서버 역할을 추가**
- Java 애플리케이션을 동작하게 하는 컨테이너 등을 표준화한 스펙
  * 이러한 Java EE 스펙에 따라 제품으로 구현한 것을 WAS(Web Application Server)라 부름
- Java EE의 기술
  + Web
    * JSP
    * Servlet
  + 비즈니스 모듈
    * EJb
  + DB
    * JDBC
  + Server
    * JNDI, JTA, EJB
- Oracle과 Eclipse Foundation 간의 상표권 문제로 인해 9 버전 이후부터 Jakarta EE로 명칭이 바뀜

### Java ME(Micro Edition)

- 모바일 장치나 내장형 장치(휴대전화, 셋톱 박스, PDA)와 같은 소형장치에서 실행되는 Java 애플리케이션을 지원하기 위해 경량화된 기술들을 지원하는 Java 플랫폼
- Java ME를 사용해 개발된 애플리케이션들은 기업용 무선 애플리케이션을 사용할 때 백엔드를 지원해주는 Java EE와 쉽게 통합할 수 있음
- 소형 단말기의 제한된 디스플레이 영역을 최대한 활용할 수 있는 UI와 이벤트 핸들링 라이브러리를 풍부하게 제공
- 소형장치에서 Java 프로그램을 실행할 수 있게 자바 MV을 경량화한 KVM, CardVM 기술을 지원

## 참고자료

- [J4BEZ, *0 _4. 자바의 종류(Java SE, Jakarta EE, Java ME, Java FX)*, 작은 고양이의 캣 타워, 2019-09-13](https://j4bez.tistory.com/13){: target="_blank" }
- 오정임, *처음 해보는 Servlet & JSP 웹 프로그래밍*(부천: 루비페이퍼, 2023), 608.