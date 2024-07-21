---
title: "[SQLP | 3과목]SQL 고급활용 및 튜닝 - 인덱스 튜닝(2024)"
categories: [Certifications, SQLP]
tags: [Certification, 자격증, SQLP]
image:
  path: /assets/img/posts/certifications/sqld/01-kdata-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: 한국데이터산업진흥원
---

# SQL 고급활용 및 튜닝 - 인덱스 튜닝(2024)

|         주요 항목           |         세부 항목       |
|:--------------------------:|:-----------------------:|
|        SQL 수행 구조        |  데이터베이스 아키텍처   |
|                            |       SQL 처리 과정      |
|                            | 데이터베이스 I/O 메커니즘 |
|        SQL 분석 도구        |       예상 실행계획      |
|                            |        SQL 트레이스      |
|                            |       응답 시간 분석     |
|       **인덱스 튜닝**	      |      인덱스 기본 원리    |
|                            |    테이블 엑세스 최소화   |
|                            |     인덱스 스캔 효율화    |
|                            |         인덱스 설계       |
|          조인 튜닝	        |          NL 조인         |
|                            |        소트 머지 조인     |
|                            |          해시 조인        |
|                            |       스칼라 서브쿼리     |
|                            |        고급 조인 기법     |
|       SQL 옵티마이저        |     SQL 옵티마이징 원리   |
|                            |      SQL 공유 및 재사용   |
|                            |          쿼리 변환        |
|        고급 SQL 튜닝        |         소트 튜닝        |
|                            |          DML 튜닝        |
|                            |  데이터베이스 Call 최소화 |
|                            |          파티셔닝         |
|                            |  대용량 배치 프로그램 튜닝 |
|                            |       고급 SQL 활용       |
| Lock과 트랜잭션 동시성 제어	|            Lock           |
|                            |          트랜잭션         |
|                            |        동시성 제어        |

## 인덱스 기본 원리

### 인덱스 구조

#### 인덱스 기본

#### 인덱스 탐색

### 다양한 인덱스 스캔 방식

#### Index Range Scan

#### Index Full Scan

#### Index Unique Scan

#### Index Skip Scan

#### Index Fast Full Scan

#### Index Range Scan Descending

### 인덱스 종류

#### 트리 기반 인덱스

##### B*Tree 인덱스

##### 비트맵 인덱스

##### 함수기반 인덱스

##### 리버스 키 인덱스

##### 클러스터 인덱스

##### 클러스터형 인덱스/IOT(SQL Server의 클러스터형 인덱스)

### 전체 테이블 스캔과 인덱스 스캔

- 여기서부터 인덱스 기본

#### 전체 테이블 스캔

#### 인덱스 스캔

#### 전체 테이블 스캔과 인덱스 스캔 방식의 비교

#### 규칙기반 옵티마이저

## 테이블 엑세스 최소화

## 인덱스 스캔 효율화

## 인덱스 설계

## 참고자료

- 한국데이터산업진흥원, *SQL 자격검정 실전문제*(서울: 한국데이터산업진흥원, 2016), 279.
- [admin, *인덱스 기본*, 데이터온에어, 2021-02-15](https://dataonair.or.kr/db-tech-reference/d-guide/sql/?pageid=2&mod=document&uid=355){: target="_blank" }
- [admin, *인덱스 기본 원리*, 데이터온에어, 2021-02-15](https://dataonair.or.kr/db-tech-reference/d-guide/sql/?pageid=1&mod=document&uid=366){: target="_blank" }
- 조시형, *친절한 SQL 튜닝*(서울: 디비안, 2018), 560.
- 조시형, *국가공인 SQLP 자격검정 핵심노트 Ⅰ*(서울: 디비안, 2021), 272
- 조시형, *국가공인 SQLP 자격검정 핵심노트 Ⅱ*(서울: 디비안, 2021), 278