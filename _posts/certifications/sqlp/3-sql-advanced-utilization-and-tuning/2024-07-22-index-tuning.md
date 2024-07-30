---
title: "[SQLP | 3과목]SQL 고급활용 및 튜닝 - 인덱스 튜닝(2024)"
categories: [Certifications, SQLP]
tags: [Certification, 자격증, SQLP]
image:
  path: /assets/img/posts/certifications/sqld/01-kdata-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: 한국데이터산업진흥원
---

# SQL 고급활용 및 튜닝 - 인덱스 튜닝(2024)

|         주요 항목           |         세부 항목       |
|:--------------------------:|:-----------------------:|
|        SQL 수행 구조        |  데이터베이스 아키텍처   |
|                            |       SQL 처리 과정      |
|                            | 데이터베이스 I/O 메커니즘 |
|        SQL 분석 도구        |       예상 실행계획      |
|                            |        SQL 트레이스      |
|                            |       응답 시간 분석     |
|       **인덱스 튜닝**	      |      인덱스 기본 원리    |
|                            |    테이블 엑세스 최소화   |
|                            |     인덱스 스캔 효율화    |
|                            |         인덱스 설계       |
|          조인 튜닝	        |          NL 조인         |
|                            |        소트 머지 조인     |
|                            |          해시 조인        |
|                            |       스칼라 서브쿼리     |
|                            |        고급 조인 기법     |
|       SQL 옵티마이저        |     SQL 옵티마이징 원리   |
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

## 인덱스 기본 원리

### 인덱스 종류

#### 트리 기반 인덱스

![b-tree-index-structure](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/index-tuning/b-tree-index-structure.jpg)
*양의 정수만 저장할 수 있는 데이터 타입이라고 가정한 B-Tree 인덱스의 구조*

- DBMS에서 가장 일반적인 인덱스 구조
- 루트(Root) 블록
  + 트리의 최상위 블록으로, 하나만 존재
- 브랜치(Branch), 내부(Internal) 블록
  + 루트 블록과 리프 블록 사이의 블록
  + 키 값의 범위와 자식 블록을 가리키는 포인터를 가짐
- 리프(Leaf) 블록
  + 트리의 최하위 블록으로, 실제 데이터에 대한 포인터를 포함거나 데이터를 직접 포함함
  + 루트 블록에서 리프 블록까지의 거리를 인덱스 깊이라고 부르며, 인덱스 깊이는 성능에 영향을 끼침
  + 인덱스 키 값과, 그 키 값에 해당하는 테이블 레코드를 찾아가는 데 필요한 주소 정보(ROWID)를 가짐
    * 키 값, ROWID 순으로 정렬
- **상위 블록들은 각 하위 블록들의 데이터 값 범위를 나타내는 키 값과, 그 키 값에 해당하는 블록을 찾는 데 필요한 주소 정보를 가지고 있음**
- **`=` 연산자로 검색하는 일치(Exact Match) 검색과 `BETWEEN`, `>` 등과 같은 연산자로 검색하는 범위(Range) 검색 모두에 적합한 구조**
- Oracle에서 인덱스 구성 컬럼이 모두 `null`인 레코드는 인덱스에 저장하지 않음
  + 인덱스 구성 컬럼럼 중 하나라도 `null` 값이 아닌 레코드는 인덱스에 저장
- SQL Server는 인덱스 구성 컬럼럼이 모두 `null`인 레코드도 인덱스에 저장

> `NULL` 값을 Oracle은 맨 뒤에 저장하고, SQL Server는 맨 앞에 저장한다.
{: .prompt-info }

> 리프 블록은 항상 인덱스 키(Key) 값 순으로 정렬돼 있기 때문에 ‘범위 스캔(Range Scan, 검색조건에 해당하는 범위만 읽다가 멈추는 것을 말함)’이 가능하고, 정방향(Ascending)과 역방향(Descending) 스캔이 둘 다 가능하도록 양방향 연결 리스트(Double linked list) 구조로 연결돼 있다.
{: .prompt-info }

##### 검색 과정

![b-tree-index-searching-process](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/index-tuning/b-tree-index-searching-process.jpg)

