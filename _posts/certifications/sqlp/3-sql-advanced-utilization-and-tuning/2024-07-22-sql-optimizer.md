---
title: "[SQLP | 3과목]SQL 고급활용 및 튜닝 - SQL 옵티마이저(2024)"
categories: [Certifications, SQLP]
tags: [Certification, 자격증, SQLP]
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

![type-of-optimizer](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/sql-optimizer/type-of-optimizer.jpg)
*옵티마이저의 종류*

- 사용자가 원하는 작업을 가장 효율적으로 수행할 수 있는 **최적의 데이터 엑세스 경로(실행계획)를 선택해주는 DBMS의 핵심 엔진**
  +  옵티마이저가 선택한 실행 방법의 적절성 여부는 질의의 수행 속도에 가장 큰 영향 미치게 됨
- 자동차의 내비게이션과 흡사
  + 경로를 검색하고 이동 경로를 미리 확인하는 기능
  + 선택한 경로가 마음에 들지 않으면 검색모드를 변경하거나 경유지를 추가해서 운전자가 원하는 경로로 변경하는 기능
  + 모의 주행 기능
- **현재 대부분의 관계형 데이터베이스는 비용기반 옵티마이저만을 제공**
  + 규칙기반 옵티마이저를 제공하더라도 신규 기능들에 대해서는 더 이상 지원하지 않음

> SQL 옵티마이저를 DBWR, LGWR, PMON, SMON 같은 백그라운드 프로세스로 이해하기 쉽다. 서버 프로세스가 SQL을 전달하면, 옵티마이저가 최적화해서 실행계획을 돌려준다고 생각하는 것이다. 하지만, **옵티마이저는 별도 프로세스가 아니라 서버 프로세스가 가진 기능(Function)**일 뿐이다. SQL 파서와 로우 소스 생성기도 마찬가지다.
{: .prompt-info }

#### 옵티마이저 최적화 단계

![1-3](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/sql-optimizer/1-3.jpg)

1. 사용자로부터 전달받은 쿼리를 수행하는 데 후보군이 될만한 실행계획들을 찾음
2. 데이터 딕셔너리(Data Dictionary)에 미리 수집해둔 오브젝트 통계 및 시스템 통계정보를 이용해 각 실행계획의 예상비용을 산정
3. 최저 비용(Cost)을 나타내는 실행계획 선택

#### 옵티마이저의 종류

##### 규칙기반 옵티마이저

- 규칙(우선 순위)을 가지고 실행계획을 생성

규칙기반 옵티마이저는 규칙(우선 순위)을 가지고 실행계획을 생성한다. 실행계획을 생성하는 규칙을 이해하면 누구나 실행계획을 비교적 쉽게 예측할 수 있다. 규칙기반 옵티마이저가 실행계획을 생성할 때 참조하는 정보에는 SQL문을 실행하기 위해서 이용 가능한 인덱스 유무와 (유일, 비유일, 단일, 복합 인덱스)종류, SQL문에서 사용하는 연산자(=, <, <>, LIKE, BETWEEN 등)의 종류 그리고 SQL문에서 참조하는 객체(힙 테이블, 클러스터 테이블 등)의 종류 등이 있다. 이러한 정보에 따라 우선 순위(규칙)가 정해져 있고, 이 우선 순위를 기반으로 실행계획을 생성한다. 결과적으로 규칙기반 옵티마이저는 우선 순위가 높은 규칙이 적은 일량으로 해당 작업을 수행하는 방법이라고 판단하는 것이다. [그림 Ⅱ-3-2]는 Oracle의 규칙기반 옵티마이저의 15가지 규칙이다. 순위의 숫자가 낮을수록 높은 우선 순위이다.

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

규칙기반 옵티마이저의 우선 순위 규칙 중에서 주요한 규칙에 대해서만 간략히 설명한다.

