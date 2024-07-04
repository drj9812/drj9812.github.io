---
title: "[SQLD | 1과목]데이터 모델링의 이해 - 데이터 모델과 SQL(2024)"
categories: [Certifications, SQLD]
tags: [Certification, 자격증, SQLD]
image:
  path: /assets/img/posts/certifications/sqld/01-kdata-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: 한국데이터산업진흥원
---

|    구분   |          시험과목        |               과목별 세부 내용             | 문항수 |       배점       |  과락 기준  |     검정시간    |
|:---------:|:-----------------------:|:-----------------------------------------:|:------:|:----------------:|:----------:|:---------------:|
| **1과목** | **데이터 모델링의 이해** | 데이터 모델링의 이해, **데이터 모델과 SQL** |   10    | 20점(문항별 2점) |   8점 미만 |                 |
|   2과목   |     SQL 기본 및 활용     |        SQL 기본, SQL 활용, 관리 구문       |   40    | 80점(문항별 2점) |  32점 미만 |                 |
|     계    |                         |                                           |   50    |      100점       |           | 90분(1시간 30분) |

# 데이터 모델의 이해 - 데이터 모델과 SQL (2024)

|          주요 항목        |        세부 항목         |
|:------------------------:|:------------------------:|
|    데이터 모델링의 이해   |     데이터 모델의 이해    |
|                          |          Entity          |
|                          |           속성           |
|                          |           관계           |
|                          |          식별자          |
|   **데이터 모델과 SQL**   |          정규화          |
|                          |      관계와 조인의 이해   |
|                          |  모델이 표현하는 트랜잭션  |
|                          |      Null 속성의 이해     |
|                          | 본질 식별자 vs 인조 식별자 |

## 정규화