1. 브랜치 블록의 가장 왼쪽 값이 찾고자 하는 값보다 작거나 같으면 왼쪽 포인터로 이동
2. 찾고자 하는 값이 브랜치 블록의 값 사이에 존재하면 가운데 포인터로 이동
3. 오른쪽에 있는 값보다 크면 오른쪽 포인터로 이동

37을 찾고자 한다면 루트 블록에서 50보다 작으므로 왼쪽 포인터로 이동한다. 37는 왼쪽 브랜치 블록의 11과 40 사이의 값이므로 가운데 포인터로 이동한다. 이동한 결과 해당 블록이 리프 블록이므로 37이 블록 내에 존재하는지 검색한다. 본 예에서는 리프 블록에 37이 존재한다. 검색하고자 하는 값을 찾은 것이다. 만약, SQL문에서 다른 컬럼이 더 필요하면 리프 블록에 존재하는 레코드 식별자를 이용해서 테이블을 액세스한다. 만약, 37과 50사이의 모든 값을 찾고자 한다면(`BETWEEN 37 AND 5`) 위와 동일한 방법으로 리프 블록에서 37을 찾고 50보다 큰 값을 만날 때까지 오른쪽으로 이동하면서 인덱스를 읽는다. **이것은 인덱스 데이터가 정렬되어 있고 리프 블록이 양방향 링크로 연결되어 있기 때문에 가능하다.** 인덱스를 경유해서 반환된 결과 데이터는 인덱스 데이터와 동일한 순서로 갖게 되는 특징을 갖는다. 인덱스를 생성할 때 동일 컬럼으로 구성된 인덱스를 중복해서 생성할 수 없다. 그렇지만 인덱스 구성 컬럼은 동일하지만 컬럼의 순서가 다르면 서로 다른 인덱스로 생성할 수 있다. 예를 들어, JOB+SAL 컬럼 순서의 인덱스와 SAL+JOB 컬럼 순서의 인덱스를 별도의 인덱스를 생성할 수 있다. 인덱스의 컬럼 순서는 질의의 성능에 중요한 영향을 미치는 요소이다. Oracle에서 트리 기반 인덱스에는 B-트리 인덱스 외에도 비트맵 인덱스(Bitmap Index), 리버스 키 인덱스(Reverse Key Index), 함수기반 인덱스(FBI, Function-Based Index) 등이 존재한다.

#### SQL Server의 클러스터형 인덱스

![clustered-index](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/index-tuning/clustered-index.jpg)

- SQL Server에서 사용하는 사전과 비슷한 구조의 인덱스
- 인덱스의 리프 페이지가 곧 데이터 페이지임
  + 테이블 탐색에 필요한 **레코드 식별자가 리프 페이지에 존재하지 않음**
    * 인덱스 키 컬럼과 나머지 컬럼을 리프 페이지에 같이 저장하기 때문에 테이블을 임의 접근할 필요가 없음
  + 리프 페이지를 탐색하면 해당 테이블의 모든 컬럼 값을 곧바로 얻을 수 있음
- 리프 페이지의 모든 로우(데이터)는 인덱스, 키, 컬럼 순으로 물리적으로 정렬되어 저장됨
  + 테이블 로우는 물리적으로 **한 가지 순서로만 정렬**될 수 있음
    * 전화번호부 한 권을 상호와 인명으로 동시에 정렬할 수 없는 것과 같음
    * 클러스터형 인덱스는 테이블당 한 개만 생성 가능

> SQL Server의 인덱스 종류는 저장 구조에 따라 클러스터형(clustered) 인덱스와 비클러스터형(nonclustered) 인덱스로 나뉜다.
{: .prompt-info }

#### 비트맵 인덱스

#### 함수기반 인덱스

#### 리버스 키 인덱스

#### 클러스터 인덱스

#### 클러스터형 인덱스/IOT(SQL Server의 클러스터형 인덱스)

### 인덱스 탐색

#### 수직적 탐색

- 정렬된 인덱스 레코드 중 조건을 만족하는 **첫 번째 레코드를 찾는 과정**
- 수평적 탐색을 위한 시작 지점을 찾는 과정
- 루트에서 리프 블록까지 아래쪽으로 진행하며 스캔
  + 수직적 탐색 과정에서 **찾고자 하는 값보다 크거나 같은 값을 만나면 바로 직전 레코드가 가리키는 하위 블록으로 이동**
  + 루트와 브랜치 블록에 저장된 각 인덱스 레코드는 하위 블록에 대한 주소값을 가지기 때문에 가능함

