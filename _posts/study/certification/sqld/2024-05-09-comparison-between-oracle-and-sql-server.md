---
title: "[SQLD]오라클과 SQL Server 비교"
categories: [Study, Certification]
tags: [certification, 자격증, SQLD]
image:
  path: /assets/img/posts/study/certification/sqld/01-sqld-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: SQLD
---

# 오라클과 SQL Server 비교

## JOIN

![01-join](/assets/img/posts/study/certification/sqld/comparison-between-oracle-and-sql-server/01-join.jpg)
*Oracle과 SQL Server의 `JOIN` 문법 비교*

- 오라클은 오라클 표준, ANSI 표준 모두 가능
- SQL Server는 ANSI 표주만 가능

## 함수

![02-function](/assets/img/posts/study/certification/sqld/comparison-between-oracle-and-sql-server/02-function.jpg)
*Oracle과 SQL Server의 주요 함수 비교*

- 함수에서 가장 많은 차이가 발생함
- 오라클은 `DATE` 타입밖에 없지만, SQL Server는 `DATE`, `DATETIME` 둘 다 있음
	+ `DATE`: 날짜
	+ `DATETIME`: 날짜 + 시분초

## 빈 문자열 입력 처리

![03-empty-string](/assets/img/posts/study/certification/sqld/comparison-between-oracle-and-sql-server/03-empty-string.jpg)
*Oracle과 SQL Server의 빈 문자열 입력 처리 비교*

- 빈 문자열이 입력되면 오라클은 `NULL`로 입력되고, SQL Server는 빈 문자열이 그대로 삽입됨

## 초과된 문자열 크기 처리

![04-exceeded-string-size](/assets/img/posts/study/certification/sqld/comparison-between-oracle-and-sql-server/04-exceeded-string-size.jpg)
*Oracle과 SQL Server의 초과된 문자열 크기 처리 비교*

- 오라클은 정해진 사이즈를 초과한 문자열 입력 불가
- SQL Server는 공백을 포함한 문자열 입력 시 사이즈 초과만큼의 공백을 자르고 입력
	+ `SET ANSI_PADDING` 파라미터 설정에 따라 달라질 수 있음

## AUTO COMMIT


![05-auto-commit](/assets/img/posts/study/certification/sqld/comparison-between-oracle-and-sql-server/05-auto-commit.jpg)
*Oracle과 SQL Server의 AUTO `COMMIT` 비교*

- 오라클은 DDL만 AUTO `COMMIT`
	+ DML의 경우 수동 `COMMIT`
- SQL Server는 DML, DDL 모두 AUTO `COMMIT`이 기본
	+ `TRANSACTION` 선언으로 트랜젝션별 수동 `COMMIT` 또는 `ROLLBACK` 처리 가능
- 세션별 또는 IDE별 설정 변경 가능
	+ 오라클도 AUTO `COMMIT` 설정 변경 가능

## 테이블 별칭

![06-table-alias](/assets/img/posts/study/certification/sqld/comparison-between-oracle-and-sql-server/06-table-alias.jpg)
*Oracle과 SQL Server의 테이블 별칭 비교*

- 오라클은 `AS` 없이 전달
	+ `AS` 전달 시 에러
- SQL Server는 테이블 별칭 시 `AS`를 전달 혹은 생략할 수 있지만, 인라인 뷰에는 반드시 `AS`를 전달해야 함

## Top N Query

![07-top-n-query](/assets/img/posts/study/certification/sqld/comparison-between-oracle-and-sql-server/07-top-n-query.jpg)
*Oracle과 SQL Server의 Top N Query 비교*

- 오라클은 `ROWNUM` 방식
- SQL Server는 TOP Query 방식
	+ `TOP n [WITH TIES]`
- 둘 다 `RANK()` 함수와 `FETCH` 절 사용 가능
	+ `OFFSET`도 가능

## DDL

![08-ddl](/assets/img/posts/study/certification/sqld/comparison-between-oracle-and-sql-server/08-ddl.jpg)
*Oracle과 SQL Server의 DDL 비교*

- **컬럼 수정, `DEFAULT` 값 변경, 컬럼 이름 변경의 문법에서 차이**가 발생함
	+ 컬럼 추가 및 삭제는 동일함

## 컬럼 수정 시 NULL 설정 여부

![09-nullable.jpg](/assets/img/posts/study/certification/sqld/comparison-between-oracle-and-sql-server/09-nullable.jpg)
*Oracle과 SQL Server의 컬럼 수정 시 `NULL` 설정 비교*

- 컬럼 생성 시 `DEFAULT`는 Nullabe
- 오라클은 `NOT NULL` 제약 조건이 설정된 컬럼의 타입이나 크기를 변경해도 `NOT NULL` 속성 유지
- SQL Server는 Nullable로 변경됨
	+ 필요시 다시 `NOT NULL` 선언 필요

## 참고자료

- [홍은혜, "SQLD 완벽정리 부록편", 홍쌤의 데이터랩, 2024-03-02](https://www.youtube.com/watch?v=ovGGaIGL2Ys&list=PLbflMVhwy2jPIAzArCK90mqFlTtndFigS&index=5){: target="_blank" }
- 한국데이터산업진흥원, SQL 자격검정 실전문제(서울: 한국데이터산업진흥원, 2016), 279.
- [yunamom, Study with yuna](https://yunamom.tistory.com/){: target="_blank" }