규칙 1. Single row by rowid : ROWID를 통해서 테이블에서 하나의 행을 액세스하는 방식이다. ROWID는 행이 포함된 데이터 파일, 블록 등의 정보를 가지고 있기 때문에 다른 정보를 참조하지 않고도 바로 원하는 행을 액세스할 수 있다. 하나의 행을 액세스하는 가장 빠른 방법이다.

규칙 4. Single row by unique or primary key : 유일 인덱스(Unique Index)를 통해서 하나의 행을 액세스하는 방식이다. 이 방식은 인덱스를 먼저 액세스하고 인덱스에 존재하는 ROWID를 추출하여 테이블의 행을 액세스한다.

칙 8. Composite index : 복합 인덱스에 동등(‘=’ 연산자) 조건으로 검색하는 경우이다. 예를 들어, 만약 A+B 칼럼으로 복합 인덱스가 생성되어 있고, 조건절에서 WHERE A=10 AND B=1 형태로 검색하는 방식이다. 복합 인덱스 사이의 우선 순위 규칙은 다음과 같다. 인덱스 구성 칼럼의 개수가 더 많고 해당 인덱스의 모든 구성 칼럼에 대해 ‘=’로 값이 주어질 수록 우선순위가 더 높다. 예를 들어, A+B로 구성된 인덱스와 A+B+C로 구성된 인덱스가 각각 존재하고 조건절에서 A, B, C 칼럼 모두에 대해 ‘=’로 값이 주어진다면 A+B+C 인덱스가 우선 순위가 높다. 만약 조건절에서 A, B 칼럼에만 ‘=’로 값이 주어진다면 A+B는 인덱스의 모든 구성 칼럼에 대해 값이 주어지고 A+B+C 인덱스 입장에서는 인덱스의 일부 칼럼에 대해서만 값이 주어졌기 때문에 A+B 인덱스가 우선 순위가 높게 된다.

규칙 9. Single column index : 단일 칼럼 인덱스에 ‘=’ 조건으로 검색하는 경우이다. 만약 A 칼럼에 단일 칼럼 인덱스가 생성되어 있고, 조건절에서 A=10 형태로 검색하는 방식이다.

규칙 10. Bounded range search on indexed columns : 인덱스가 생성되어 있는 칼럼에 양쪽 범위를 한정하는 형태로 검색하는 방식이다. 이러한 연산자에는 BETWEEN, LIKE 등이 있다. 만약 A 칼럼에 인덱스가 생성되어 있고, A BETWEEN ‘10’ AND ‘20’ 또는 A LIKE '1%' 형태로 검색하는 방식이다.

규칙 11. Unbounded range search on indexed columns : 인덱스가 생성되어 있는 칼럼에 한쪽 범위만 한정하는 형태로 검색하는 방식이다. 이러한 연산자에는 >, >=, <, <= 등이 있다. 만약 A 칼럼에 인덱스가 생성되어 있고, A > '10' 또는 A < '20' 형태로 검색하는 방식이다.

규칙 15. Full table scan : 전체 테이블을 액세스하면서 조건절에 주어진 조건을 만족하는 행만을 결과로 추출한다.

규칙기반 옵티마이저는 인덱스를 이용한 액세스 방식이 전체 테이블 액세스 방식보다 우선 순위가 높다. 따라서 규칙기반 옵티마이저는 해당 SQL문에서 이용 가능한 인덱스가 존재한다면 전체 테이블 액세스 방식보다는 항상 인덱스를 사용하는 실행계획을 생성한다. 규칙기반 옵티마이저가 조인 순서를 결정할 때는 조인 칼럼 인덱스의 존재 유무가 중요한 판단의 기준이다. 조인 칼럼에 대한 인덱스가 양쪽 테이블에 모두 존재한다면 앞에서 설명한 규칙에 따라 우선 순위가 높은 테이블을 선행 테이블(Driving Table)로 선택한다. 한쪽 조인 칼럼에만 인덱스가 존재하는 경우에는 인덱스가 없는 테이블을 선행 테이블로 선택해서 조인을 수행한다. 조인 칼럼에 모두 인덱스가 존재하지 않으면 FROM 절의 뒤에 나열된 테이블을 선행 테이블로 선택한다. 만약 조인 테이블의 우선 순위가 동일하다면 FROM 절에 나열된 테이블의 역순으로 선행 테이블을 선택한다. 규칙기반 옵티마이저의 조인 기법의 선택은 다음과 같다. 양쪽 조인 칼럼에 모두 인덱스가 없는 경우에는 Sort Merge Join을 사용하고 둘 중하나라도 조인 칼럼에 인덱스가 존재한다면 일반적으로 NL Join을 사용한다.