##### 예시

![index-scan.jpg](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/index-tuning/index-scan.jpg)

'이재희'를 찾아보자. 루트 블록에는 '이재희'보다 크거나 같은 값이 없다. 그럴 때는 맨 마지막 '서' 레코드가 가리키는 하위 블록으로 이동하면 된다. 브랜치 블록에서는 '이재희'보다 큰 레코드 '정재우'를 찾았다. 바로 직전 레코드('이재룡')가 가리키는 하위 블록으로 이동하면 된다. 이제 리프 블록에 도달했고 거기서 조건을 만족하는(고객명 = '이재희') 첫 번째 레코드를 찾았다.

'강덕승'을 찾아보자. 루트 블록에 '강덕승'보다 큰 값('서')이 있으므로 바로 직전 레코드(LMC)가 가리키는 하위 블록으로 이동한다. 이동한 브랜치 블록에느 찾고자 하는 값과 정확히 일치하는 값이 있다. 그렇다고 그 레코드가 가리키는 하위 블록으로 이동하면 안된다. 바로 직전 레코드(LMC)가 가리키는 하위 블록으로 이동해야 첫 번째 리프 블록 맨 마지막에 저장된 '강덕승' 레코드를 빠뜨리지 않는다. 수직적 탐색은 '조건을 만족하는 레코드'를 찾는 과정이 아니라 '조건을 만족하는 첫 번째 레코드'를 찾는 과정임을 반드시 기억하자.

![b-tree-index-structure](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/index-tuning/b-tree-index-structure.jpg)
*양의 정수만 저장할 수 있는 데이터 타입이라고 가정한 B-Tree 구조의 인덱스에서 키 값이 53인 레코드 찾기*

1. 우선 루트 블록에서 53이 속한 키 값을 찾는다.
2. 3번 블록에서 다시 53이 속한 키 값을 찾는다.
3. 9번 블록의 세 번째 레코드에서 찾아지므로 함께 저장된 ROWID를 이용해 테이블 블록을 찾아간다
  + 9번 블록은 리프 블록이므로 거기서 값을 찾거나 못 찾거나 둘 중 하나
  + ROWID를 분해해 보면, 오브젝트 번호, 데이터 파일번호, 블록번호, 블록 내 위치 정보를 알 수 있음
4. 테이블 블록에서 레코드를 찾아간다.

> 인덱스가 Unique 인덱스가 아닌 한, 값이 53인 레코드가 더 있을 수 있기 때문에 4번이 끝이 아니다. 따라서 9번 블록에서 레코드 하나를 더 읽어 53인 레코드가 더 있는지 확인한다. 53인 레코드가 더 이상 나오지 않을 때까지 스캔하면서 ④번 테이블 액세스 단계를 반복한다. 만약 9번 블록을 다 읽었는데도 계속 53이 나오면 10번 블록으로 넘어가서 스캔을 계속한다.
{: .prompt-info }

#### 수평적 탐색

- 인덱스 리프 블록에 저장된 레코드끼리 연결된 순서에 따라 좌에서 우, 또는 우에서 좌로 스캔
  + 찾고자 하는 데이터가 더 안 나타날 때까지 스캔
  + 인덱스 리프 블록끼리는 서로 앞뒤 블록에 대한 주소값을 갖기 때문에 가능
    * 양방향 연결 리스트(double linked list) 구조
- 인덱스에서 본격적으로 데이터를 찾는 과정
  + 조건절에 만족하는 데이터를 모두 찾고 ROWID를 얻기 위함
    * ROWID는 테이블 엑세스할 때 필요함

#### 결합 인덱스 구조와 탐색

- 두 개 이상 컬럼이 결합된 인덱스
- 인덱스 컬럼을 어떤 순서로 구성하든 읽는 인덱스 블록 개수는 똑같음
  + 블록 I/O 개수가 같으므로 성능도 똑같음

### 인덱스 스캔 방식

#### Index Range Scan

