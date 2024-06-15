---
title: "[SQLD | 2과목]SQL 기본 및 활용 - 관리 구문(2024)"
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
| **2과목** | **SQL 기본 및 활용** |    SQL 기본, SQL 활용, **관리 구문**   |   40    | 80점(문항별 2점) |  32점 미만 |                 |
|     계    |                     |                                       |   50    |      100점       |           | 90분(1시간 30분) |

# SQL 기본 및 활용 - 관리 구문(2024)

|  주요 항목   |          세부 항목        |
|:------------:|:------------------------:|
|   SQL 기본   |  관계형 데이터베이스 개요  |
|              |        `SELECT` 문       |
|              |            함수          |
|              |         `WHERE` 절       |
|              | `GROUBP BY`, `HAVING` 절 |
|              |            JOIN          |
|              |          표준 JOIN       |
|   SQL 활용   |          서브 쿼리        |
|              |         집합 연산자       |
|              |          그룹 함수        |
|              |         윈도우 함수       |
|              |         TOP N 쿼리        |
|              |   계층형 질의와 셀프 JOIN  |
|              | `PIVOT` 절과 `UNPIVOT` 절 |
|              |         정규 표현식       |
| **관리 구문** |             DML           |
|              |             TCL           |
|              |             DDL           |
|              |             DCL           |

## DML(Data Manipulation Language)

- 비절차적 데이터 조작어로서 사용자가 무슨 데이터를 원하는 지 명세
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

![01-merge_old-table(1)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/01-merge_old-table(1).jpg)
*`merge_old` 테이블*

![02-ex-insert(1)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/02-ex-insert(1).jpg)
*여러 행을 한 행씩 `INSERT`*

- 테이블의 각 컬럼별 데이터 타입과 사이즈에 맞게 입력
- 문자 컬럼에 숫자값 입력 가능
	+ 권장되지 않음
- 숫자 컬럼에 '001'과 같은 문자값 입력 가능
	+ 권장되지 않음

![03-ex-result-insert](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/03-ex-result-insert.jpg)
*여러 행을 한 행씩 `INSERT`한 결과*

![04-ex-insert(2)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/04-ex-insert(2).jpg)
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
		* **`WHERE` 절을 명시하지 않으면 수정하려는 모든 컬럼의 데이터가 업데이트되므로 주의**
- 컬럼 단위 수행
- 다중 컬럼 수정 가능

#### 예시

![05-ex-update(1)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/05-ex-update(1).jpg)
*americano의 `price`를 1500으로 변경*

![06-ex-update(2)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/06-ex-update(2).jpg)
*3번의 `name`을 hot_milk로, `price`를 2500으로 변경*

![07-ex-update(3)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/07-ex-update(3).jpg)
*서브 쿼리를 사용해서 여러 컬럼 동시 수정*

### DELETE

```sql
DELETE[FROM] 테이블명
[WHERE 조건];
```

- 데이터를 삭제할 때 사용
	+ `WHERE` 절로 수정 대상 선택 가능
		* **`WHERE` 절이 없으면 모든 행의 데이터가 삭제됨**
- 행 단위 실행

#### 예시

![08-ex-delete](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/08-ex-delete.jpg)
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

![09-merge_new-table](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/09-merge_new-table.jpg)
*`merge_new` 테이블*

![10-merge_old-table(2)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/10-merge_old-table(2).jpg)
*`merge_old` 테이블*

![11-ex-merge](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/11-ex-merge.jpg)
*`merge_old` 테이블을 `merge_new` 테이블에 병합*

- 수정할 테이블명을 `MERGE INTO` 절에 명시
	+ 참조 테이블을 `USING` 절에 명시
- 두 테이블의 데이터를 참조할 참조 조건을 `ON` 절에 명시
	+ 괄호 필수
- `UPDATE` 문에서는 테이블명을 명시하지 않음
	+ 이미 `MERGE INTO`절에 수정할 테이블명이 명시되어 있으므로
- `SET` 절의 왼쪽이 수정 테이블, 오른쪽이 참조 테이블 컬럼
- `INSERT` 문에는 `INTO`절 없이 `VALUES`로 참조 컬럼명 전달
	+ 이미 `MERGE INTO`절에 입력할 테이블명이 명시되어 있으므로

![12-ex-result-merge](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/12-ex-result-merge.jpg)
*`merge_old` 테이블을 `merge_new` 테이블에 병합*

## TCL(Transaction Control Language)

- 트랜잭션 제어어로 `COMMIT`, `ROLLBACK`이 포함됨
- **DML에 의해 조작된 결과를 작언 단위(트랜잭션) 별로 제어하는 명령어**
- DML 수행 후 트랜잭션을 정상 종료하지 않는 경우 `LOCK`이 발생할 수 있음

### 잠금(LOCK)

- 트랜잭션이 수행하는 동안 **특정 데이터에 대해서 다른 트랜잭션이 동시에 접근하지 못하도록 제한**
- 잠금이 걸린 데이터는 잠금을 실행한 트랜잭션만이 접근 및 해제 가능
	+ 관리자 권한 계정 제외

### 트랜잭션