다음 SQL문을 이용해서 규칙기반 옵티마이저의 최적화 과정을 알아보자.


SELECT ENAME FROM EMP WHERE JOB = 'SALESMAN' AND SAL BETWEEN 3000 AND 6000 INDEX --------------------------------- EMP_JOB : JOB EMP_SAL : SAL PK_EMP : EMPNO (UNIQUE)

조건절에서 JOB 칼럼의 조건은 ‘=’, SAL 칼럼의 조건은 ‘BETWEEN’으로 값이 주어졌고 각각의 칼럼에 단일 칼럼 인덱스가 존재한다. 우선 순위 규칙에 따라 JOB 조건은 규칙 9의 단일 칼럼 인덱스를 만족하고 SAL 조건은 규칙 10의 인덱스상의 양쪽 한정 검색을 만족한다. 따라서 우선 순위가 높은 EMP_JOB 인덱스를 이용해서 조건을 만족하는 행에 대해 EMP 테이블을 액세스하는 방식을 선택할 것이다. 다음은 규칙기반 옵티마이저가 생성한 실행계획이다.


Execution Plan ------------------------------------------------------------ SELECT STATEMENT Optimizer=CHOOSE TABLE ACCESS (BY INDEX ROWID) OF 'EMP' INDEX (RANGE SCAN) OF 'EMP_JOB' (NON-UNIQUE)

##### 비용기반 옵티마이저

- SQL문을 처리하는데 필요한 비용이 가장 적은 실행계획을 선택하는 방식
- 규칙기반 옵티마이저의 단점을 극복하기 위해서 출현

규칙기반 옵티마이저는 조건절에서 ‘=’ 연산자와 'BETWEEN' 연산자가 사용되면 규칙에 따라 ‘=’ 칼럼의 인덱스를 사용하는 것이 보다 적은 일량 즉, 보다 적은 처리 범위로 작업을 할 것이라고 판단한다. 그러나 실제로는 ‘BETWEEN’ 칼럼을 사용한 인덱스가 보다 일량이 적을 수 있다. 단순한 몇 개의 규칙만으로 현실의 모든 사항을 정확히 예측할 수는 없다. 비용기반 옵티마이저는 이러한 규칙기반 옵티마이저의 단점을 극복하기 위해서 출현하였다. 비용기반 옵티마이저는 SQL문을 처리하는데 필요한 비용이 가장 적은 실행계획을 선택하는 방식이다. 여기서 비용이란 SQL문을 처리하기 위해 예상되는 소요시간 또는 자원 사용량을 의미한다. 비용기반 옵티마이저는 비용을 예측하기 위해서 규칙기반 옵티마이저가 사용하지 않는 테이블, 인덱스, 칼럼 등의 다양한 객체 통계정보와 시스템 통계정보 등을 이용한다. 통계정보가 없는 경우 비용기반 옵티마이저는 정확한 비용 예측이 불가능해져서 비효율적인 실행계획을 생성할 수 있다. 그렇기 때문에 정확한 통계정보를 유지하는 것은 비용기반 최적화에서 중요한 요소이다.

![optimizer-based-cost](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/sql-optimizer/optimizer-based-cost.jpg)

### 실행계획(Execution Plan)과 비용

#### 실행계획

