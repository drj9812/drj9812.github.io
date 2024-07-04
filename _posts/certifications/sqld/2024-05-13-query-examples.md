---
title: "[SQLD]쿼리 예제"
categories: [Certifications, SQLD]
tags: [Certification, 자격증, SQLD]
image:
  path: /assets/img/posts/certifications/sqld/01-kdata-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: 한국데이터산업진흥원
---

# 쿼리 예제

## SELECT

```sql
SELECT deptno, first_name
  FROM emp
 ORDER BY hiredate ASC;
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
 ORDER BY deptno; --에러
```

- <font color="red">에러</font>
	+ **`GROUP BY` 절을 사용하는 경우 `GROUP BY` 절에 명시된 컬럼이나, 그룹함수에 사용된 컬럼만 명시 가능**

```sql
SELECT submission_date, hacker_id, COUNT(hacker_id) AS cnt, score
  FROM submissions
 GROUP BY submission_date, hacker_id, score;
```

- `GROUP BY` 절에 명시되어있다면, `SELECT` 절의 집계함수 뒤에 컬럼 명시 가능

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

```sql
SELECT svc_id, COUNT(*) AS cnt
  FROM svc_join
 WHERE svc_end_date >= TO_DATE('20150101', 'YYYYMMDD')
       AND
       svc_end_date < TO_DATE('20150201', 'YYYYMMDD')
       AND
       (join_ymd, join_hh) IN (('20141201', '00'))
 GROUP BY svc_id;
 ```

 - **2014년 12월 1일 00시에 가입한** 25년 1월부터 2월까지의 `svc_id`별 개수

```sql
SELECT menu_id, code, COUNT(*) AS cnt
  FROM system_histroy
 WHERE start_day BETWEEN SYSDATE - 1 AND SYSDATE
 GROUP BY menu_id, code
HAVING menu_id = 3 AND code = 100;
```

- `GROUP BY` 절로 그룹핑된 컬럼에 대한 `HAVING` 절에 집계함수가 없어도 됨

```sql
SELECT menu_id, code, AVG(COUNT(*)) AS avgcnt
  FROM system_histroy
 GROUP BY menu_id, code;
 ```

 - 중첩된 그룹함수 사용 불가

```sql
SELECT COUNT(*)
  FROM t1 a
 WHERE a.col NOT IN (SELECT co2 FROM t2);
```

| col |
|:---:|
|  1  |
|  2  |
|  3  |
|  4  |

|   co2  |
|:------:|
|    2   |
| `NULL` |

- 0
    + 서브쿼리의 결과 중에 `NULL`이 포함되는 경우 데이터가 출력되지 않음

```sql
SELECT *
  FROM t1 
 WHERE col1 IN(1, 2, NULL);
```

|  col1  | col2 |
|:------:|:----:|
| `NULL` |   A  |
|   1    |   B  |
|   2    |   C  |
|   3    |   D  |
|   4    |   E  |

| col1 | col2 |
|:----:|:----:|
|   1  |   B  |
|   2  |   C  |

- `NULL`은 비교에서 제외되고 주어진 테이블의 `col1` 속성값 1 or 2 을 갖는 행만 출력됨

```sql
SELECT (SELECT SUM(order_price)
          FROM daily_order_history)
  FROM customer GROUP BY user_id;
```

- 에러 X

### JOIN

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

```sql
SELECT a.*, b.*
  FROM daily_sales a, daily_sales b
 WHERE a.day >= b.day
 ORDER BY a.day ASC;
```

|    day     |  sales |    day_1   |  sales_1 |
|:----------:|:------:|:----------:|---------:|
| 2015-11-01 |   1000 | 2015-11-01 |   1000   |
| 2015-11-02 |   1000 | 2015-11-01 |   1000   |
| 2015-11-02 |   1000 | 2015-11-02 |   1000   |
| 2015-11-03 |   1000 | 2015-11-01 |   1000   |
| 2015-11-03 |   1000 | 2015-11-02 |   1000   |
| 2015-11-03 |   1000 | 2015-11-03 |   1000   |
|     ...    |   1000 |     ...    |   1000   |

```sql
SELECT a.day, SUM(b.sales) AS '누적매출액'
  FROM daily_sales a, daily_sales b
 WHERE a.day >= b.day
 GROUP BY a.day
 ORDER BY a.day ASC;
 ```

