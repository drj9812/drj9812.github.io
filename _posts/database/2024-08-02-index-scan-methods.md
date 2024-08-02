---
title: "[Database]인덱스 스캔 방식"
categories: [Database]
tags: [Database, Index Scan, 인덱스 스캔]
---

# 인덱스 스캔 방식

## Index Range Scan

![index-range-scan(1)](/assets/img/posts/database/index-scan-methods/index-range-scan(1).jpg)

- 인덱스 **루트 블록에서 리프 블록까지 수직적으로 탐색**한 후에 리프 블록을 **필요한 범위만 스캔**하는 방식
  + 시작 단어 검색
    * `LIKE abc%`
  + 범위 검색
    * `BETWEEN`, `>`, `<`
- **인덱스를 스캔하는 범위(Range)와 테이블로 액세스하는 횟수를 최소화하는 것이 튜닝의 핵심**
- B-Tree 인덱스의 가장 일반적이고 정상적인 형태의 액세스 방식
- **인덱스를 구성하는 선두 컬럼이 가공되지 않은 채로 조건절에서 사용되어야 함**
  + 그렇지 않으면 Index Full Scan 방식으로 처리됨
- Index Range Scan 과정을 거쳐 생성된 **결과집합은 인덱스 컬럼 순으로 정렬된 상태**가 됨
  + `ORDER BY` 절이나 `MIN()`, `MAX()` 함수 사용을 생략할 수 있음
    * 옵티마이저가 해당 연산을 수행하지 않음

### 예시

```sql
CREATE INDEX emp_deptno_idx ON emp(deptno);
-- SET autotrace traceonly explain;

SELECT *
  FROM emp
 WHERE deptno = 20;
```

![ex-index-range-scan-execution-plan](/assets/img/posts/database/index-scan-methods/ex-index-range-scan-execution-plan.jpg)
*Index Range Scan 실행계획*

## Index Full Scan

![index-full-scan](/assets/img/posts/database/index-scan-methods/index-full-scan.jpg)

- 수직적 탐색없이 인덱스 리프 블록을 **처음부터 끝까지 수평적으로 탐색하는 방식**
  + 중간 값 검색
  + 가공된 값 검색
    * `WHERE UPPER(column_name) = 'VALUE'`
  + 전체 데이터 접근
    * 인덱스를 통해 모든 데이터를 정렬된 순서로 읽어야 하는 경우 등 인덱스의 모든 데이터에 접근해야 할 때 사용됨
  + `OR`, `IN` 연산자 사용
    * `UNION ALL` 연산자로 사용해서 쿼리를 변환하면 Index Range Scan 사용
    * `use_concat` 힌트를 사용하여 OR Expansion을 유도하는 경우 옵티마이저는 Index Range Scan을 사용할 수도 있음
  + 데이터 검색을 위한 최적의 인덱스가 없어 Index Range Scan의 차선으로 선택됨
    * 수행빈도가 낮은 SQL이라면 상관 없지만, 그렇지 않다면 Index Ragne Scan을 사용하는 것이 좋음

### 예시

```sql
CREATE INDEX emp_idx ON emp (ename, sal);
-- SET autotrace traceonly exp;

SELECT *
  FROM emp
 WHERE sal > 2000
 ORDER BY ename;
```

![ex-index-full-scan-execution-plan(1)](/assets/img/posts/database/index-scan-methods/ex-index-full-scan-execution-plan(1).jpg)
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

![ex-index-full-scan-execution-plan(2)](/assets/img/posts/database/index-scan-methods/ex-index-full-scan-execution-plan(2).jpg)
*Index Full Scan이 효과를 발휘하는 전형적인 케이스*

![efficiency-of-index-full-scan](/assets/img/posts/database/index-scan-methods/efficiency-of-index-full-scan.jpg)