실행계획(Execution Plan)이란 SQL에서 요구한 사항을 처리하기 위한 절차와 방법을 의미한다. 실행계획을 생성한다는 것은 SQL을 어떤 순서로 어떻게 실행할 지를 결정하는 작업이다. 동일한 SQL에 대해 결과를 낼 수 있는 다양한 처리 방법(실행계획)이 존재할 수 있지만 각 처리 방법마다 실행 시간(성능)은 서로 다를 수 있다. 옵티마이저는 다양한 처리 방법들 중에서 가장 효율적인 방법을 찾아준다. 즉, 옵티마이저는 최적의 실행계획을 생성해 준다. 생성된 실행계획을 보는 방법은 데이터베이스 벤더마다 서로 다르다. 실행계획에서 표시되는 내용 및 형태도 약간씩 차이는 있지만 실행계획이 SQL 처리를 위한 절차와 방법을 의미한다는 기본적인 사항은 모두 동일하다. 실행계획을 보고 SQL이 어떻게 실행되는지 정확히 이해할 수 있다면 보다 향상된 SQL의 이해 및 활용이 가능하다. Oracle의 실행계획 형태는 [그림 Ⅱ-3-4]와 같다. 실행계획을 구성하는 요소에는 조인 순서(Join Order), 조인 기법(Join Method), 액세스 기법(Access Method), 최적화 정보(Optimization Information), 연산(Operation) 등이 있다

![11-components-of-execution-plan](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/sql-optimizer/11-components-of-execution-plan.jpg)
*실행계획 정보의 구성요소*

```sql
SELECT STATEMENT Optimizer=ALL_ROWS(Cost=209 Card=5 Bytes=175)
  TABLE ACCESS (BY INDEX ROWID) OF 'EMP' (Cost=2 Card=5 Bytes=85)
    NESTED LOOPS (Cost=209 Card=5 Bytes=175)
      TABLE ACCESS (BY INDEX ROWID) OF 'DEPT' (Cost=207 Card=1 Bytes=18)
        INDEX (RANGE SCAN) OF 'DEPT_LOC_IDX'(NON-UNIQUE) (Cost=7 Card=1)
      INDEX (RANGE SCAN) OF 'EMP_DEPTNO_IDX'(NON-UNIQUE) (Cost-1 Card=5)
```

조인 순서는 조인작업을 수행할 때 참조하는 테이블의 순서이다. 예를 들어, FROM 절에 A, B 두 개의 테이블이 존재할 때 조인 작업을 위해 먼저 A 테이블을 읽고 B 테이블을 읽는 작업을 수행한다면 조인 순서는 A → B이다. [그림 Ⅱ-3-4]에서 조인 순서는 EMP → DEPT이다. 논리적으로 가능한 조인 순서는 n! 만큼 존재한다. 여기서는 n은 FROM 절에 존재하는 테이블 수이다. 그러나 현실적으로 옵티마이저가 적용 가능한 조인 순서는 이보다는 적거나 같다. 조인 기법은 두 개의 테이블을 조인할 때 사용할 수 있는 방법으로서 여기에는 NL Join, Hash Join, Sort Merge Join 등이 있다. [그림 Ⅱ-3-4]에서 조인 기법은 NL Join을 사용하고 있다. 액세스 기법은 하나의 테이블을 액세스할 때 사용할 수 있는 방법이다. 여기에는 인덱스를 이용하여 테이블을 액세스하는 인덱스 스캔(Index Scan)과 테이블 전체를 모두 읽으면서 조건을 만족하는 행을 찾는 전체 테이블 스캔(Full Table Scan) 등이 있다. [그림 Ⅱ-3-4]에서 액세스 기법은 인덱스 스캔을 사용하고 있다. 최적화 정보는 옵티마이저가 실행계획의 각 단계마다 예상되는 비용 사항을 표시한 것이다. 실행계획에 비용 사항이 표시된다는 것은 비용기반 최적화 방식으로 실행계획을 생성했다는 것을 의미한다. 최적화 정보에는 Cost, Card, Bytes가 있다. Cost는 상대적인 비용 정보이고 Card는 Cardinality의 약자로서 주어진 조건을 만족한 결과 집합 혹은 조인 조건을 만족한 결과 집합의 건수를 의미한다. Bytes는 결과 집합이 차지하는 메모리 양을 바이트로 표시한 것이다. 이러한 비용 정보는 실제로 SQL을 실행하고 얻은 결과가 아니라 통계 정보를 바탕으로 옵티마이저가 계산한 예상치이다. 만약 이러한 비용 사항이 실행계획에 표시되지 않았다면 이것은 규칙기반 최적화 방식으로 실행계획을 생성한 것이다. [그림 Ⅱ-3-4] 실행계획의 예에서는 비용 정보가 표시되어 있으므로 비용기반 최적화 방식으로 생성된 실행계획이다. 연산(Operation)은 여러 가지 조작을 통해서 원하는 결과를 얻어내는 일련의 작업이다. 연산에는 조인 기법(NL Join, Hash Join, Sort Merge Join 등), 액세스 기법(인덱스 스캔, 전체 테이블 스캔 등), 필터, 정렬, 집계, 뷰 등 다양한 종류가 존재한다. 예를 들어, SQL에서 정렬을 목적으로 ORDER BY를 수행했다면 정렬 연산이 표시되다.

