---
title: "[SQLD | 2과목]SQL 기본 및 활용 - 관리 구문(2024)"
categories: [Study, Certification]
tags: [certification, 자격증, SQLD]
image:
  path: /assets/img/posts/study/certification/sqld/01-sqld-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
---

|     구분     |               시험과목            |                    과목별 세부 내용                 |  문항수  |           배점          |  과락 기준  |        검정시간       |
|:-------------:|:--------------------------------:|:--------------------------------------------------:|:----------:|:---------------------:|:--------------:|:----------------------:|
|    1과목     |     데이터 모델링의 이해    |  데이터 모델링의 이해, 데이터 모델과 SQL  |     10     |  20점(문항별 2점)  |   8점 미만   |                           |
|  **2과목**  |     **SQL 기본 및 활용**    |        SQL 기본, SQL 활용, **관리 구문**      |     40     |  80점(문항별 2점)  |  32점 미만  |                           |
|      계        |                                      |                                                            |     50     |         100점          |                 |  90분(1시간 30분)  |

# SQL 기본 및 활용 - 관리 구문(2024)

|     주요 항목    |             세부 항목               |
|:------------------:|:----------------------------------:|
|      SQL 기본    |   관계형 데이터베이스 개요   |
|                       |           `SELECT` 문              |
|                       |                함수                  |
|                       |           `WHERE` 절              |
|                       |  `GROUBP BY`, `HAVING` 절  |
|                       |               `JOIN`                  |
|                       |            표준 `JOIN`              |
|      SQL 활용     |              서브 쿼리              |
|                       |             집합 연산자            |
|                       |               그룹 함수             |
|                       |             윈도우 함수            |
|                       |             TOP N 쿼리            |
|                       |   계층형 질의와 셀프 `JOIN`   |
|                       |  `PIVOT` 절과 `UNPIVOT` 절   |
|                       |            정규 표현식             |
|  **관리 구문**   |                 DML                 |
|                       |                  TCL                  |
|                       |                 DDL                  |
|                       |                 DCL                  |

## DML(Data Manipulation Language)

- 데이터의 삽입(INSERT), 수정(UPDATE), 삭제(DELETE), 병합(MERGE)
- <font color="red">저장(commit) 혹은 취소(rollback) 반드시 필요</font>

### INSERT

```sql
INSERT INTO 테이블 VALUES(값1, 값2, ...);
```

```sql
INSERT INTO 테이블(컬럼1, 컬럼2, ...) VALUES(값1, 값2, ...);
```

- 테이블에 행을 삽입할 때 사용
- **한번에 한 행만 입력 가능**
	+ SQL Server에서는 여러 행 동시 삽입 가능
- 하나의 컬럼에는 하나의 값만 삽입 가능
- **컬럼별 데이터 타입과 사이즈에 맞게 삽입**
- `INTO` 절에 컬럼명을 명시하여 일부 컬럼만 입력 가능
	+ 작성하지 않은 컬럼은 `NULL`이 입력됨
		* `NOT NULL` 제약 조건이 있는 컬럼의 경우 오류 발생
- 전체 컬럼에 대한 데이터 입력시 테이블명 뒤의 컬럼명 생략 가능

#### 예시

![01-merge_old-table(1)](/assets/img/posts/study/certification/sqld/chapter2-sql-basic-and-utilization/administration-statements/01-merge_old-table(1).jpg)
*`merge_old` 테이블*

![02-ex-insert(1)](/assets/img/posts/study/certification/sqld/chapter2-sql-basic-and-utilization/administration-statements/02-ex-insert(1).jpg)
*여러 행을 한 행씩 `INSERT`*

- 테이블의 각 컬럼별 데이터 타입과 사이즈에 맞게 입력
- 문자 컬럼에 숫자값 입력 가능
	+ 권장되지 않음
- 숫자 컬럼에 "001"과 같은 문자값 입력 가능
	+ 권장되지 않음

![03-ex-result-insert](/assets/img/posts/study/certification/sqld/chapter2-sql-basic-and-utilization/administration-statements/03-ex-result-insert.jpg)
*여러 행을 한 행씩 `INSERT`한 결과*

![04-ex-insert(2)](/assets/img/posts/study/certification/sqld/chapter2-sql-basic-and-utilization/administration-statements/04-ex-insert(2).jpg)
*서브 쿼리를 사용한 여러 행 `INSERT`*

