---
title: "[SQLD | 2과목]SQL 기본 및 활용 - SQL 활용(2024)"
categories: [Certifications, SQLD]
tags: [Certification, 자격증, SQLD]
image:
  path: /assets/img/posts/certifications/sqld/01-sqld-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: SQLD
---

|    구분   |        시험과목      |             과목별 세부 내용           | 문항수 |       배점       |  과락 기준  |     검정시간    |
|:---------:|:-------------------:|:-------------------------------------:|:-------:|:---------------:|:----------:|:---------------:|
|   1과목   | 데이터 모델링의 이해 | 데이터 모델링의 이해, 데이터 모델과 SQL |   10    | 20점(문항별 2점) |   8점 미만 |                 |
| **2과목** | **SQL 기본 및 활용** |    SQL 기본, **SQL 활용**, 관리 구문   |   40    | 80점(문항별 2점) |  32점 미만 |                 |
|     계    |                     |                                       |   50    |      100점       |           | 90분(1시간 30분) |

# SQL 기본 및 활용 - SQL 활용(2024)

|  주요 항목   |          세부 항목        |
|:------------:|:------------------------:|
|   SQL 기본   |  관계형 데이터베이스 개요  |
|              |        `SELECT` 문       |
|              |            함수          |
|              |         `WHERE` 절       |
|              | `GROUBP BY`, `HAVING` 절 |
|              |            JOIN          |
|              |          표준 JOIN       |
| **SQL 활용** |          서브 쿼리        |
|              |         집합 연산자       |
|              |          그룹 함수        |
|              |         윈도우 함수       |
|              |         TOP N 쿼리        |
|              |   계층형 질의와 셀프 JOIN  |
|              | `PIVOT` 절과 `UNPIVOT` 절 |
|              |         정규 표현식       |
|  관리 구문   |             DML           |
|              |             TCL           |
|              |             DDL           |
|              |             DCL           |

## 서브쿼리

- **하나의 SQL 문 안에 포함되어 있는 또다른 SQL 문을 말함**
- 반드시 **괄호로 묶어야 함**

### 서브쿼리 사용 가능한 곳

- `SELECT` 절
- `FROM` 절
- `WHERE` 절
- `HAVING` 절
- **`ORDER BY` 절**
- 기타 DML(`INSERT`, `DELETE`, `UPDATE`) 절

> 서브쿼리는 `GROUP BY` 절에서 사용할 수 없다.
{: .prompt-warning }

### 서브쿼리의 종류

|               서브쿼리 종류              |                                      설명                                   |
|:---------------------------------------:|:---------------------------------------------------------------------------:|
|   Single Row 서브쿼리(단일 행 서브쿼리)   |             서브쿼리의 실행 결과가 항상 1건 이하인 서브쿼리를 의미            |
|                                          |              단일 행 서브쿼리는 단일 행 비교 연산자와 함께 사용됨             |
|                                          |                  단일 행 비교 연산자 = =, <, <=, >, >=, <>                  |
|    Multi Row 서브쿼리(다중 행 서브쿼리)   |                서브쿼리의 실행 결과가 여러 건인 서브쿼리를 의미               |
|                                          |              다중 행 서브쿼리는 다중 행 비교 연산자와 함꼐 사용됨             |
|                                          |                다중 행 비교 연산자 IN, ALL, ANY, SOME, EXSIST               |
| Multi Column 서브쿼리(다중 컬럼 서브쿼리) |                     서브쿼리의 실행 결과로 여러 컬럼을 반환                   |
|                                          |             메인 쿼리의 조건절에 여러 컬럼을 동시에 비교할 수 있음            |
|                                          | 서브 쿼리와 메인 쿼리에서 비교하고자 하는 컬럼 개수와 컬럼의 위치가 동일해야 함 |

#### 동작하는 방식에 따라

##### UN-CORRELATED(비연관) 서브쿼리

- 서브쿼리가 메인쿼리 컬럼을 가지고 있지 않은 형태의 서브쿼리
- 서브쿼리가 실행된 결과 값을 메인 쿼리에 제공하기 위한 목적으로 사용

##### CORRELATED(연관) 서브쿼리

- 서브쿼리가 메인쿼리 컬럼을 가지고 있는 형태의 서브쿼리
- 일반적으로 메인쿼리가 먼저 수행된 후, 서브쿼리에서 조건이 맞는지 확인하고자 할 때 사용

#### 위치에 따라

##### 스칼라 서브쿼리

![01-ex-scalar-subquery](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/01-ex-scalar-subquery.jpg)
*`emp` 테이블의 각 직원의 사번, 이름과 부서이름을 출력*

```sql
SELECT * | 컬럼명 | 표현식, (SELECT * | 컬럼명 표현식
                               FROM 테이블명 또는 뷰명
			      WHERE 조건)
  FROM 테이블명 또는 뷰명;
```

- `SELECT` 절에 사용하는 서브쿼리
- 서브쿼리 결과를 마치 **하나의 컬럼처럼 사용하기 위해 주로 사용**
	+ 하나의 출력 대상만 표현 가능
- 각 행마다 스칼라 서브쿼리의 결과가 하나여야 함
	+ 단일행 서브쿼리 형태
- JOIN의 대체 연산
- **성능이 좋지 않기 때문에 잘 사용되지 않음**

#### 인라인 뷰(Inline View)

![02-ex-inline-view](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/02-ex-inline-view.jpg)
*`emp` 테이블에서 부서별 최대 급여자를 출력하되, 최대 급여와 함께 출력*

```sql
SELECT * | 컬럼명 | 표현식
  FROM (SELECT * | 컬럼명 | 표현식
          FROM 테이블명 또는 뷰명)
 WHERE 조건;
```

- `FROM` 절에 사용하는 서브쿼리
- **서브쿼리 결과를 쿼리 안의 뷰의 형태로 테이블처럼 사용하기 위해 주로 사용**
- 테이블명이 존재하지 않기 떄문에 다른 테이블과 조인시 **반드시 테이블 별칭 명시**
	+ 단독으로 사용하는 경우 별칭 필요 없음