![index-range-scan(1)](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/index-tuning/index-range-scan(1).jpg)

- 인덱스 **루트 블록에서 리프 블록까지 수직적으로 탐색**한 후에 리프 블록을 **필요한 범위만 스캔**하는 방식
- **인덱스를 스캔하는 범위(Range)와 테이블로 액세스하는 횟수를 최소화하는 것이 튜닝의 핵심**
- B-Tree 인덱스의 가장 일반적이고 정상적인 형태의 액세스 방식
- **인덱스를 구성하는 선두 컬럼이 조건절에 사용되어야 함**
  + 그렇지 않으면 Index Full Scan 방식으로 처리됨
- Index Range Scan 과정을 거쳐 생성된 **결과집합은 인덱스 컬럼 순으로 정렬된 상태**가 됨
  + `ORDER BY` 절이나 `MIN()`, `MAX()` 함수 사용을 생략할 수 있음

##### 예시

```sql
CREATE INDEX emp_deptno_idx ON emp(deptno);
-- SET autotrace traceonly explain;

SELECT *
  FROM emp
 WHERE deptno = 20;
```

![ex-index-range-scan-execution-plan](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/index-tuning/ex-index-range-scan-execution-plan.jpg)
*Index Range Scan 실행계획*

#### Index Full Scan

![index-full-scan](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/index-tuning/index-full-scan.jpg)

- 수직적 탐색을 통해 찾은 인덱스 리프 블록을 **처음부터 끝까지 수평적으로 탐색하는 방식**
  + 데이터 검색을 위한 최적의 인덱스가 없을 때 차선으로 선택됨

##### 예시

```sql
CREATE INDEX emp_idx ON emp (ename, sal);
-- SET autotrace traceonly exp;

SELECT *
  FROM emp
 WHERE sal > 2000
 ORDER BY ename;
```

![ex-index-full-scan-execution-plan(1)](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/index-tuning/ex-index-full-scan-execution-plan(1).jpg)
*Index Full Scan 실행계획*

위 SQL처럼 인덱스 선두 칼럼(`ename`)이 조건절에 없으면 옵티마이저는 우선적으로 Table Full Scan을 고려한다. 그런데 대용량 테이블이어서 Table Full Scan의 부담이 크다면 옵티마이저는 인덱스를 활용하는 방법을 다시 생각해 보지 않을 수 없다. 데이터 저장공간은 ‘가로×세로’ 즉, ‘칼럼길이×레코드수’에 의해 결정되므로 대개 인덱스가 차지하는 면적은 테이블보다 훨씬 적게 마련이다. 만약 인덱스 스캔 단계에서 대부분 레코드를 필터링하고 일부에 대해서만 테이블 액세스가 발생하는 경우라면 테이블 전체를 스캔하는 것보다 낫다. 이럴 때 옵티마이저는 Index Full Scan 방식을 선택할 수 있다.

```sql
CREATE INDEX emp_idx ON emp (ename, sal);
-- SET autotrace traceonly exp

SELECT *
  FROM emp
 WHERE sal > 5000
 ORDER BY ename;
```

![ex-index-full-scan-execution-plan(2)](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/index-tuning/ex-index-full-scan-execution-plan(2).jpg)
*Index Full Scan이 효과를 발휘하는 전형적인 케이스*

![efficiency-of-index-full-scan](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/index-tuning/efficiency-of-index-full-scan.jpg)

연봉이 5,000을 초과하는 사원이 전체 중 극히 일부라면 Table Full Scan보다는 Index Full Scan을 통한 필터링이 큰 효과를 가져다준다. 하지만 이런 방식은 적절한 인덱스가 없어 Index Range Scan의 차선책으로 선택된 것이므로, 할 수 있다면 인덱스 구성을 조정해 주는 것이 좋다.

```sql
CREATE INDEX emp_idx ON emp (ename, sal);
-- SET autotrace traceonly exp

SELECT /* first_rows(10) */ *
  FROM emp 
 WHERE sal > 1000 
 ORDER BY ename;
```

![ex-index-full-scan-execution-plan(3)](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/index-tuning/ex-index-full-scan-execution-plan(3).jpg)

