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
|            |             cursor_sharing_exact             | |
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

## 참고자료

- 한국데이터산업진흥원, *SQL 자격검정 실전문제*(서울: 한국데이터산업진흥원, 2016), 279.
- 조시형, *친절한 SQL 튜닝*(서울: 디비안, 2018), 560.
- [admin, *옵티마이저*, 데이터온에어, 2021-02-15](https://dataonair.or.kr/db-tech-reference/d-guide/sql/?pageid=2&mod=document&uid=364){: target="_blank" }
- [admin, *옵티마이저와 실행계획*, 데이터온에어, 2021-02-15](https://dataonair.or.kr/db-tech-reference/d-guide/sql/?pageid=3&mod=document&uid=354){: target="_blank" }
- 조시형, *국가공인 SQLP 자격검정 핵심노트 Ⅰ*(서울: 디비안, 2021), 272
- 조시형, *국가공인 SQLP 자격검정 핵심노트 Ⅱ*(서울: 디비안, 2021), 278