- [[Database]데이터 정규화(Normalization)](https://drj9812.github.io/posts/normalization/){: target="_blank" } 참고

### 정규화의 개념

- 하나의 Entity에 많은 속성을 넣게 되면, 해당 Entity를 조회할 때마다 많은 양의 데이터가 조회될 것이므로 **최소한의 데이터만을 하나의 Entity에 넣는 식으로 데이터를 분해하는 과정**을 정규화라고 함
- 데이터의 일관성 최소한의 데이터 중복, 최대한의 데이터 유연성을 과정이라고 볼 수 있음
- 데이터의 **중복을 제거하고 데이터 모델의 독립성을 확보**
- 데이터 **이상현상을 줄이기 위한** DB 설계 기법
- Entity를 상세화하는 과정으로 **논리 데이터 모델링 수행 시점**에서 고려됨
- 제1 정규화부터 제5 정규화까지 존재하며, 실질적으로는 제3 정규화까지만 수행

#### 이상현상(Abnormality)

- **정규화를 하지 않아 발생하는 현상**
	+ 삽입이상
		* 특정 인스턴스가 삽입될 때 정의되지 않아도 될 속성까지도 반드시 입력되는 현상
	+ 삭제이상
	+ 갱신이상

### 정규화 단계

#### 제1 정규화(1NF)

![01-1nf.jpg](/assets/img/posts/certifications/sqld/1-understanding-data-modeling/data-model-and-sql/01-1nf.jpg)

- 테이블의 컬럼이 원자성(한 속성이 하나의 값을 갖는 특성)을 갖도록 테이블을 분해하는 단계
- 하나의 행과 컬럼의 값이 반드시 한 값만 입력되도록 행을 분리하는 단계
- 하나의 열에 여러 개의 값이 저장되는 반복 그룹을 제거하는 단계
	+ sns1, sns2, sns3 등 sns 컬럼을 여러 개 둬서 하나의 행이 여러 sns 값들을 갖게되는 것을 제거해야 함

#### 제2 정규화(2NF)

![02-2nf](/assets/img/posts/certifications/sqld/1-understanding-data-modeling/data-model-and-sql/02-2nf.jpg)

- 제1 정규화를 진행한 테이블에 대해 완전 함수 종속을 만들도록 테이블을 분해
- **완전 함수 종속이란, 기본 키를 구성하는 모든 컬럼의 값이 다른 컬럼을 결정짓는 상태**
- 기본 키의 부분 집합이 다른 컬럼과 1:1 대응 관계를 갖지 않는 상태를 의미
- **키(Key)가 2개 이상일 때 발생하며 키의 일부와 종속되는 관계가 있다면 분리**
- 위 `수강이력` 테이블에서는 기본 키(`학생번호` + `강의명`) 중, `강의명`에 의해 `강의실`이 결정되어 완전 함수 종속성을 위배함

#### 제3 정규화(3NF)

![03-3nf](/assets/img/posts/certifications/sqld/1-understanding-data-modeling/data-model-and-sql/03-3nf.jpg)

- 제2 정규화를 진행한 테이블에 대해 이행적 종속을 없애도록 테이블을 분리
- 이행적 종속성이란 A → B, B → C의 관계가 성립할 때, A → C가 성립되는 것을 말함
	+ (A, B)와 (B, C)로 분리하는 것이 제3 정규화
- 위 `구매` 테이블에서는 `고객번호`(A)에 의해 `상품명`(B)이 결정되고, `상품명`(B)에 의해 `가격`이 결정되므로 이행적 종속(A → C)이 성립됨

#### BCNF(Boyce-Codd Normal Form) 정규화

- 모든 결정자가 후보 키가 되도록 테이블을 분해하는 것
	+ 결정자가 후보 키가 아닌 다른 컬럼에 종속되면 안됨

#### 제4 정규화(4NF)

- 여러 컬럼들이 하나의 컬럼을 종속시키는 경우 분해하여 다중갑 종속성을 제거

#### 제5 정규화(5NF)

- `JOIN`에 의해서 종속성이 발생되는 경우를 분해

### 반정규화, 역정규화(De-Normalization)의 개념

- DB의 성능 향상을 위해 데이터 중복을 허용하고 `JOIN`을 줄이는 DB 성능 향상 방법
	+ 정규화를 통해 수 많은 테이블이 생성되는 경우, 조회 시 잦은 `JOIN`이 발생하여 DB의 성능이 저하될 수 있음
- 시스템의 성능 향상, 개발 및 운영의 단순화를 위해 정규화된 데이터 모델을 중복, 통합, 분리하는 데이터 모델링 기법
- 조회 속도를 향상시키지만, 데이터 모델의 유연성은 낮아짐

> 비정규화는 정규화를 수행하지 않는 것을 의미한다.
{: .prompt-info }

> 2024년부터 반정규화, 역정규화에 대한 문제가 출제되지 않는다.
{: .prompt-info }
 
#### 반정규화 수행 케이스

- 정규화에 충실하여 종속성, 활용성은 향상되지만 수행 속도가 느려지는 경우
- 다량의 범위를 자주 처리해야 하는 경우
	+ 테이블을 분리하지 않아 `JOIN` 연산을 피할 수 있음
- 특정 범위의 데이터만 자주 처리하는 경우
- 요약/집계 정보가 자주 요구되는 경우

## 관계와 조인의 이해

### 관계(Relationship)의 개념

![04-relation](/assets/img/posts/certifications/sqld/1-understanding-data-modeling/data-model-and-sql/04-relation.jpg)

- **Entity의 인스턴스 사이의 논리적인 연관성**
- Entity의 정의, 속성 정의 및 관계 정의에 따라서 다양하게 변할 수 있음
- **관계를 맺는다는 의미는 부모의 식별자를 자식에 상속하고, 상속된 속성을 매핑키(`JOIN` 키)로 활용**
	+ 부모, 자식의 연결

### 관계의 분류

- 관계는 존재에 의한 관계와 행위에 의한 관계로 분류
- **존재 관계는 Entity 간의 상태를 의미**
	+ 사원 Entity는 부서 Entity에 소속
- **행위 관계는 Entity 간의 어떤 행위가 있는 것을 의미**
	+ 주문은 고객이 주문할 때 발생

### JOIN의 의미

- 데이터의 중복을 피하기 위해 정규화에 의해 분리된 테이블이 관계를 맺는 과정
- 분리된 두 테이블의 데이터를 동시에 출력하거나, 관계가 있는 테이블을 참조하기 위해서 데이터를 연결하는 과정
 
#### 예시

![05-join](/assets/img/posts/certifications/sqld/1-understanding-data-modeling/data-model-and-sql/05-join.jpg)

```sql
SELECT a.계좌번호, b.관리점
  FROM 계좌 a, 관리점 b
 WHERE a.관리점코드 = b.관리점코드 AND a.계좌번호 = '100111'
```

- `계좌번호` 100111의 `관리점` 찾기
	1. `계좌` 테이블에서 `계좌번호`가 10111인 데이터 확인
	2. `계좌` 테이블에서 `계좌번호`가 100111` 데이터의 `관리점코드`인 1000을 확인
	3. `관리점코드`인 1000을 `관리점` 테이블에 전달하여 관리점 확인

### 계층형 데이터 모델

![06-hierarchical-database-model](/assets/img/posts/certifications/sqld/1-understanding-data-modeling/data-model-and-sql/06-hierarchical-database-model.jpg)

- **자기 자신끼리 관계가 발생하는 것**
- 하나의 Entity 내의 인스턴스끼리 계층 구조를 가지는 경우
- 계층 구조를 갖는 인스턴스끼리 연결하는 `JOIN`을 셀프 `JOIN`이라고 함
	+ 같은 테이블을 여러 번 `JOIN`

### 상호배타적 관계

![07-mutually-exclusive-relationship-ie-notation](/assets/img/posts/certifications/sqld/1-understanding-data-modeling/data-model-and-sql/07-mutually-exclusive-relationship-ie-notation.jpg)

- 두 테이블 중 하나만 가능한 관계
- 주문 Entity는 고객번호 또는 법인번호 둘 중 하나만 상속될 수 있음
	- 상호배타적 관계

## 모델이 표현하는 트랜잭션의 이해

### 트랜잭션이란

- **하나의 연속적인 업무 단위**
- 트랜잭션에 의한 관계는 **필수적인 관계 형태**를 가짐
- 하나의 트랜잭션에는 여러 쿼리가 포함될 수 있음

#### 예시

- A 고객이 B 고객에게 100만원을 이체하는 경우
	1. A 고객의 잔액이 100만원 이상인지 확인
	2. A 고객의 잔액이 100만원 이상이면, A 고객의 잔액에서 100만원 이체(`UPDATE`)
	3. B 고객 잔액에 100만원 입금(`UPDATE`)
		+ 2번과 3번 과정이 동시에 수행되어야 함(독립적으로 발생하면 안됨)
			* A 고객의 잔액에서 100만원이 빠져나가지 않았는데 B 고객의 잔액에 100만원이 입금되면 안됨
			* 각각의 `INSERT` 문으로 개발되면 안됨
		+ 모두 성공하거나 모두 취소되어야 함
			* 동시 커밋 또는 롤백 처리(부분 커밋 불가)

### 필수적, 선택적 관계와 ERD

- 두 Entity의 관계가 서로 필수적일 때 하나의 트랜잭션을 형성
- 두 Entity가 서로 독립적 수행이 가능하다면 선택적 관계로 표현

#### IE 표기법

![1083-essential-and-optional-relationship-ie-notation](/assets/img/posts/certifications/sqld/1-understanding-data-modeling/data-model-and-sql/08-essential-and-optional-relationship-ie-notation.jpg)

- **원을 사용하여** 필수적 관계와 선택적 관계를 **구분**
- **필수적 관계**에는 원을 그리지 않음
- **선택저 관계**에는 관계선 끝에 **원을 그림**

#### Barker 표기법

![09-essential-and-optional-relationship-barker-notation](/assets/img/posts/certifications/sqld/1-understanding-data-modeling/data-model-and-sql/09-essential-and-optional-relationship-barker-notation.jpg)

- **실선과 점선으로 구분**
- **필수적 관계**는 관계선을 **실선**으로 표기
- **선택적 관계**는 관계선을 **점선**으로 표기

## Null 속성의 이해

### Null이란

- DBMS에서 **아직 정해지지 않은 값을 의미**
- 0과 빈문자열(`''`)과는 다른 개념
- 모델 설계 시 각 컬럼별로 `NULL`을 허용할 지를 결정(Nullabe Column)

### Null의 특성

![10-null(1)](/assets/img/posts/certifications/sqld/1-understanding-data-modeling/data-model-and-sql/10-null(1).jpg)

- **`NULL`을 포함한 연산 결과는 항상 `NULL`**
- **`NULL` 값과의 비교연산은 항상 거짓을 반환**

![11-null(2)](/assets/img/posts/certifications/sqld/1-understanding-data-modeling/data-model-and-sql/11-null(2).jpg)

![12-null(3)](/assets/img/posts/certifications/sqld/1-understanding-data-modeling/data-model-and-sql/12-null(3).jpg)

![13-null(4)](/assets/img/posts/certifications/sqld/1-understanding-data-modeling/data-model-and-sql/13-null(4).jpg)

- 집계함수는 `NULL`을 제외한 연산 결과를 반환

### Null의 ERD 표기법

![14-null-erd-notation](/assets/img/posts/certifications/sqld/1-understanding-data-modeling/data-model-and-sql/14-null-erd-notation.jpg)

- IE 표기법에서는`NULL` 허용 여부를 알 수 없음
- Barker 표기법에서는 속성 앞에 '○'가 `NULL` 허용 속성을 의미함

## 본질식별자 vs 인조식별자

### 본질 식별자

- 업무에 의해 만들어지는 식별자
	+ 필수

### 인조 식별자

- 인위적으로 만들어지는 식별자
	+ 꼭 필요하지는 않지만 관리의 편이성 등의 이유로 인위적으로 만들어지는 식별자
- 본질 식별자가 복잡한 구성을 가질 때 인위적으로 생성
- 주로 각 행을 구분하기 위한 기본 키로 사용되며 자동으로 증가하는 일련번호같은 형태

### 예시

주문과 주문이력에 대한 Entity 설계 과정을 예로 들자면, 주문이력 테이블 설계 시 다음과 같은 식별자를 고려할 수 있다.

#### 기본 키를 `주문번호` + `상품번호`로 설계

![15-ex-natural-Identifier-vs-artificial-identifier(1)](/assets/img/posts/certifications/sqld/1-understanding-data-modeling/data-model-and-sql/15-ex-natural-Identifier-vs-artificial-identifier(1).jpg)

주문을 하면 `주문번호`와 `상품번호`가 필요하므로 본질 식별자(`주문번호` + `상품번호`)가 된다. 하지만, PK가 `주문번호` + `상품번호`일 경우 하나의 주문번호로 같은 상품의 주문 결과를 저장할 수 없게 된다.

#### 기본 키를 `주문번호` + `주문순번`(주문순번이라는 컬럼을 생성)로 설계

![16-ex-natural-Identifier-vs-artificial-identifier(2)](/assets/img/posts/certifications/sqld/1-understanding-data-modeling/data-model-and-sql/16-ex-natural-Identifier-vs-artificial-identifier(2).jpg)

하나의 주문에 여러 상품에 대한 주문 결과를 저장하고, 이를 `주문순번`으로 구분한다. 하지만 매 주문마다 동일한 상품 주문 시 `주문순번`을 저장하기 위해 상품의 주문 횟수를 세어야 한다는 점이 불편하다. 즉, 사과를 총 3번 구매했으니 `주문순번`은 1, 2, 3 순서대로 입력되야 한다.

#### 기본 키를 `주문상세번호`(인조식별자 생성)로 설계

![17-ex-natural-Identifier-vs-artificial-identifier(3)](/assets/img/posts/certifications/sqld/1-understanding-data-modeling/data-model-and-sql/17-ex-natural-Identifier-vs-artificial-identifier(3).jpg)

`주문상세번호`로 각 주문이력을 구분하기 때문에 같은 주문의 같은 상품이력이 저정될 수 있다. 하지만, `주문상세번호`만이 주 식별자이므로 나머지 정보들이 불필요하게 중복 저장될 위험 발생하고, 실제 업무와 상관없는 `주문상세번호`를 주 식별자로 생성하면 불필요한 인덱스가 생성된다는 단점이 있다.

> 인덱스는 조회 성능을 향상시키기 위한 객체이며, 인덱스는 DML을 사용할 때 Index Split 현상으로 인해 성능이 저하된다.
{: .prompt-info }

## 다음 글

- [[SQLD \| 2과목]SQL 기본 및 활용 - SQL 기본](https://drj9812.github.io/posts/sql-basic){: target="_blank" }

## 참고자료

- [홍은혜, "SQLD 1과목 완벽 정리 (2024년 신유형 반영)", 홍쌤의 데이터랩, 2024-02-14](https://www.youtube.com/watch?v=QB_GYdHUHmA){: target="_blank" }
- 한국데이터산업진흥원, *SQL 자격검정 실전문제*(서울: 한국데이터산업진흥원, 2016), 279.