- 트랜잭션은 DB의 **논리적 연산 단위**
	+ 하나의 연속적인 업무 단위
- 하나의 트랜잭션에는 하나 이상의 SQL 문장이 포함됨
- 분할할 수 없는 최소 단위
- ALL OR NOTHING 개념
	+ 모두 `COMMIT` 하거나 `ROLLBACK` 처리해야 함

#### 트랜잭션의 특징

- **원자성(Atomicity)**
	+ 트랜잭션 내에서 정의된 연산들 **모두 성공적으로 실행되던지 아니면 전혀 실행되지 않은 채로 남아있어야 함**
- **일관성(Consistency)**
	+ 트랜잭션 실행 전 DB 내용이 잘못되지 않았다면 **트랜잭션 실행 이후에도 DB 내용이 잘못되지 않아야 함**
- **고립성(Isolation)**
	+ 트랜잭션 실행 도중 **다른 트랜잭션의 영향을 받아 잘못된 결과를 만들어서는 안됨**
- **지속성(Durability)**
	+ 트랜잭션이 성공적으로 수행되면 갱신한 DB 내용이 **영구적으로 저장**됨

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

- 트랜잭션 내에서 롤백을 부분적으로 수행하기 위해 사용되는 지점을 지정하는 데 사용
- 사용자가 원하는 위치에 원하는 이름으로 설정 가능
- `ROLLBACK TO savepoint_name` 명령어로 원하는 지점으로 원복 가능
	+ `COMMIT` 이전으로는 원복 불가
- SQL Server
    + `SAVEPOINT TRANSACTION savepoint_name;`
        * `ROLLBACK TRANSACTION savepoint_naem;`

##### 예시

![13-rollback_test-table](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/13-rollback_test-table.jpg)
*`rollback_test` 테이블*

![14-ex-rollback](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/14-ex-rollback.jpg)
*여러 DML 수행*

![15-ex-result-rollback.](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/15-ex-result-rollback.jpg)
*결과*

- `SAVEPOINT` 이전에 수행한 `UDPATE`는 취소되지 않음

## DDL(Data Definition Language)

- 비절차적 데이터 조작어로서 사용자가 어떻게 데이터를 접근해야 하는지 명세
- 데이터 정의어
- **데이터 구조 정의 언어**
	+ `CREATE`: 객체 생성
	+ `ALTER`: 객체 변경
	+ `DROP`: 객체 삭제
	+ **`TRUNCATE`**: 데이터 삭제
- 명령어를 실행하면 즉시 저장
	+ 원복 불가
	+ AUTO `COMMIT`
        * SQL Server는 AUTO `COMMIT`을 비활성화할 수 있음
        * 오라클도 23c부터 AUTO COMMIT`을 비활성화할 수 있음

### CREATE

```sql
CREATE TABLE [소유자.]테이블명(
    컬럼1 데이터타입 [DEFAULT 기본값] [제약조건],
    컬럼2 데이터타입 [DEFAULT 기본값] [제약조건],
    ...);
```

- 테이블이나 인덱스와 같은 **객체를 생성하는 명령어**
- 테이블 생성 시 테이블명, 컬럼명, 컬럼 순서, 컬럼 크기, 컬럼의 데이터 타입 정의 필수
- 테이블 생성 시 각 컬럼의 제약 조건 및 기본 값은 생략 가능
- 테이블 생성 시 소유자 명시 가능
	+ 생략 시 명령어 수행 계정 소유
- 숫자 컬럼의 경우 컬럼 사이즈 생략 가능
	+ 날짜 컬럼은 사이즈를 명시하지 않음

#### 테이블 복제(Create Table AS Select)

```sql
CREATE TABLE 테이블명
    AS
SELECT *
  FROM 복제테이블명;