연봉이 5,000을 초과하는 사원이 전체 중 극히 일부라면 Table Full Scan보다는 Index Full Scan을 통한 필터링이 큰 효과를 가져다준다. 하지만 이런 방식은 적절한 인덱스가 없어 Index Range Scan의 차선책으로 선택된 것이므로, 할 수 있다면 인덱스 구성을 조정해 주는 것이 좋다.

```sql
CREATE INDEX emp_idx ON emp (ename, sal);
-- SET autotrace traceonly exp

SELECT /* first_rows(10) */ *
  FROM emp 
 WHERE sal > 1000 
 ORDER BY ename;
```

![ex-index-full-scan-execution-plan(3)](/assets/img/posts/database/index-scan-methods/ex-index-full-scan-execution-plan(3).jpg)

![replace-sort-operation-with-index](/assets/img/posts/database/index-scan-methods/replace-sort-operation-with-index.jpg)
*인덱스를 이용한 소트 연산 대체*

Index Full Scan은 Index Range Scan과 마찬가지로 그 결과집합이 인덱스 칼럼 순으로 정렬되므로 Sort Order By 연산을 생략할 목적으로 사용될 수도 있는데, 이는 차선책으로 선택됐다기보다 옵티마이저가 전략적으로 선택한 경우에 해당한다.

[그림 Ⅲ-4-5]에서 대부분 사원의 연봉이 1,000을 초과하므로 Index Full Scan을 하면 거의 모든 레코드에 대해 테이블 액세스가 발생해 Table Full Scan 보다 오히려 불리하다. 만약 `SAL`이 인덱스 선두 칼럼이어서 Index Range Scan 하더라도 마찬가지다. 그럼에도 여기서 인덱스가 사용된 것은 사용자가 first_rows 힌트(SQL Server에서는 fastfirstrow 힌트)를 이용해 옵티마이저 모드를 바꾸었기 때문이다. 즉, 옵티마이저는 소트 연산을 생략함으로써 전체 집합 중 처음 일부만을 빠르게 리턴할 목적으로 Index Full Scan 방식을 선택한 것이다. 그러나 사용자가 처음 의도와 다르게 데이터 읽기를 멈추지 않고 끝까지 fetch 한다면 Full Table Scan한 것보다 훨씬 더 많은 I/O를 일으키면서 서버 자원을 낭비할 텐데, 이는 옵티마이저의 잘못이 결코 아니며 first_rows 힌트를 사용한 사용자에게 책임이 있다.

## Index Unique Scan

![index-unique-scan](/assets/img/posts/database/index-scan-methods/index-unique-scan.jpg)

- **수직적 탐색만으로 데이터를 찾는 스캔 방식**
  + Unique 인덱스를 `=` 조건으로 탐색하는 경우에 작동
    * Unique 인덱스라고 해도 범위검색 조건(부등호, `BETWEEN`,`LIKE`)으로 검색할 때는 Index Range Scan이 사용됨
    * Unique 결합 인덱스에 대해 일부 컬럼만으로 검색할 때도 Index Range Scan이 사용됨

### 예시

```sql
CREATE UNIQUE INDEX pk_emp on emp(empno);
ALTER TABLE emp ADD 2 CONSTRAINT pk_emp PRIMARY KEY(empno) USING INDEX pk_emp;
-- SET autotrace traceonly explain

SELECT empno, ename
  FROM emp
 WHERE empno = 7788;
```

![ex-unique-index-scan-execution-plan](/assets/img/posts/database/index-scan-methods/ex-unique-index-scan-execution-plan.jpg)

## Index Skip Scan

![index-skip-scan](/assets/img/posts/database/index-scan-methods/index-skip-scan.jpg)