+ SQL 옵티마이저가 생성한 처리절차를 사용자가 확인할 수 있게 위와 같이 트리 구조로 표현한 것
+ 실행계획을 통해 자신이 작성한 SQL이 테이블을 스캔하는지 인덱스를 스캔하는지 확인할 수 있음
  * 인덱스를 스캔한다면 어떤 인덱스인지를 확인할 수 있고, 예상과 다른 방식으로 처리된다면 실행경로를 변경할 수 있음

#### 비용

- 비용은 쿼리를 수행하는 동안 발생할 것으로 예상하는 I/O 횟수 또는 예상 소요시간을 표현한 값
- SQL 실행계획에 표시되는 비용은 실측치가 아닌 예상치임

##### 예시

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

SQL 처리 흐름도(Access Flow Diagram)란 SQL의 내부적인 처리 절차를 시각적으로 표현한 도표이다. 이것은 실행계획을 시각화한 것이다. [그림 Ⅱ-3-5]와 같이 액세스 처리 흐름도에는 SQL문의 처리를 위해 어떤 테이블을 먼저 읽었는지(조인 순서), 테이블을 읽기 위해서 인덱스 스캔을 수행했는지 또는 테이블 전체 스캔을 수행했는지(액세스 기법)과 조인 기법 등을 표현할 수 있다. 예를 들어, [그림 Ⅱ-3-5]에서 조인 순서는 TAB1 → TAB2이다. 여기서 TAB1을 Outer Table 또는 Driving Table이라고 하고, TAB2를 Inner Table 또는 Lookup Table이라고 한다. 테이블의 액세스 방법은 TAB1은 테이블 전체 스캔을 의미하고 TAB2는 I01_TAB2 이라는 인덱스를 통한 인덱스 스캔을 했음을 표시한 것이다. 조인 방법은 NL Join을 수행했음을 표시한 것이다. [그림 Ⅱ-3-5]에서 TAB1에 대한 액세스는 스캔(Scan) 방식이고 조인시도 및 I01_TAB2 인덱스를 통한 TAB2 액세스는 랜덤(Random) 방식이다. 대량의 데이터를 랜덤 방식으로 액세스하게 되면 많은 I/O가 발생하여 성능상 좋지 않다.

![sql-processing-flowchart](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/sql-optimizer/sql-processing-flowchart.jpg)

- SQL의 내부적인 처리 절차를 시각적으로 표현한 도표
  + 실행계획의 시각화

성능적인 관점을 살펴보기 위해서 SQL 처리 흐름도에 일량을 함께 표시할 수 있다. [그림 Ⅱ-3-5]에서 건수(액세스 건수, 조인 시도 건수, 테이블 액세스 건수, 성공 건수)라고 표시된 곳에 SQL 처리를 위해 작업한 건수 또는 처리 결과 건수 등의 일량을 함께 표시할 수 있다. 이것을 통해 어느 부분에서 비효율이 발생하고 있는지에 대한 힌트를 얻을 수 있다.