- `WHERE` 절 서브쿼리와 다르게 **서브쿼리 결과를 메인 쿼리의 어느 절에서도 사용할 수 있음**
- 인라인 뷰의 결과와 메인 쿼리 테이블과 조인할 목적으로 주로 사용
- 모든 연산자 사용 가능

#### WHERE 절 서브쿼리

- **가장 일반적인 서브쿼리**
- 비교 상수 자리에 **값을 전달하기 위한 목적으로 주로 사용(상수항의 대체)**
- 반환 데이터의 형태에 따라 단일행 서브쿼리, 다중행 서브쿼리, 다중컬럼 서브쿼리, 상호연관 서브쿼리로 구분

##### WHERE 절 서브쿼리의 종류

###### 단일행 서브쿼리

![03-ex-where-subquery](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/03-ex-where-subquery.jpg)

- **서브쿼리 결과로 1개의 행이 반환**되는 형태

###### 단일행 서브쿼리 연산자

| 연산자 |    의미    |
|:------:|:----------:|
|   `=`  |    같다    |
|  `<>`  |  같지 않다 |
|   `>`  |    크다    |
|  `>=`  | 크거나 같다 |
|   `<`  |    작다    |
|  `<=`  | 작거나 같다 |

- 다중행 서브쿼리의 비교연산자로 단일행 서브쿼리의 비교연산자를 사용할 수 없음
	+ 그 반대는 가능함

###### 다중행 서브쿼리

![04-ex-where-multiple-subquery](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/04-ex-where-multiple-subquery.jpg)
*`emp` 테이블에서 부서번호가 10인 급여자들의 급여보다 큰 급여자들 출력*

- **서브쿼리 결과로 여러 행이 반환되는 형태**
- `=`, `>`, `<`와 같은 비교 연산자 사용 불가능
	+ 여러 값이랑 비교할 수 없는 연산자들 사용 불가능
- 서브쿼리 결과를 하나로 요약하거나 다중행 서브쿼리 연산자를 사용

###### 다중행 서브쿼리 연산자

|  연산자 |     의미      |
|:-------:|:-------------:|
|  `IN`   | 같은 값을 찾음 |
| `> ANY` |  최솟값을 반환 |
| `< ANY` |  최댓값을 반환 |
| `< ALL` |  최솟값을 반환 |
| `> ALL` |  최댓값을 반환 |

- 단일행 서브쿼리의 비교연산자로 다중행 서브쿼리의 비교연산자를 사용할 수 있음
	+ 그 반대는 불가능

###### 다중컬럼 서브쿼리

![05-ex-where-mulity-column-subquery](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/05-ex-where-mulity-column-subquery.jpg)
*`emp` 테이블에서 부서별 최대 급여자 출력*

- **서브쿼리 결과로 여러 컬럼이 반환되는 형태**
- 메인 쿼리와의 비교 컬럼이 2개 이상
- 대소 비교 전달 불가능
	+ 두 값을 동시에 묶어 대소를 비교할 수 없음
+ SQL Server에서는 지원하지 않음

###### 상호 연관 서브쿼리

![06-ex-correlated-subquery](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/06-ex-correlated-subquery.jpg)
*`emp` 테이블에서 부서별로 해당 부서의 평균 급여보다 높은 급여를 받는 사원 정보 출력*

- **메인 쿼리와 서브쿼리의 비교를 수행하는 형태**
- 비교할 집단이나 조건은 서브쿼리에 명시
	+ 메인 쿼리 절에는 서브쿼리 컬럼이 정의되지 않있기 때문에 에러가 발생함

### 서브쿼리 주의사항

- 특별한 경우(TOP-N 분석 등)을 제외하고는 서브쿼리 절에 `ORDER BY` 절을 사용할 수 없음
- 단일행 서브쿼리와 다중행 서브쿼리에 따라 연산자의 선택이 중요함

## 집합 연산자

- `SELECT` 문 결과를 하나의 집합으로 간주하고, 그 **집합에 대한 합집합, 교집합, 차집합을 연산**
- `SELECT` 문과 `SELECT` 문 사이에 집합 연산자를 정의
- **두 집합의 컬럼이 동일하게 구성되어야 함**
	+ **각 컬럼의 데이터 타입과 순서 일치 필요**
- 전체 집합의 데이터타입과 컬럼명은 첫 번째 집합에 의해 결정됨

### 합집합

- 두 집합의 총 합(전체) 출력
- `UNION`과 `UNION ALL` 사용 가능

#### UNION

![07-ex-union](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/07-ex-union.jpg)
*10번 부서 소속이 아닌 직원 정보와 20번 소속 직원 정보가 각각 분리되어있다고 가정할 때 두 집합의 합집합(중복 X)*
 
- 중복된 데이터는 한 번만 출력
- **중복된 데이터를 제거하기 위해 내부적으로 정렬 수행**
- **중복된 데이터가 없을 경우에는 `UNION` 대신 `UNION ALL` 사용**
	+ 불필요한 정렬이 발생할 수 있으므로

#### UNION ALL

![08-ex-union-all](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/08-ex-union-all.jpg)
*10번 부서 소속이 아닌 직원 정보와 20번 소속 직원 정보가 각각 분리되어있다고 가정할 때 두 집합의 합집합(중복 O)*

- 중복된 데이터도 전체 출력
- 테이블 별칭을 사용하는 경우 첫 번째 `SELECT` 절의 테이블의 별칭을 사용

### 교집합

![09-ex-intersect](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/09-ex-intersect.jpg)
*10번 부서 정보와 20번 부서 정보가 각각 분리되어있다고 가정할 때 두 집합의 교집합*

- 두 집합 사이에 `INTERSECT` 연산자 명시
- 두 집합의 교집합(공통으로 있는 행) 출력

### 차집합