- 인덱스 선두 컬럼이 조건절에 빠졌어도 인덱스를 활용하는 스캔 방식
  + 루트 또는 브랜치 블록에서 읽은 컬럼 값 정보를 이용해 조건에 부합하는 레코드를 포함할 가능성이 있는 하위 블록만 골라서 액세스
    * 조건절에 빠진 인덱스 선두 컬럼의 Distinct Value 개수가 적고 후행 컬럼의 Distinct Value 개수가 많을 때 유용
  + 인덱스 선두 컬럼을 조건절에서 사용하지 않으면 옵티마이저는 기본적으로 Table Full Scan을 선택함
    * Table Full Scan보다 I/O를 줄일 수 있거나, 정렬된 결과를 쉽게 얻을 수 있따면, Index Full Scan을 사용하기도 함
  + 선두 컬럼에 대한 조건절은 있고, 중간 컬럼에 대한 조건절이 없는 경우
  + Distinct Value가 적은 두 개의 선두 컬러밍 모두 조건절에 없는 경우
  + 선두 컬럼이 부등호, `BETWEEN`, `LIKE` 같은 범위 검색 조건에 퐇ㅁ되는 경우
- Index Range Scan이 불가능하거나 효율적이지 못한 상황에서 사용됨
  + Index Skip Scan은 최선책이 아니며, 인덱스는 기본적으로 최적의 Index Range Scan을 목표로 설계해야 함
  + Oracle 9i부터 등장

### 예시

![ex-index-skip-scan](/assets/img/posts/database/index-scan-methods/ex-index-skip-scan.jpg)

```sql
CREATE INDEX emp_idx ON emp (ename, sal);
-- SET autotrace traceonly exp

SELECT *
  FROM emp
 WHERE sal BETWEEN 2000 AND 4000;
```

![ex-index-skip-scan-execution-plan](/assets/img/posts/database/index-scan-methods/ex-index-skip-scan-execution-plan.jpg)

Index Skip Scan은 루트 또는 브랜치 블록에서 읽은 컬럼 값 정보를 이용해 조건절에 부합하는 레코드를 포함할 '가능성이 있는' 리프 블록만 골라서 액세스하는 스캔 방식이다. 그림 2-20 인덱스 루트 블록에서 첫 번째 레코드가 가리키는 리프 블록은 '남 & 800' 이하인 레코드를 담고 있다. 이 블록은 액세스하지 않아도 될 것 같다. 하지만 '남'보다 작은 성별 값이 혹시 존재한다면, 그 사원에 대한 인덱스 레코드는 모두 1번 리프 블록에 저장되므로 액세스해야만 한다. 우리는 성별에 '남'과 '여' 두 개 값만 존재한다는 사실을 알지만 옵티마이저는 모른다.

두 번째 레코드가 가리키는 리프 블록은 '남 & 800' 이상이면서 '남 & 1500' 이하인 레코드를 담고 있다. '2000 <= 연봉 <= 4000'인 값이 존재할 가능성이 없으므로 이 블록은 액세스하지 않고 Skip한다.

세 번째 레코드가 가리키는 리프 블록은 '남 & 1500' 이상이면서 '남 & 5000' 이하인 레코드를 담고 있으므로 액세스 한다.

네 번째 레코드가 가리키는 리프 블록은 '남 & 5000' 이상이면서 '남 & 8000' 이하인 레코드를 담고 있으므로 Skip한다. 같은 이유로 다섯 번째 리프 블록도 Skip한다.

여섯 번째 리프 블록의 액세스 여부를 이해하는 게 중요하다. 여섯 번째 레코드가 가리키는 리프 블록은 '남 & 10000' 이상이므로 '2000 <= 연봉 <= 4000' 구간을 초과한다. 따라서 액세스하지 않아도 될 거 같지만 액세스해야 한다. 여자 중에서 '연봉 < 3000'이거나 '남'과 '여' 사이에 다른 성별이 혹시라도 존재한다면 이 리프 브록에 저장되고, '연봉 = 3000'인 여자 직원도 뒤쪽에 일부 저장돼 있을 수 있기 때문이다.

일곱 번째 레코드가 가리키는 리프 블록은 액세스하고, 여덟 번째와 아홉 번째 레코드가 가리키는 리프 블록은 Skip 해도 된다.

마지막으로 열 번째 리프 블록은 어떨까? '여 & 10000' 이상이므로 '2000 <= 연봉 <= 4000' 구간을 초과하지만 '여'보다 값이 큰 미지의 성별 값이 존재한다면 여기에 모두 저장될 것이므로 액세스해야만 한다.

