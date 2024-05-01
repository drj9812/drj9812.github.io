---
title: "[SQLD | 2과목]SQL 기본 및 활용 - SQL 활용(2024)"
categories: [Study, Certification]
tags: [certification, 자격증, SQLD]
image:
  path: /assets/img/posts/study/certification/sqld/01-sqld-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
---

|     구분     |               시험과목            |                         과목별 세부 내용                  |  문항수  |            배점          |        시험기간       |
|:-------------:|:--------------------------------:|:-------------------------------------------------------:|:----------:|:-----------------------:|:----------------------:|
|    1과목     |     데이터 모델링의 이해    |      데이터 모델링의 이해, 데이터 모델과 SQL     |     10    |   20점(문항별 2점)  |  90분(1시간 30분)  |
|  **2과목**  |     **SQL 기본 및 활용**    |  SQL 기본, **SQL 활용**, SQL 최적화 기본원리   |     40    |  100점(문항별 2점)  |  90분(1시간 30분)  |

# SQL 기본 및 활용 - SQL 활용(2024)

|     주요 항목    |             세부 항목               |
|:------------------:|:----------------------------------:|
|      SQL 기본    |   관계형 데이터베이스 개요   |
|                       |           `SELECT` 문              |
|                       |                함수                  |
|                       |           `WHERE` 절              |
|                       |  `GROUBP BY`, `HAVING` 절  |
|                       |              `JOIN`                  |
|                       |           표준 `JOIN`              |
|   **SQL 활용**  |             서브 쿼리              |
|                       |            집합 연산자            |
|                       |              그룹 함수             |
|                       |            윈도우 함수            |
|                       |            TOP N 쿼리             |
|                       |  계층형 질의와 셀프 `JOIN`    |
|                       |  `PIVOT` 절과 `UNPIVOT` 절   |
|                       |            정규 표현식             |
|     관리 구문     |                 DML                  |
|                       |                  TCL                  |
|                       |                 DDL                   |
|                       |                 DCL                   |

## 서브쿼리

- **하나의 SQL 문 안에 포함되어 있는 또다른 SQL 문을 말함**
- 반드시 **괄호로 묶어야 함**

### 서브쿼리 사용 가능한 곳

- `SELECT` 절
- `FROM` 절
- `WHERE` 절
- `HAVING` 절
- `ORDER BY` 절
- 기타 DML(`INSERT`, `DELETE`, `UPDATE`) 절

> 서브쿼리는 `GROUP BY` 절에서 사용할 수 없다.
{: .prompt-warning }

### 서브쿼리의 종류

#### 동작하는 방식에 따라

##### UN-CORRELATED(비연관) 서브쿼리

- 서브쿼리가 메인쿼리 컬럼을 가지고 있지 않은 형태의 서브쿼리
- 서브쿼리가 실행된 결과 값을 메인 쿼리에 제공하기 위한 목적으로 사용

##### CORRELATED(연관) 서브쿼리

- 서브쿼리가 메인쿼리 컬럼을 가지고 있는 형태의 서브쿼리
- 일반적으로 메인쿼리가 먼저 수행된 후, 서브쿼리에서 조건이 맞는지 확인하고자 할 때 사용

#### 위치에 따라

##### 스칼라 서브쿼리

![01-ex-scalar-subquery](/assets/img/posts/study/certification/sqld/chapter2-sql-basic-and-utilization/sql-utilization/01-ex-scalar-subquery.jpg)

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
- `JOIN`의 대체 연산
- 성능이 좋지 않기 때문에 잘 사용되지 않음

#### 인라인 뷰(Inline View)

![02-ex-inline-view](/assets/img/posts/study/certification/sqld/chapter2-sql-basic-and-utilization/sql-utilization/02-ex-inline-view.jpg)

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

![03-ex-where-subquery](/assets/img/posts/study/certification/sqld/chapter2-sql-basic-and-utilization/sql-utilization/03-ex-where-subquery.jpg)

- **서브쿼리 결과로 1개의 행이 반환**되는 형태

###### 단일행 서브쿼리 연산자

|  연산자  |       의미       |
|:----------:|:----------------:|
|      =     |       같다       |
|     <>    |    같지 않다   |
|      >     |       크다       |
|     >=    |  크거나 같다  |
|      <     |       작다       |
|     <=    |  작거나 같다  |

###### 다중행 서브쿼리

![04-ex-where-multiple-subquery](/assets/img/posts/study/certification/sqld/chapter2-sql-basic-and-utilization/sql-utilization/04-ex-where-multiple-subquery.jpg)

- **서브쿼리 결과로 여러 행이 반환되는 형태**
- `=`, `>`, `<`와 같은 비교 연산자 사용 불가능
	+ 여러 값이랑 비교할 수 없는 연산자들 사용 불가능
- 서브쿼리 결과를 하나로 요약하거나 다중행 서브쿼리 연산자를 사용

###### 다중행 서브쿼리 연산자

|   연산자  |        의미          |
|:-----------:|:-------------------:|
|      IN     |  같은 값을 찾음  |
|  > ANY   |  최솟값을 반환   |
|  < ANY   |  최댓값을 반환   |
|  < ALL    |  최솟값을 반환   |
|  > ALL    |  최댓값을 반환   |

###### 다중컬럼 서브쿼리

![05-ex-where-mulity-column-subquery](/assets/img/posts/study/certification/sqld/chapter2-sql-basic-and-utilization/sql-utilization/05-ex-where-mulity-column-subquery.jpg)

- **서브쿼리 결과로 여러 컬럼이 반환되는 형태**
- 메인 쿼리와의 비교 컬럼이 2개 이상
- 대소 비교 전달 불가능
	+ 두 값을 동시에 묶어 대소를 비교할 수 없음

###### 상호 연관 서브쿼리

![06-ex-correlated-subquery](/assets/img/posts/study/certification/sqld/chapter2-sql-basic-and-utilization/sql-utilization/06-ex-correlated-subquery.jpg)

- **메인 쿼리와 서브쿼리의 비교를 수행하는 형태**
- 비교할 집단이나 조건은 서브쿼리에 명시
	+ 메인 쿼리 절에는 서브쿼리 컬럼이 정의되지 않있기 때문에 에러가 발생함

### 서브쿼리 주의사항

- 특별한 경우(TOP-N 분석 등)을 제외하고는 서브쿼리 절에 `ORDER BY` 절을 사용할 수 없음
- 단일행 서브쿼리와 다중행 서브쿼리에 따라 연산자의 선택이 중요함

## 집합 연산자






## 그룹 함수






## 윈도우 함수





## TOP N QUERY






## 계층형 질의






## PIVOT과 UNPIVOT






## 정규 표현식






## 다음 글

- [[SQLD \| 2과목]SQL 기본 및 활용 - SQL 활용](https://drj9812.github.io/posts/sql-utilization){: target="_blank" }

## 참고자료

- [홍은혜, "SQLD 2과목 PART2. SQL 활용 완벽 정리(2024 신유형 반영)", 홍쌤의 데이터랩, 2024-02-14](https://www.youtube.com/watch?v=EXx6fjxycSY&list=PLbflMVhwy2jPIAzArCK90mqFlTtndFigS&index=3){: target="_blank" }
- 한국데이터산업진흥원, SQL 자격검정 실전문제(서울: 한국데이터산업진흥원, 2016), 279.
- [yunamom, Study with yuna](https://yunamom.tistory.com/){: target="_blank" }