![replace-sort-operation-with-index](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/index-tuning/replace-sort-operation-with-index.jpg)
*인덱스를 이용한 소트 연산 대체*

Index Full Scan은 Index Range Scan과 마찬가지로 그 결과집합이 인덱스 칼럼 순으로 정렬되므로 Sort Order By 연산을 생략할 목적으로 사용될 수도 있는데, 이는 차선책으로 선택됐다기보다 옵티마이저가 전략적으로 선택한 경우에 해당한다.

[그림 Ⅲ-4-5]에서 대부분 사원의 연봉이 1,000을 초과하므로 Index Full Scan을 하면 거의 모든 레코드에 대해 테이블 액세스가 발생해 Table Full Scan 보다 오히려 불리하다. 만약 `SAL`이 인덱스 선두 칼럼이어서 Index Range Scan 하더라도 마찬가지다. 그럼에도 여기서 인덱스가 사용된 것은 사용자가 first_rows 힌트(SQL Server에서는 fastfirstrow 힌트)를 이용해 옵티마이저 모드를 바꾸었기 때문이다. 즉, 옵티마이저는 소트 연산을 생략함으로써 전체 집합 중 처음 일부만을 빠르게 리턴할 목적으로 Index Full Scan 방식을 선택한 것이다. 그러나 사용자가 처음 의도와 다르게 데이터 읽기를 멈추지 않고 끝까지 fetch 한다면 Full Table Scan한 것보다 훨씬 더 많은 I/O를 일으키면서 서버 자원을 낭비할 텐데, 이는 옵티마이저의 잘못이 결코 아니며 first_rows 힌트를 사용한 사용자에게 책임이 있다.

#### Index Unique Scan

![index-unique-scan](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/index-tuning/index-unique-scan.jpg)

- **수직적 탐색만으로 데이터를 찾는 스캔 방식**
  + Unique 인덱스를 `=` 조건으로 탐색하는 경우에 작동

##### 예시

```sql
CREATE UNIQUE INDEX pk_emp on emp(empno);
ALTER TABLE emp ADD 2 CONSTRAINT pk_emp PRIMARY KEY(empno) USING INDEX pk_emp;
-- SET autotrace traceonly explain

SELECT empno, ename
  FROM emp
 WHERE empno = 7788;
```

![ex-unique-index-scan-execution-plan](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/index-tuning/ex-unique-index-scan-execution-plan.jpg)

#### Index Skip Scan

![index-skip-scan](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/index-tuning/index-skip-scan.jpg)

- 인덱스 선두 컬럼이 조건절에 빠졌어도 인덱스를 활용하는 스캔 방식
  + 루트 또는 브랜치 블록에서 읽은 컬럼 값 정보를 이용해 조건에 부합하는 레코드를 포함할 가능성이 있는 하위 블록만 골라서 액세스
    * 조건절에 빠진 인덱스 선두 컬럼의 Distinct Value 개수가 적고 후행 컬럼의 Distinct Value 개수가 많을 때 유용
  + Oracle 9i부터 등장

##### 예시

```sql
CREATE INDEX emp_idx ON emp (ename, sal);
-- SET autotrace traceonly exp

SELECT *
  FROM emp
 WHERE sal BETWEEN 2000 AND 4000;
```

![ex-index-skip-scan-execution-plan](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/index-tuning/ex-index-skip-scan-execution-plan.jpg)

#### Index Fast Full Scan

|                 Index Full Scan                |              Index Fast Full Scan            |
|:----------------------------------------------:|:--------------------------------------------:|
|              인덱스 구조를 따라 스캔            |              세그먼트 전체를 스캔             |
|                결과 집합 순서 보장              |            결과집합 순서 보장 안 됨           |
|                 Single Block I/O               |                Multiblock I/O                |
|       병렬 스캔 불가(파티션 돼 있지 않다면)      |                 병렬스캔 가능                 |
| 인덱스에 포함되지 않은 컬럼 조회 시에도 사용 가능 | 인덱스에 포함된 칼럼으로만 조회할 때 사용 가능 |

- 인덱스 트리 구조를 무시하고 인덱스 세그먼트 전체를 Multiblock Read 방식으로 스캔
  + Index Full Scan보다 빠름

#### Index Range Scan Descending

