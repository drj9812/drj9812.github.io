---
title: "[SQLP | 3과목]SQL 고급활용 및 튜닝 - SQL 옵티마이저(2024)"
categories: [Certifications, SQLP]
tags: [Certification, 자격증, SQLP]
math: true
image:
  path: /assets/img/posts/certifications/sqld/01-kdata-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: 한국데이터산업진흥원
---

# SQL 고급활용 및 튜닝 - SQL 옵티마이저(2024)

|         주요 항목           |         세부 항목       |
|:--------------------------:|:-----------------------:|
|        SQL 수행 구조        |  데이터베이스 아키텍처   |
|                            |       SQL 처리 과정      |
|                            | 데이터베이스 I/O 메커니즘 |
|        SQL 분석 도구        |       예상 실행계획      |
|                            |        SQL 트레이스      |
|                            |       응답 시간 분석     |
|         인덱스 튜닝	        |      인덱스 기본 원리    |
|                            |    테이블 엑세스 최소화   |
|                            |     인덱스 스캔 효율화    |
|                            |         인덱스 설계       |
|          조인 튜닝  	      |          NL 조인         |
|                            |        소트 머지 조인     |
|                            |          해시 조인        |
|                            |       스칼라 서브쿼리     |
|                            |        고급 조인 기법     |
|     **SQL 옵티마이저**      |     SQL 옵티마이징 원리   |
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

## SQL 옵티마이징 원리

### SQL 옵티마이저

- 사용자가 원하는 작업을 가장 효율적으로 수행할 수 있는 **최적의 데이터 엑세스 경로(실행계획)를 선택해주는 DBMS의 핵심 엔진**
  +  옵티마이저가 선택한 실행 방법의 적절성 여부는 질의의 수행 속도에 가장 큰 영향 미치게 됨
- **현재 대부분의 관계형 데이터베이스는 CBO만을 제공**
  + RBO를 제공하더라도 신규 기능들에 대해서는 더 이상 지원하지 않음

> SQL 옵티마이저를 DBWR, LGWR, PMON, SMON 같은 백그라운드 프로세스로 이해하기 쉽다. 서버 프로세스가 SQL을 전달하면, 옵티마이저가 최적화해서 실행계획을 돌려준다고 생각하는 것이다. 하지만, **옵티마이저는 별도 프로세스가 아니라 서버 프로세스가 가진 기능(Function)**일 뿐이다. SQL 파서와 로우 소스 생성기도 마찬가지다.
{: .prompt-info }

#### 옵티마이저 최적화 단계

![1-3](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/sql-optimizer/1-3.jpg)

1. 사용자로부터 전달받은 쿼리를 수행하는 데 후보군이 될만한 실행계획들을 찾음
2. 데이터 딕셔너리(Data Dictionary)에 미리 수집해둔 오브젝트 통계 및 시스템 통계정보를 이용해 각 실행계획의 예상비용을 산정
3. 최저 비용(Cost)을 나타내는 실행계획 선택

### 옵티마이저의 종류

![type-of-optimizer](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/sql-optimizer/type-of-optimizer.jpg)
*옵티마이저의 종류*

#### RBO(Rule Based Optimizer)

- **규칙(우선 순위)을 가지고 실행계획 생성**
  + SGA의 **딕셔너리 캐시에 저장된 정보들을 참조**해서 실행계획 생성
    * 이용 가능한 인덱스 유무
    * 인덱스 종류
    * 연산자
    * SQL 문에서 참조하는 객체 등
  + 규칙을 이해하면 누구나 실행계획을 비교적 쉽게 예측할 수 있음

##### Oracle의 RBO의 15가지 규칙

| 순위 |                         액세스 기법                        |
|:----:|:---------------------------------------------------------:|
|   1  |                   Single row by rowid                     |
|   2  |                Single row by cluster join                 |
|   3  | Single row by hash cluster key with unique or primary key |
|   4  |           Single row by unique or primary key             |
|   5  |                       Cluster join                        |
|   6  |                     Hash cluster key                      |
|   7  |                   indexed cluster key                     |
|   8  |                     Composite index                       |
|   9  |                   Single column index                     |
|  10  |            Bounded range search on indexed columns        |
|  11  |           Unbounded range search on indexed columns       |
|  12  |                     Sort merge join                       |
|  13  |              MAX or MIN of indexed column                 |
|  14  |               ORDER BY on indexed column                  |
|  15  |                      Full table scan                      |

###### 1. Single row by rowid

- **ROWID**를 통해서 테이블에서 하나의 행을 액세스하는 방식
- ROWID는 행이 포함된 데이터 파일, 블록 등의 정보를 가지고 있기 때문에 다른 정보를 참조하지 않고도 바로 원하는 행을 액세스할 수 있음
- 하나의 행을 액세스하는 가장 빠른 방법

###### 4. Single row by unique or primary key

- **유일 인덱스(Unique Index)**를 통해서 하나의 행을 액세스하는 방식
- 인덱스를 먼저 액세스하고 인덱스에 존재하는 ROWID를 추출하여 테이블의 행을 액세스

###### 8. Composite index

- **복합 인덱스에 동등(`=` 연산자) 조건으로 검색**하는 경우
  + `A`+`B` 컬럼으로 복합 인덱스가 생성되어 있고, `WHERE` 절에서 `A = 10 AND B = 1` 형태로 검색
- **인덱스 구성 컬럼의 개수가 더 많고 해당 인덱스의 모든 구성 컬럼에 대해 `=` 연산자로 값이 주어질 수록 우선순위가 더 높음**
  + `A` + `B`로 구성된 인덱스와 `A` + `B` + `C`로 구성된 인덱스가 각각 존재하고 `WHERE` 절에서 `A`, `B`, `C` 컬럼 모두에 대해 `=`로 값이 주어진다면 `A`+ `B` + `C` 인덱스가 우선 순위가 높음
  + `WHERE` 절에서 `A`, `B` 컬럼에만 `=` 연산자로 값이 주어진다면 `A` + `B`는 인덱스의 모든 구성 컬럼에 대해 값이 주어지고 `A` + `B` + `C` 인덱스 입장에서는 인덱스의 일부 컬럼에 대해서만 값이 주어졌기 때문에 `A` + `B` 인덱스가 우선 순위가 높음

###### 9. Single column index

- 단일 컬럼 인덱스에 `=` 조건으로 검색하는 경우
  + `A` 컬럼에 단일 컬럼 인덱스가 생성되어 있고, `WHERE` 절에서 `A = 10` 형태로 검색

###### 10. Bounded range search on indexed columns

- 인덱스가 생성되어 있는 컬럼의 양쪽 범위를 한정하는 형태로 검색하는 방식
  + `BETWEEN`, `LIKE` 연산자
  + `A` 컬럼에 인덱스가 생성되어 있고, `WHERE` 절에서 `A BETWEEN ‘10’ AND ‘20’` 또는 `A LIKE '1%'` 형태로 검색

###### 11. Unbounded range search on indexed columns

- 인덱스가 생성되어 있는 컬럼의 한쪽 범위만 한정하는 형태로 검색하는 방식
  + `>`, `>=`, `<`, `<=` 연산자
  + `A` 컬럼에 인덱스가 생성되어 있고, `WHERE` 절에서 `A > '10'` 또는 `A < '20'` 형태로 검색

###### 15. Full table scan

- **전체 테이블을 액세스**하면서 `WHERE` 절에 주어진 조건을 만족하는 행만을 결과로 추출
- RBO는 인덱스를 이용한 액세스 방식이 전체 테이블 액세스 방식보다 우선 순위가 높음
  + 해당 SQL문에서 **이용 가능한 인덱스가 존재한다면** 전체 테이블 액세스 방식보다는 항상 **인덱스를 사용하는 실행계획을 생성**
  + **양쪽 조인 컬럼에 모두 인덱스가 있는 경우 우선 순위가 높은 테이블을 선행 테이블(Driving Table)로 선택**
  + **한쪽 조인 컬럼에만 인덱스가 있는 경우 인덱스가 없는 테이블을 선행 테이블로 선택**
    * 인덱스가 없는 테이블을 먼저 처리하여 조인의 효율성을 높이려는 접근
    * **NL Join** 사용
  + **조인 컬럼에 모두 인덱스가 없는 경우 `FROM` 절에서 나열된 테이블의 순서를 역순으로 선택하여 조인을 수행**
    * 두 테이블을 정렬한 후에 병합하는 Sort Merge Join 사용

##### 예시

```sql
CREATE INDEX emp_job ON emp(job);
CREATE INDEX emp_sal ON emp(sal);
CREATE UNIQUE INDEX pk_emp ON emp(empno);

SELECT enmae
  FROM emp
 WHERE job = 'SALESMAN'
       AND
       sal BETWEEN 3000 AND 6000
```

![ex-cost-based-optimizer](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/sql-optimizer/ex-cost-based-optimizer.jpg)
*실행계획*

`WHERE` 절에서 `job` 컬럼의 조건은 `=` 연산자로, `SAL` 컬럼의 조건은 `BETWEEN` 연산자로 값이 주어졌고 각각의 컬럼에 단일 컬럼 인덱스가 존재한다. 우선 순위 규칙에 따라 `job` 조건은 규칙 9의 단일 컬럼 인덱스를 만족하고 `sal` 조건은 규칙 10의 인덱스상의 양쪽 한정 검색을 만족한다. 따라서 우선 순위가 높은 `emp_job` 인덱스를 이용해서 조건을 만족하는 행에 대해 `emp` 테이블을 액세스하는 방식을 선택할 것이다.

#### CBO(Cost Based Optimizer)

- SQL 문을 처리하는 데 필요한 **비용(Cost)이 가장 적은 실행계획을 선택**하는 방식
  + 예상 디스크 I/O 요청 횟수가 적은 실행계획을 선택
    * I/O 비용 모델
    * **캐싱을 고려하지 않으나, `optimizer_index_caching` 파라미터를 통해 일부 캐싱 효과를 고려하도록 설정할 수 있음**
    * 최근 방식인 CPU 비용 모델에서는 예상 I/O 발생량을 디스크에서 단일 블록을 읽을 때의 시간으로 환산한 Cost를 사용함
  + **규칙을 사용하지만 RBO와는 다르게, 규칙을 주된 최적화 메커니즘으로 사용하지 않음**
- RBO의 단점을 극복하기 위해서 출현
  + RBO의 규칙은 예측일 뿐 정확하지 않을 수 있음
    * RBO는 `WHERE` 절에서 `=` 연산자와 `BETWEEN` 연산자가 사용되면 규칙에 따라 `=` 컬럼의 인덱스를 사용하는 것이 보다 적은 작업을 할 것이라고 판단하지만 실제로는 `BETWEEN` 컬럼을 사용한 인덱스가 보다 일량이 적을 수 있음