### Index Skip Scan이 작동하기 위한 조건

Index Skip Scan은 Distinct Value 개수가 적은 선두 컬럼이 조건절에 없고 후행 컬럼의 Distinct Value 개수가 많을 때 효과적이라고 했다. 하지만 인덱스 선두 컬럼이 없을 때만 Index Skip Scan이 작동하는 것은 아니다. 예를 들어, 인덱스 구성이 다음과 같다고 하자.

일별업종별거래_PK: 업종유형코드 + 업종코드 + 기준일자

이때, 아래 SQL처럼 선두 컬럼(=업종유형코드)에 대한 조건절은 있고, 중간 컬럼(=업종코드)에 대한 조건절이 없는 경우에도 Skip Scan을 사용할 수 있다.

```sql
SELECT /*+ INDEX_SS(A 일별업종별거래_PK) */
       기준일자, 업종코드, 체결건수, 체결수량, 거래대금
  FROM 일별업종별거래 A
 WHERE 업종유형코드 = '01'
       AND
       기준일자 BETWEEN '2008051' AND '20080531';
```

Execution Plan ------------------------------------------------------------- 
0 SELECT STATEMENT Optimizer=ALL_ROWS (Cost=91 Card=7 Bytes=245)
1 0 TABLE ACCESS (BY LOCAL INDEX ROWID) OF '일별업종별거래' (TABLE) (Cost=91 ...)
2 1   INDEX (SKIP SCAN) OF '일별업종별거래_PK' (INDEX (UNIQUE)) (Cost=102 ...)

만약 위 SQL에 Index Range Scan을 사용한다면, 업종유형코드 = '01'인 인덱스 구간을 '모두' 스캔해야 한다. Index Skip Scan을 사용한다면, 업종유형코드 = '01'인 구간에서 기준일자가 '20080501'보다 크거나 같고 '20080531'보다 작거나 같은 레코드를 '포함할 가능성이 있는 리프 블록만'골라서 액세스할 수 있다.

아래와 같이 Distinct Value가 적은 두 개의 선두 컬럼이 모두 조건절에 없는 경우에도 유용하게 사용할 수 있다.

```sql
SELECT /*+ INDEX_SS(A 일별업종별거래_PK) */
       기준일자, 업종코드, 체결건수, 체결수량, 거래대금
  FROM 일별업종별거래 A
 WHERE 기준일자 BETWEEN '2008051' AND '20080531';
```

Execution Plan ------------------------------------------------------------- 
0 SELECT STATEMENT Optimizer=ALL_ROWS (Cost=91 Card=37 Bytes=1K)
1 0 TABLE ACCESS (BY LOCAL INDEX ROWID) OF '일별업종별거래' (TABLE) (Cost=91 ...)
2 1   INDEX (SKIP SCAN) OF '일별업종별거래_PK' (INDEX (UNIQUE)) (Cost=90 Card = 1)

선두 컬럼이 부등호, `BETWEEN`, `LIKE` 같은 범위 검색 조건일 때도 Index Skip Scan을 사용할 수 있다. 예를 들어, 일별업종별거래 테이블에 아래와 같은 인덱스가 있다고 하자.

일별업종별거래_X01: 기준일자 + 업종유형코드

SQL은 아래와 같다. 즉, 2008년 5월 1일부터 2008년 5월 31일 구간에서 업종유형코드가 '01'인 레코드만 선택하고자 하는 것이다.

```sql
SELECT /*+ INDEX_SS(A 일별업종별거래_PK) */
       기준일자, 업종코드, 체결건수, 체결수량, 거래대금
  FROM 일별업종별거래 A
 WHERE 기준일자 BETWEEN '2008051' AND '20080531'
       AND
       업종유형코드 = '01';
```