![10-ex-minus](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/10-ex-minus.jpg)
*10번이 아닌 부서 정보와 20번 부서 정보가 각각 분리되어있다 가정할 때 두 집합의 차집합*

- 두 집합 사이에 `MINUS` 연산자 명시
- 두 집합의 차집합(한 쪽 집합에만 존재하는 행) 출력
- A - B와 B - A는 다르므로 <font color="red">집합의 순서 주의</font>
- SQL Server
	+ `EXCEPT`

### 집합 연산자 사용시 주의 사항

- 두 집합의 컬럼 수 일치
- 두 집합의 컬럼 순서 일치
- 두 집합의 각 컬럼의 데이터 타입 일치
- 각 컬럼의 사이즈는 달라도 됨

## 그룹 함수

- 숫자 함수 중 여러 값을 전달하여 하나의 요약값을 출력하는 다중행 함수
- 수학/통계 함수(기술 통계 함수)
- `GROUP BY` 절에 의해 그룹별 연산 결과를 반환함
- <font color="red">반드시 한 컬럼만 전달</font>
- **`NULL`은 무시하고 연산**
- 중첩 불가능
	+ `AVG(COUNT(*))`는 에러

### COUNT()

![11-ex-count()](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/11-ex-count().jpg)
*각 커럼의 count 결과*

- 행의 수를 세는 함수
- 대상 컬럼은 **`*` 또는 하나의 컬럼만 전달 가능**
	+ `*` 사용시 모든 컬럼의 값이 `NULL`일 때만 카운트하지 않음
	+ 하나의 컬럼만 전달했을 때 `NULL`인 값에 대해서는 카운트하지 않음
- **문자, 숫자, 날짜 컬럼 모두 전달 가능**
- 행의 수를 세는 경우 `NOT NULL` 컬럼(PK 컬럼)을 찾아 세는 것이 좋음

### SUM()

- 총 합 출력
- 숫자 컬럼만 전달 가능

### AVG()

![12-ex-avg()](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/12-ex-avg().jpg)

- 평균 출력
- 숫자 컬럼만 전달 가능
- <font color="red">`NULL`을 제외한 대상의 평균을 반환하므로 전체 대상 평균 연산 시 주의</font>

### MIN(), MAX()

- 최소, 최대 출력
- **날짜, 숫자, 문자 모두 가능**
	+ 오름차순 정렬

### VARIANCE(), STDDEV()

- 분산과 표준편차
	+ 분산: 각 값과 평균 값의 차이를 제곱하여 모두 합한 후, 그 값들의 개수로 나눈 것
	+ 표준편차: 각 값과 평균 값의 차이를 제곱하여 모두 합한 후, 그 값을 데이터의 개수로 나눈 후에 제곱근을 취한 것

### GROUP BY FUNCTION

- `GROUP BY` 절에 사용하는 함수
- **여러 `GROUP BY` 절의 결과를 동시에 출력(합집합)하는 기능**
- 그룹핑할 그룹을 정의
	+ 전체 소계 등

### GROUPING()

- 그룹화된 결과 집합에서 특정 컬럼에 대해 그룹화가 적용되었는지 여부를 식별하는 데 사용
    + 1: 그룹 함수가 포함된 열이 그룹의 최상위 수준인 경우
        * `ROLLUP()` 함수 또는 `CUBE()` 함수의 소계, 총계 행
    + 0: 그룹 함수가 포함된 컬럼이 특정 그룹의 일부인 경우
    + `NULL`: 그룹 함수가 포함된 컬럼이 아닌 경우
- `GROUP BY ROLLUP()`, `CUBE()`, `GROUPING SETS()` 등의 고급 그룹화 연산과 함께 사용될 때 유용

#### GROUPING SETS(a, b, ...)

- **`a`별, `b`별 그룹 연산 결과 출력**
- **그룹으로 묶을 대상의 나열 순서가 중요하지 않음**
- 기본 출력에 전체 **총계는 출력되지 않음**
	+ `NULL` 혹은 괄호를 사용하여 전체 총 합을 출력할 수 있음
- `UNION ALL` 연산자로 대체 가능

##### 예시

![13-grouping-sets()(1)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/13-grouping-sets()(1).jpg)
*부서번호별 급여의 총 합 결과와 직무별 급여의 총 합 결과의 합집합*

![14-grouping-sets()(2)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/14-grouping-sets()(2).jpg)
*`UNION ALL`로 대체 가능*

![15-grouping-sets()(3)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/15-grouping-sets()(3).jpg)
*전체 총계 출력 가능*
	
#### ROLLUP(a, b)

![16-ex-rollup()](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/16-ex-rollup().jpg)

- **`a`별, (`a`, `b`)별, 전체 그룹 연산 결과 출력**
- <font color="red">그룹으로 묶을 대상의 나열 순서가 중요함</font>
    + **(`a`, `b`)별, `a`별, 전체 그룹 순서로 그룹핑**
- 기본적으로 **전체 총계가 출력됨**
- `UNION ALL` 연산자로 대체 가능

#### CUBE(a, b)

![17-ex-cube()](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/17-ex-cube().jpg)

- **`a`별, `b`별, (`a`,`b`)별 전체 그룹 연산 결과 출력**
- **그룹으로 묶을 대상의 나열 순서가 중요하지 않음**
- 기본적으로 전체 총계가 출력됨
- `UNION ALL`로 연산자로 대체 가능
- `GROUPING SETS()` 함수로 대체 가능
- 인자로 주어진 컬럼의 결합 가능한 모든 조합에 대해서 집계를 수행하므로 다른 그룹 함수에 비해 시스템에 대한 부하가 큼

## 윈도우 함수

![18-error-group-function](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/18-error-group-function.jpg)
*윈도우 함수가 필요한 이유: 전체를 출력하는 컬럼과 그룹 함수의 결과는 함께 출력할 수 없음*

```sql
SELECT 윈도우 함수([대상]) OVER([PARTITION BY 컬럼]
                                [ORDER BY 컬럼 ASC | DESC]
                                [ROWS | RANGE BETWEEN a AND b]);
```