![ex-index-range-scan-descending-execution-plan](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/index-tuning/index-range-scan-desending.jpg)

- Index Range Scan과 기본적으로 동일한 스캔 방식이지만, 인덱스를 뒤에서부터 앞쪽으로 스캔하기 때문에 내림차순으로 정렬된 결과집합을 얻음

##### 예시

```sql
CREATE INDEX emp_empno_idx ON emp(empno);
-- SET autotrace traceonly exp

SELECT *
  FROM emp
 WHERE empno IS NOT NULL
 ORDER BY empno DESC;
```

![ex-index-range-scan-descending-execution-plan](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/index-tuning/ex-index-range-scan-descending-execution-plan.jpg)

## 테이블 엑세스 최소화

### 전체 테이블 스캔과 인덱스 스캔

#### 전체 테이블 스캔(Table Full Scan)

![full-table-scan](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/index-tuning/full-table-scan.jpg)

- 검색 조건에 맞는 데이터를 찾기 위해서 테이블의 **HWM(High Water Mark) 아래의 모든 블록을 읽는 방식**
  + Oracle
  + HWM는 테이블에 데이터가 쓰여졌던 블록 상의 최상위 위치를 의미함
    * 데이터가 삭제되더라도 HWM는 낮아지지 않음(한 번이라도 데이터가 기록되었던 블록이라면 HWM 이하에 포함됨)
  + 모든 블록을 읽기 때문에 **시간이 오래 걸릴 수 있음**
    * 이렇게 읽은 블록들은 **재사용성이 떨어지기 때문에 메모리에서 곧 제거될 수 있도록 관리해야 함**

##### 옵티마이저가 전체 테이블 스캔을 선택하는 이유

- SQL 문에 조건이 존재하지 않는 경우
- SQL 문의 주어진 조건에 사용 가능한 인덱스가 존재하는 않는 경우
  + 사용 가능한 인덱스는 존재하나 함수를 사용하여 인덱스 컬럼을 변형한 경우에도 인덱스를 사용할 수 없음
- 조건을 만족하는 데이터가 많은 경우
  + 결과를 추출하기 위해서 테이블의 대부분의 블록을 액세스해야 한다고 옵티마이저가 판단하면 조건에 사용 가능한 인덱스가 존재해도 전체 테이블 스캔 방식으로 읽을 수 있음
- 병렬처리 방식
- 전체 테이블 스캔 방식의 힌트 사용

#### 인덱스 스캔

![index-range-scan(2)](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/index-tuning/index-range-scan(2).jpg)

- 인덱스를 구성하는 컬럼의 값을 기반으로 데이터를 추출하는 액세스 기법
- 인덱스가 구성 컬럼으로 정렬되어 있기 때문에 인덱스를 경유하여 데이터를 읽으면 그 결과 또한 정렬되어 반환됨

## 인덱스 스캔 효율화

## 인덱스 설계

- 인덱스 튜닝

### 결합 인덱스 구성을 위한 기본 공식

### 추가적인 고려사항

### 인덱스 설계도 작성

## 참고자료

- 한국데이터산업진흥원, *SQL 자격검정 실전문제*(서울: 한국데이터산업진흥원, 2016), 279.
- [admin, *인덱스 기본*, 데이터온에어, 2021-02-15](https://dataonair.or.kr/db-tech-reference/d-guide/sql/?pageid=2&mod=document&uid=355){: target="_blank" }
- [admin, *인덱스 기본 원리*, 데이터온에어, 2021-02-15](https://dataonair.or.kr/db-tech-reference/d-guide/sql/?pageid=1&mod=document&uid=366){: target="_blank" }
- [admin, *인덱스 튜닝*, 데이터온에어, 2021-02-15](https://dataonair.or.kr/db-tech-reference/d-guide/sql/?pageid=1&mod=document&uid=367){: target="_blank" }
- 조시형, *친절한 SQL 튜닝*(서울: 디비안, 2018), 560.
- 조시형, *국가공인 SQLP 자격검정 핵심노트 Ⅰ*(서울: 디비안, 2021), 272
- 조시형, *국가공인 SQLP 자격검정 핵심노트 Ⅱ*(서울: 디비안, 2021), 278