### UPDATE

```sql
UPDATE 테이블명
   SET 수정할컬럼명 = 수정값
 WHERE 조건;
```

```sql
UPDATE 테이블명
   SET 수정할컬럼명1 = 수정값1, 수정할 컬럼명2 = 수정값2, ...
 WHERE 조건;
```

- 데이터를 수정할 때 사용
	+ `WHERE` 절로 수정 대상 선택 가능
- 컬럼 단위 수행
- 다중 컬럼 수정 가능

#### 예시

![05-ex-update(1)](/assets/img/posts/study/certification/sqld/chapter2-sql-basic-and-utilization/administration-statements/05-ex-update(1).jpg)
*americano의 `price`를 1500으로 변경*

![06-ex-update(2)](/assets/img/posts/study/certification/sqld/chapter2-sql-basic-and-utilization/administration-statements/06-ex-update(2).jpg)
*3번의 `name`을 hot_milk로, `price`를 2500으로 변경*

![07-ex-update(3)](/assets/img/posts/study/certification/sqld/chapter2-sql-basic-and-utilization/administration-statements/07-ex-update(3).jpg)
*서브 쿼리를 사용해서 여러 컬럼 동시 수정*

### DELETE

```sql
DELETE[FROM] 테이블명
[WHERE 조건];
```

- 데이터를 삭제할 때 사용
	+ `WHERE` 절로 수정 대상 선택 가능
		* `WHERE` 절이 없으면 모든 행의 데이터가 삭제됨
- 행 단위 실행

#### 예시

![08-ex-delete](/assets/img/posts/study/certification/sqld/chapter2-sql-basic-and-utilization/administration-statements/08-ex-delete.jpg)
*`no`가 3인 행 삭제*

### MERGE

```sql
MERGE INTO 테이블명
USING 참조테이블
   ON (연결조건)
  WHEN MATCHED THEN
       UPDATE
          SET 수정할내용
  WHEN NOT MATCHED THEN
       INSERT VALUES(삽입할내용)
```

- 데이터 병합
- 참조 테이블과 동일하게 맞추는 작업
	+ 참조 테이블에만 있는 데이터에 대해서는 데이터가 입력됨
	+ 참조 테이블에도 있는 데이터에 대해서는 데이터가 수정됨
- **`INSERT`, `UPDATE`, `DELETE` 작업을 동시에 수행**

#### 예시

![09-merge_new-table](/assets/img/posts/study/certification/sqld/chapter2-sql-basic-and-utilization/administration-statements/09-merge_new-table.jpg)
*`merge_new` 테이블*

![10-merge_old-table(2)](/assets/img/posts/study/certification/sqld/chapter2-sql-basic-and-utilization/administration-statements/10-merge_old-table(2).jpg)
*`merge_old` 테이블*

![11-ex-merge](/assets/img/posts/study/certification/sqld/chapter2-sql-basic-and-utilization/administration-statements/11-ex-merge.jpg)
*`merge_old` 테이블을 `merge_new` 테이블에 병합*

- 수정할 테이블명을 `MERGE INTO` 절에 명시
	+ 참조 테이블을 `USING` 절에 명시
- 두 테이블의 데이터를 참조할 참조 조건을 `ON` 절에 명시
	+ 괄호 필수
- `UPDATE` 문에서는 테이블명을 명시하지 않음
	+ 이미 `MERGE INTO`절에 수정할 테이블명이 명시되어 있으므로
- `SET` 절의 왼쪽이 수정 테이블, 오른쪽이 참조 테이블 컬럼
- `INSERT` 문에는 `INTO`절 없이 `VALUES`로 참조 컬럼명 전달
	+ 이미 `USING` 절에 참조할 테이블명이 명시되어 있으므로

![12-ex-result-merge](/assets/img/posts/study/certification/sqld/chapter2-sql-basic-and-utilization/administration-statements/12-ex-result-merge.jpg)
*`merge_old` 테이블을 `merge_new` 테이블에 병합*

## TCL(Transaction Control Language)

- 트랜젝션 제어어로 `COMMIT`, `ROLLBACK`이 포함됨
- **DML에 의해 조작된 결과를 작언 단위(트랜젝션) 별로 제어하는 명령어**
- DML 수행 후 트랜젝션을 정상 종료하지 않는 경우 `LOCK`이 발생할 수 있음