만약 위 SQL에 Index Range Scan을 사용한다면, 기준일자 `BETWEEN` 조건을 만족하는 인덱스 구간 '모두' 스캔해야 한다. Index Skip Scn을 사용한다면, 기준일자 `BETWEEN` 조건을 만족하는 인덱스 구간에서 업종유형코드 = '01'인 레코드를 '포함할 가능성이 있는 리프 블록만' 골라서 액세스할 수 있다.

## Index Fast Full Scan

|                 Index Full Scan                |              Index Fast Full Scan            |
|:----------------------------------------------:|:--------------------------------------------:|
|              인덱스 구조를 따라 스캔            |              세그먼트 전체를 스캔             |
|                결과 집합 순서 보장              |            결과집합 순서 보장 안 됨           |
|                 Single Block I/O               |                Multiblock I/O                |
|       병렬 스캔 불가(파티션 돼 있지 않다면)      |     병렬 스캔 가능(파티션 돼 있지 않아도)      |
| 인덱스에 포함되지 않은 컬럼 조회 시에도 사용 가능 | 인덱스에 포함된 칼럼으로만 조회할 때 사용 가능 |

![ex-index-fast-full-scan](/assets/img/posts/database/index-scan-methods/ex-index-fast-full-scan.jpg)

- 논리적인 인덱스 트리 구조를 무시하고 물리적인 **인덱스 세그먼트 전체를 Multiblock I/O 방식으로 스캔**
  + Index Full Scan보다 빠름
  + **정렬되지 않음**
  + **쿼리에 사용한 컬럼이 모두 인덱스에 포함돼 있을 때만 사용할 수 있음**
- 위 사진 왼쪽 익스텐트에서 1 → 2 → 10 → 3 → 9 순서, 오른쪽 익스텐트에서 8 → 7 → 4 → 5 → 6 순서
  + 루트와 두 개의 브랜치 블록도 있지만 필요 없는 블록이므로 버림
  + Index Full Scan은 인덱스의 논리적 구조를 따라 루트 → 브랜치 → 1 → 2 → 3 → 4 → 5 → 6 → 7 → 8 → 9 → 10 순서로 읽음

## Index Range Scan Descending

![ex-index-range-scan-descending-execution-plan](/assets/img/posts/database/index-scan-methods/index-range-scan-desending.jpg)

- Index Range Scan과 기본적으로 동일한 스캔 방식이지만, 인덱스를 뒤에서부터 앞쪽으로 스캔하기 때문에 내림차순으로 정렬된 결과집합을 얻음

### 예시

```sql
CREATE INDEX emp_empno_idx ON emp(empno);
-- SET autotrace traceonly exp

SELECT *
  FROM emp
 WHERE empno IS NOT NULL
 ORDER BY empno DESC;
```

![ex-index-range-scan-descending-execution-plan](/assets/img/posts/database/index-scan-methods/ex-index-range-scan-descending-execution-plan.jpg)


## 참고자료

- 한국데이터산업진흥원, *SQL 자격검정 실전문제*(서울: 한국데이터산업진흥원, 2016), 279.
- [admin, *인덱스 기본*, 데이터온에어, 2021-02-15](https://dataonair.or.kr/db-tech-reference/d-guide/sql/?pageid=2&mod=document&uid=355){: target="_blank" }
- [admin, *인덱스 기본 원리*, 데이터온에어, 2021-02-15](https://dataonair.or.kr/db-tech-reference/d-guide/sql/?pageid=1&mod=document&uid=366){: target="_blank" }
- [admin, *인덱스 튜닝*, 데이터온에어, 2021-02-15](https://dataonair.or.kr/db-tech-reference/d-guide/sql/?pageid=1&mod=document&uid=367){: target="_blank" }
- 조시형, *친절한 SQL 튜닝*(서울: 디비안, 2018), 560.
- 조시형, *국가공인 SQLP 자격검정 핵심노트 Ⅰ*(서울: 디비안, 2021), 272
- 조시형, *국가공인 SQLP 자격검정 핵심노트 Ⅱ*(서울: 디비안, 2021), 278