- **서로 다른 행의 비교나 연산을 위해 만든 함수**
- `GROUP BY` 절을 사용하지 않고 그룹 연산 가능
- `LAG()`, `LEAD()`, `SUM()`, `AVG()`, `MIN()`, `MAX()`, `COUNT()`, `RANK()`
- **`PARTITION BY`, `ORDER BY`, `ROWS` 절의 전달 순서 중요**
	+ `ORDER BY` 절을 `PARITION BY` 절 전에 사용 불가
- 윈도우 함수의 적용 범위는 `PARTITION BY` 절을 넘을 수 없음

### PARTITION BY 절

- 출력할 총 데이터 수의 변화 없이 그룹 연산을 수행할 `GROUP BY` 컬럼

### ORDER BY 절

- `RANK()`, `DENS_RANK()` 함수의 경우 필수
	+ 정렬 컬럼 및 정렬 순서에 따라 순위 변화
- `SUM()`, `AVG()`, `MIN()`, `MAX()`, `COUNT()` 등은 **누적 값 출력 시 사용**

### ROWS | RANGE BETWEEN a AND b

- 연산 범위 설정
- `ORDER BY` 절 필수

#### ROWS, RANGE의 차이

![19-ex-over()-rows](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/19-ex-over()-rows.jpg)
*ROWS*

- `ROWS`: 값이 같더라도 각 행씩 연산
	+ `BETWEEN a AND b` 필수

![20-ex-over()-range](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/20-ex-over()-range.jpg)
*RANGE*

- `RANGE`: 같은 값의 경우 하나의 범위로 묶어서 동시 연산
	+ default

#### BETWEEN a AND b

![21-ex-between-a-and-b](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/21-ex-between-a-and-b.jpg)

- a: 시작점 정의
	+ `CURRENT ROW`: 현재 행부터
	+ `UNBOUNDED PRECEDING`: 처음부터
		* default
	+ `n PRECEDING`: n 이전부터
- b: 마지막 시점 정의
	+ `CURRENT ROW`: 현재 행까지
		* default
	+ `UNBOUNDED FOLLOWING`: 마지막까지
	+ `n FOLLOWING`: n 이후까지

### 그룹 함수의 형태

```sql
SELECT 그룹함수(대상), OVER([PARTITION BY 컬럼]
                            [ORDER BY 컬럼 ASC | DESC]
                            [ROWS | RANGE BETWEEN a AND b]);
```

- `SUM()`, `COUNT()`, `AVG()`, `MIN()`, `MAX()` 등
- **`OVER` 절을 사용하여 윈도우 함수로 사용 가능**
- 반드시 연산할 대상을 그룹 함수의 입력 값으로 전달

#### SUM OVER()

![22-ex-sum-window-function](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/22-ex-sum-window-function.jpg)
*각 직원 정보와 함께 급여 총합 추력*

- 전체 총합, 그룹별 총합 출력 가능

### 순위 관련 함수

#### RANK() WITHIN GROUP()

![23-ex-rank()-within-group()](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/23-ex-rank()-within-group().jpg)
*`emp` 테이블에서 3000의 전체 급여 순위 출력*

```sql
SELECT RANK(값) WITHIN GROUP(ORDER BY 컬럼);
```

- **특정 값에 대한 순위 확인**
- 윈도우 함수가 아닌 일반 함수
- `ORDER BY` 절 필수

#### RANK() OVER()

![24-ex-rank()-over()](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/24-ex-rank()-over().jpg)
*각 직원의 급여의 전체 순위*

```sql
SELECT RANK() OVER([PARTITION BY 컬럼]
                    ORDER BY 컬럼 ASC | DESC);
```

- **전체 또는 특정 그룹 중 값의 순위 확인**
- **`ORDER BY` 절 필수**
	+ `ORDER BY` 절에 순위를 구할 대상 명시
		* 여러 대상 명시 가능

![25-ex-rank()-over(partition-by)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/25-ex-rank()-over(partition-by).jpg)
*각 직원의 급여의 부서별 순위*

- 그룹 내 순위 구할 때는 `PARTITION BY` 절 사용

#### DENSE_RANK()

![26-ex-rank(),dense_rank()](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/26-ex-rank(),dense_rank().jpg)
*`RANK()`, `DENSE_RANK()` 비교*

- **누적 순위**
- 값이 같을 때 **동일한 순위를 부여 후, 다음 순위가 바로 이어지는 순위 부여 방식**
	+ 1등이 5명이더라도 그 다음 순위가 2등

#### ROW_NUMBER()

![27-ex-rank(),dense_rank(),row_number()](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/27-ex-rank(),dense_rank(),row_number().jpg)
*`RANK()`, `DENSE_RANK()`, `ROW_NUMBER()` 비교*

- **연속된 행 번호**
- 순위를 인정하지 않고, **단순히 순서대로 나열한 순서 값 반환**

### LAG(), LEAD()

![28-ex-lag()(1)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/28-ex-lag()(1).jpg)
*`emp` 테이블에서 바로 이전 입사자와의 급여 비교*

```sql
SELECT LAG(컬럼,
	         n)
       OVER([PARTITION BY 컬럼]
             ORDER BY 컬럼 ASC | DESC);
```

- **행 순서대로 각각 이전 값(`LAG()`), 이후 값(`LEAD()`)을 반환**
- 세 번째 인자는 선택적(default) 값으로, 현재 행에 대한 이전 행의 값이 없는 경우에 반환할 값을 지정
- **`ORDER BY` 절 필수**

![28-ex-lag()(2)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/28-ex-lag()(2).jpg)

> 이전/이후 값을 가져올 때 이전 값이 같더라도 항상 행의 순서대로 이전/이후의 값을 가져오기 때문에 `ORDER BY` 절을 통해 사용자가 원하는 행 배치의 이전/이후 값을 가져올 수 있다.
{: .prompt-info }

### FIRST_VALUE(), LAST_VALUE()