```

- 복제 테이블의 컬럼명과 컬럼의 데이터 타입이 복제됨
- `SELECT` 문에서 컬럼 별칭 사용시 컬럼 별칭 이름으로 생성
- `CREATE` 문에서 컬럼명 변경 가능
- **`NULL` 속성을 제외한 제약 조건, 인덱스 등은 복제되지 않음**
- `WHERE` 절을 사용해서 일부 데이터만 복제할 수 있음

#### 예시

![16-ex-create(1)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/16-ex-create(1).jpg)
*`merge_old` 테이블 생성*

![17-ex-create(2)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/17-ex-create(2).jpg)
*`emp` 테이블을 복제하여 `test` 테이블 생성*

![18-ex-create(3)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/18-ex-create(3).jpg)
*`emp` 테이블의 데이터 없이 구조만 복제*

- 항상 거짓인 조건을 `SELECT` 절에 전달하면, 데이터는 아무것도 출력되지 않지만 컬럼 정보들은 출력됨

![19-ex-create(4)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/19-ex-create(4).jpg)
*테이블 복제 시 컬럼명 변경 가능*

- `SELECT` 절에서 `AS`를 사용하여 컬럼명을 변경할 수도 있음

### 데이터 타입

|   데이터 타입   |                                              설명                                               |
|:--------------:|:-----------------------------------------------------------------------------------------------:|
|    `CHAR(n)`   | 고정형 문자 타입으로 사이즈 전달 필수, 사이즈만큼 확정형 데이터가 입력됨(빈 자리수는 공백으로 채워짐) |
|  `VARCHAR2(n)` |   가변형 문자 타입으로 사이즈 전달 필수, 사이즈보다 작은 문자 값이 입력되더라도 입력 값 그대로 유지   |
| `NUMBER(p, s)` |             숫자형 타입으로 자리수 생략 가능, 소수점 자리 제한 시 s 전달(p는 총 자리수)             |
|     `DATE`     |                                 날짜 타입으로 사이즈 전달 불가                                    |

- SQL Server
	+ `VARCHAR2` → `VARCHAR`
	+ `NUMBER` → `NUMERIC`
	+ 문자 타입도 사이즈 생략 가능
		* 생략 시 1

> 오라클과 달리 SQL Server의 `VARCHAR` 타입의 경우 문자 타입 입력 시 제한된 길이보다 큰 '공백을 포함한 문자열' 값을 입력하면 고정 길이에 맞게 값에서 뒤의 공백을 제거하고 에러없이 입력된다.
{: .prompt-tip }

#### 예시

![20-data-type(1)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/20-data-type(1).jpg)
*NUMBER(7,2)의 경우 총 자리수가 7을 초과할 수 없음*

![21-data-type(2)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/21-data-type(2).jpg)
*`CHAR` 타입의 데이터 입력*

- `col2` 컬럼에 7보다 작은 문자열을 삽입해도 모두 7의 사이즈로 할당됨

![22-data-type(3)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/22-data-type(3).jpg)
*`VARCHAR` 타입의 데이터 입력*

- 컬럼 사이즈와 상관없이 입력된 문자열의 크기 그대로 저장됨

#### CHAR 타입 컬럼의 문자 상수 비교

![23-data-type(4)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/23-data-type(4).jpg)
*`CHAR` 타입끼리 비교 시 뒤 공백 무시, 앞 공백 중요*

- 왼쪽에서부터 서로 다른 문자가 나올 때까지 비교
	+ `CHAR` 타입의 컬럼끼리 서로 비교될 때는 뒤 **공백의 수만 다르고 값이 같다면 같은 것으로 간주**
		* 뒤 공백 무시
		* 'ORACLE'과 'ORACLE '은 같은 값
	+ **앞의 공백은 무시할 수 없음**
		* 'ORACLE'과 ' ORACLE'은 서로 다른 값

![24-data-type(5)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/24-data-type(5).jpg)
*문자값 정렬*

- 달라진 첫 번째 문자 값에 따라 문자의 크기를 결정
	+ 왼쪽부터의 값이 같을 떄까지 체크 후 크기가 더 큰 쪽이 더 큰 값이 됨
		* '가'보다 '가나'가 더 큰 값
	+ 일반적으로 공백 < 특수기호 < 숫자 < 영어 < 한글의 크기 순서
		* DBMS나 캐릭터셋 설정에 따라 다를 수 있음
	+ 대소문자를 구분하는 경우 대문자 < 소문자 크기 순서

#### VARCHAR 타입 컬럼의 문자 상수 비교

![25-data-type(6)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/25-data-type(6).jpg)
*`VARCHAR` 타입끼리의 비교*

- 왼쪽에서부터 서로 다른 문자가 나올 때까지 비교하여 **공백을 포함한 모든 값이 같을 때 같은 값으로 인정**

#### 문자 타입 컬럼의 숫자 상수 비교

![26-data-type(7)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/26-data-type(7).jpg)
*문자 컬럼과 숫자 상수의 비교(이미 문자 컬럼에 숫자로 변경 불가능한 문자가 있는 경우)*

- <font color="red">항상 숫자에 맞춰 형 변환 후 비교</font>
	+ 문자 컬럼에 숫자 변환이 불가능한 문자가 이미 존재하는 경우 비교 불가
	+ 문자 컬럼에 숫자 변환이 가능한 문자만 존재하는 경우 비교 가능

> 오라클에서는 문자 컬럼을 숫자 상수와 비교할 때 내부적으로 비교 컬럼에 `TO_NUMBER()` 함수를 사용해서 숫자로 형변환 후 처리한다.
{: .prompt-info }

#### 숫자 타입 컬럼의 문자 상수(숫자로 변경 가능한) 비교

![27-data-type(8)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/27-data-type(8).jpg)
*숫자 컬럼과 문자 상수의 비교*

- 문자와 숫자 비교 시 숫자에 맞춰 비교하게 되는데, 문자 컬럼이 아닌 문자 상수를 숫자 타입으로 바꾸는 것은 제한이 없으므로 언제나 비교 가능

#### 문자 컬럼에 숫자 상수 입력

![28-data-type(9)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/28-data-type(9).jpg)

- 문자 컬럼에 숫자 상수 입력 가능
- 숫자 컬럼에 숫자처럼 생긴 문자 상수 입력 가능

### ALTER

- 테이블 구조 변경
	+ 컬럼명, 컬럼 데이터 타입, 컬럼 사이즈, `DEFAULT` 값, 컬럼 삭제, 컬럼 추가, 제약 조건
- 컬럼 순서 변경 불가
	+ 재생성으로 해결

#### 컬럼 추가

```sql
ALTER TABLE 테이블명
  ADD 컬럼명 데이터타입 [DEFAULT] [제약조건];