### 잠금(LOCK)

- 트랜젝션이 수행하는 동안 **특정 데이터에 대해서 다른 트랜젝션이 동시에 접근하지 못하도록 제한**
- 잠금이 걸린 데이터는 잠금을 실행한 트랜젝션만이 접근 및 해제 가능
	+ 관리자 권한 계정 제외

### 트랜젝션

- 트랜젝션은 DB의 **논리적 연산 단위**
	+ 하나의 연속적인 업무 단위
- 하나의 트랜젝션에는 하나 이상의 SQL 문장이 포함됨
- 분할할 수 없는 최소 단위
- ALL OR NOTHING 개념
	+ 모두 `COMMIT` 하거나 `ROLLBACK` 처리해야 함

#### 트랜젝션의 특징

- 원자성(Atomicity)
	+ 트랜젝션 내에서 정의된 연산들 모두 성공적으로 실행되던지 아니면 전혀 실행되지 않은 채로 남아있어야 함
- 일관성(Consistency)
	+ 트랜젝션 실행 전 DB 내용이 잘못되지 않았다면 트랜젝션 실행 이후에도 DB 내용이 잘못되지 않아야 함
- 고립성(Isolation)
	+ 트랜젝션 실행 도중 다른 트랜젝션의 영향을 받아 잘못된 결과를 만들어서는 안됨
- 지속성
	+ 트랜젝션이 성공적으로 수행되면 갱신한 DB 내용이 영구적으로 저장됨

### COMMIT

- 입력, 수정, 삭제한 데이터에 이상이 없을 경우 데이터를 저장하는 명령어
- 한번 `COMMIT`을 수행하면 **`COMMIT` 이전에 수행된 DML은 모두 저장되며 되돌릴 수 없음**
- 오라클은 DDL 시 AUTO `COMMIT`이지만 SQL Server는 AUTO `COMMIT` 기능을 비활성화할 수 있음
	+ 23c 버전부터 오라클도 비활성화할 수 있음

### ROLLBACK

- 테이블 내 입력한 데이터나 수정한 데이터, 삭제한 데이터에 대해 변경을 취소하는 명령어
- DB에 저장되지 않고 **최종 `COMMIT` 지점/변경 전/특정 `SAVEPOINT` 지점으로 원복됨**
- **최종 `COMMIT` 시점 이전까지 `ROLLBACK` 가능**
- `SAVEPOINT`를 설정하여 최종 `COMMIT` 시점이 아닌, 그 이후의 원하는 시점으로의 원복 가능

#### SAVEPOINT

```sql
SAVEPOINT savepoint_name;
```

- 트랜젝션 내에서 롤백을 부분적으로 수행하기 위해 사용되는 지점을 지정하는 데 사용
- 사용자가 원하는 위치에 원하는 이름으로 설정 가능
- `ROLLBACK TO savepoint_name` 명령어로 원하는 지점으로 원복 가능
	+ `COMMIT` 이전으로는 원복 불가

##### 예시

![13-rollback_test-table](/assets/img/posts/study/certification/sqld/chapter2-sql-basic-and-utilization/administration-statements/13-rollback_test-table.jpg)
*`rollback_test` 테이블*

![14-ex-rollback](/assets/img/posts/study/certification/sqld/chapter2-sql-basic-and-utilization/administration-statements/14-ex-rollback.jpg)
*여러 DML 수행*

![15-ex-result-rollback.](/assets/img/posts/study/certification/sqld/chapter2-sql-basic-and-utilization/administration-statements/15-ex-result-rollback.jpg)
*결과*

- `SAVEPOINT` 이전에 수행한 `UDPATE`는 취소되지 않음

## DDL(Data Definition Language)

## DCL(Data Control Language)

## 참고자료

- [홍은혜, "SQLD 2과목 PART3. 관리 구문 완벽정리", 홍쌤의 데이터랩, 2024-02-25](https://www.youtube.com/watch?v=ijpxmi4DPj4&list=PLbflMVhwy2jPIAzArCK90mqFlTtndFigS&index=4){: target="_blank" }
- 한국데이터산업진흥원, SQL 자격검정 실전문제(서울: 한국데이터산업진흥원, 2016), 279.
- [yunamom, Study with yuna](https://yunamom.tistory.com/){: target="_blank" }