![29-ex-first_value()](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/29-ex-first_value().jpg)
*`FIRST_VALUE()` 함수를 사용한 최솟값, 최댓값 출력*

![30-ex-last_value()](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/30-ex-last_value().jpg)
*`LAST_VALUE()` 함수를 사용한 최댓값 출력*

- 정렬 순서대로 **정해진 범위에서의 처음 값, 마지막 값 출력**
- 순서와 범위 정의에 따라 최솟값과 최댓값 반환

### NTILE()

![31-ex-ntile()](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/31-ex-ntile().jpg)
*`NTILE()` 함수를 사용한 그룹 분리*

```sql
SELECT NTILE(n) OVER([PARTITON BY 컬럼]
                      ORDER BY 컬럼 ASC | DESC);
```

- 행을 특정 컬럼 순서에 따라 **정해진 수의 그룹으로 나누기 위한 함수**
- **그룹 번호가 반환됨**
- **`ORDER BY` 절 필수**
- `PARTITION BY` 절을 사용하여 추가로 특정 그룹을 원하는 수만큼 그룹 분리 가능 

### 비율관련 함수

#### RATIO_TO_REPORT()

![32-ex-ratio_to_report()](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/32-ex-ratio_to_report().jpg)
*`RATIO_TO_REPORT()` 함수를 사용한 부서 내의 각 값의 비율*

```sql
RATIO_TO_REPORT(대상) OVER([PARTITION BY ...]);
```

- **각 값의 비율 반환**
	+ 전체 비율 또는 특정 그룹 내 비율 가능
+ `ORDER BY` 절 사용 불가

#### CUME_DIST()

![33-ex-ratio_to_report(),cume_dist()](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/33-ex-ratio_to_report(),cume_dist().jpg)
*`RATIO_TO_REPORT()`, `CUME_DIST()` 함수를 사용한 누적 비율 비교*

```sql
CUME_DIST() OVER([PARTITION BY 컬럼]
                  ORDER BY 컬럼);
```

- **각 값의 누적 비율 반환**
	+ 전체 비율 또는 특정 그룹 내 비율 가능
- `ORDER BY` 절 필수
	+ 누적 비율을 구하는 순서를 정할 수 있음

#### PERCENT_RANK()

![34-ex-percent_rank()](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/34-ex-percent_rank().jpg)

```sql
PERCENT_RANK() OVER([PARTITION BY ...]
                     ORDER BY ...])
```

- **`PERCENTILE(분위수)` 출력**
- **전체 count 중 상대적 위치 출력**
	+ 0~1 범위 내
- **`ORDER BY` 절 필수**

## TOP N QUERY

- 페이징 처리를 효과적으로 수행하기 위해 사용
- **전체 결과에서 특정 n개 추출**

### TOP-n 행 추출 방법

#### ROWNUM

![35-ex-top-n-query-with-rownum(1)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/35-ex-top-n-query-with-rownum(1).jpg)

- 출력된 데이터를 기준으로 행 번호 부여
- 절대적인 행 번호가 아닌 가상의 번호이므로 특정 행을 지정할 수 없음
	+ 연산 불가
- 첫 번째 행이 증가한 이후 할당되므로 `>` 연산 사용 불가
	+ 0은 가능

##### ROWNUM의 잘못된 사용

![36-ex-top-n-query-with-rownum(2)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/36-ex-top-n-query-with-rownum(2).jpg)

- 크다(`WHERE > n`) 조건 전달 불가

![37-ex-top-n-query-with-rownum(3)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/37-ex-top-n-query-with-rownum(3).jpg)

- 항상 불변하는 절대적 번호가 아니므로 `=` 연산자 단독 전달 불가
	+ 1이 먼저 정의가 되야 함

![38-ex-top-n-query-with-rownum(4)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/38-ex-top-n-query-with-rownum(4).jpg)
*`emp` 테이블에서 급여가 높은 순서대로 상위 5명의 직원 정보 출력*

- 실제로 상위 명 출력 안됨
	+ 최대 급여 5000
- `WHERE` 절에 의해 먼저 5개를 추출한 뒤 이 결과 집합에 대해 정렬 수행하므로 의도한대로 출력되지 않음
	+ 서브쿼리(인라인 뷰)를 사용하여 `sal`에 대해 내림차순 정렬을 해놓고 상위 5개를 가져와야 함

![39-ex-top-n-query-with-rownum(5)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/39-ex-top-n-query-with-rownum(5).jpg)
*`emp` 테이블에서 상위 4~6순위의 급여를 갖는 직원 정보 출력*

- `ROWNUM`의 시작 값(1)이 정의되지 않았으므로 1을 건너뛰고 그 다음 행 번호에 대한 추출 불가
	+ 서브쿼리(인라인 뷰)를 사용하여 해결해야 함

#### RANK()

![40-ex-top-n-query-with-rank()-over()](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/40-ex-top-n-query-with-rank()-over().jpg)
*`emp` 테이블에서 상위 4~6순위의 급여를 갖는 직원 정보 출력*

#### FETCH 

- **출력될 행의 수를 제한하는 절**
- **오라클 12c** 이상부터 제공
	+ 이전 버전에서는 `ROWNUM` 주로 사용
- SQL Server에서 사용 가능
- `ORDER BY` 절 뒤에 사용
	+ 내부 파싱 순서도 `ORDER BY` 절 이후

```sql
SELECT
  FROM
 WHERE
 GROUP BY
HAVING
 ORDER BY
OFFSET n { ROW | ROWS }
 FETCH { FIRST | NEXT } n { ROW | ROWS } ONLY
```

- `OFFEST`
	+ 건너뛸 행의 수
	+ 선택
- `n`
	+ 출력할 행의 수
- `FETCH`
	+ 출력할 행의 수를 전달하는 구문
- `FIRST`
	+ `OFFEST`을 사용하지 않았을 때 처음부터 n행 출력 명령
- `NEXT`
	+ `OFFSET`을 사용했을 경우, 제외한 행 다음부터 n행 출력 명령