|    day     | 누적매출액 |
|:----------:|:---------:|
| 2015-11-01 |    1000   |
| 2015-11-02 |    2000   |
| 2015-11-03 |    3000   |
| 2015-11-04 |    4000   |
| 2015-11-05 |    5000   |
| 2015-11-06 |    6000   |
|     ...    |     ...   |

## DML

### INSERT

### UPDATE

### DELETE

```sql
DELETE 품목 WHERE 품목id = '002';
```

- `FROMR` 절 생략 가능

### MERGE

| COL1 | COL2 | COL3 |
|:----:|:----:|:----:|
|   A  |   X  |   1  |
|   B  |   Y  |   2  |
|   C  |   Z  |   3  |

| COL1 | COL2 | COL3 |
|:----:|:----:|:----:|
|   A  |   X  |   1  |
|   B  |   Y  |   2  |
|   C  |   Z  |   3  |
|   D  |  가  |   4  |
|   E  |  나  |   5  |

```sql
 MERGE INTO TEST1
 USING TEST2
    ON (TEST1.COL1 = TEST2.COL1)
  WHEN MATCHED THEN
UPDATE SET TEST1.COL3 = 4
 WHERE TEST1.COL3 = 2
DELETE WHERE TEST1.COL3 <= 2
  WHEN NOT MATCHED THEN
INSERT(TEST1.COL1, TEST1.COL2, TEST1.COL3) VALUES(TEST2.COL1, TEST2.COL2, TEST2.COL3);
```

| COL1 | COL2 | COL3 |
|:----:|:----:|:----:|
|   A  |   X  |   1  |
|   B  |   Y  |   4  |
|   C  |   Z  |   3  |
|   E  |  나  |   5  |
|   D  |  가  |   4  |

- 위 쿼리에서 `DELETE` 절은 실행되지 않음
    + `DELETE` 절은 `MERGE UPDATE` 절로 갱신된 행을 대상으로 수행되며, 갱신된 값을 기준으로 행을 삭제

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
    개시day DATE NOT NULL);
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
ALTER TABLE tab1 ALTER COLUMN a VARCHAR(30) NOT NULL;
ALTER TABLE tab1 ALTER COLUMN b DATE NOT NULL;
```

- SQL Server
- 여러 컬럼 변경

```sql
ALTER TABLE sqld MODIFY(a VARCHAR2(100), b VARCHAR2(100));
```

- 오라클
    + SQL Server는 불가능
- 여러 컬럼 동시 변경 가능

```sql
ALTER TABLE emp
 DROP COLUMN comm;
```

- 오라클
- 컬럼 삭제

```sql
ALTER TABLE order ADD CONSTRAINT fk_001 FOREIGN KEY(customer_id) REFERENCES customer(customer_id) ON DELETE SET NULL;
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

```sql
UPDATE a_user.tb_a
   SET col1 = 'aaa'
 WHERE col 2 = 3;
 ```

 - 위 쿼리를 수행하려면 `UPDATE` 문의 권한 뿐만 아니라 `SELECT` 문의 권한도 필요함
    + `WHERE` 절의 조건을 만족하는 행을 찾는 과정에서 `SELECT` 권한이 요구됨

## 함수

```sql
SELECT TO_CHAR(TO_DATE('2015.01.10 10', 'YYYY.MM.DD HH24') + 1/24/(60/10), 'YYYY.MM.DD HH24:MI:SS')
  FROM DUAL;
```

- 오라클
- `1/24/(60/10)`
	+ 10분
		* 1 → 하루
		* 1/24 → 1시간
		* 1/24(60/10) → 10분

```sql
SELECT ename, empno, mg, NULLIF(mgr, 7698) AS nm
  FROM emp;
```

- 오라클
- `mgr` 컬럼이 "7698"과 같으면 `NULL` 반환

```sql
SELECT SUM(COALESCE(c1, c2, c3))
  FROM tab1
```

|  c1  |  c2 |  c3  |
|:-----|:---:|:----:|
|   1  |  2  |   3  |
|      |  2  |   3  |
|      |     |   3  |

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

- 오라클의 계층형 질의에서 `START WITH` 절에 의해 시작되는 행은 `CONNECT BY` 절의 조건을 따르지 않음
  + `START WITH` 절에 의해 선택된 루트 노드는 `CONNECT BY` 절의 조건을 만족하지 않아도 결과에 포함될 수 있음

## 참고자료

- 한국데이터산업진흥원, *SQL 자격검정 실전문제*(서울: 한국데이터산업진흥원, 2016), 279.