다음은 [그림 Ⅱ-3-5]가 다음 SQL문에 대한 SQL 처리 흐름도라는 가정으로 설명한다.


SELECT … FROM TAB1 A, TAB2 B WHERE A.KEY = B.KEY AND A.COL1 = :condition1 AND B.COL2 = :condition2

[그림 Ⅱ-3-5]에서 액세스 건수는 SQL 처리를 위해 TAB1을 액세스한 건수이다. 여기서는 TAB1의 A.COL1 칼럼에 이용 가능한 인덱스가 존재하지 않아 전체 테이블 스캔을 수행했음을 의미한다. 따라서 액세스 건수는 TAB1 테이블의 총 건수와 동일하다. 조인 시도 건수는 TAB1을 액세스한 후 즉, 테이블에서 읽은 해당 건에 대해 A.COL1 = :condition1 조건을 만족한 건만이 TAB2와 조인을 시도한다. 즉, TAB1을 액세스한 후 A.COL1 = :condition1 조건을 만족하지 않는다면 더 이상 조인 작업을 진행할 필요가 없다. 따라서 조인 시도 건수는 TAB1에 주어진 조건인 A.COL1 = :condition1을 만족한 건수가 된다. 테이블 액세스 건수는 B.KEY 칼럼만으로 구성된 인덱스(B.KEY 칼럼만으로 구성된 인덱스라고 가정함)인 I01_TAB2에서 B.KEY = A.KEY (TAB1은 이미 읽혀졌기 때문에 A.KEY 값은 상수임) 조건을 만족한 건만이 TAB2 테이블을 액세스한다. 즉, 조인 시도한 건들 중에서 B.KEY = A.KEY 조건까지 만족한 건과 같다. 성공 건수는 SQL 실행을 통해 사용자에게 답으로서 보여지는 결과 건수이다. TAB2 테이블을 액세스해서 B.COL2 = :condition2 조건까지 만족해야 비로서 사용자에게 보여질 수 있다.

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
  + 서브 쿼리에 unnest와 push_subq를 같이 기술
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

- **SQL 옵티마이저**가 미리 수집한 시스템 및 오브젝트 통계정보를 바탕으로 다양한 실행경로를 생성해서 비교한 후 **가장 효율적인 하나를 선택**하는 단계
  + 옵티마이저는 DB 성능을 결정하는 가장 핵심적인 엔진
  + 딕셔너리 캐시에 미리 수집해 둔 오브젝트 통계 및 시스템 정보를 활용

> SQL을 실행하려면 사전에 SQL 파싱과 최적화 과정을 거친다. 세부적인 SQL 처리 과정을 설명할 목적이 아니면 일반적으로 이 둘을 구분할 필요는 없다. 최적화 과정을 포함히 'SQL 파싱'이라고 표현하기도 하고, 파싱 과정을 포함해 'SQL 최적화'라고 표현하고디 한다. 여기서는 'SQL 최적화'라고 표현했다.
{: .prompt-info }

#### 3. 로우 소스 생성

- SQL 옵티마이저가 선택한 실행경로를 실제 실행 가능한 코드 또는 프로시저 형태로 포맷팅하는 단계
- 로우 소스 생성기(Row-Source Generator)가 담당

### 최적화 목표

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
- [admin, *옵티마이저와 실행계획*, 데이터온에어, 2021-02-15](https://dataonair.or.kr/db-tech-reference/d-guide/sql/?pageid=3&mod=document&uid=354){: target="_blank" }
- [admin, *옵티마이저*, 데이터온에어, 2021-02-15](https://dataonair.or.kr/db-tech-reference/d-guide/sql/?pageid=2&mod=document&uid=364){: target="_blank" }
- 조시형, *국가공인 SQLP 자격검정 핵심노트 Ⅰ*(서울: 디비안, 2021), 272
- 조시형, *국가공인 SQLP 자격검정 핵심노트 Ⅱ*(서울: 디비안, 2021), 278