```

- 새로 추가된 컬럼 위치는 맨 마지막에 위치
	+ 절대 중간 위치에 추가 불가
- 컬럼 추가 시 데이터 타입 필수
- 컬럼 추가 시 `DEFAULT` 값, 제약 조건 생략 가능
- **여러 컬럼 동시 추가 가능(반드시 괄호 사용)**
	+ 하나의 컬럼만 추가할 경우 괄호 생략 가능

##### 예시

![29-ex-alter(1)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/29-ex-alter(1).jpg)
*동시에 여러 컬럼을 추가할 경우 반드시 괄호와 함께 전달*

![30-ex-alter(2)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/30-ex-alter(2).jpg)
*컬럼 추가 시 `NOT NULL` 속성 전달 불가*

- 컬럼 추가 시 모두 `NULL`인 값이 추가되므로 `NOT NULL` 속성을 전달할 수 없음

![31-ex-alter(3)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/31-ex-alter(3).jpg)
*컬럼 추가 시 `DEFAULT`를 선언하면 `NOT NULL` 속성 전달 가능*

> `NOT NULL` 속성은 `DEFAULT` 값을 선언한 뒤 명시되야 하므로 순서를 주의한다.
{: .prompt-warning }

#### 컬럼(속성) 변경

```sql
ALTER TABLE 테이블명 MODIFY(컬럼명 DEFAULT 값);
```

- 컬럼 사이즈, 데이터 타입, `DEFAULT` 값 변경 가능
- 여러 컬럼 동시 변경 가능
	+ 괄호 필수

##### 컬럼 사이즈 변경

- 컬럼 사이즈 증가는 항상 가능
- 컬럼 사이즈 축소는 데이터 존재 여부에 따라 제한
	+ 데이터가 있는 경우 데이터의 최대 사이즈만큼 축소 가능
- **동시 변경 가능**
	+ 괄호 필수

##### 컬럼 데이터 타입 변경

- **빈 컬럼일 경우 데이터 타입 변경 가능**
- **`CHAR`, `VARCHAR` 타입일 경우 데이터가 있어도 서로 변경 가능**

##### DEFAULT 값 변경

- `DEFAULT` 값이란 **특정 컬럼에 값이 생략될 경우(입력 시 언급되지 않을 경우) 자동으로 부여되는 값**
- `INSERT` 시 `DEFAULT` 값이 선언된 컬럼에 `NULL`을 직접 입력할 때는 `DEFAULT` 값이 아닌 `NULL`이 입력
- 이미 데이터가 존재하는 테이블에 `DEFAULT` 값 선언 시 **기존 데이터는 수정 안 됨**
	+ 이후 입력된 데이터부터 `DEFAULT` 값 적용
- `DEFAULT` 값 해제 시 `DEFAULT` 값을 `NULL`로 선언

##### 예시

![32-ex-alter(4)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/32-ex-alter(4).jpg)
*여러 컬럼 사이즈 수정*

- 최대 길이보다 크거나 같은 사이즈로 변경 가능

![33-ex-alter(5)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/33-ex-alter(5).jpg)
*컬럼 데이터 타입 변경*

- 데이터 타입이 일치하지 않아도 `NULL`이면 변경 가능
- 데이터 타입이 일치하지 않고, `NULL`이 아니면 변경 불가능

![34-ex-alter(6)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/34-ex-alter(6).jpg)
*`CHAR` ↔ `VARCHAR` 데이터 타입 변경*

![35-ex-alter(7)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/35-ex-alter(7).jpg)
*`DEFAULT` 값이 설정되지 않은 컬럼에 `DEFAULT` 값 설정*

- `INSERT` 시 `sal` 컬럼에 값을 명시하지 않으면 "3000" 입력
- `INSERT` 시 `sal` 컬럼에 `NULL`을 명시하면 `DEFAULT` 값이 있음에도 `NULL` 입력

#### 컬럼 이름 변경

```sql
ALTER TABLE 테이블명 RENAME COLUMN 기존컬럼명 TO 새컬럼명;
```

- 항상 가능
- **동시에 여러 컬럼 이름 변경 불가능**
	+ 괄호 전달 불가능

#### 컬럼 삭제

```sql
ALTER TABLE 테이블명 DROP COLUMN 삭제할컬럼명;
```

- **데이터 존재 여부와 상관없이 언제나 가능**
- `RECYCLEBIN`에 남지 않음
	+ `FLASHBACK`으로 복구 불가능
- **동시 삭제 불가능**

### DROP

```sql
DROP TABLE 테이블명 [PURGE];
```

- 객체(테이블, 인덱스 등) 삭제
- `DROP` 이후에는 조회 불가
- `PURGE`로 테이블 삭제시 영구 삭제
	+ `RECYCLEBIN`에서 조회 불가

### TRUNCATE

```sql
TRUNCATE TABLE 테이블명;
```

- 테이블 구조를 남기고 데이터만 즉시 삭제
	+ AUO `COMMIT`
	+ `TRUNCATE` 이후에 조회 가능
- `RECYCLEBIN`에 남지 않음
	+ 영구 삭제

### DELETE, TRUNCATE, DROP의 차이

|                        DELETE                     |                             TRUNCATE                             |                  DROP                 |
|:-------------------------------------------------:|:----------------------------------------------------------------:|:-------------------------------------:|
|                          DML                      |                       DDL(일부 DML 성격 가짐)                     |                  DDL                  |
|                Commit 이전 Rollback 가능          |                            Rollback 불가능                        |             Rollback 불가능            |
|                     사용자 Commit                 |                             Auto Commit                          |               Auto Commit              |
| 데이터를 삭제해도 사용했던 저장 공간은 해제되지 않음 | 테이블이 사용한 저장 공간 중 최초 할당된 공간만 남기고 나머지는 해제 | 테이블이 사용했던 모든 저장 공간이 해제 |
|                     데이터만 삭제                  |                 테이블을 최초 생성된 초기 상태로 만듦              |     테이블의 정의 자체를 완전히 삭제    |

- `DELETE`
	+ 데이터의 일부 또는 전체 삭제
	+ 디스크 사용량 초기화 X
	+ 롤백 가능
		* 로그 남음
- `TRUNCATE`
	+ 데이터 전체 삭제만 가능
		* 일부 삭제 불가능
	+ 디스크 사용량 초기화
	+ 즉시 반영
		* 롤백 불가능
- `DROP`
	+ 데이터와 구조를 동시 삭제
	+ 디스크 사용량 초기화
	+ 즉시 반영
		* 롤백 불가능

> `TRUNCATE` 문은 `UNDO`를 위한 데이터를 생성하지 않기 때문에 동일한 데이터량 삭제 시 `DELETE` 문보다 빠르다.
{: .prompt-info }

### 제약 조건

- 데이터 무결성을 위해 각 컬럼에 생성하는 데이터의 제약 장치
- 테이블 생성, 컬럼 추가 시 정의 가능
- 이미 생성된 컬럼에 제약 조건만 추가 가능

#### 문법

```sql
CRATEE TABLE [소유자.]테이블명(
    컬럼1 데이터타입 [DEFAULT 기본값] [제약조건],
    컬럼2 데이터타입 [DEFAULT 기본값] [제약조건],
    ....);
