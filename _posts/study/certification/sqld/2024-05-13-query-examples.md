---
title: "[SQLD]쿼리 예제"
categories: [Study, Certification]
tags: [certification, 자격증, SQLD]
image:
  path: /assets/img/posts/study/certification/sqld/01-sqld-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: SQLD
---

# 쿼리 예제

## DML

## TCL

## DDL

### CREATE

```sql
CREATE TABLE product(
    prod_id VARCHAR2(10) NOT NULL,
    prod_nm VARCHAR2(100) NOT NULL,
    reg_dt DATE NOT NULL,
    regr_no NUMBER(10),
    CONSTRAINT product_pk PRIMARY KEY(prod_id));
```
{: file="Oracle" }

- 테이블 생성 시 PK 생성
- `CONSTRAINT product_pk` 생략 가능

```sql
CREATE TABLE 부서(
    부서번호 CHAR(10),
    부서명 CHAR(10),
    PRIMARY KEY(부서번호));
```
{: file="Oracle" }

- 테이블 생성 시 PK 생성 

```sql
CREATE TABLE emp(
    emp_no VARCHAR2(10) PRIMARY KEY,
    emp_nm VARCHAR2(30) NOT NULL,
    dept_code VARCHAR2(4) DEFAULT '0000' NOT NULL,
    join_date DATE NOT NULL,
    regist_date DATE NULL);

CREATE INDEX idx_emp_01 ON emp(join_date);
```
{: file="Oracle" }

- 테이블 생성 시 PK 생성
- 인덱스 생성

```sql
CREATE TABLE emp(
    emp_no VARCHAR2(10) NOT NULL,
    emp_nm VARCHAR2(30) NOT NULL,
    dept_code VARCHAR2(4) DEFAULT '0000' NOT NULL,
    join_date DATE NOT NULL,
    regist_date DATE);

ALTER TABLE emp ADD CONSTRAINT emp_pk PRIMARY KEY(emp_no);
CREATE INDEX idx_emp_01 ON emp(join_date);
```
{: file="Oracle" }

- 테이블 생성과 PK 생성 분리
- 인덱스 생성

### ALTER

```sql
ALTER TABLE 기관분류 ALTER COLUMN 분류명 VARCHAR(30) NOT NULL;
ALTER TABLE 기관분류 ALTER COLUMN 등록일자 DATE NOT NULL;
```
{: file="SQL Server" }

- 여러 컬럼 변경

```sql
ALTER TABLE emp
  DROP COLUMN comm;
```
{: file="Oracle" }

- 컬럼 삭제

### DROP

### TRUNCATE

#### RENAME

```sql
RENAME stadium TO stadium_jsc;
```
{: file="Oracle" }

- 테이블 이름 변경

## DCL