- `ROW | ROWS`
	+ 행의 수에 따라 하나일 경우 단수, 여러 값이면 복수형
		* 특별히 구분하지 않아도 됨

##### 예시

![41-ex-top-n-query-with-fetch(1)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/41-ex-top-n-query-with-fetch(1).jpg)
*`emp` 테이블에서 급여가 높은 상위 5명 직원 정보 출력*

- `FIRST` 대신 `NEXT` 사용해도 상관없음
- `ROWS` 대신 `ROW` 사용해도 상관없음

![42-ex-top-n-query-with-fetch(2)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/42-ex-top-n-query-with-fetch(2).jpg)
*`emp` 테이블에서 급여가 높은 순서대로 4~6번째에 해당하는 직원 정보 출력*

- `FIRST` 대신 `NEXT` 사용해도 상관없음
- `ROWS` 대신 `ROW` 사용해도 상관없음

## 계층형 질의

```sql
 SELECT
   FROM 테이블명
  START WITH 시작조건
CONNECT BY PRIOR 연결조건;
```

- **하나의 테이블 내 각 행끼리 관계를 가질 때, 연결고리를 통해 행과 행 사이의 계층(상하관계)을 표현하는 기법**
- **`PRIOR`의 위치에 따라 연결하는 데이터가 달라짐**
	+ **`CONNECT BY PRIOR a = b`**
		* `PRIOR a`는 현재 행의 부모의 `a`를 의미
		* **즉, 현재 행의 `b`가 부모 행의 `a`와 같아야 함 **
	+ **`CONNECT BY a = PRIOR  b`**
		* `PRIOR b`는 현재 행의 부모의 `b`를 의미
		* **즉, 현재 행의 `a`가 부모 행의 `b`와 같아야 함**
- `START WITH`
	+ 데이터를 출력할 시작점을 지정하는 조건
	+ 여러 개의 조건 명시 가능
- `CONNECT BY PRIOR`
	+ 시작점을 기준으로 하위 계급을 찾아가는 조건
	+ 행을 이어나갈 조건
	+ **오라클의 계층형 질의에서 `PRIOR` 키워드는 `SELECT`, `WHERE` 절에서도 사용할 수 있음**
- **오라클의 계층형 질의에서 `WHERE` 절은 모든 전개를 진행한 이후 필터 조건으로서 조건을 만족하는 데이터만을 추출하는 데 활용됨**
- SQL Server
	+ SQL Server에서의 계층형 질의문은 CTE(Common Table Expression)를 재귀 호출함으로써 계층 구조를 전개함
	+ SQL Server에서의 계층형 질의문은 앵커 멤버를 실행하여 기본 결과 집합을 만들고 이후 재귀 멤버를 지속적으로 실행함

### 예시

![43-ex-hierarchical-query(1)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/43-ex-hierarchical-query(1).jpg)
*`dept2` 테이블에 대해 각 부서의 레벨을 출력(최상위 부서가 1레벨)*

![44-ex-hierarchical-query(2)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/44-ex-hierarchical-query(2).jpg)
*`dept2` 테이블에 대해 각 부서의 레벨을 출력(최상위 부서가 1레벨)의 잘못된 예*

![45-ex-hierarchical-query(3)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/45-ex-hierarchical-query(3).jpg)
*연결 조건에 추가 조건이 붙는 경우*

- `CONNECT BY` 절에서의 추가 조건은 메인 쿼리에서의 대상을 선택하기 위함이 아닌, 연결 조건의 일부라고 해석해야 함

![46-ex-hierarchical-query(4)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/46-ex-hierarchical-query(4).jpg)
*`dept2` 테이블에서 부서의 상하관계를 들여쓰기를 반영해서 출력*

![47-ex-hierarchical-query(5)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/47-ex-hierarchical-query(5).jpg)
*`dep2` 테이블에서 영업 1팀 기준 상위 부서들을 출력*

### 계층형 질의 가상 컬럼, 함수

![48-ex-hierarchical-query-virtual-column-and-virtual-function](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/48-ex-hierarchical-query-virtual-column-and-virtual-function.jpg)

#### 계층형 질의 가상 컬럼

- `LEVEL`
	+ 각 계층(depth) 을 표현
	+ 시작점부터 1

- `CONNECT_BY_ISLEAF`
	+ 최하위 노드(Leaf Node) 여부
		* 1은 참, 0은 거짓

#### 계층형 질의 가상 함수

- `CONNECT_BY_ROOT 컬럼명`
	+ 루트 노드의 해당 컬럼명의 값이 출력
- `SYS_CONNECT_BY_PATH(컬럼, 구분자)`
	+ 이어지는 경로 출력
- `ORDER SIBLINGS BY 컬럼`
	+ 같은 `LEVEL`일 경우 정렬 수행

## PIVOT과 UNPIVOT

### 데이터의 구조

#### Long Data(Tidy data)

![49-long-data](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/49-long-data.jpg)

- 하나의 속성이 하나의 컬럼으로 정의되어 값들이 여러 행으로 쌓이는 구조
- RDBMS의 테이블 설계 방식
- 다른 테이블과의 JOIN 연산이 가능한 구조

#### Wide Data(Cross table)

![50-wide-data](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/50-wide-data.jpg)

- 행과 컬럼에 유의미한 정보 전달을 목적으로 작성하는 교차표
- 하나의 속성 값이 여러 컬럼으로 분리되어 표현
- RDBMS에서 WIDE 형식으로 테이블 설계 시 값이 추가될 때마다 컬럼이 추가돼야 하므로 비효율적임
- 다른 테이블과의 JOIN 연산이 불가능
- 주로 데이터를 요약할 목적으로 사용

### 데이터 구조 변경

#### Pivot

![51-long-to-wide.jpg](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/51-long-to-wide.jpg)
*Long Data → Wide Data* 

#### Unpivot

![52-wide-to-long](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/52-wide-to-long.jpg)
*Wide Data → Long Data*

