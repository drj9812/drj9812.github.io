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

## SELECT

```sql
SELECT 지역, 매출금액
  FROM 지역별매출
 ORDER BY 년 ASC;
```

- 오라클, SQL Server
- `SELECT` 절에서 선택되지 않은 컬럼을 `ORDER BY` 절에 명시할 수 있음
	+ 인라인 뷰를 사용하는 경우 `SELECT` 절에서 선택된 컬럼만 명시 가능
	+ **`GROUP BY` 절을 사용하는 경우 `GROUP BY` 절에 명시된 컬럼이나, 그룹함수에 사용된 컬럼만 명시 가능**

```sql
SELECT deptno AS dept_no, sal, ename
  FROM emp
 ORDER BY dept_no, 2, ename;
```

- 오라클, SQL Server
- `ORDER BY` 절에 컬럼 별칭, 순서, 컬럼 이름 혼용해서 명시 가능

```sql
SELECT MIN(sal) AS min, MAX(sal) AS max
  FROM emp
 GROUP BY job
 ORDER BY deptno;
```

- <font color="red">에러</font>
	+ **`GROUP BY` 절을 사용하는 경우 `GROUP BY` 절에 명시된 컬럼이나, 그룹함수에 사용된 컬럼만 명시 가능**

```sql
SELECT submission_date, hacker_id, COUNT(hacker_id) AS cnt, score
  FROM submissions
 GROUP BY submission_date, hacker_id, score;
```

- `GROUP BY` 절에 명시되어있다면, 집계함수 뒤에 컬럼 명시 가능

```sql
WITH t AS (SELECT '빨간' a, '사과' b FROM dual
            UNION ALL
           SELECT '파란', '사과' FROM dual
            UNION ALL
           SELECT '빨간', '자두' FROM dual)
SELECT *
  FROM t
 WHERE (a, b) NOT IN (('빨간', '사과'));
```

| 파란 | 사과 |
| 빨간 | 자두 |

- Oracle
  + SQL Server는 `NOT IN` 연산자를 지원하지 않음

```sql
WITH t AS (SELECT '빨간' a, '사과' b FROM dual
            UNION ALL
           SELECT '파란', '사과' FROM dual
            UNION ALL
           SELECT '빨간', '자두' FROM dual)
SELECT *
  FROM t
 WHERE a NOT IN ('빨간')
       AND
       b NOT IN ('사과')
```

- `NULL`

```sql
SELECT SUM(sal)
  FROM emp
 HAVING SUM(sal) > 10;
 ```

```sql
SELECT 1
  FROM emp
HAVING COUNT(*) > 10;
```

```sql
SELECT MAX(sal)
  FROM emp;
```

 - `GROUP BY` 절을 사용하지 않아도 `HAVING` 절을 사용할 수 있고, 집계함수도 사용할 수 있음

```sql
SELECT employee_id, salary
  FROM employees
 WHERE salary IN (SELECT MAX(salary) FROM employees WHERE department_id = 30);
```

- 단일행 서브쿼리의 비교연산자로 다중행 서브쿼리의 비교연산자를 사용할 수 있음
  + 그 반대는 불가능

## DML

### INSERT

### UPDATE

### DELETE

```sql
DELETE 품목 WHERE 품목id='002';
```

- `FROMR` 절 생략 가능

### MERGE

## TCL

### SAVEPOINT

```sql
SAVEPOINT svpt1;
```

- 오라클

```sql
SAVE TRANSACTION svtr1;
```

- SQL Server

## DDL

### CREATE

```sql
CREATE TABLE 서비스(
    서비스번호 VARCHAR2(10) PRIMARY KEY
    서비스명 VARCHAR2(100) NULL,
    개시일자 DATE NOT NULL);
```

- 오라클
- 테이블 생성 시 PK 생성
- 테이블 생성 시 `NULL` 제약 조건 명시 가능

```sql
CREATE TABLE product(
    prod_id VARCHAR2(10) NOT NULL,
    prod_nm VARCHAR2(100) NOT NULL,
    reg_dt DATE NOT NULL,
    regr_no NUMBER(10),
    CONSTRAINT product_pk PRIMARY KEY(prod_id));
```

- 오라클
- 테이블 생성 시 PK 생성
- `CONSTRAINT product_pk` 생략 가능

```sql
CREATE TABLE 부서(
    부서번호 CHAR(10),
    부서명 CHAR(10),
    PRIMARY KEY(부서번호));
```

- 오라클
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

- 오라클
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

- 오라클
- 테이블 생성과 PK 생성 분리
- 인덱스 생성

### ALTER

```sql
ALTER TABLE 기관분류 ALTER COLUMN 분류명 VARCHAR(30) NOT NULL;
ALTER TABLE 기관분류 ALTER COLUMN 등록일자 DATE NOT NULL;
```

- SQL Server
- 여러 컬럼 변경

```sql
ALTER TABLE emp
 DROP COLUMN comm;
```

- 오라클
- 컬럼 삭제

```sql
ALTER TABLE 주문 ADD CONSTRAINT fk_001 FOREIGN KEY(고객id) REFERENCES 고객(고객id) ON DELETE SET NULL;
```

