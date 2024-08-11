---
title: "[CS]2-Tier, 3-Tier"
categories: [CS]
tags: [CS]
---

# 2-Tier, 3-Tier

## 2-Tier 아키텍처

- 클라이언트 ↔ 서버
  + 클라이언트가 직접 DB에 접근
- 트래픽이 많지 않은 경우 자주 사용되는 구조
- 하나의 클라이언트에 서버 프로세스가 하나씩 생성되는 방식이 가장 일반적인 방법

### 구성 요소

#### 클라이언트

- 일반적으로 사용자가 **직접 상호작용하는 애플리케이션**
- 사용자 인터페이스와 애플리케이션 로직의 일부를 담당
- 사용자 입력을 수집하고, 서버에 요청을 보내며, 서버로부터 받은 데이터를 사용자에게 표시

#### 서버

- DB와 애플리케이션 로직을 처리
- 클라이언트로부터 요청을 받아 DB에서 필요한 데이터를 검색하거나 저장
- DB와의 상호작용을 관리하며, 클라이언트의 요청에 대한 응답을 생성

### 장점

- 단순성
  + 구조가 간단하여 구현과 유지 관리가 상대적으로 용이함
- 빠른 개발
  + 두 계층만 필요하므로 개발과 배포가 더 빠름

### 단점

- 확장성 문제
  + 클라이언트와 서버 간의 직접적인 통신으로 인해 서버의 부하가 커질 수 있음
    * 확장이 어려울 수 있음
- 유지 관리의 어려움
  + 모든 비즈니스 로직이 서버에 집중되어 있어 변경이나 업데이트 시 영향이 클 수 있음
- 보안 취약
  + 클라이어트가 직접 서버의 DB에 접속하여 자원을 활용하기 때문에 보안에 취약함

## 3-Tier 아키텍처

- 프레젠테이션 계층(Presentation Layer) ↔ 비즈니스 로직 계층(Business Logic Layer) ↔ 데이터 계층데이터 계층(Data Layer)
  + 클라이언트는 서버를 통해서 DB에 접근
- 서버와 WAS를 구분하는 것이 핵심

### 구성 요소

#### 프레젠테이션 계층(Presentation Layer)

- 사용자 인터페이스를 담당하며, 사용자가 애플리케이션과 상호작용하는 부분
- 클라이언트 측의 애플리케이션, 웹 페이지, 또는 모바일 애플리케이션이 여기에 해당함

#### 비즈니스 로직 계층(Business Logic Layer)

- 애플리케이션의 비즈니스 규칙과 로직을 처리
- 데이터 처리, 검증, 비즈니스 규칙 적용 등을 담당하며, 프레젠테이션 계층과 데이터 계층 간의 중재 역할을 함
- WAS

#### 데이터 계층데이터 계층(Data Layer)

- DB와의 상호작용을 관리함
- 데이터의 저장, 검색, 수정, 삭제 등의 작업을 처리하며, 비즈니스 로직 계층과 연결되어 데이터 작업을 수행

### 장점

- 확장성
  + 계층이 분리되어 있어 각 계층을 독립적으로 확장할 수 있음
    * 시스템의 성능과 용량을 유연하게 조절할 수 있음
- 유지 관리 용이
  + 비즈니스 로직, 데이터, 사용자 인터페이스가 분리되어 있어 각 계층을 개별적으로 업데이트하거나 유지 관리할 수 있음
- 재사용성
  + 각 계층이 분리되어 있어 다른 애플리케이션이나 모듈에서 해당 계층을 재사용할 수 있음

### 단점

- 복잡성
  + 구조가 복잡해지며, 설계와 구현이 더 어렵고 시간이 걸릴 수 있음
- 성능 문제
  + 계층 간의 상호작용이 많아질 수 있어 성능에 영향을 미칠 수 있음

## 참고자료

- [Sting!, *왜 3 Tier를 써야하는가?*, 스팅의 불(火)로그, 2008-12-24](https://blog.sting.pe.kr/41){: target="blank" }