- 비용을 예측하기 위해서 RBO가 사용하지 않는 딕셔너리 캐시의 테이블, 인덱스, 컬럼 등의 다양한 객체 **통계정보와 시스템 통계정보** 등을 이용
  + 오브젝트 통계
    * 테이블 통계: 레코드 수, 블록 수, 평균 행 길이 등
    * 인덱스 통계: 인덱스 높이, 리프 블록 개수, 클러스터링 팩터 등
    * 컬럼 통계: 중복을 제거한 컬럼 값의 수, 최솟값, 최댓값, `NULL` 값 개수, 히스토그램
  + 시스템 통계
    * CPU 속도, Single Blcok I/O 속도, Multiblock I/O 속도, 평균적인 Multiblok I/O 개수
  + [통계정보를 이용한 비용계산 원리](#통계정보를-이용한-비용계산-원리) 참고
  + **통계정보가 없는 경우** CBO는 정확한 비용 예측이 불가능해져서 **비효율적인 실행계획을 생성할 수 있음**
    * **통계정보를 유지**하는 것은 비용기반 최적화에서 **중요한 요소**임
- 동일한 SQL 문도 통계정보, DBMS 버전, DBMS 설정 정보 등의 차이로 인해 서로 다른 실행계획이 생성될 수 있음
- 다양한 한계들로 인해 실행계획의 예측 및 제어가 어려움

> RBO는 항상 인덱스를 사용할 수 있다면 전체 테이블 스캔 보다는 인덱스를 사용하는 실행계획을 생성한다고 하지만 CBO는 인덱스를 사용하는 비용이 전체 테이블 스캔 비용보다 크다고 판단되면 전체 테이블 스캔을 수행하는 방법으로 실행계획을 생성할 수도 있다
{: .prompt-info }

##### CBO의 구성 요소

![optimizer-based-cost](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/sql-optimizer/optimizer-based-cost.jpg)
*CBO의 구성 요소*

- 옵티마이저가 최적화를 수행할 때 세부적으로 사용하는 서브 엔진

###### 질의 변환기(Query Transformer)

- 사용자로부터 전달받은 SQL을 그대로 최적화하지 않고 우선 최적화에 유리한 형태로 변환을 시도
  + 의미적으로 동일하면서도 더 나은 성능이 기대되는 형태로 재작성

###### 대안 계획 생성기(Plan Generator)

- **동일한 결과를 생성하는 다양한 대안 계획을 생성**하는 모듈
  + 연산의 적용 순서 변경, 연산 방법 변경, 조인 순서 변경 등을 통해서 생성함
  + 동일한 결과를 생성하는 가능한 모든 대안 계획을 생성해야 보다 나은 최적화를 수행할 수 있음
  + 대안 계획의 생성이 너무 많아지면 최적화를 수행하는 시간이 그만큼 오래 걸릴 수 있음
    * 대부분의 상용 옵티마이저들은 대안 계획의 수를 제약하는 다양한 방법들을 사용하며, 이로 인해 생성된 대안 계획들 중에서 최적의 대한 계획이 포함되지 않을 수도 있음

###### 비용 예측기(Estimator)

- 대안 계획 생성기에 의해서 생성된 **대안 계획의 비용을 예측**하는 모듈
  + 쿼리 오퍼레이션 각 단계의 선택도(Seletivity), 카디널리티(Cardinality), 비용(Cost) 등을 계산
    * 궁극적으로는 실행계획 전체에 대한 총 비용을 계산함
  + 대안 계획의 정확한 비용을 예측하기 위해서 연산의 중간 집합의 크기 및 결과 집합의 크기, 분포도, 각 연산에 대한 비용 계산식 등의 예측이 정확해야 함
    * 정확한 통계정보를 필요로 함

### 실행계획(Execution Plan)

- SQL에서 요구한 사항을 처리하기 위한 **절차와 방법**
  + SQL을 어떤 순서로 어떻게 실행할 지를 결정하는 작업
- 옵티마이저는 다양한 실행 계획들 중에서 가장 효율적인 계획을 찾아서 생성함
- 실행계획을 보는 방법은 데이터베이스 벤더마다 서로 다름
- 실행계획을 구성하는 요소에는 조인 순서(Join Order), 조인 기법(Join Method), 액세스 기법(Access Method), 최적화 정보(Optimization Information), 연산(Operation) 등이 있음

#### 실행계획의 구성요소

![11-components-of-execution-plan](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/sql-optimizer/11-components-of-execution-plan.jpg)
*Oracle의 실행계획 정보의 구성요소*

```sql
SELECT STATEMENT Optimizer=ALL_ROWS(Cost=209 Card=5 Bytes=175)
  TABLE ACCESS (BY INDEX ROWID) OF 'EMP' (Cost=2 Card=5 Bytes=85)
    NESTED LOOPS (Cost=209 Card=5 Bytes=175)
      TABLE ACCESS (BY INDEX ROWID) OF 'DEPT' (Cost=207 Card=1 Bytes=18)
        INDEX (RANGE SCAN) OF 'DEPT_LOC_IDX'(NON-UNIQUE) (Cost=7 Card=1)
      INDEX (RANGE SCAN) OF 'EMP_DEPTNO_IDX'(NON-UNIQUE) (Cost-1 Card=5)
```

##### 조인 순서(Join Order)

- 조인작업을 수행할 때 참조하는 테이블의 순서
- `FROM` 절에 `A`, `B` 두 개의 테이블이 존재할 때 조인 작업을 위해 먼저 `A` 테이블을 읽고 `B` 테이블을 읽는 작업을 수행한다면 조인 순서는 `A` → `B`임
- 논리적으로 가능한 조인 순서는 n(`FROM` 절에 존재하는 테이블 수)! 만큼 존재
  + 현실적으로 옵티마이저가 적용 가능한 조인 순서는 이보다는 적거나 같음

##### 조인 기법(Join Method)

- 두 개의 테이블을 조인할 때 사용할 수 있는 방법
  + NL Join, Hash Join, Sort Merge Join 등

##### 액세스 기법(Access Method)

- 하나의 테이블을 액세스할 때 사용할 수 있는 방법
  + 인덱스 스캔(Index Scan)
    * 인덱스를 이용하여 테이블을 액세스
  + 전체 테이블 스캔(Full Table Scan)
    * 테이블 전체를 모두 읽으면서 조건을 만족하는 행을 엑세스

##### 최적화 정보(Optimization Information)

- 옵티마이저가 실행계획의 각 단계마다 통계 정보를 바탕으로 **예상되는 비용 사항**을 표시한 것
  + 실제로 SQL을 실행하고 얻은 결과가 아님
  + 실행계획에 비용 사항이 표시된다는 것은 비용기반 최적화 방식으로 실행계획을 생성했다는 것을 의미함
    * 비용 사항이 실행계획에 표시되지 않았다면 규칙기반 최적화 방식으로 실행계획을 생성한 거임
- `Cost`: 상대적인 비용 정보
- `Card`: 카디널리티(Cardinality)의 약자로서 주어진 조건을 만족한 결과 집합 혹은 조인 조건을 만족한 결과 집합의 건수
- `Bytes`: 결과 집합이 차지하는 메모리의 양

##### 연산(Operation)

- 여러 가지 조작을 통해서 원하는 결과를 얻어내는 일련의 작업
  + SQL에서 정렬을 목적으로 `ORDER BY` 절을 수행했다면 정렬 연산이 표시됨
- 조인 기법, 액세스 기법, 필터, 정렬, 집계, 뷰 등

#### 예시

```sql
-- 테이블 생성
CREATE TABLE t
    AS
SELECT d.no, e.*
  FROM emp e, (SELECT ROWNUM AS no
                 FROM dual
              CONNECT BY LEVEL <= 1000) d;

-- 인덱스 생성
CREATE INDEX t_x01 ON t(deptno, no);
CREATE INDEX t_x02 ON t(deptno, job, no);

-- SQL *PLUS에서 테이블 t에 대한 통계정보를 수집하는 명령어
EXEC dbms_stats.gather_table_stats(user,'t');

SELECT *
  FROM t
 WHERE deptno = 10 AND no = 1;
```

![12-ex-execution-plan(1)](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/sql-optimizer/12-ex-execution-plan(1).jpg)
*인덱스 `t_x01` 선택*

```sql
-- 인덱스 t_x02를 사용하도록 힌트 지정
SELECT /*+ index(t t_x02) */ *
  FROM t
 WHERE deptno = 10 AND no = 1;
```

![13-ex-execution-plan(2)](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/sql-optimizer/13-ex-execution-plan(2).jpg)
*인덱스 `t_x02` 선택*

```sql
-- Table Full Scan 하도록 힌트 지정
SELECT /*+ full(t) */ *
  FROM t
 WHERE deptno = 10 AND no = 1;
```

![14-ex-execution-plan(3)](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/sql-optimizer/14-ex-execution-plan(3).jpg)

- 옵티마이저가 인덱스 `t_xo1`를 선택한 근거가 비용(Cost)임을 알 수 있음

#### SQL 처리 흐름도

- SQL의 내부적인 처리 절차를 시각적으로 표현한 도표
  + 실행계획의 시각화
- 성능적인 관점을 살펴보기 위해서 SQL 처리 흐름도에 일량을 함께 표시할 수 있음

##### 예시

```sql
SELECT …
  FROM `tab1` a, `tab2` b
 WHERE `a.key` = b.key
       AND
       `a.col1 = :condition1` AND b.col2 = :condition2
```

![sql-processing-flowchart](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/sql-optimizer/sql-processing-flowchart.jpg)
*SQL 처리 흐름도*

- Outer Table(Driving Table)
  + 조인 작업에서 가장 먼저 액세스되는 테이블
    * Inner Table(Lookup Table)에서 검색해야 할 행 수를 줄이기 위해 일반적으로 작은 테이블이 선택됨
  + 조인 과정에서 각 행을 가져와 Inner Table(Lookup Table)과 비교
- Inner Table(Lookup Table)
  + Outer Table의 각 행에 대해 조인 조건을 만족하는지 검사하기 위해 참조되는 테이블
    * 큰 테이블이거나, 인덱스를 이용하여 빠르게 조회할 수 있는 테이블이 선택됨
  + Outer Table의 각 행과 조인 조건에 따라 비교되어 최종 결과를 생

테이블의 액세스 방법은 `TAB1`은 테이블 전체 스캔을 의미하고 `TAB2`는 `I01_TAB2`이라는 인덱스를 통한 인덱스 스캔을 했음을 표시한 것이다. 조인 방법은 NL Join을 수행했음을 표시한 것이다. 사진(*SQL 처리 흐름도*)에서 `TAB1`에 대한 액세스는 스캔(Scan) 방식이고 조인시도 및 `I01_`TAB2`` 인덱스를 통한 `TAB2` 액세스는 랜덤(Random) 방식이다. 대량의 데이터를 랜덤 방식으로 액세스하게 되면 많은 I/O가 발생하여 성능상 좋지 않다.

사진(*SQL 처리 흐름도*)에서 액세스 건수는 SQL 처리를 위해 `TAB1`을 액세스한 건수이다. 여기서는 `TAB1`의 `A.COL1` 컬럼에 이용 가능한 인덱스가 존재하지 않아 전체 테이블 스캔을 수행했음을 의미한다. 따라서 액세스 건수는 `TAB1` 테이블의 총 건수와 동일하다. 조인 시도 건수는 `TAB1`을 액세스한 후 즉, 테이블에서 읽은 해당 건에 대해 `A.COL1 = :condition1` 조건을 만족한 건만이 `TAB2`와 조인을 시도한다. 즉, `TAB1`을 액세스한 후 `A.COL1 = :condition1` 조건을 만족하지 않는다면 더 이상 조인 작업을 진행할 필요가 없다. 따라서 조인 시도 건수는 `TAB1`에 주어진 조건인 `A.COL1 = :condition1`을 만족한 건수가 된다. 테이블 액세스 건수는 `B.KEY` 컬럼만으로 구성된 인덱스(`B.KEY` 컬럼만으로 구성된 인덱스라고 가정함)인 `I01_TAB2`에서 `B.KEY = A.KEY` (`TAB1`은 이미 읽혀졌기 때문에 `A.KEY` 값은 상수임) 조건을 만족한 건만이 `TAB2` 테이블을 액세스한다. 즉, 조인 시도한 건들 중에서 `B.KEY = A.KEY` 조건까지 만족한 건과 같다. 성공 건수는 SQL 실행을 통해 사용자에게 답으로서 보여지는 결과 건수이다. `TAB2` 테이블을 액세스해서 `B.COL2 = :condition2` 조건까지 만족해야 비로서 사용자에게 보여질 수 있다.

### 옵티마이저 힌트

```sql
SELECT /*+ INDEX(A 고객_PK) */
       고객명, 연락처, 주소, 가입일시
  FROM 고객 A
 WHERE 고객ID = '000000008';
```

- **옵티마이저에게 특정한 실행계획을 선택하도록 지시하는 방법**
  + 통계정보가 정화하지 않거나 기타 다른 이유로 옵티마이저가 잘못된 판단을 할 수 있어, 직접 인덱스를 지정하거나 조인 방식을 변경함으로써 더 좋은 실행계획으로 유도하는 것
- **성능 최적화**를 위해 사용되며
- 쿼리 내에 주석 형태로 삽입되며, 주석 기호에 `+`를 붙이면 됨
  + `--+ INDEX(A 고객_PK)`와 같은 방식도 있지만 권장되지 않음
  + 힌트 이후의 코드들이 전부 주석처리 됨

> 옵티마이저의 자율적 판단에 의한 선택이 큰 손실을 발생시킬 수 있기 때문에 힌트는 빈틈없이 기술되어야 한다.
{: .prompt-warning }

#### 힌트가 무시되는 경우

- 옵티마이저는 힌트를 선택 가능한 옵션 정도가 아닌, 사용자로부터 주어진 명령어로 인식
  + Oracle은 힌트 잘못 기술하거나 잘못된 참조 시 에러가 발생하지 않음
  + SQL Server는 에러 발생

```sql
/*+ INDEX(A A_X01) INDEX(B, B_X03) */ -- 모두 유효
/*+ INDEX(C), FULL(D) */ -- 첫 번째 힌트만 유효
```

- 힌트 안에 인자를 나열할 때 콤마(`,`)를 사용할 수 있음
- 힌트와 힌트 사이에 콤마(`,`)를 사용할 수 없음

```sql
SELECT /*+ FULL(SCOTT.EMP) */ -- 무효
  FROM emp;
```

- 테이블을 지정할 때 스키마명을 사용할 수 없음

```sql
SELECT /*+ FULL(EMP) */ -- 무효
  FROM emp; e
```

- `FROM` 절에 별칭을 지정했는데 힌트에서 별칭을 사용하지 않으면 그 힌트는 무시됨

- 논리적으로 불가능한 액세스 경로
  + `JOIN` 절에 등치(`=`) 조건이 하나도 없는데 해시 `JOIN`으로 유도하는 경우
  + 테이블 전체 건수를 COUNT하는 쿼리에 `NULL`을 허용하는 단일 컬럼으로 생선 인덱스를 사용하도록 힌트를 지정하는 경우
- 의미적으로 맞지 않는 힌트 기술
  + 서브 쿼리에 `unnest`와 `push_subq`를 같이 기술
    * unnest되지 않은 서브쿼리만이 `push_subq` 힌트의 적용 대상임
- 옵티마이저에 의해 내부적으로 쿼리가 변환된 경우
- 버그

#### 종류

|    분류    |                    힌트                      |  설명 |
|:----------:|:-------------------------------------------:|:--:|
| 최적화 목표 |                 all_rows                    | 전체 처리속도 최적화 |
|            |               forst_rows(n)                 | 최초 N건 응답속도 최적화 |
| 액세스 경로 |                   full                      | Table Full Scan으로 유도 |
|            |                  cluster                    |  |
|            |                    hash                     |  |
|            |               index, no_index               | Index Scan으로 유도 |
|            |            index_asc, index_desc            | Index 순서대로 스캔하도록 유도 |
|            |                index_combine                | |
|            |                 index_join                  | |
|            |           index_ffs, no_index_ffs           | Index Fast Full Scan 으로 유도 및 방지 |
|            |            index_ss, no_index_ss            | Index Skip Scan으로 유도 및 방지 |
|            |         index_ss_asc, index_ss_desc         | |
|  쿼리 변환  |          no_query_transformation            | |
|            |                 use_concat                  | `OR` 또는 IN-List 조건을 OR-Expansion으로 유도 |
|            |                  no_expand                  | `OR` 또는 IN-List 조건에 대한 OR-Expansion 방지 |
|            |             rewrite, no_rewrite             | |
|            |               merge, no_merge               | 뷰 머징 유도 및 방지 |
|            | star_transformation, no_star_transformation | |
|            |                fact, no_fact                | |
|            |              unnest, no_unnest              | 서브쿼리 Unnesting 유도 및 방지 |
|  조인 순서  |                   ordered                   | `FROM` 절에 나열된 순서대로 조인 |
|            |                    leading                  | `LEADING` 힌트 괄호에 기술한 순서대로 조인 |
|  조인 방식  |              use_nl, no_use_nl              | NL 조인으로 유도 및 방지 |
|            |              use_nl_with_index              | |
|            |           use_merge, no_use_merge           | 소트 머지 조인으로 유도 및 방지|
|            |            use_hash, no_use_hash            | 해시 조인으로 유도 및 방지|
|  병렬 처리 |             parallel, no_parallel            | 테이블 스캔 또는 DML을 병렬방식으로 처리하도록 유도 및 방지|
|            |                  pq_distribute               | 병렬 수행 시 데이터 분배 방식 결정|
|            |        parallel_index, no_parallel_index     | 인덱스 스캔을 병렬 방식으로 처리하도록 유도 및 방지|
|    기타    |               append, noappend               | Direct-Path Insert로 유도 및 방지 |
|            |                 cache, nocache               | |
|            |            push_pred, no_push_pred           | 조인 조거건 Pushdown 유도 및 방지|
|            |            push_subq, no_push_subq           | 서브쿼리를 가급적 빨리/늦게 필터링하도록 유도 |
|            |                   qb_name                    | |
|            |             cursor_sharing_exact             | SQL 문장이 100% 일치할 때만 캐싱된 커서를 공유 및 재사용 |
|            |                 driving_site                 | DB Link Remote 쿼리에 대한 최적화 및 실행 주체 지정(Local 또는 Reomote) |
|            |               dynamic_sampling               | |
|            |              model_min_analysis              | |

### SQL 최적화 과정

![09-sql-optimization-and-execution-process](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/sql-optimizer/09-sql-optimization-and-execution-process.jpg)
*SQL 최적화 및 수행 과정*

![10-role-of-each-sub-engine](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/sql-optimizer/10-role-of-each-sub-engine.jpg)
*서브엔진별 역할*

#### 1. SQL 파싱

- 사용자로부터 전달받은 SQL을 SQL 파서(Parser)가 파싱하는 단계

##### 파서의 역할

- 파싱 트리 생성
  + SQL 문을 이루는 개별 구성요소를 분석해서 쿼리의 구조를 나타내는 파싱 트리 생성
- Syntax 체크
  + **문법적 오류가 없는지 확인**
    * 사용할 수 없는 키워드 사용
    * 올바르지 않은 순서
    * 누락된 키워드
- Semantic 체크
  + **의미상 오류가 없는지 확인**
    * 존재하지 않는 테이블 또는 컬럼 사용
    * 사용한 오브젝트에 대한 권한 확인

#### 2. SQL 최적화

- **SQL 옵티마이저**가 미리 수집한 시스템 및 오브젝트 통계정보를 바탕으로 다양한 실행계획를 생성해서 비교한 후 **가장 효율적인 하나를 선택**하는 단계
  + 옵티마이저는 DB 성능을 결정하는 가장 핵심적인 엔진
  + 딕셔너리 캐시에 미리 수집해 둔 오브젝트 통계 및 시스템 정보를 활용

> SQL을 실행하려면 사전에 SQL 파싱과 최적화 과정을 거친다. 세부적인 SQL 처리 과정을 설명할 목적이 아니면 일반적으로 이 둘을 구분할 필요는 없다. 최적화 과정을 포함히 'SQL 파싱'이라고 표현하기도 하고, 파싱 과정을 포함해 'SQL 최적화'라고 표현하고디 한다. 여기서는 'SQL 최적화'라고 표현했다.
{: .prompt-info }

#### 3. 로우 소스 생성

- SQL 옵티마이저가 선택한 실행경로를 실제 실행 가능한 코드 또는 프로시저 형태로 포맷팅하는 단계
- 로우 소스 생성기(Row-Source Generator)가 담당

### 최적화 목표

#### 전체 처리속도 최적화

```sql
-- 시스템 레벨에서 변경
ALTER system SET optimizer_mode = all_rows;

-- 세션 레벨에서 변경
ALTER session SET optimizer_mode = all_rows;

-- 쿼리 레벨에서 변경
SELECT /*+ all_rows */ *
  FROM t 
 WHERE …;
```

- 쿼리 최종 결과집합을 끝까지 읽는 것을 전제로, 시스템 리소스(I/O, CPU, 메모리 등)를 **가장 적게 사용하는 실행계획을 선택**
  + **Table Full Scan**처럼 큰 데이터를 처리하는 방법을 **선호할 수 있음**
  + **Hash 조인이나 병렬 처리와 같은 대용량 데이터 처리에 적합한 조인 방식을 더 많이 선택할 수 있음**
- Oracle, SQL Server 등을 포함해 대부분 DBMS의 기본 옵티마이저 모드는 전체 처리속도 최적화에 맞춰져 있음


#### 최초 응답속도 최적화

```sql
-- Oracle 쿼리 레벨에서 변경
SELECT /*+ first_rows(10) */ *
  FROM t
 WHERE ...;

-- SQL Server 쿼리 레벨에서 변경
SELECT *
  FROM t
 WHERE OPTION(FAST 10); -- 쿼리 힌트로 fast 10 지정
```

- 전체 결과집합 중 일부만 읽다가 멈추는 것을 전제로, **가장 빠른 응답 속도를 낼 수 있는 실행계획을 선택**
  + **인덱스를 더 많이 사용할 가능성이 높음**
    * 인덱스를 통해 첫 번째 행들을 더 빨리 검색할 수 있기 때문
  + **NL 조인을 더 많이 선택할 가능성이 있음**
    * 작은 데이터 세트나 인덱스가 잘 잡혀 있는 경우 빠른 첫 번째 행 검색을 가능하게 함
  + 이 모드에서 생성한 실행계획으로 데이터를 끝까지 읽는다면 전체 처리속도 최적화 실행계획보다 더 많은 리소스를 사용하고 전체 수행 속도도 느려질 수 있음
- Oracle에서 옵티마이저 모드를 시스템 또는 세션 레벨에서 `first_rows_10`으로 지정하면, 사용자가 전체 결과집합 중 처음 10개 로우만 읽고 멈추는 것을 전제로 가장 빠른 응답 속도를 낼 수 있는 실행계획을 선택

### 옵티마이저 행동에 영향을 미치는 요소

#### SQL과 연산자 형태

- 결과가 같더라도 **SQL을 어떤 형태로 작성**했는지 또는 **어떤 연산자를 사용**했는지에 따라 옵티마이저가 다른 선택을 할 수 있음
  + 쿼리 성능에 영향을 미침

#### 옵티마이징 팩터

- 쿼리를 똑같이 작성하더라도 인덱스, IOT, 클러스터링, 파티셔닝, MV 등을 **어떻게 구성**했는지에 따라 실행계획과 성능이 크게 달라짐

#### DBMS 제약 설정

- 개체 무결성, 참조 무결성, 도메인 무결성 등을 위해 DBMS가 제공하는 PK, FK, Check, Not Null 같은 **제약 설정** 기능을 이용할 수 있고, 이들 제약 설정은 옵티마이저가 쿼리 성능을 최적화하는 데에 매우 중요한 정보를 제공
- 인덱스 컬럼에 Not Null 제약이 설정돼 있으면 옵티마이저는 전체 개수를 구하는 Count 쿼리에 이 인덱스를 활용할 수 있음

#### 옵티마이저 힌트

- 옵티마이저의 판단보다 **사용자가 지정한 옵티마이저 힌트가 우선**됨

#### 통계정보

- 통계정보가 옵티마이저에게 미치는 영향력은 절대적임
- CBO의 모든 판단 기준은 통계정보에서 나옴
  + 내부적으로 규칙이 사용되지 않는 건 아님

#### 옵티마이저 관련 파라미터

- SQL, 데이터, 통계정보, 하드웨어 등 모든 환경이 동일하더라도 DBMS 버전을 업그레이드하면 옵티마이저가 다르게 작동할 수 있음
  + 옵티마이저 관련 파라미터가 추가 또는 변경되면서 나타나는 현상

#### DBMS 버전과 종류

- 옵티마이저 관련 **파라미터가 같더라도 버전에 따라 실행계획이 다를 수 있음**
- 같은 SQL이더라도 DBMS 종류에 따라 내부적으로 처리하는 방식이 다를 수 있음

### 옵티마이저의 한계

> *옵티마이저가 사람이 만든 소프트웨어 엔진에 불과하며 결코 완벽할 수 없음을 이해하는 것은 매우 중요하다. 현재의 기술수준으로 해결하기 어려운 문제가 있는가 하면, 기술적으론 가능한데 현실적인 제약(통계정보 수집량과 최적화를 위해 허락된 시간) 때문에 아직 적용하지 못하는 것들도 있다. 옵티마이저가 완벽하지 못하게 만드는 요인이 어디에 있는지 구체적으로 살펴보자.*

#### 옵티마이징 팩터의 부족

옵티마이저는 주어진 환경에서 가장 최적의 실행계획을 수립하기 위해 정해진 기능을 수행할 뿐이다. 옵티마이저가 아무리 정교하고 기술적으로 발전하더라도 **사용자가 적절한 옵티마이징 팩터(효과적으로 구성된 인덱스, IOT, 클러스터링, 파티셔닝 등)를 제공하지 않는다면 결코 좋은 실행계획을 수립할 수 없다.**

#### 통계정보의 부정확성

최적화에 필요한 모든 정보를 수집해서 보관할 수 있다면 옵티마이저도 그만큼 고성능 실행계획을 수립하겠지만, **100% 정확한 통계정보를 유지하기는 현실적으로 불가능하다.** 특히, 컬럼 분포가 고르지 않을 때 컬럼 히스토그램이 반드시 필요한데, 이를 수집하고 유지하는 비용이 만만치 않다. 컬럼을 결합했을 때의 모든 결합 분포를 미리 구해두기 어려운 것도 큰 제약 중 하나다. 이는 상관관계에 있는 두 컬럼이 조건절에 사용될 때 옵티마이저가 잘못된 실행계획을 수립하게 만드는 주요인이다. 아래 쿼리를 예를 들어 보자.

```sql
SELECT *
  FROM 사원
 WHERE 직급 = '부장'
       AND
       연봉 >= 5000;
```

직급이 {부장, 과장, 대리, 사원}의 집합이고 각각 25%의 비중을 갖는다. 그리고 전체 사원이 1,000명이고 히스토그램상 '연봉 >= 5000' 조건에 부합하는 사원 비중이 10%이면, 옵티마이저는 위 쿼리 조건에 해당하는 사원 수를 25(=1,000×0.25×0.1)명으로 추정한다. 하지만 잘 알다시피 직급과 연봉 간에는 상관관계가 매우 높아서, 만약 모든 부장의 연봉이 5,000만원 이상이라면 실제 위 쿼리 결과는 250(=1,000×0.25×1)건이다. 이런 조건절에 대비해 모든 컬럼 간 상관관계와 결합 분포를 미리 저장해 두면 좋겠지만 이것은 거의 불가능에 가깝다. 테이블 컬럼이 많을수록 잠재적인 컬럼 조합의 수는 기하급수적으로 증가하기 때문이다.

#### 바인드 변수 사용 시 균등분포 가정

아무리 정확한 컬럼 히스토그램을 보유하더라도 바인드 변수를 사용한 SQL에는 무용지물이다. **조건절에 바인드 변수를 사용하면 옵티마이저가 균등분포를 가정**하고 비용을 계산하기 때문이다.

#### 비현실적인 가정

옵티마이저는 쿼리 수행 비용을 평가할 때 여러 가정을 사용하는데, 그 중 일부는 상당히 비현실적이어서 종종 이해할 수 없는 실행계획을 수립하곤 한다. 예전 Oracle 버전에선 Single Block I/O와 Multiblock I/O의 비용을 같게 평가하고 데이터 블록의 캐싱 효과도 고려하지 않았는데, 그런 것들이 비현실적인 가정의 좋은 예다. DBMS 버전이 올라가면서 이런 비현실적인 가정들이 계속 보완되고 있지만 완벽하지 않고, 모두 해결되리라고 기대하는 것도 무리다.

#### 규칙에 의존하는 CBO

**아무리 비용기반 옵티마이저라 하더라도 부분적으로는 규칙에 의존한다.** 예를 들어, 최적화 목표를 최초 응답속도에 맞추면(Oracle을 예로 들면, optimizer_mode = first_rows), order by 소트를 대체할 인덱스가 있을 때 무조건 그 인덱스를 사용한다. 다음 절에서 설명할 휴리스틱(Heuristic) 쿼리 변환도 좋은 예라고 할 수 있다.

> 결과만 보장된다면 무조건 쿼리 변환 수행하는 것을 휴리스틱(Heuristic) 쿼리 변환이라고 한다.
{: .prompt-info }

#### 하드웨어 성능

옵티마이저는 기본적으로 옵티마이저 개발팀이 사용한 하드웨어 사양에 맞춰져 있다. 따라서 **실제 운영 시스템의 하드웨어 사양이 그것과 다를 때 옵티마이저가 잘못된 실행계획을 수립할 가능성이 높아진다.** 또한 애플리케이션 특성(I/O 패턴, 부하 정도 등)에 의해서도 하드웨어 성능은 달라진다.

### 통계정보를 이용한 비용계산 원리

실행계획을 수립할 때 CBO는 SQL 문장에서 액세스할 데이터 특성을 고려하기 위해 통계정보를 이용한다. 최적의 실행계획을 위해 통계정보가 항상 데이터 상태를 정확하게 반영하고 있어야 하는 이유다. DBMS 버전이 올라갈수록 자동 통계관리 방식으로 바뀌고 있지만, 가끔 DB 관리자가 수동으로 수집관리해 주어야 할 때도 있다. 옵티마이저가 참조하는 통계정보 종류로 아래 네 가지가 있다.

###### 옵티마이저 통계 유형

|  통계 유형	|                          세부 통계  항목                          |
|:----------:|:-----------------------------------------------------------------:|
| 테이블 통계	|     전체 레코드 수, 총 블록 수, 빈 블록 수, 한 행당 평균 크기 등    |
| 인덱스 통계	|    인덱스 높이, 리프 블록 수, 클러스터링 팩터, 인덱스 레코드 수 등  |
|   컬럼 통계	| 값의 수, 최저 값, 최고 값, 밀도, `NULL` 값 개수, 컬럼 히스토그램 등 |
| 시스템 통계 |	          CPU 속도, 평균적인 I/O 속도, 초당 I/O 처리량 등          |

지금부터, 데이터 딕셔너리에 미리 수집해 둔 통계정보가 옵티마이저에 의해 구체적으로 어떻게 활용되는지 살펴보자.

> 평균적인 Single Block I/O 개수는 당연히 1이므로 수집하지 않는다. Multiblock I/O 단위를 128로 설정하더라도 실제 평균적인 Multiblock I/O 개수는 128 미만이 된다. Full Scan할 때 읽어야 할 익스텐트 개수, 익스텐트 크기, 버퍼 캐시 히트율 등에 따라 달라지기 때문이다. 따라서 Oracle은 이를 Full Scan 비용 계산 시 활용하기 위해 별도 시스템 통계항목으로 측정해 둔다.
{: .prompt-info }

#### 선택도

$$ \text{선택도} = \frac{1}{\text{Distinct Value개수}} = \frac{1}{\text{num distinct}} $$

- **전체 대상 레코드 중에서 특정 조건에 의해 선택될 것으로 예상되는 레코드 비율**
- 선택도 → 카디널리티 → 비용 → 액세스 방식, 조인 순서, 조인 방법 등 결정 
  + 선택도는 **최적의 실행계획을 수립하는 데 있어 가장 중요한 요인**
  + 히스토그램이 있으면 그것으로 선택도를 산정하며, 단일 컬럼에 대해서는 비교적 정확한 값을 구함
    * 히스토그램이 없거나, 있더라도 조건절에 바인드 변수를 사용하면 옵티마이저는 히스토그램을 사용하지않고 데이터 분포가 균일하다고 가정한 상태에서 선택도를 구함

#### 카디널리티(Cardinality)

$$ \text{카디널리티} = \text{총 로우 수} \times \text{선택도} = \text{총 로우 수} \times \frac{\text{num_rows}}{\text{num_distinct}}$$

- 특정 액세스 단계를 거치고 난 후 **출력될 것으로 예상되는 결과 건수**
  + 전체 레코드 중에서 조건절에 의해 선택되는 래코드 개수

##### 예시

```sql
SELECT DISTINCT(*)
  FROM 사원
 WHERE 부서 =:부서
```

위 쿼리에서 부서 컬럼의 Distinct Value 개수가 10이면 선택도는 0.1(=1/10)이고, 총 사원 수가 1,000명일 때 카디널리티는 100이 된다. 옵티마이저는 위 조건절에 의한 결과집합이 100건일 것으로 예상한다는 뜻이다. 조건절이 두 개 이상일 때는 각 컬럼의 선택도와 전체 로우 수를 곱해 주기만 하면 된다.

```sql
SELECT *
  FROM 사원
 WHERE 부서 = :부서 
       AND
       직급 = :직급;
```

직급의 도메인이 {부장, 과장, 대리, 사원}이면 Distinct Value 개수가 4이므로 선택도는 0.25(=1/4)다. 따라서 위 쿼리의 카디널리티는 25(=1000 × 0.1 × 0.25)로 계산된다.

#### 히스토그램

미리 저장된 히스토그램 정보가 있으면, 옵티마이저는 그것을 사용해 더 정확하게 카디널리티를 구할 수 있다. 특히, 분포가 균일하지 않은 컬럼으로 조회할 때 효과를 발휘한다. 히스토그램에는 아래 두 가지 유형이 있다.

> Oracle 12c 이상 버전에서 사용하는 히스토그램 유형으로는 도수분포(Frequency), 높이균형(Height-Balanced), 상위도수분포(Top-Frequency), 하이브리드(Hybrid)가 있다.
{: .prompt-info }

##### 도수분포 히스토그램

![frequency-historgram](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/sql-optimizer/frequency-historgram.jpg)

- **값별로 빈도수(frequency number)를 저장하는 히스토그램**
- 컬럼이 가진 값의 수가 적을 때 사용되며, 컬럼 값의 수가 적기 때문에 각각 하나의 버킷을 할당하는 것이 가능
  + 값의 수 = 버킷 개수

##### 높이균형 히스토그램

![height-balanced-histogram](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/sql-optimizer/height-balanced-histogram.jpg)

- 컬럼이 가진 값의 수가 아주 많아 각각 하나의 버킷을 할당하기 어려울 때 사용
  + 히스토그램 버킷을 값의 수보다 적게 할당하기 때문에 하나의 버킷이 여러 개 값을 담당
    * 값의 수가 1,000개인데 히스토그램을 위해 할당된 버킷 개수가 100개이면, 하나의 버킷이 평균적으로 10개의 값을 대표
- 각 버킷의 높이가 같음
  + $ \text{각 버킷의 데이터 분포} = \frac{1}{\text{버킷 개수}} \times 100 \% $
     * $ \text{각 버킷이 갖는 빈도수} = \frac{\text{총 레코드 개수}}{\text{버킷 개수}} $
- 빈도 수가 많은 값(popular value)에 대해서는 두 개 이상의 버킷이 할당됨
- 위 사진에서 x 축은 연령대를 의미하는데, age = 40인 레코드 비중이 50%이어서 총 20개 중 10개 버킷을 차지한 것을 볼 수 있음

> 바인드 변수를 사용하면, 최초 수행할 때 최적화를 거친 실행계획을 캐시에 적재하고 실행시점에는 그것을 그대로 가져와 값만 다르게 바인딩하면서 반복 재사용하게 된다. 여기서, 변수를 바인딩하는 시점이(최적화 시점보다 나중인) 실행시점이라는 사실이 중요하다. 즉, SQL을 최적화하는 시점에 조건절 컬럼의 데이터 분포를 활용하지 못한다. 바인드 변수를 사용할 때 옵티마이저가 평균 분포를 가정한 실행계획을 생성하는 것도 이 때문이다. 컬럼 분포가 균일할 때는 상관없겠지만, 그렇지 않을 때는 실행 시점에 바인딩되는 값에 따라 쿼리 성능이 다르게 나타날 수 있어 문제다. 따라서 DW, OLAP, 배치 프로그램(Loop 내에서 수행되는 쿼리 제외)에서 수행되는 쿼리는 바인드 변수보다 상수를 사용하는 것이 좋은데, 날짜 컬럼처럼 부등호, `BETWEEN` 같은 범위 조건으로 자주 검색되는 컬럼일 때 특히 그렇다. OLTP성 쿼리이더라도 값의 종류가 적고 분포가 균일하지 않을 떄는 상수 조건을 쓰는 것이 유용할 수 있다.
{:. prompt-info }

#### 비용

- 쿼리를 수행하는데 소요되는 일량 또는 시간
  + 실제치가 아닌 예상치
- 옵티마이저 비용 모델에는 I/O 비용 모델과 CPU 비용 모델 두 가지가 있음
  + I/O 비용 모델
    * **예상되는 I/O 요청 횟수**만을 쿼리 수행 비용으로 간주해 실행계획을 평가
  + CPU 비용 모델
    * **I/O 비용 모델에 시간 개념을 더해 비용을 산정**

##### 인덱스를 경유한 테이블 액세스 비용

$$ \text{비용} = \text{blevel} + \text{(리프 블록 수} \times \text{유효 인덱스 선택도)} + \text{(클러스터링 팩터} \times \text{유효 테이블 선택도)} $$

- I/O 비용 모델에서의 비용은 디스크 I/O 요청 횟수
  + 논리적/물리적으로 읽은 블록 개수가 아닌 I/O 요청 횟수
- 인덱스를 경유한 테이블 액세스 시에는 Single Block I/O 방식이 사용됨
  + 디스크에서 한 블록을 읽을 때마다 한 번의 I/O 요청을 일으키는 방식이므로 읽게 될 물리적 블록 개수가 I/O 요청 횟수와 일치
- $ \text{blevel} $
  + 인덱스 수직적 탐색 비용
- $ \text{리프 블록 수} \times \text{유효 인덱스 선택도} $
  + 인덱스 수평적 탐색 비용
- $ \text{클러스터링 팩터} \times \text{유효 테이블 선택도} $
  + 테이블 임의 접근 비용

###### 인덱스를 경유한 테이블 액세스 비용 항목

|        항목       |                                                     설명                                                       |
|:-----------------:|:--------------------------------------------------------------------------------------------------------------:|
|       blevel      |                   브랜치 레벨을 의미하며, 리프 블록에 도달하기 전에 읽게 될 브랜치 블록 개수임                     |
|   클러스터링 팩터  |                         특정 컬럼을 기준으로 같은 값을 갖는 데이터가 서로 모여있는 정도                           | 
|                   |         인덱스를 경유해 테이블 전체 로우를 액세스할 때 읽을 것으로 예상되는 논리적인 블록 개수로 계수화 함          |
| 유효 인덱스 선택도 |            전체 인덱스 레코드 중에서 조건절을 만족하는 레코드를 찾기 위해 스캔할 것으로 예상되는 비율(%)            |
|                   |            리프 블록에는 인덱스 레코드가 정렬된 상태로 저장되므로 이 비율이 곧, 방문할 리프 블록 비율임             |
| 유효 테이블 선택도 |           전체 레코드 중에서 인덱스 스캔을 완료하고서 최종적으로 테이블을 방문할 것으로 예상되는 비율(%)            |
|                   | 클러스터링 팩터에 유효 테이블 선택도를 곱함으로써 조건절에 대해 읽힐 것으로 예상되는 테이블 블록 개수를 구할 수 있음 |

##### Full Scan에 의한 테이블 액세스 비용

- 테이블 전체를 순차적으로 읽어 들이는 과정에서 발생하는 I/O 요청 횟수로 비용을 계산
- Full Scan할 때는 한 번의 I/O 요청으로써 여러 블록을 읽어 들이는 Multiblock I/O 방식을 사용하므로 총 블록 수를 Multiblock I/O 단위로 나눈 만큼 I/O 요청이 발생함
  + 100블록을 8개씩 나누어 읽는다면 13번의 I/O 요청이 발생하고, I/O 요청 횟수로써 Full Scan 비용을 추정
  + Multiblock I/O 단위가 증가할수록 I/O 요청 횟수가 줄고 예상비용도 줄게 됨

## SQL 공유 및 재사용

### 소프트 파싱, 하드 파싱

![parsing-schematic](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/sql-optimizer/parsing-schematic.jpg)
*파싱 도식화*

- SQL을 캐시에서 찾아 곧바로 실행단계로 넘어가는 것을 소프트 파싱(Soft Parsing)이라고 함
- 캐시에서 SQL을 찾는 데 실패해 최적화 및 로우 소스 생성 단계까지 모두 거치는 것을 하드 파싱(Hard Parsing)이라고 함

#### SQL 최적화 과정은 왜 하드(Hard)한가

### 바인드 변수의 중요성

#### 이름없는 SQL 문제

- SQL은 따로 이름이 없고, SQL 텍스트 자체가 이름임

#### 공유 가능 SQL

- [[JDBC]Statement 대신 PrepareStatment를 사용해야 하는 이유](https://drj9812.github.io/posts/why-use-preparestatement-instead-of-statement/){: target="_blank" } 참고

## 쿼리 변환

### 쿼리변환이란?

쿼리 변환(Query Transformation)은, 옵티마이저가 SQL을 분석해 의미적으로 동일(→ 같은 결과를 리턴)하면서도 더 나은 성능이 기대되는 형태로 재작성하는 것을 말한다. 이는 본격적으로 실행계획을 생성하고 비용을 계산하기에 앞서 사용자 SQL을 최적화에 유리한 형태로 재작성하는 것으로서, DBMS 버전이 올라갈수록 그 종류가 다양해짐은 물론 더 적극적인 시도가 이루어지고 있다. 비용기반 옵티마이저의 서브엔진으로서 Query Transformer, Estimator, Plan Generator가 있다고 설명했는데, 이 중 Query Transformer가 그런 역할을 담당한다([그림 Ⅲ-3-1] 참조). 쿼리 변환은 다음 두 가지 방식으로 작동한다.

#### 휴리스틱(Heuristic) 쿼리 변환

결과만 보장된다면 무조건 쿼리 변환을 수행한다. 일종의 규칙 기반(Rule-based) 최적화 기법이라고 할 수 있으며, 경험적으로 (최소한 동일하거나) 항상 더 나은 성능을 보일 것이라는 옵티마이저 개발팀의 판단이 반영된 것이다.

#### 비용기반(Cost-based) 쿼리 변환

변환된 쿼리의 비용이 더 낮을 때만 그것을 사용하고, 그렇지 않을 때는 원본 쿼리 그대로 두고 최적화를 수행한다.

### 서브쿼리 Unnesting

‘서브쿼리 Unnesting’은 중첩된 서브쿼리(Nested Subquery)를 풀어내는 것을 말한다. 서브쿼리를 메인쿼리와 같은 레벨로 풀어낸다면 다양한 액세스 경로와 조인 메소드를 평가할 수 있다. 특히 옵티마이저는 많은 조인테크닉을 가지기 때문에 조인 형태로 변환했을 때 더 나은 실행계획을 찾을 가능성이 높아진다. 아래는 하나의 쿼리에 서브쿼리가 이중삼중으로 중첩(nest)될 수 있음을 보여준다.

```sql
SELECT *
  FROM emp a 
 WHERE EXISTS (SELECT 'x' 
                 FROM dept 
                WHERE deptno = a.deptno)
       AND
       sal > (SELECT AVG(sal)
                FROM emp b
               WHERE EXISTS (SELECT 'x'
                               FROM salgrade
                              WHERE b.sal BETWEEN losal AND hisal
                                    AND
                                    grade = 4))
```

위 쿼리의 논리적인 포함관계를 상자로 표현하면 [그림 Ⅲ-3-4]와 같다.

![logical inclusion of subquery](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/sql-optimizer/logical%20inclusion%20of%20subquery.jpg)

위 쿼리와 [그림 Ⅲ-3-4]에서 알 수 있듯이 ‘중첩된 서브쿼리(nested subquery)’는 메인쿼리와 부모와 자식이라는 종속적이고 계층적인 관계가 존재한다. 따라서 논리적인 관점에서 그 처리과정은 IN, Exists를 불문하고 필터 방식이어야 한다. 즉, 메인 쿼리에서 읽히는 레코드마다 서브쿼리를 반복 수행하면서 조건에 맞지 않는 데이터를 골라내는 것이다. 하지만 서브쿼리를 처리하는 데 있어 필터 방식이 항상 최적의 수행속도를 보장하지 못하므로 옵티마이저는 아래 둘 중 하나를 선택한다.

- 동일한 결과를 보장하는 조인문으로 변환하고 나서 최적화한다. 이를 일컬어 ‘서브쿼리 Unnesting’이라고 한다.
- 서브쿼리를 Unnesting하지 않고 원래대로 둔 상태에서 최적화한다. 메인쿼리와 서브쿼리를 별도의 서브플랜(Subplan)으로 구분해 각각 최적화를 수행하며, 이때 서브쿼리에 필터(Filter) 오퍼레이션이 나타난다.

1번 서브쿼리 Unnesting은 메인과 서브쿼리 간의 계층구조를 풀어 서로 같은 레벨(flat한 구조)로 만들어 준다는 의미에서 ‘서브쿼리 Flattening’이라고도 부른다. 이렇게 쿼리 변환이 이루어지고 나면 일반 조인문처럼 다양한 최적화 기법을 사용할 수 있게 된다. 2번처럼, Unnesting하지 않고 쿼리 블록별로 최적화할 때는 각각의 최적이 쿼리문 전체의 최적을 달성하지 못할 때가 많다. 그리고 Plan Generator가 고려대상으로 삼을만한 다양한 실행계획을 생성해 내는 작업이 매우 제한적인 범위 내에서만 이루어진다. 실제 서브쿼리 Unnesting이 어떤 식으로 작동하는지 살펴보자. 아래처럼 IN 서브쿼리를 포함하는 SQL문이 있다.

```sql
SELECT *
  FROM emp
 WHERE deptno IN (SELECT deptno
                    FROM dept)
```

이 SQL문을 Unnesting하지 않고 그대로 최적화한다면 옵티마이저는 아래와 같이 필터 방식의 실행계획을 수립한다.

| Id |      Operation    |   Name  | Rows | Bytes | Cost (%CPU) |
|:--:|:-----------------:|:-------:|:----:|:-----:|:-----------:|
|  0 |  SELECT STATEMENT |         |   3  |   99  |    3 (0)    |
| *1 |       FILTER      |         |      |       |             |
|  2 | TABLE ACCESS FULL |    EMP  |  10  |  330  |    3 (0)    |
| *3 | INDEX UNIQUE SCAN | DEPT_PK |   1  |   2   |    0 (0)    |

- Predicate Information (identified by operation id):

1 - filter( EXISTS (SELECT 0 FROM "DEPT" "DEPT" WHERE "DEPTNO"=:B1))
3 - access("DEPTNO"=:B1)

Predicate 정보를 보면 필터 방식으로 수행된 서브쿼리의 조건절이 바인드 변수로 처리된 부분(DEPTNO = :B1)이 눈에 띄는데, 이것을 통해 옵티마이저가 서브쿼리를 별도의 서브플랜(Subplan)으로 최적화한다는 사실을 알 수 있다. 메인 쿼리도 하나의 쿼리 블록이므로 서브쿼리를 제외한 상태에서 별도로 최적화가 이루어졌다. (아무 조건절이 없으므로 Full Table Scan이 최적이다.) 이처럼, Unnesting하지 않은 서브쿼리를 수행할 때는 메인 쿼리에서 읽히는 레코드마다 값을 넘기면서 서브쿼리를 반복 수행한다. (내부적으로 IN 서브쿼리를 Exists 서브쿼리로 변환한다는 사실도 Predicate 정보를 통해 알 수 있다.) 위 서브쿼리가 Unnesting 되면, 변환된 쿼리는 아래와 같은 조인문 형태가 된다.

```sql
SELECT *
  FROM (SELECT
        deptno
          FROM dept) a, emp b
 WHERE b.deptno = a.deptno
```

그리고 이것은 바로 이어서 설명할 뷰 Merging 과정을 거쳐 최종적으로 아래와 같은 형태가 된다.

```sql
SELECT emp.*
  FROM dept, emp
 WHERE emp.deptno = dept.deptno
```

아래가 서브쿼리 Unnesting이 일어났을 때의 실행계획이다. 서브쿼리인데도 일반적인 Nested Loop 조인 방식으로 수행된 것을 볼 수 있다. 위 조인문을 수행할 때와 정확히 같은 실행계획이다.

```sql
SELECT *
  FROM emp
 WHERE deptno IN(SELECT deptno
                   FROM dept)
```

| Id |            Operation        |      Name      | Rows | Bytes | Cost (%CPU) |
|:--:|:---------------------------:|:--------------:|:----:|:-----:|:-----------:|
|  0 |       SELECT STATEMENT      |                |  10  |  350  |    2 (0)    |
|  1 | TABLE ACCESS BY INDEX ROWID |       EMP      |   3  |   99  |    1 (0)    |
|  2 |         NESTED LOOPS        |                |  10  |  350  |    2 (0)    |
|  3 |       INDEX FULL SCAN       |     DEPT_PK    |   4  |   8   |    1 (0)    |
| *4 |      INDEX RANGE SCAN       | EMP_DEPTNO_IDX |   3  |       |    0 (0)    |

- Predicate Information (identified by operation id):

4 - access("DEPTNO"="DEPTNO")

주의할 점은, 서브쿼리를 Unnesting한 결과가 항상 더 나은 성능을 보장하지 않는다는 사실이다. 따라서 최근 옵티마이저는 서브쿼리를 Unnesting 했을 때 쿼리 수행 비용이 더 낮은지를 비교해 보고 적용 여부를 판단하는 쪽으로 발전하고 있다. 기본적으로 옵티마이저에게 맡기는 것이 바람직하지만, 앞서 얘기했듯이 옵티마이저가 항상 완벽할 순 없으므로 사용자가 직접 이 기능을 제어할 필요성이 생긴다. 이를 위해 Oracle은 아래 두 가지 힌트를 제공하고 있다.

- unnest : 서브쿼리를 Unnesting 함으로써 조인방식으로 최적화하도록 유도한다.
- no_unnest : 서브쿼리를 그대로 둔 상태에서 필터 방식으로 최적화하도록 유도한다.
서브쿼리가 M쪽 집합이거나 Nonunique 인덱스일 때

지금까지 본 예제는 메인 쿼리의 `emp` 테이블과 서브쿼리의 `dept` 테이블이 M:1 관계이기 때문에 일반 조인문으로 바꾸더라도 쿼리 결과가 보장된다. 옵티마이저는 `dept` 테이블 `deptno` 컬럼에 PK 제약이 설정된 것을 통해 dept 테이블이 1쪽 집합이라는 사실을 알 수 있다. 따라서 안심하고 쿼리 변환을 실시한다. 만약 서브쿼리 쪽 테이블의 조인 컬럼에 PK/Unique 제약 또는 Unique 인덱스가 없다면, 일반 조인문처럼 처리했을 때 어떻게 될까?

#### 예시

```sql
SELECT *
  FROM dept
 WHERE deptno IN (SELECT deptno 
                    FROM emp)
```

위 쿼리는 1쪽 집합을 기준으로 M쪽 집합을 필터링하는 형태이므로 당연히 서브쿼리 쪽 `emp` 테이블 `deptno` 컬럼에는 `Unique` 인덱스가 없다. dept 테이블이 기준 집합이므로 결과집합은 이 테이블의 총 건수를 넘지 않아야 한다. 그런데 옵티마이저가 임의로 아래와 같은 일반 조인문으로 변환한다면 M쪽 집합인 `emp` 테이블 단위의 결과집합이 만들어지므로 결과 오류가 생긴다.

```sql
SELECT *
  FROM (SELECT deptno FROM emp) a, dept b
 WHERE b.deptno = a.deptno
```

```sql
SELECT *
  FROM emp
 WHERE deptno IN (SELECT deptno
                    FROM dept)
```

위 쿼리는 M쪽 집합을 드라이빙해 1쪽 집합을 서브쿼리로 필터링하도록 작성되었으므로 조인문으로 바꾸더라도 결과에 오류가 생기지는 않는다. 하지만 `dept` 테이블 `deptno` 컬럼에 PK/Unique 제약이나 Unique 인덱스가 없으면 옵티마이저는 `emp`와 `dept` 간의 관계를 알 수 없고, 결과를 확신할 수 없으니 일반 조인문으로의 쿼리 변환을 시도하지 않는다. (만약 SQL 튜닝 차원에서 위 쿼리를 사용자가 직접 조인문으로 바꿨는데, 어느 순간 `dept` 테이블 `deptno` 컬럼에 중복 값이 입력되면서 결과에 오류가 생기더라도 옵티마이저에게는 책임이 없다.) 이럴 때 옵티마이저는 두 가지 방식 중 하나를 선택하는데, Unnesting 후 어느 쪽 집합을 먼저 드라이빙 하느냐에 따라 달라진다.

1쪽 집합임을 확신할 수 없는 서브쿼리 쪽 테이블이 드라이빙된다면, 먼저 sort unique 오퍼레이션을 수행함으로써 1쪽 집합으로 만든 다음에 조인한다.
메인 쿼리 쪽 테이블이 드라이빙된다면 세미 조인(Semi Join) 방식으로 조인한다. 이것이 세미 조인(Semi Join)이 탄생하게 된 배경이다.
아래는 Sort Unique 오퍼레이션 방식으로 수행할 때의 실행계획이다.

```sql
ALTER table dept DROP PRIMARY KEY;
CREATE INDEX dept_deptno_idx ON dept(deptno);

SELECT *
  FROM emp
 WHERE deptno IN (SELECT deptno
                    FROM dept);
```

| Id |            Operation        |       Name      | Rows | Bytes |
|:--:|:---------------------------:|:---------------:|:----:|:-----:|
|  0 |       SELECT STATEMENT      |                 |  11  |  440  |
|  1 | TABLE ACCESS BY INDEX ROWID |        EMP      |   4  |  148  |
|  2 |         NESTED LOOPS        |                 |  11  |  440  |
|  3 |          SORT UNIQUE        |                 |   4  |   12  |
|  4 |       INDEX FULL SCAN       | DEPT_DEPTNO_IDX |   4  |   12  |
| *5 |      INDEX RANGE SCAN       |  EMP_DEPTNO_IDX |   5  |       |

- Predicate Information (identified by operation id):
5 - access("DEPTNO"="DEPTNO")

실제로 `dept` 테이블은 Unique한 집합이지만 옵티마이저는 이를 확신할 수 없어 sort unique 오퍼레이션을 수행하였다. 아래와 같은 형태로 쿼리 변환이 일어난 것이다.

```sql
SELECT b.*
  FROM (SELECT /*+ no_merge */ DISTINCT deptno
          FROM dept
         ORDER BY deptno) a, emp b
 WHERE b.deptno = a.deptno
```

아래는 세미 조인 방식으로 수행할 때의 실행계획이다.

```sql
SELECT *
  FROM emp
 WHERE deptno IN (SELECT deptno
                    FROM dept)
```

| Id |      Operation    |   Name   | Rows | Bytes | Cost (%CPU) |
|:--:|:-----------------:|:--------:|:----:|:-----:|:-----------:|
|  0 |  SELECT STATEMENT |          |  10  |  350  |    3 (0)    |
|  1 | NESTED LOOPS SEMI |          |  10  |  350  |    3 (0)    |
|  2 | TABLE ACCESS FULL |    EMP   |  10  |  330  |    3 (0)    |
| *3 |  INDEX RANGE SCAN | DEPT_IDX |   4  |   8   |    0 (0)    |

- Predicate Information (identified by operation id):
3 - access("DEPTNO"="DEPTNO")

NL 세미 조인으로 수행할 때는 sort unique 오퍼레이션을 수행하지 않고도 결과집합이 M쪽 집합으로 확장되는 것을 방지하는 알고리즘을 사용한다. 기본적으로 NL Join과 동일한 프로세스로 진행하지만, Outer (=Driving) 테이블의 한 로우가 Inner 테이블의 한 로우와 조인에 성공하는 순간 진행을 멈추고 Outer 테이블의 다음 로우를 계속 처리하는 방식이다. 아래 pseudo 코드를 참고한다면 어렵지 않게 이해할 수 있다.

```java
for(i=0; ; i++) {
    // outer loop for(j=0; ; j++)
    
    {
        // inner loop if(i==j) break;
    } 
}
```

### 뷰 Merging

아래 <쿼리1>처럼 인라인 뷰를 사용하면 쿼리 내용을 파악하기가 더 쉽다. 서브쿼리도 마찬가지다. 서브쿼리로 표현하면 아무래도 조인문보다 더 직관적으로 읽힌다.

```sql
SELECT *
  FROM (SELECT *
          FROM emp
         WHERE job = 'SALESMAN') a ,
       (SELECT *
          FROM dept
         WHERE loc = 'CHICAGO') b
 WHERE a.deptno = b.deptno
```

그런데 사람의 눈으로 볼 때는 쿼리를 블록화하는 것이 더 읽기 편할지 모르지만 최적화를 수행하는 옵티마이저의 시각에서는 더 불편하다. 그런 탓에 옵티마이저는 가급적 <쿼리2>처럼 쿼리 블록을 풀어내려는 습성을 갖는다. (옵티마이저 개발팀이 그렇게 만들었다.)

```sql
SELECT *
  FROM emp a, dept b
 WHERE a.deptno = b.deptno
       AND
       a.job = 'SALESMAN'
       AND
       b.loc = 'CHICAGO'
```

따라서 위에서 본 <쿼리1>의 뷰 쿼리 블록은 액세스 쿼리 블록(뷰를 참조하는 쿼리 블록)과의 머지(merge) 과정을 거쳐 <쿼리2>와 같은 형태로 변환되는데, 이를 ‘뷰 Merging’이라고 한다. 뷰를 Merging해야 옵티마이저가 더 다양한 액세스 경로를 조사 대상으로 삼을 수 있게 된다. 아래와 같이 조건절 하나만을 가진 단순한 emp_salesman 뷰가 있다.

```sql
CREATE OR REPLACE VIEW emp_salesman
    AS
SELECT empno, ename, job, mgr, hiredate, sal, comm, deptno
  FROM emp
 WHERE job = 'SALESMAN';
```

위 `emp_salesman` 뷰와 조인하는 간단한 조인문을 작성해 보자.

```sql
SELECT e.empno, e.ename, e.job, e.mgr, e.sal, d.dname
  FROM emp_salesman e, dept d
 WHERE d.deptno = e.deptno
       AND
       e.sal >= 1500 ;
```

위 쿼리를 뷰 Merging 하지 않고 그대로 최적화한다면 아래와 같은 실행계획이 만들어진다.

Execution Plan ------------------------------------------------------------- 
0 SELECT STATEMENT Optimizer=ALL_ROWS (Cost=3 Card=2 Bytes=156)
1 0 NESTED LOOPS (Cost=3 Card=2 Bytes=156)
2 1 VIEW OF 'EMP_SALESMAN' (VIEW) (Cost=2 Card=2 Bytes=130)
3 2 TABLE ACCESS (BY INDEX ROWID) OF 'EMP' (TABLE) (Cost=2 Card=2 )
4 3 INDEX (RANGE SCAN) OF 'EMP_SAL_IDX' (INDEX) (Cost=1 Card=7)
5 1 TABLE ACCESS (BY INDEX ROWID) OF 'DEPT' (TABLE) (Cost=1 Card=1 Bytes=13)
6 5 INDEX (UNIQUE SCAN) OF 'DEPT_PK' (INDEX (UNIQUE)) (Cost=0 Card=1)

뷰 Merging이 작동한다면 변환된 쿼리는 아래와 같은 모습일 것이다.

```sql
SELECT e.empno, e.ename, e.job, e.mgr, e.sal, d.dname
  FROM emp e, dept d
 WHERE d.deptno = e.deptno
       AND
       e.job = 'SALESMAN'
       AND
       e.sal >= 1500
```

그리고 이때의 실행계획은 다음과 같이 일반 조인문을 처리하는 것과 똑같은 형태가 된다.

Execution Plan -------------------------------------------------------------
0 SELECT STATEMENT Optimizer=ALL_ROWS (Cost=3 Card=2 Bytes=84)
1 0 NESTED LOOPS (Cost=3 Card=2 Bytes=84)
2 1 TABLE ACCESS (BY INDEX ROWID) OF 'EMP' (TABLE) (Cost=2 Card=2 Bytes=58)
3 2 INDEX (RANGE SCAN) OF 'EMP_SAL_IDX' (INDEX) (Cost=1 Card=7)
4 1 TABLE ACCESS (BY INDEX ROWID) OF 'DEPT' (TABLE) (Cost=1 Card=1 Bytes=13)
5 4 INDEX (UNIQUE SCAN) OF 'DEPT_PK' (INDEX (UNIQUE)) (Cost=0 Card=1)

위와 같이 단순한 뷰는 Merging하더라도 성능이 나빠지지 않는다. 하지만 아래와 같이 복잡한 연산을 포함하는 뷰를 Merging하면 오히려 성능이 더 나빠질 수도 있다.

- group by 절
- SELECT-list에 distinct 연산자 포함

따라서 뷰를 Merging했을 때 쿼리 수행 비용이 더 낮아지는지를 조사한 후에 적용 여부를 판단하는 쪽으로 옵티마이저가 발전하고 있다. 가급적 옵티마이저의 판단과 기능에 의존하는 것이 좋지만, 필요하다면 개발자가 이를 직접 조정할 줄도 알아야 한다. Oracle의 경우 이 기능을 제어할 수 있도록 merge와 no_merge 힌트를 제공하는데, 이를 사용하기에 앞서 실행계획을 통해 뷰 Merging이 발생했는지, 그리고 그것이 적정한지를 판단하는 능력이 더 중요하다. 아래는 뷰 Merging이 불가능한 경우인데, 힌트가 제공기도 한다.

- 집합(set) 연산자(union, union all, intersect, minus)
- connect by절
- ROWNUM pseudo 컬럼
- SELECT-list에 집계 함수(avg, count, max, min, sum) 사용
- 분석 함수(Analytic Function)

### 조건절 Pushing

옵티마이저가 뷰를 처리함에 있어 1차적으로 뷰 Merging을 고려하지만, 조건절(Predicate) Pushing을 시도할 수도 있다. 이는 뷰를 참조하는 쿼리 블록의 조건절을 뷰 쿼리 블록 안으로 밀어 넣는 기능을 말한다. 조건절이 가능한 빨리 처리되도록 뷰 안으로 밀어 넣는다면, 뷰 안에서의 처리 일량을 최소화하게 됨은 물론 리턴되는 결과 건수를 줄임으로써 다음 단계에서 처리해야 할 일량을 줄일 수 있다. 조건절 Pushing과 관련해 DBMS가 사용하는 기술로는 다음 3가지가 있다.

- 조건절(Predicate) Pushdown : 쿼리 블록 밖에 있는 조건절을 쿼리 블록 안쪽으로 밀어 넣는 것을 말함
- 조건절(Predicate) Pullup : 쿼리 블록 안에 있는 조건절을 쿼리 블록 밖으로 내오는 것을 말하며, 그것을 다시 다른 쿼리 블록에 Pushdown 하는 데 사용함
- 조인 조건(Join Predicate) Pushdown : NL Join 수행 중에 드라이빙 테이블에서 읽은 값을 건건이 Inner 쪽(=right side) 뷰 쿼리 블록 안으로 밀어 넣는 것을

#### 조건절(Predicate) Pushdown

group by절을 포함한 아래 뷰를 처리할 때, 쿼리 블록 밖에 있는 조건절을 쿼리 블록 안쪽에 밀어 넣을 수 있다면 group by 해야 할 데이터량을 줄일 수 있다. 인덱스 상황에 따라서는 더 효과적인 인덱스 선택이 가능해지기도 한다.

```sql
SELECT deptno, avg_sal
  FROM (SELECT deptno, AVG(sal) AS avg_sal
          FROM emp GROUP BY deptno) a
 WHERE deptno = 30
```

| Id |            Operation        |       Name     | Rows | Bytes |
|:--:|:---------------------------:|:--------------:|:----:|:-----:|
|  0 |        SELECT STATEMENT     |                |   1  |   26  |
|  1 |              VIEW           |                |   1  |   26  |
|  2 |     SORT GROUP BY NOSORT    |                |  10  |    7  |
|  3 | TABLE ACCESS BY INDEX ROWID |       EMP      |   6  |   42  |
| *4 |       INDEX RANGE SCAN      | EMP_DEPTNO_IDX |   6  |       |

- Predicate Information (identified by operation id):
4 - access("DEPTNO"=30)

위 쿼리에 정의한 뷰 내부에는 조건절이 하나도 없다. 만약 쿼리 변환이 작동하지 않는다면, `emp` 테이블을 Full Scan 하고서 `group by` 이후에 `deptno = 30` 조건을 필터링했을 것이다. 하지만, 조건절 Pushing이 작동함으로써 `emp_deptno_idx` 인덱스를 사용한 것을 실행계획에서 볼 수 있다. 이번에는 조인문으로 테스트해 보자.

```sql
SELECT b.deptno, b.dname, a.avg_sal
  FROM (SELECT deptno, AVG(sal) AS avg_sal
          FROM emp
         GROUP BY deptno) a, dept b
 WHERE a.deptno = b.deptno
       AND 
       b.deptno = 30
```

| Id |            Operation        |       Name     | Rows | Bytes |
|:--:|:---------------------------:|:--------------:|:----:|:-----:|
|  0 |        SELECT STATEMENT     |                |   1  |   39  |
|  1 |          NESTED LOOPS       |                |   1  |   39  |
|  2 | TABLE ACCESS BY INDEX ROWID |      DEPT      |   1  |   13  |
| *3 |       INDEX UNIQUE SCAN     |     DEPT_PK    |   1  |       |
|  4 |              VIEW           |                |   1  |   26  |
|  5 |         SORT GROUP BY       |                |   1  |    7  |
|  6 | TABLE ACCESS BY INDEX ROWID |       EMP      |   6  |   42  |
| *7 |      INDEX RANGE SCAN       | EMP_DEPTNO_IDX |   6  |       |

- Predicate Information (identified by operation id):
3 - access("B"."DEPTNO"=30)
7 - access("DEPTNO"=30)

위 실행계획과 Predicate Information을 보면, 인라인 뷰에 `deptno = 30` 조건절을 적용해 데이터량을 줄이고서 `group by`와 조인연산을 수행한 것을 알 수 있다. `deptno = 30` 조건이 인라인 뷰에 pushdown 될 수 있었던 이유는, 뒤에서 설명할 ‘조건절 이행’ 쿼리변환이 먼저 일어났기 때문이다. `b.deptno = 30` 조건이 조인 조건을 타고 a쪽에 전이됨으로써 아래와 같이 `a.deptno = 30` 조건절이 내부적으로 생성된 것이다. 이 상태에서 `a.deptno = 30` 조건절이 인라인 뷰 안쪽으로 Pushing 된 것이다.

```sql
SELECT b.deptno, b.dname, a.avg_sal
  FROM (SELECT deptno, AVG(sal) avg_sal
          FROM emp
         GROUP BY deptno) a, dept b
 WHERE a.deptno = b.deptno
       AND
       b.deptno = 30
       AND
       a.deptno = 30
```

#### 조건절(Predicate) Pullup

조건절을 쿼리 블록 안으로 밀어 넣을 뿐만 아니라 안쪽에 있는 조건들을 바깥 쪽으로 끄집어 내기도 하는데, 이를 ‘조건절(Predicate) Pullup’이라고 한다. 그리고 그것을 다시 다른 쿼리 블록에 Pushdown 하는 데 사용한다. 아래 실행계획을 보자.

```sql
SELECT *
  FROM (SELECT deptno, AVG(sal)
          FROM emp
         WHERE deptno = 10
         GROUP BY deptno) e1,
       (SELECT deptno, MIN(sal), MAX(sal)
          FROM emp
         GROUP BY deptno) e2
 WHERE e1.deptno = e2.deptno
```

| Id |            Operation        |       Name     | Rows | Bytes |
|:--:|:---------------------------:|:--------------:|:----:|:-----:|
|  0 |        SELECT STATEMENT     |                |   1  |   65  |
| *1 |           HASH JOIN         |                |   1  |   65  |
|  2 |              VIEW           |                |   1  |   26  |
|  3 |         HASH GROUP BY       |                |   1  |    5  |
|  4 | TABLE ACCESS BY INDEX ROWID |       EMP      |   5  |   25  |
| *5 |       INDEX RANGE SCAN      | EMP_DEPTNO_IDX |   5  |       |
|  4 |              VIEW           |                |   1  |   39  |
|  5 |         SORT GROUP BY       |                |   1  |    7  |
|  6 | TABLE ACCESS BY INDEX ROWID |       EMP      |   6  |   42  |
|  7 |         HASH GROUP BY       |                |   1  |    5  |
|  8 | TABLE ACCESS BY INDEX ROWID |       EMP      |   5  |   25  |
| *9 |      INDEX RANGE SCAN       | EMP_DEPTNO_IDX |   5  |       |

- Predicate Information (identified by operation id):
1 - access("E1"."DEPTNO"="E2"."DEPTNO")
5 - access("DEPTNO"=10)
9 - access("DEPTNO"=10)

인라인 뷰 `e2`에는 `deptno = 10` 조건이 없지만 Predicate 정보를 보면 양쪽 모두 이 조건이 `emp_deptno_idx` 인덱스의 액세스 조건으로 사용된 것을 볼 수 있다. 아래와 같은 형태로 쿼리 변환이 일어난 것이다.

```sql
SELECT *
  FROM (SELECT deptno, AVG(sal)
          FROM emp
         WHERE deptno = 10
         GROUP BY deptno) e1,
       (SELECT deptno, MIN(sal), MAX(sal)
          FROM emp
         WHERE deptno = 10
         GROUP BY deptno) e2
 WHERE e1.deptno = e2.deptno
```

#### 조인 조건(Join Predicate) Pushdown

‘조인 조건(Join Predicate) Pushdown’은 말 그대로 조인 조건절을 뷰 쿼리 블록 안으로 밀어 넣는 것으로서, NL Join 수행 중에 드라이빙 테이블에서 읽은 조인 컬럼 값을 Inner 쪽(=right side) 뷰 쿼리 블록 내에서 참조할 수 있도록 하는 기능이다. 아래 실행계획에서 group by절을 포함한 뷰를 액세스하는 단계에서 ‘view pushed predicate’ 오퍼레이션(id=3)이 나타났다. 그 아래 쪽에 `emp_deptno_idx` 인덱스가 사용된 것을 볼 수 있는데, 이는 `dept` 테이블로부터 넘겨진 `deptno`에 대해서만 `group by`를 수행함을 의미한다.

```sql
SELECT d.deptno, d.dname, e.avg_sal
  FROM dept d, (SELECT deptno, AVG(sal) avg_sal
                  FROM emp
                 GROUP BY deptno) e
 WHERE e.deptno(+) = d.deptno
```

| Id |            Operation        |       Name     | Rows | Bytes |
|:--:|:---------------------------:|:--------------:|:----:|:-----:|
|  0 |        SELECT STATEMENT     |                |   1  |  116  |
|  1 |       NESTED LOOPS OUTER    |                |   4  |  116  |
|  2 |       TABLE ACCESS FULL     |      DEPT      |   4  |   64  |
|  3 |    VIEW PUSHED PREDICATE    |                |   1  |   13  |
|  4 |            FILTER           |       EMP      |   6  |   42  |
|  5 |        SORT AGGREGATE       |                |      |       |
|  6 | TABLE ACCESS BY INDEX ROWID |       EMP      |   5  |   35  |
| *7 |      INDEX RANGE SCAN       | EMP_DEPTNO_IDX |   5  |       |

- Predicate Information (identified by operation id):
4 - filter(COUNT(*)>0) 7 - access("DEPTNO"="D"."DEPTNO")

이 기능은 부분범위처리가 필요한 상황에서 특히 유용한데, Oracle 11g에 이르러서야 구현되었다. 만약 위 SQL을 Oracle 10g 이하 버전에서 실행한다면, 조인 조건 Pushdown이 작동하지 않아 아래와 같이 `emp` 쪽 인덱스를 Full Scan하는 실행계획이 나타난다. `dept` 테이블에서 읽히는 `deptno`마다 `emp` 테이블 전체를 `group by` 하므로 성능상 불리한 것은 당연하다.

| Id |            Operation        |       Name     | Rows | Bytes |
|:--:|:---------------------------:|:--------------:|:----:|:-----:|
|  0 |        SELECT STATEMENT     |                |   4  |  148  |
|  1 |       NESTED LOOPS OUTER    |                |   4  |  148  |
|  2 |       TABLE ACCESS FULL     |      DEPT      |   4  |   44  |
| *3 |              VIEW           |                |   1  |   26  |
|  4 |         SORT GROUP BY       |                |   3  |   21  |
|  5 | TABLE ACCESS BY INDEX ROWID |       EMP      |  14  |   98  |
|  6 |       INDEX FULL SCAN       | EMP_DEPTNO_IDX |  14  |       |

- Predicate Information (identified by operation id):
3 - filter("E"."DEPTNO"(+)="D"."DEPTNO")

위 쿼리는 다행히 집계함수가 하나뿐이므로 10g 이하 버전이더라도 아래 처럼 쉽게 스칼라 서브쿼리로 변환함으로써 부분범위 처리가 가능하도록 할 수 있다.

```sql
SELECT d.deptno, d.dname ,(SELECT AVG(sal)
                             FROM emp
                            WHERE deptno = d.deptno)
  FROM dept d
```

집계함수가 여러 개일 때가 문제인데, 만약 아래와 같이 쿼리하면 `emp`에서 같은 범위를 반복적으로 액세스하는 비효율이 생긴다.

```sql
SELECT d.deptno, d.dname,
       (SELECT AVG(sal)
          FROM emp
         WHERE deptno = d.deptno) AS avg_sal,
       (SELECT MIN(sal)
          FROM emp
         WHERE deptno = d.deptno) AS min_sal,
       (SELECT MAX(sal)
          FROM emp
         WHERE deptno = d.deptno) AS max_sal
  FROM dept d
```

이럴 때는 아래 처럼 구하고자 하는 값들을 모두 결합하고서 바깥쪽 액세스 쿼리에서 `SUBSTR` 함수로 분리하는 방법이 유용할 수 있다.

```sql
SELECT deptno, dname,
       TO_NUMBER(SUBSTR(sal, 1, 7)) AS avg_sal,
       TO_NUMBER(SUBSTR(sal, 8, 7)) AS min_sal,
       TO_NUMBER(SUBSTR(sal, 15)) AS max_sal
  FROM (SELECT /*+ no_merge */ d.deptno, d.dname, (SELECT LPAD(AVG(sal), 7) || LPAD(MIN(sal), 7) || MAX(sal)
                                                     FROM emp
                                                    WHERE deptno = d.deptno) AS sal
          FROM dept d)
```

#### 조건절 이행

‘조건절 이행(Transitive Predicate Generation, Transitive Closure)’을 한마디로 요약하면, 「(A = B)이고 (B = C)이면 (A = C)이다」 라는 추론을 통해 새로운 조건절을 내부적으로 생성해 주는 쿼리변환이다. 「(A > B)이고 (B > C)이면 (A > C)이다」와 같은 추론도 가능하다. 예를 들어, A 테이블에 사용된 필터 조건이 조인 조건절을 타고 반대편 B 테이블에 대한 필터 조건으로 이행(移行)될 수 있다. 한 테이블 내에서도 두 컬럼간 관계정보(예를 들어, `col1 >= col2`)를 이용해 조건절이 이행된다.

```sql
SELECT *
  FROM dept d, emp e
 WHERE e.job = 'MANAGER'
       AND
       e.deptno = 10
       AND
       d.deptno = e.deptno
```

위 쿼리에서 `deptno = 10`은 `emp` 테이블에 대한 필터 조건이다. 하지만 아래 실행계획에 나타나는 Predicate 정보를 확인해 보면, `dept` 테이블에도 같은 필터 조건이 추가된 것을 볼 수 있다.

| Id |            Operation        |   Name  | Rows | Bytes | Cost (%CPU) |
|:--:|:---------------------------:|:-------:|:----:|:-----:|:-----------:|
|  0 |       SELECT STATEMENT      |         |   1  |   57  |    2 (0)    |
|  1 |         NESTED LOOPS        |         |   1  |   57  |    2 (0)    |
|  2 | TABLE ACCESS BY INDEX ROWID |   DEPT  |   1  |   20  |    1 (0)    |
| *3 |      INDEX UNIQUE SCAN      | DEPT_PK |   1  |       |    0 (0)    |
|  4 | TABLE ACCESS BY INDEX ROWID |   EMP   |   1  |   37  |    1 (0)    |
| *5 |      INDEX RANGE SCAN       | EMP_IDX |   1  |       |    0 (0)    |

- Predicate Information (identified by operation id):
 3 - access("D"."DEPTNO"=10) 5 - access("E"."DEPTNO"=10 AND "E"."JOB"='MANAGER')

[e.deptno = 10]이고 [e.deptno = d.deptno]이므로 [d.deptno = 10]으로 추론되었고, 이런 조건절 이행(transitive)을 통해 쿼리가 아래와 같은 형태로 변환된 것이다.

```sql
SELECT *
  FROM dept d, emp e
 WHERE e.job = 'MANAGER'
       AND
       e.deptno = 10
       AND
       d.deptno = 10
```

위와 같이 변환한다면, Hash Join 또는 Sort Merge Join을 수행하기 전에 `emp`와 `dept` 테이블에 각각 필터링을 적용함으로써 조인되는 데이터량을 줄일 수 있다. 그리고 `dept` 테이블 액세스를 위한 인덱스 사용을 추가로 고려할 수 있게 돼 더 나은 실행계획을 수립할 가능성이 커진다.


#### 불필요한 조인 제거

1:M 관계인 두 테이블을 조인하는 쿼리문에서 조인문을 제외한 어디에서도 1쪽 테이블을 참조하지 않는다면, 쿼리 수행 시 1쪽 테이블은 읽지 않아도 된다. 결과집합에 영향을 미치지 않기 때문이다. 옵티마이저는 이 특성을 이용해 M쪽 테이블만 읽도록 쿼리를 변환하는데, 이를 ‘조인 제거(Join Elimination)’ 또는 ‘테이블 제거(Table Elimination)’라고 한다.

```sql
SELECT e.empno, e.ename, e.deptno, e.sal, e.hiredate
  FROM dept d, emp e
 WHERE d.deptno = e.deptno
```

Rows Row Source Operation
---- ---------------------------------------------------
14 TABLE ACCESS FULL EMP (cr=8 pr=0 pw=0 time=58 us)

위 쿼리에서 조인 조건식을 제외하면 1쪽 집합인 `dept`에 대한 참조가 전혀 없다. 따라서 `emp` 테이블만 액세스한 것을 볼 수 있다. 이러한 쿼리 변환이 Oracle의 경우 10g부터 작동하기 시작했지만 SQL Server 등에서는 이미 오래 전부터 적용돼 온 기능이다. 조인 제거 기능이 작동하려면 아래와 같이 PK와 FK 제약이 설정돼 있어야만 한다. 이는 옵티마이저가 쿼리 변환을 수행하기 위한 지극히 당연한 조건이다. 만약 PK가 없으면 두 테이블 간 조인 카디널리티를 파악할 수 없고, FK가 없으면 조인에 실패하는 레코드가 존재할 수도 있어 옵티마이저가 함부로 쿼리 변환을 수행할 수가 없다.

```sql
ALTER TABLE dept ADD 2 CONSTRAINT deptno_pk PRIMARY KEY(deptno);
ALTER TABLE emp ADD 2 CONSTRAINT fk_deptno FOREIGN KEY(deptno) 3 REFERENCES dept(deptno);
```

FK가 설정돼 있더라도 `emp`의 `deptno` 컬럼이 `Null` 허용 컬럼이면 결과가 틀리게 될 수 있다. 조인 컬럼 값이 `Null`인 레코드는 조인에 실패해야 정상인데, 옵티마이저가 조인문을 함부로 제거하면 그 레코드들이 결과집합에 포함되기 때문이다. 이런 오류를 방지하기 위해 옵티마이저가 내부적으로 `e.deptno is not null` 조건을 추가해 준다. Outer 조인일 때는 `not null` 제약이나 `is not null` 조건은 물론, FK 제약이 없어도 논리적으로 조인 제거가 가능하지만, Oracle 10g까지는 아래에서 보듯 조인 제거가 일어나지 않았다.

```sql
SELECT e.empno, e.ename, e.sal, e.hiredate
  FROM emp e, dept d
 WHERE d.deptno(+) = e.deptno -- Outer 조인
```

Rows Row Source Operation
---- ---------------------------------------------------
15 NESTED LOOPS OUTER (cr=10 pr=0 pw=0 time=119 us)
15 TABLE ACCESS FULL EMP (cr=8 pr=0 pw=0 time=255 us)
14 INDEX UNIQUE SCAN DEPT_PK (cr=2 pr=0 pw=0 time=265 us)(Object ID 58557)

11g에서는 아래와 같이 불필요한 Inner 쪽 테이블 제거 기능이 구현된 것을 볼 수 있다.

```sql
SELECT e.empno, e.ename, e.sal, e.hiredate
  FROM emp e, dept d
 WHERE d.deptno(+) = e.deptno -- Outer 조인
```

Rows Row Source Operation
---- ---------------------------------------------------
14 TABLE ACCESS FULL EMP (cr=8 pr=0 pw=0 time=0 us cost=3 size=770 card=14)

아래는 SQL Server에서 테스트한 것인데, 마찬가지로 Inner 쪽 테이블이 제거된 것을 볼 수 있다.

```sql
SELECT e.empno, e.ename, e.sal, e.hiredate
  FROM dbo.emp e
  LEFT OUTER JOIN dbo.dept d
    ON d.deptno = e.deptno
```

'Emp' 테이블.
스캔 수 1
논리적 읽기 수 2
물리적 읽기 수 0
미리 읽기 수 0

SQL Server 실행 시간:
CPU 시간 = 0ms,
경과 시간 = 0ms.

Rows Executes StmtText
-- ----- --------------------------------------
| 14 | 1 | SELECT e.empno, e.ename, e.sal, e.hiredate |
| 14 | 1 |                                            |--Clustered Index Scan(OBJECT:([MyDB].[dbo].[Emp].[PK_Emp] AS [e]))

#### OR 조건을 Union으로 변환

아래 쿼리가 그대로 수행된다면 OR 조건이므로 Full Table Scan으로 처리될 것이다. (아니면, `job` 컬럼 인덱스와 `deptno` 컬럼 인덱스를 결합하고 비트맵 연산을 통해 테이블 액세스 대상을 필터링하는 Index Combine이 작동할 수도 있다.)

```sql
SELECT *
  FROM emp
 WHERE job = 'CLERK'
       OR
       deptno = 20
```

만약 `job`과 `deptno`에 각각 생성된 인덱스를 사용하고 싶다면 아래와 같이 union all 형태로 바꿔주면 된다.

```sql
SELECT *
  FROM emp
 WHERE job = 'CLERK'
 UNION ALL
SELECT *
  FROM emp
 WHERE deptno = 20
       AND
       LNNVL(job='CLERK')
```

사용자가 쿼리를 직접 바꿔주지 않아도 옵티마이저가 이런 작업을 대신해 주는 경우가 있는데, 이를 ‘OR-Expansion’이라고 한다. 아래는 OR-Expansion 쿼리 변환이 일어났을 때의 실행계획과 Predicate 정보다.

| Id |            Operation        |       Name     | Rows | Bytes |
|:--:|:---------------------------:|:--------------:|:----:|:-----:|
|  0 |       SELECT STATEMENT      |                |   7  |  224  |
|  1 |         CONCATENATION       |                |      |       |
|  2 | TABLE ACCESS BY INDEX ROWID |        EMP     |   3  |   96  |
| *3 |       INDEX RANGE SCAN      |   EMP_JOB_IDX  |   3  |       |
| *4 | TABLE ACCESS BY INDEX ROWID |        EMP     |   4  |  128  |
| *5 |       INDEX RANGE SCAN      | EMP_DEPTNO_IDX |   5  |       |

- Predicate Information (identified by operation id):
3 - access("JOB"='CLERK')
4 - filter(LNNVL("JOB"='CLERK'))
5 - access("DEPTNO"=20)

`job`과 `deptno` 컬럼을 선두로 갖는 두 인덱스가 각각 사용되었고, `union all` 위쪽 브랜치는 `job = ‘CLERK’`인 집합을 읽고 아래쪽 브랜치는 `deptno = 20`인 집합만을 읽는다. 분기된 두 쿼리가 각각 다른 인덱스를 사용하긴 하지만, `emp` 테이블 액세스가 두 번 일어난다. 따라서 중복 액세스되는 영역(`deptno=20`이면서 `job=‘CLERK`’)의 데이터 비중이 작을수록 효과적이고, 그 반대의 경우라면 오히려 쿼리 수행 비용이 증가한다. OR-Expansion 쿼리 변환이 처음부터 비용기반으로 작동한 것도 이 때문이다. 중복 액세스되더라도 결과집합에는 중복이 없게 하려고 `union all` 아래쪽에 Oracle이 내부적으로 LNNVL 함수를 사용한 것을 확인하기 바란다. job < > ‘CLERK’ 이거나 `job is null`인 집합만을 읽으려는 것이며, 이 함수는 조건식이 false이거나 알 수 없는(Unknown) 값일 때 true를 리턴한다. Oracle에서 OR-Expansion을 제어하기 위해 사용하는 힌트로는 use_concat과 no_expand 두 가지가 있다. use_concat은 OR-Expansion을 유도하고자 할 때 사용하고, no_expand는 이 기능을 방지하고자 할 때 사용한다.

```sql
SELECT /*+ USE_CONCAT */ *
  FROM emp
 WHERE job = 'CLERK'
       or
       deptno = 20;

SELECT /*+ NO_EXPAND */ *
  FROM emp
 WHERE job = 'CLERK'
       OR
       deptno = 20;
```

#### 기타 쿼리 변환

##### 집합 연산을 조인으로 변환

`INTERSECT`나 `MINUS` 같은 집합(Set) 연산을 조인 형태로 변환하는 것을 말한다. 아래는 `deptno = 10`에 속한 사원들의 `job`, `mgr`을 제외시키고 나머지 `job`, `mgr` 집합만을 찾는 쿼리인데, 각각 Sort Unique 연산을 수행한 후에 `MINUS` 연산을 수행하는 것을 볼 수 있다.

```sql
SELECT job, mgr
  FROM emp
 MINUS
SELECT job, mgr
  FROM emp 4
 WHERE deptno = 10;
```

| Id |       Operation   | Name | Rows | Bytes | Cost (%CPU) |   Time   |
|:--:|:-----------------:|:----:|:----:|:-----:|:-----------:|:--------:|
|  0 |  SELECT STATEMENT |      |  14  |  362  |    8 (53)   | 00:00:01 |
|  1 |        MINUS      |      |      |       |             |          |
|  2 |     SORT UNIQUE   |      |  14  |  266  |    4 (25)   | 00:00:01 |
|  3 | TABLE ACCESS FULL |  EMP |  14  |  266  |    3 (0)    | 00:00:01 |
|  4 |     SORT UNIQUE   |      |   3  |   96  |    4 (25)   | 00:00:01 |
| *5 | TABLE ACCESS FULL |  EMP |   3  |   96  |    3 (0)    | 00:00:01 |

- Predicate Information (identified by operation id):
5 - filter("DEPTNO"=10)

아래는 옵티마이저가 `Minus` 연산을 조인 형태로 변환했을 때의 실행계획이다.

| Id |       Operation   | Name | Rows | Bytes | Cost (%CPU) |   Time   |
|:--:|:-----------------:|:----:|:----:|:-----:|:-----------:|:--------:|
|  0 |  SELECT STATEMENT |      |  13  |  663  |    8 (25)   | 00:00:01 |
|  1 |    HASH UNIQUE    |      |  13  |  663  |    8 (25)   | 00:00:01 |
| *2 |    HASH JOIN ANTI |      |  13  |  663  |    7 (15)   | 00:00:01 |
|  3 | TABLE ACCESS FULL |  EMP |  14  |  266  |    3 (0)    | 00:00:01 |
| *4 | TABLE ACCESS FULL |  EMP |   3  |   96  |    3 (0)    | 00:00:01 |

- Predicate Information (identified by operation id):
2 - access(SYS_OP_MAP_NONNULL("JOB")=SYS_OP_MAP_NONNULL("JOB") AND SYS_OP_MAP_NONNULL("MGR")=SYS_OP_MAP_NONNULL("MGR"))
4 - filter("DEPTNO"=10)

해시 Anti 조인을 수행하고 나서 중복 값을 제거하기 위한 Hash Unique 연산을 수행하는 것을 볼 수 있다. 아래와 같은 형태로 쿼리 변환이 일어난 것이다.

```sql
SELECT DISTINCT job, mgr
  FROM emp e
 WHERE NOT EXISTS (SELECT 'x'
                     FROM emp
                    WHERE deptno = 10
                          AND
                          sys_op_map_nonnull(job) = sys_op_map_nonnull(e.job)
                          AND sys_op_map_nonnull(mgr) = sys_op_map_nonnull(e.mgr)) ;
```

Oracle의 sys_ p_map_nonnull 함수는 비공식적인 함수지만 가끔 유용하게 사용할 수 있다. `NULL` 값끼리 ‘=’ 비교(null = null)하면 false이지만 true가 되도록 처리해야 하는 경우가 있고, 그럴 때 이 함수를 사용하면 된다. 위에서는 `job`과 `mgr`이 `null` 허용 컬럼이기 때문에 위와 같은 처리가 일어났다.


##### 조인 컬럼에 IS NOT NULL 조건 추가

```sql
SELECT COUNT(e.empno), COUNT(d.dname)
  FROM emp e, dept d
 WHERE d.deptno = e.deptno
       AND
       sal <= 2900
```

위와 같은 조인문을 처리할 때 조인 컬럼 `deptno`가 `null`인 데이터는 조인 액세스가 불필요하다. 어차피 조인에 실패하기 때문이다. 따라서 아래와 같이 필터 조건을 추가해 주면 불필요한 테이블 액세스 및 조인 시도를 줄일 수 있어 쿼리 성능 향상에 도움이 된다.

```sql
SELECT COUNT(e.empno), COUNT(d.dname)
  FROM emp e, dept d
 WHERE d.deptno = e.deptno
       AND
       sal <= 2900
       AND
       e.deptno IS NOT NULL
       AND
       d.deptno IS NOT NULL;
```

`is not null` 조건을 사용자가 직접 기술하지 않더라도, 옵티마이저가 필요하다고 판단되면(Oracle의 경우, null 값 비중이 5% 이상일 때) 내부적으로 추가해 준다.


##### 필터 조건 추가

아래와 같이 바인드 변수로 `BETWEEN` 검색하는 쿼리가 있다고 하자. 쿼리를 수행할 때 사용자가 :mx보다 :mn 변수에 더 큰 값을 입력한다면 쿼리 결과는 공집합이다.

```sql
SELECT *
  FROM emp
 WHERE sal BETWEEN :mn and :mx
```

사전에 두 값을 비교해 알 수 있음에도 쿼리를 실제 수행하고서야 공집합을 출력한다면 매우 비합리적이다. 잦은 일은 아니겠지만 초대용량 테이블을 조회하면서 사용자가 값을 거꾸로 입력하는 경우를 상상해 보라. Oracle 9i부터 이를 방지하려고 옵티마이저가 임의로 필터 조건식을 추가해 준다. 아래 실행계획에서 1번 오퍼레이션 단계에 사용된 Filter Predicate 정보를 확인하기 바란다.


| Id |       Operation   | Name | Rows | Bytes | Cost |
|:--:|:-----------------:|:----:|:----:|:-----:|:----:|
|  0 |  SELECT STATEMENT |      |   1  |   32  |   2  |
| *1 |       FILTER      |      |      |       |      |
| *2 | TABLE ACCESS FULL |  EMP |   1  |   32  |   2  |

- Predicate Information (identified by operation id):
1 - filter(TO_NUMBER(:MN)<=TO_NUMBER(:MX))
2 - filter("EMP"."SAL">=TO_NUMBER(:MN) AND "EMP"."SAL"<=TO_NUMBER(:MX))

아래는 :mn에 5000, :mx에 100을 입력하고 실제 수행했을 때의 결과인데, 블록 I/O가 전혀 발생하지 않은 것을 볼 수 있다. 실행계획 상으로는 Table Full Scan을 수행하고 나서 필터 처리가 일어나는 것 같지만 실제로는 Table Full Scan 자체를 생략한 것이다.

- Statistics

0 recursive calls 0 db block gets 0 consistent gets 0 physical reads .. .....

##### 조건절 비교 순서


| A | ... |   1 |   1 |   1 |   1 |   1 |   1 |   1 |   1 |   1 |   1 |    1 |    1 | ... |
| B | ... | 990 | 991 | 992 | 993 | 994 | 995 | 996 | 997 | 998 | 999 | 1000 | 1001 | ... |

위 데이터를 아래 SQL문으로 검색하면 `B` 컬럼에 대한 조건식을 먼저 평가하는 것이 유리하다. 왜냐하면, 대부분 레코드가 `B = 1000` 조건을 만족하지 않아 `A` 컬럼에 대한 비교 연산을 수행하지 않아도 되기 때문이다.

```sql
SELECT *
  FROM T
 WHERE A = 1
       AND
       B = 1000 ;
```

반대로 `A = 1` 조건식을 먼저 평가한다면, `A` 컬럼이 대부분 1이어서 `B` 컬럼에 대한 비교 연산까지 그만큼 수행해야 하므로 CPU 사용량이 늘어날 것이다. 아래와 같은 조건절을 처리할 때도 부등호(`>`) 조건을 먼저 평가하느냐 `like` 조건을 먼저 평가하느냐에 따라 일량에 차이가 생긴다.

```sql
SELECT /*+ full(도서) */ 도서번호, 도서명, 가격, 저자, 출판사, isbn
  FROM 도서
 WHERE 도서명 LIKE '데이터베이스%' -- 사용자가 입력한 검색 키워드
       AND
       도서명 > '데이터베이스성능고도?서명;
```

DBMS 또는 버전에 따라 다르지만, 예전 옵티마이저는 `WHERE` 절에 기술된 순서 또는 반대 순서로 처리하는 내부 규칙을 따름으로써 비효율을 야기하곤 했다. 하지만 최신 옵티마이저는 비교 연산해야 할 일량을 고려해 선택도가 낮은 컬럼의 조건식부터 처리하도록 내부적으로 순서를 조정한다.

## 참고자료

- 한국데이터산업진흥원, *SQL 자격검정 실전문제*(서울: 한국데이터산업진흥원, 2016), 279.
- 조시형, *친절한 SQL 튜닝*(서울: 디비안, 2018), 560.
- [admin, *옵티마이저*, 데이터온에어, 2021-02-15](https://dataonair.or.kr/db-tech-reference/d-guide/sql/?pageid=2&mod=document&uid=364){: target="_blank" }
- [admin, *옵티마이저와 실행계획*, 데이터온에어, 2021-02-15](https://dataonair.or.kr/db-tech-reference/d-guide/sql/?pageid=3&mod=document&uid=354){: target="_blank" }
- [admin, *쿼리변환*, 데이터온에어, 2021-02-15](https://dataonair.or.kr/db-tech-reference/d-guide/sql/?uid=365&mod=document&pageid=1){: target="_blank" }
- 조시형, *국가공인 SQLP 자격검정 핵심노트 Ⅰ*(서울: 디비안, 2021), 272
- 조시형, *국가공인 SQLP 자격검정 핵심노트 Ⅱ*(서울: 디비안, 2021), 278