- 오라클
- 이미 존재하는 컬럼에 부모 테이블의 PK를 참조하는 FK 제약 조건 추가

### DROP

### TRUNCATE

#### RENAME

```sql
RENAME stadium TO stadium_jsc;
```

- 오라클
- 테이블 이름 변경

## DCL

## 함수

```sql
SELECT TO_CHAR(TO_DATE('2015.01.10 10', 'YYYY.MM.DD HH24') + 1/24/(60/10), 'YYYY.MM.DD HH24:MI:SS')
  FROM DUAL;
```

- 오라클
- `+1/24/(60/10)`
	+ 10분 더하기
		* +1 → 하루 더하기
		* +1/24 → 1시간 더하기
		* +1/24(60/10) → 10분 더하기

```sql
SELECT ename, empno, mg, NULLIF(mgr, 7698) AS nm
  FROM emp;
```

- 오라클
- `mgr` 컬럼이 "7698"과 같으면 `NULL` 반환

|  c1  |  c2 |  c3  |
|:-----|:---:|:----:|
|   1  |  2  |   3  |
|      |  2  |   3  |
|      |     |   3  |

```sql
SELECT SUM(COALESCE(c1, c2, c3))
  FROM tab1
```

- 오라클
- 6 출력
	+ 1 + 2 + 3

```sql
SELECT TOP(2) WITH TIES ename, sal
  FROM emp
 ORDER BY sal DESC;
```

- SQL Server
- 급여가 높은 2명을 내림차순으로 출력하되 같은 급여를 받는 사원이 있으면 같이 출력
	+ `TOP (2) WITH TIES ename`도 가능

## JOIN

```sql
 SELECT LEVEL
   FROM dual
CONNECT BY LEVEL < 2
  CROSS JOIN (SELECT LEVEL
                FROM daul
             CONNECT BY LEVEL < 2);
```

- <font color="red">에러</font>
	+ `JOIN` 연산자 이후에 hierachical_query_clause(`CONNECT BY` 절)이 와야 함
		* <https://docs.oracle.com/en/database/oracle/oracle-database/19/sqlrf/SELECT.html>{: target="_blank" }
	+ 메인 쿼리의 `SELECT LEVEL FROM dual CONNECT BY LEVEL < 2` 절을 인라인 뷰로 두면 해결 가능

```sql
SELECT empno
  FROM emp
 INNER JOIN dept
    ON emp.deptno = dept.deptno;
```

- 테이블 별칭 꼭 명시 안 해도 됨
- Ambiguous한 컬럼이 아닌 경우 컬럼 소유자(테이블) 명시 안 해도 됨

### SELF JOIN

```sql
SELECT a.*, b.*
  FROM 일자별매출 a, 일자별매출 b
 WHERE a.일자 >= b.일자
 ORDER BY a.일자 ASC;
```

|    일자    | 매출액 |    일자_1   | 매출액_1 |
|:----------:|:------:|:----------:|---------:|
| 2015-11-01 |   1000 | 2015-11-01 |   1000   |
| 2015-11-02 |   1000 | 2015-11-01 |   1000   |
| 2015-11-02 |   1000 | 2015-11-02 |   1000   |
| 2015-11-03 |   1000 | 2015-11-01 |   1000   |
| 2015-11-03 |   1000 | 2015-11-02 |   1000   |
| 2015-11-03 |   1000 | 2015-11-03 |   1000   |
|     ...    |   1000 |     ...    |   1000   |

```sql
SELECT a.일자, SUM(b.매출액) AS 누적매출액
  FROM 일자별매출 a, 일자별매출 b
 WHERE a.일자 >= b.일자
 GROUP BY a.일자
 ORDER BY a.일자 ASC;
 ```

|    일자    | 누적매출액 |
|:----------:|:---------:|
| 2015-11-01 |    1000   |
| 2015-11-02 |    2000   |
| 2015-11-03 |    3000   |
| 2015-11-04 |    4000   |
| 2015-11-05 |    5000   |
| 2015-11-06 |    6000   |
|     ...    |     ...   |

## 계층형 질의

```sql
 SELECT employee_id, first_name, last_name, manager_id, hire_date
   FROM employees
  START WITH manager_id IS NULL
CONNECT BY PRIOR employee_id = manager_id
                 AND
                 hire_date BETWEEN DATE '2999-01-05' AND DATE '2999-01-10';
```

| employee_id | first_name | last_name | manager_id |      hire_date      |
|:-----------:|:----------:|:---------:|:----------:|:-------------------:|
|     100	    |   Steven	 |   King		 |   `NULL`   | 2013/06/17 00:00:00 |

- 오라클의 계층형 쿼리에서 `START WITH` 절에 의해 시작되는 행은 `CONNECT BY` 절의 조건을 따르지 않음
  + `START WITH` 절에 의해 선택된 루트 노드는 `CONNECT BY` 절의 조건을 만족하지 않아도 결과에 포함될 수 있음

## 참고자료

- 한국데이터산업진흥원, SQL 자격검정 실전문제(서울: 한국데이터산업진흥원, 2016), 279.