```

```sql
CREATE TABLE [소유자.]테이블명(
    컬럼1 데이터타입 [DEFAULT 기본값],
    컬럼2 데이터타입 [DEFAULT 기본값],
    [CONSTRAINT 제약조건명 제약조건종류(제약조건컬럼)];
```

- 테이블 생성 시 제약 조건 생성

```sql
ALTER TABLE 테이블명 ADD 컬럼명 데이터타입 [DEFAULT 기본값] [제약조건];
```

- 컬럼 추가 시 제약 조건 생성

```sql
ALTER TABLE 테이블명 ADD [CONSTRAINT 제약조건명] 제약조건종류(제약조건컬럼);
```

- 이미 생성된 컬럼에 제약조건만 추가

```sql
ALTER TABLE 테이블명 DROP CONSTRAINT 제약조건명;
```

- 제약 조건 삭제

#### Primary Key(기본 키)

- 유일한 식별자
	+ 각 행을 구별할 수 있는 식별자 기능
- 중복, `NULL`을 허용하지 않음
	* `UNIQUE` + `NOT NULL`
- 특정 컬럼에 기본 키를 생성하면 `NOT NULL` 속성이 자동 부여
	+ <font color="red">CTAS(Create Table As Select)로 테이블 복사 시 복사되지 않음</font>
- 하나의 테이블에 여러 기본 키를 생성할 수 없음
- 하나의 기본 키를 여러 컬럼과 결합하여 생성할 수 있음
- 기본 키 생성 시 자동으로 `UNIQUE INDEX` 생성

##### 예시

![36-ex-create-pk(1)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/36-ex-create-pk(1).jpg)
*테이블 생성 시 이름 전달 없이 제약 조건 설정*

- 제약 조건 생성 시 이름을 설정하지 않으면 자동으로 이름이 부여됨

![37-ex-create-pk(2)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/37-ex-create-pk(2).jpg)
*테이블 생성 시 이름을 전달해서 제약 조건 설정*

![38-ex-create-pk(3)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/38-ex-create-pk(3).jpg)
*컬럼 추가 시 제약 조건 생성*

![39-ex-create-pk(4)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/39-ex-create-pk(4).jpg)
*이미 존재하는 컬럼에 제약 조건만 생성*

- `PRIMARY KEY(컬럼명)`으로 생성할 제약 조건의 컬럼명을 명시해야 함
	+ 생성할 컬럼명이 명시되기 때문에 `ADD` 이후에 생성할 컬럼명 생략 가능

#### UNIQUE

![40-ex-unique](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/40-ex-unique.jpg)

- 중복을 허용하지 않음
- **`NULL` 허용**
- `UNIQUE` 인덱스 자동 생성

#### NOT NULL

![41-ex-not-null](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/41-ex-not-null.jpg)

- 다른 제약 조건과 다르게 컬럼의 특징을 나타냄
	+ <font color="red">CTAS(Create Table As Select)로 복제 시 같이 복제됨</font>
- **컬럼 생성 시 `NOT NULL`을 선언하지 않으면 Nullable 컬럼으로 생성됨**
    + `NOT NULL` 제약 조건을 가진 컬럼을 수정할 때 `NOT NULL` 제약 조건을 명시하지 않으면 Nullable 컬럼으로 수정됨
- 이미 만들어진 컬럼에 **`NOT NULL` 선언 시 제약 조건 생성이 아닌 컬럼 수정(`MODIFY`)으로 해결**
- `NOT NULL` 제약 조건의 컬럼을 `NOT NULL` 제약 조건으로 수정할 수 없음
    + 오라클

> 애초에 모든 컬럼은 `NOT NULL` 또는 `NOT NULL` 제약 조건이 아니기 때문에 `NOT NULL` 제약 조건은 추가(`ADD`)가 아닌 수정(`MODIFY`)를 통해 '변경'해야 한다.
{: .prompt-info }

#### FOREIGN KEY

```sql
CREATE TABLE 테이블명(
    컬럼명 데이터타입 [DEFAULT 값] REFERENCES 참조테이블(참조키),
    ....);
```

- **참조 테이블의 참조 컬럼에 있는 데이터를 확인하면서 본 테이블의 데이터를 관리할 목적으로 생성**
- <font color="red">반드시 참조(부모) 테이블의 참조 컬럼(Reference key)이 사전에 Primary Key 또는 Unique Key를 가져야 함</font>
	+ 참조 무결성 제약을 받음
- `NULL` 값을 가질 수 있음
- 하나의 테이블에 여러 FK가 존재할 수 있음

##### FOREIGN KEY 옵션

- `ON DELETE CASCADE`: 부모 데이터 삭제 시 자식 데이터 함께 삭제
- `ON DELETE SET NULL`: 부모 데이터 삭제 시 자식 데이터의 참조 값은 `NULL`

##### 예시

![42-ex-fk(1)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/42-ex-fk(1).jpg)
*부모 테이블(`dept_test1`)에 PK를 설정하고, 자식 테이블(`emp_test1`)에 부모 테이블의 PK를 참조하는 FK 생성*

> 참조(부모) 테이블의 참조 컬럼(Reference key)이 사전에 Primary Key 또는 Unique Key를 가져야 한다.
{: .prompt-warn }

![43-ex-fk(2)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/43-ex-fk(2).jpg)
*자식 테이블(`emp_test1`)에서 10번 부서원 삭제*

- 자식 테이블의 FK는 언제나 삭제 가능

![44-ex-fk(3)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/44-ex-fk(3).jpg)
*자식 테이블(`emp_test1`)에서 20번 부서원을 50번으로 변경 불가능*

- 자식 테이블의 FK 변경은 제약이 있을 수 있음
	+ **부모 테이블에 50번 부서번호가 정의되어 있지 않아 자식 테이블에서 해당 값으로 수정 불가능**

![45-ex-fk(4)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/45-ex-fk(4).jpg)
*자식 테이블(`emp_test1`)에서 50번 부서원 입력 불가능*

- 자식 테이블의 FK 삽입은 제약이 있을 수 있음
	+ **부모 테이블에 50번 부서번호가 정의되어 있지 않아 자식 테이블에서 해당 값으로 입력 불가능**
 
![46-ex-fk(5)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/46-ex-fk(5).jpg)
*부모 테이블(`dept_test1`)에서 10번 부서원 삭제 불가능*

- 부모 테이블의 PK 삭제는 제약이 있을 수 있음
	+ **20번 부서 정보가 자식 테이블에 존재하므로 삭제 불가능**

![47-ex-fk(6)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/47-ex-fk(6).jpg)
*부모 테이블(`dept_test1`)에서 20번 부서원의 부서번호를 60번으로 변경 불가능*

- 부모 테이블의 PK 변경은 제약이 있을 수 있음
	+ **20번 부서 정보가 자식 테이블에 존재하므로 다른 값으로 변경 불가능**

![48-ex-fk-on-delete-cascade(1)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/48-ex-fk-on-delete-cascade(1).jpg)
*자식 테이블(`emp_test1`)에서 `ON DELETE CASCADE` 옵션으로 FK를 생성*

![49-ex-fk-on-delete-cascade(2)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/49-ex-fk-on-delete-cascade(2).jpg)
*부모 데이터 삭제*

- 부모 데이터 삭제 시, 자식 데이터도 함께 삭제됨

![50-ex-fk-on-delete-set-null(1)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/50-ex-fk-on-delete-set-null(1).jpg)
*자식 테이블(`emp_test1`)에서 `ON DELETE SET NULL` 옵션으로 FK를 생성*

![51-ex-fk-on-delete-set-null(2)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/51-ex-fk-on-delete-set-null(2).jpg)
*부모 데이터 삭제*

- 자식 테이블의 데이터도 함께 삭제되지 않음
	+ `NULL`로 변경됨

#### CHECK

![52-ex-check](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/52-ex-check.jpg)
*`emp_test1` 테이블의 `sal` 값은 0 이상이어야 한다는 `CHECK` 제약 조건 추가*

- 데이터의 무결성을 유지하기 위하여 테이블의 특정 컬럼을 설정하는 제약
- 직접적으로 데이터의 값을 제한

### 기타 오브젝트

#### 뷰(View)

- 저장 공간을 가지지는 않지만 테이블처럼 조회 및 수정할 수 있는 객체

##### 뷰의 종류

- 단순 뷰: 하나의 테이블을 조회하는 뷰
- 복합 뷰: 둘 이상의 테이블을 JOIN하는 뷰

##### 뷰의 특징

- 뷰는 기본 테이블로부터 유도된 테이블이기에 기본 테이블과 같은 형태의 구조를 가지고 있으며, 조작도 기본 테이블과 거의 같음
- 뷰는 가상의 테이블이기에 물리적으로 구현되어 있지 않으며, 저장 공간을 차지하지 않음
	+ 데이터가 아닌 객체로서 저장됨
- 데이터를 안전하게 보호할 수 있음
- 이미 정의되어 있는 뷰는 다른 뷰의 정의에 기초가 될 수 있음
- **기본 테이블이 변경 또는 삭제되면 그 테이블을 참조하여 만든 뷰 역시 변경 또는 삭제됨**
- 실제 데이터를 저장하고 있는 뷰를 생성하는 기능을 지원하는 DBMS도 있음

##### 뷰의 장점

- 논리적 독립성을 제공
- 데이터의 접근을 제어함으로써 보안 유지
- 사용자의 데이터 관리 단순화
- 데이터의 다양한 지원 가능

##### 뷰의 단점

- 뷰의 정의 변경 불가
- 삽입, 삭제, 갱신 연산에 재한
- 인덱스 구성 불가능

##### 문법

```sql
CREATE [OR REPLACE] VIEW 뷰명
   AS 조회쿼리;
```

- 뷰의 생성
- `OR REPLACE`
	+ 생성된 뷰를 삭제하지 않고 연동된 쿼리를 변경하여 대체

```sql
DROP VIEW 뷰명;
```

- 뷰의 삭제

##### 예시

![53-ex-view](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/53-ex-view.jpg)
*`emp` 테이블과 `dept` 테이블을 JOIN하는 복합 뷰의 생성 및 조회*

#### 시퀀스(SEQUENCE)

```sql
   CREATE SEQEUENCE 시퀀스명
INCREMENT BY        -- 증가값(DEFAULT = 1)
    START WITH	    -- 시작값(DEFAULT = 1)
 MAXVALUE           -- 마지막값(증가시퀀스), 재사용시 시작값(감소시퀀스)
 MINVALUE           --재사용시 시작값(증가시퀀스), 마지막값(감소시퀀스)
    CYCLE | NOCYCLE -- 시퀀스 번호 재사용(DEFAULT = NOCYCLE)
    CACHE n;        -- 캐시값(DEFAULT = 20)
```

- 자동으로 연속적인 숫자를 부여해주는 객체

#### 시노님(SYSNONYM)

```sql
CREATE [OR REPLACE] [PUBLIC] SYSNONYM 별칭 FOR 테이블명;
```

- 테이블 별칭 생성
- 본인 소유가 아닌 테이블에 접근하기 위해서는 `소유자명.테이블명`으로 접근해야 하지만 `테이블명`으로 접근 가능
- `OR REPLACE`
	+ 기존에 같은 이름으로 시노님이 생성되어 있는 경우 대체
- `PUBLIC`
	+ 시노님을 생성한 유저만 사용할 수 있는 Private `SYNONYM`의 반대
		* 누구나 사용 가능
	+ `PUBLIC`으로 생성한 시노님은 반드시 `PUBLIC`으로 삭제

## DCL(Data Control Language)

- 데이터 제어어로 객체에 대한 권한을 부여(`GRANT`)하거나 회수(`REVOKE`)하는 기능
- **테이블 소유자는 타 계정에 테이블 조회 및 수정 권한을 부여할 수 있고 회수할 수 있음**

### 권한

- 일반적으로 본인(접속한 계정) 소유가 아닌 테이블에 대해 원칙적으로 접근 불가능
	+ 권한 통제
- 업무적으로 필요시 테이블 소유자가 아닌 계정에 테이블 조회, 수정 권한 부여 가능

#### 권한의 종류

##### 오브젝트 권한

- 테이블에 대한 권한 제어
	+ 특정 테이블에 대한 `SELECT`, `INSERT`, `UPDATE`, `DELETE`, `MERGE` 권한
- **테이블 소유자는 타 계정에 테이블 조회 및 수정 권한을 부여할 수 있고 회수할 수 있음**

##### 시스템 권한

- 시스템 작업 제어
	+ 테이블 생성 권한, 인덱스 삭제 권한
- 관리자 권한만 권한을 부여 또는 회수할 수 있음

### GRANT

```sql
GRANT 권한 ON 테이블명 TO 유저;
```

- 권한 부여 시 반드시 테이블 소유자나 관리자 계정(SYS, SYSTEM)으로 접속하여 권한을 부여해야 됨
- **동시에 여러 유저에 대해 권한을 부여 할 수 있음**
- **동시에 여러 권한을 부여할 수 있음**
- 동시에 여러 객체에 대해 권한을 부여할 수 없음
- 즉시 반영
	+ 재접속하지 않아도 됨

#### 예시

![54-ex-grant(1)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/54-ex-grant(1).jpg)
*`professor` 테이블 소유자의 오브젝트 권한 부여*

![55-ex-grant(2)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/55-ex-grant(2).jpg)
*관리자 권한으로 시스템 권한 부여*

### REVOKE

```sql
REVOKE 권한 ON 테이블명 FROM 유저;
```

- **동시에 여러 권한을 회수할 수 있음**
- 이미 회수된 권한 재회수 불가능
- **동시에 여러 유저로부터 권한을 회수할 수 있음**
- 즉시 반영
	+ 재접속하지 않아도 됨

### 롤(ROLE)

```sql
CREATE ROLE 롤명;
```

- 권한의 묶음
	+ 생성 가능한 객체
- SYSTEM 계정에서 `ROLE` 생성 가능
- `GRANT`, `REOVOKE`와 달리 즉시 반영 안 됨
	+ 재접속해야 반영됨

#### 예시

![56-ex-role(1)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/56-ex-role(1).jpg)
*`ROLE` 생성*

![57-ex-role(2)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/57-ex-role(2).jpg)
*`ROLE`에 권한 담기*

![58-ex-role(3)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/58-ex-role(3).jpg)
*`hr` 계정에 `ROLE` 부여*

![59-ex-role(4)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/59-ex-role(4).jpg)
*`ROLE`의 권한 회수*

- `ROLE`에서 회수된 권한은 즉시 반영되므로 다시 `ROLE`을 부여할 필요가 없음

![60-ex-role(5)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/60-ex-role(5).jpg)
*`ROLE`을 통해 `hr` 계정에 부여한 권한 직접 회수*

- `ROLE`을 통해 부여한 권한은 직접 회수할 수 없음
	+ `ROLE`을 통한 회수만 가능

### 권한 부여 옵션(중간 관리자의 권한)

#### WITH GRANT OPTION

- `WITH GRANT OPTION`으로 받은 **오브젝트 권한을 다른 사용자에게 부여할 수 있음**
- 중간 관리자(`WITH GRANT OPTION`으로 권한을 부여받은 자)가 부여한 권한은 **중간 관리자만 회수 가능**
- **중간 관리자에게 부여된 권한을 회수할 때 제 3자에게 부여된 권한도 함께 회수됨**
    + `CASCADE`
    
#### WITH ADMIN OPTION

- `WITH ADMIN OPTION`을 통해 부여 받은 **시스템 권한 또는 `ROLE` 권한을 다른 사용자에게 부여할 수 있음**
- 중간 관리자를 거치지 않고 **직접 회수 가능**
- **중간 관리자의 권한을 회수할 때 제 3자에게 부여된 권한은 함께 회수되지 않음**
	+ 제 3자에게 부여된 권한은 여전히 남아있음

#### 예시

![61-ex-grant-option(1)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/61-ex-grant-option(1).jpg)
*권한 부여*

![62-ex-grant-option(2)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/62-ex-grant-option(2).jpg)
*중간 관리자가 부여한 오브젝트 권한을 관리자가 직접 회수*

- 중간 관리자를 통해 부여한 제 3계정의 권한은 관리자가 직접 회수할 수 없음

![63-ex-grant-option(3)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/63-ex-grant-option(3).jpg)
*중간 관리자에게 부여된 오브젝트 권한을 관리자가 직접 회수*

- 중간 관리자에게 부여된 권한은 관리자가 직접 회수할 수 있음
	+ 회수 시 중간 관리자를 통해 제 3의 계정에 부여된 권한도 함께 회수됨

![64-ex-grant-option(4)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/64-ex-grant-option(4).jpg)

- 중간 관리자에게 부여된 권한이 회수되면, 중간 관리자가 제 3의 계정에 부여한 권한도 함께 회수됨

![65-ex-grant-option(5)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/65-ex-grant-option(5).jpg)

- 제 3의 계정에 부여된 권한을 관리자가 직접 회수할 수 있음

![66-ex-grant-option(6)](/assets/img/posts/certifications/sqld/chapter2-sql-basic-and-utilization/administration-statements/66-ex-grant-option(6).jpg)

- 중간 관리자의 시스템 권한을 회수하더라도 중간 관리자가 제 3의 계정에게 부여한 권한은 회수되지 않음

## 참고자료

- [홍은혜, "SQLD 2과목 PART3. 관리 구문 완벽정리", 홍쌤의 데이터랩, 2024-02-25](https://www.youtube.com/watch?v=ijpxmi4DPj4&list=PLbflMVhwy2jPIAzArCK90mqFlTtndFigS&index=4){: target="_blank" }
- 한국데이터산업진흥원, SQL 자격검정 실전문제(서울: 한국데이터산업진흥원, 2016), 279.