### PIVOT()

![53-ex-pivot-table](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/53-ex-pivot-table.jpg)

```sql
SELECT *
  FROM 테이블명 또는 서브쿼리
 PIVOT (Value컬럼명 FOR Unstack컬럼명 IN (값1, 값2, 값3));
```

- **교차 표를 만드는 기능**
- **Stack 컬럼, Unstack 컬럼, Value 컬럼의 정의가 중요**
- `FROM` 절에 **Stack, Unstack, Value 컬럼명만 정의** 필요
	+ 필요시 서브 쿼리를 사용하여 필요 컬럼 제한
- `PIVOT` 절에 **Unstack, Value 컬럼명 정의**
	+ **Value 컬럼은 집계 함수를 사용해서 정의**
- **`PIVOT` 절의 `IN` 연산자에 Unstack 컬럼 값을 정의**
- `FROM` 절에 선언된 컬럼 중 `PIVOT` 절에서 선언한 Value 컬럼, Unstack 컬럼을 제외한 모든 컬럼은 Stack 컬럼이 됨

#### 예시

![54-ex-pivot()](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/54-ex-pivot().jpg)
*`emp` 테이블에서 `job`별, `deptno`별 도수(COUNT) 출력*

### UNPIVOT()

![55-ex-unpivot-table](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/55-ex-unpivot-table.jpg)

```sql
 SELECT *
   FROM 테이블명 또는 서브쿼리
UNPIVOT (Value컬럼명 FOR Stack컬럼명 IN (값1, 값2, 값, ...)); 
```

- Wide Data를 Long Data로 변경하는 문법
- Stack 컬럼
	+ 이미 Unstack되어 있는 여러 컬럼을 하나의 컬럼으로 Stack 시 새로 만들 컬럼명
		* 사용자 정의
- Value 커럼
	+ 교차표에서 Value 값을 하나의 컬럼으로 표현하고자 할 때 새로 만들 컬럼명
		* 사용자 정의
- 값n
	+ 실제 Unstack 되어 있는 컬럼명들

#### 예시

![56-ex-unpivot()](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/56-ex-unpivot().jpg)

- `IN` 연산자 뒤의 값은 Unstack 데이터의 컬럼명이 숫자지만, 컬럼명은 문자로 저장되므로 문자열로 전달해야 함

## 정규 표현식

- **문자열의 공통된 규칙을 보다 일반화하여 표현하는 방법**
- 정규 표현식 문자함수 제공
	+ `REGEXP_REPLACE()`, `REGEXP_SUBSTR()`, `REGEXP_INSTR()`, ...

### 정규 표현식 종류

|  정규 표현식  |                  의미                    |
|:------------:|:----------------------------------------:|
|      `\d`    |                  숫자                     |
|      `\D`    |              숫자가 아닌 것               |
|      `\s`    |                  공백                     |
|      `\S`    |              공백이 아닌 것               |
|      `\w`    |     단어(`_`를 제외한 모든 숫자, 문자)     |
|      `\W`    |              단어가 아닌 것               |
|      `\t`    |                  Tab                     |
|      `\n`    |                  개행                    |
|      `^`     |               시작되는 글자               |
|      `$`     |               마지막 글자                 |
|      `\`     |   Escape Character(뒤에 기호 의미 제거)    |
|       `|`    |                   또는                    |
|      `.`     |           개행을 제외한 모든 글자          |
|     `[ab]`   |             a 또는 b의 한 글자             |
|    `[^ab]`   |          a와 b를 제외한 모든 문자          |
|    `[0-9]`   |                   숫자                    |
|    `[A-Z]`   |                영어 대문자                 |
|    `[a-z]`   |                영어 소문자                 |
|    `[A-z]`   |                모든 영문자                 |
|      `i+`    |              i가 1회 이상 반복             |
|      `i*`    |              i가 0회 이상 반복             |
|      `u?`    |              0회에서 1회 반복              |
|     `i{n}`   |                i가 n회 반복                |
|  `i{n1, n2}` |             i가 n1에서 n2회 반복           |
|    `i{n,}`   |              i가 n회 이상 반복             |
|      `()`    |                  그룹 지정                 |
|      `\n`    |                  그룹 번호                 |
|  `[:alnum:]` |              알파벳 문자와 숫자             |
|  `[:alpha:]` |                    문자                    |
|  `[:blank:]` |                    공백                    |
|  `[:cntrl:]` |                  제어 문자                 |
|  `[:digit:]` |                   Digits                   |
|  `[:graph:]` | Graphical Characters(공백이 아닌 모든 문자) |
|  `[:lower:]` |                    소문자                  |
|  `[:print:]` |        모든 숫자, 문자, 특수문자, 공백      |
|  `[:punct:]` |                  구두점 문자               |
|  `[:space:]` |                   공백 문자                |
|  `[:upper:]` |                    대문자                  |
| `[:xdigit:]` |                   16 진수                  |

### 예시

![57-ex-regexp](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/57-ex-regexp.jpg)
*전화번호의 일반화*

- 전화번호는 숫자와 하이픈(`-`)으로 구성
	+ `[0-9-]+`로 표현 가능
		* `[]` 안에 들어가는 패턴은 한 자리의 문자열을 구성할 수 있는 값들
- 두 전화번호가 `tel` 값은 동시에 잇지만, `)`가 있는 경우와 없는 경우를 모두 표현
	+ `?` 사용
		* `?`는 값이 없거나 1개 있음을 의미

### REGEXP_REPLACE()

```sql
REGEXP_REPLACE(대상, 찾을문자열, [바꿀문자열], [검색위치], [발견횟수], [옵션])
```

- 정규식 표현을 사용한 문자열 치환 가능
- `바꿀문자열` 생략 시 문자열 삭제
- `검색위치` 생략 시 1
- `발견횟수` 생략 시 0(모든)
- `옵션`
	+ `c`: 대소를 구분하여 검색
	+ `i`: 대소를 구분하지 않고 검색
	+ `m`: 패턴을 다중 라인으로 선언 가능

#### 예시

![58-ex-regexp_replace()(1)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/58-ex-regexp_replace()(1).jpg)
*`id`에서 숫자 삭제*

- 빈 문자열을 전달하여 숫자를 모두 삭제 처리

![59-ex-regexp_replace()(2)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/59-ex-regexp_replace()(2).jpg)
*`id`에서 특수기호 삭제*

- `\w`는 문자와 숫자, `_`를 포함
- `\W`는 `\w`의 반대 집합이므로 문자와 숫자, `_`가 아닌 특수기호와 공백을 의미

![60-ex-regexp_replace()(3)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/60-ex-regexp_replace()(3).jpg)
*`professor` 테이블의 `id`에[서 문자와 문자 바로 뒤에 오는 숫자를 삭제(대소문자 구분 X)*

![61-ex-regexp_replace()(4)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/61-ex-regexp_replace()(4).jpg)
*`kong-12`에서 `g-1`을 지우는 방법*

![62-ex-regexp_replace()(5)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/62-ex-regexp_replace()(5).jpg)
*`product` 테이블의 상품명에서 괄호를 포함해서 괄호 안에 들어가는 모든 글자 삭제*

- 괄호는 서브 그룹을 만드는 정규 표현식이므로 일반 괄호를 표현하기 위해서는 `\)`로 전달해야 함
- `\(.+\)`
	+ `()` 안에 개행을 제외한 모든 값 허용

![63-ex-regexp_replace()(6)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/63-ex-regexp_replace()(6).jpg)
*`student` 테이블의 이름에서 두 번째 발견된 문자 값을 `X`로 치환*

- 문자 삭제 시 찾고자 하는 시작 위치와 발견 횟수를 전달할 수 있음
- `RESULT3`
	+ 처음부터 스캔하여 두 번째로 발견되는 문자를 `X`로 치환
		* 마스킹 처리

### REGEXP_SUBSTR()

```sql
REGEXP_SUBSTR(대상, 패턴, [검색위치], [발견횟수], [옵션], [추출그룹])
```

- 정규 표현식을 사용한 문자열 추출
- 옵션은 `REGEXP_SUBSTR()` 함수와 동일
- `검색위치` 생략 시 1
- `발견횟수` 생략 시 1
- `추출그룹`은 서브 패턴을 추출 시 그 중 추출할 서브 패턴 번호
	+ **`추출그룹`을 사용하려면 `추출그룹` 앞의 모든 인자가 전달되야 함**

#### 예시

![64-ex-regexp_substr()(1)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/64-ex-regexp_substr()(1).jpg)
*전화번호를 분리하여 지역번호 추출*

- `추출그룹`을 사용하기 위해 `옵션` 인자에 `NULL`을 전달

![65-ex-regexp_substr()(2)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/65-ex-regexp_substr()(2).jpg)
*서브 패턴을 활용해서 이메일 아이디 추출*

- `추출그룹`을 사용하기 위해 `옵션` 인자에 `NULL`을 전달

### REGEXP_INSTR()

```sql
REGEXP_INSTR(원본, 찾을문자열, [시작위치], [발견횟수])
```

- 주어진 문자열에서 특정 패턴의 시작 위치를 반환
- `시작위치` 생략 시 처음부터 확인
	+ defualt = 1
- `발견횟수` 생략 시 처음 발견된 문자열 위치 반환
- `옵션` 전달 불가

#### 예시

![66-ex-regexp_instr()(1)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/66-ex-regexp_instr()(1).jpg)
*`id` 값에서 두 번째 발견된 숫자 위치*

- `\d`는 숫자를 나타내는 표현이고, 뒤에 횟수를 지정하지 않으면 한 자리수의 숫자를 의미함

![67-ex-regexp_instr()(2)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/67-ex-regexp_instr()(2).jpg)
*정규 표현식을 사용한 패턴에 일치하는 n번째 문자열 위치 출력*

- 다음과 같은 문자열에서 공백이 아닌 문자열의 반복들 중 처음부터 스캔하여 두 번째 발견된 것의 위치 반환

### REGEXP_LIKE()

```sql
REGEXP_LIKE(원본, 찾을문자열, [옵션])
```

- 주어진 문자열에서 특정 패턴을 갖는 경우 반환
	+ `WHERE` 절에서만 사용 가능
- `시작위치`, `추출개수` 인자 없음
- `옵션`
	+ `REGEXP_REPLACE()` 함수와 동일

#### 예시

![68-ex-regexp_like()](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/68-ex-regexp_like().jpg)
*`id` 값이 숫자로 끝나는 교수 정보 출력*

### REGEXP_COUNT()

```sql
REGEXP_COUNT(원본, 찾을문자열, 시작위치, [옵션])
```

- 주어진 문자열에서 특정 패턴의 횟수를 반환
- `옵션`
	+ `REGEXP_REPLACE()` 함수와 동일

#### 예시

![69-ex-regexp_count()](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/sql-utilization/69-ex-regexp_count().jpg)
*`id` 값이 숫자로 끝나는 교수 정보 출력*

- `\d`는 한 자리수의 숫자를 의미하고, `\d+`는 연속적인 숫자를 의미하므로 COUNT 시 연속적인 숫자를 하나로 취급함

## 다음 글

- [[SQLD \| 2과목]SQL 기본 및 활용 - 관리 구문](https://drj9812.github.io/posts/administration-statements){: target="_blank" }

## 참고자료

- [홍은혜, "SQLD 2과목 PART2. SQL 활용 완벽 정리(2024 신유형 반영)", 홍쌤의 데이터랩, 2024-02-20](https://www.youtube.com/watch?v=EXx6fjxycSY&list=PLbflMVhwy2jPIAzArCK90mqFlTtndFigS&index=3){: target="_blank" }
- 한국데이터산업진흥원, *SQL 자격검정 실전문제*(서울: 한국데이터산업진흥원, 2016), 279.