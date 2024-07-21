---
title: "[Database]순차 접근(Sequential Access), 임의 접근(Random Access)"
categories: [Database]
tags: [Database, 데이터베이스, DB, index, 인덱스, Sequential Access, 순차 접근, Random Access, 임의 접근]
---

# 순차 접근(Sequential Access), 임의 접근(Random Access)

## 순차 접근(Sequential Access)

![01-sequential-access-and-radom-access](/assets/img/posts/database/sequential-access-and-radom-access/01-sequential-access-and-radom-access.jpg)
*굵은 실선 화살표*

- 레코드간 **논리적 또는 물리적인 순서**를 따라 차례대로 읽어 나가는 방식
  + 인덱스 리프 블록은 앞뒤를 가리키는 주소값을 통해 논리적으로 서로 연결돼 있음
- **인덱스가 쿼리의 조건을 만족하는 범위를 효율적으로 찾을 수 있는 경우에 사용됨**
  + 인덱스에 명시된 컬럼이 쿼리의 조건에 포함된 경우
- **I/O 성능을 높이기 위해서는 순차 접근에 의한 선택 비중을 높여야 함**
- 사진의 굵은 실선 화살표
  + 포인터를 따라 논리적으로 연결돼 있는 인덱스 리프 블록에 위치한 레코드를 스캔
  + 논리적 연결고리를 가지고 있지 않는 테이블은 세그먼트 헤더에 저장된 익스텐트 맵을 이용해 각 인스텐트의 첫 번째 블록 뒤에 연속해서 저장된 블록을 순서대로 읽음
    * Full Table Scan

> 테이블을 스캔할 때는 테이블 세그먼트 헤더에 저장된 익스텐트 맵을 이용한다. 익스텐트 맵을 통해 각 익스텐트의 첫 번째 블록 DBA를 알 수 있다. 익스텐트는 연속된 블록 집합이므로 테이블을 스캔할 때는 첫 번째 블록 뒤에 연속해서 저장된 블록을 읽으면 된다.
{: .prompt-info }

### 예시

```sql
-- 테스트용 테이블 생성
CREATE TABLE test 
    AS 
SELECT * 
  FROM all_objects 
 ORDER BY dbms_random.value;

-- 테스트용 테이블 전체 데이터 건수 : 49,906
SELECT COUNT(*) 
  FROM test;

SELECT COUNT(*) 
  FROM test
 WHERE owner LIKE 'SYS%';
```

|  Rows |                 Row Source Operation                |
|:-----:|:---------------------------------------------------:|
|   1   |    SORT AGGREGATE(cr=691 pr=0 pw=0 time=13037 us)   |
| 24613 | TABLE ACCESS FULL T(cr=691 pr=0 pw=0 time=98473 us) |


위 SQL은 24,613개 레코드를 선택하려고 49,906개 레코드를 읽었으므로 49%가 선택되었다. Table Full Scan에서 이 정도면 나쁘지 않다. 읽은 블록 수는 691개였다.

```sql
-- 인덱스가 없는 경우
SELECT COUNT(*) 
  FROM test
 WHERE owner LIKE 'SYS%' AND object_name = 'ALL_OBJECTS';
```

| Rows |                 Row Source Operation                |
|:----:|:---------------------------------------------------:|
|  1   |    SORT AGGREGATE (cr=691 pr=0 pw=0 time=7191 us)   |
|  1   | TABLE ACCESS FULL T (cr=691 pr=0 pw=0 time=7150 us) |


위 SQL은 49,906개 레코드를 스캔하고 1개 레코드를 선택했다. 선택 비중이 0.002% 밖에 되지 않으므로 Table Full Scan 비효율이 크다. 여기서도 읽은 블록 수는 똑같이 691개다. 이처럼 테이블을 스캔하면서 읽은 레코드 중 대부분 필터링되고 일부만 선택된다면 아래 처럼 인덱스를 이용하는 게 효과적이다.

```sql
-- 인덱스 생성: owner, object_name 순서
CREATE INDEX t_idx ON t(owner, object_name);

SELECT /*+ index(t t_idx) */ count(*)
  FROM t
 WHERE owner LIKE 'SYS%' AND object_name = 'ALL_OBJECTS';
```

| Rows |                           Row Source Operation                          |
|:----:|:-----------------------------------------------------------------------:|
|  1   |              SORT AGGREGATE (cr=76 pr=0 pw=0 time=7009 us)              |
|  1   | INDEX RANGE SCAN T_IDX (cr=76 pr=0 pw=0 time=6972 us) (Object ID 55337) |

위 SQL에서 **참조하는 칼럼이 모두 인덱스에 있으므로** 인덱스만 스캔하고 결과를 구할 수 있었다. 하지만 1개의 레코드를 읽기 위해 76개의 블록을 읽어야 했다. 테이블뿐만 아니라 인덱스를 Sequential 액세스 방식으로 스캔할 때도 비효율이 나타날 수 있고, 조건절에 사용된 칼럼과 연산자 형태, 인덱스 구성에 의해 효율성이 결정된다. 아래는 인덱스 구성 칼럼의 순서를 변경한 후에 테스트한 결과다.

```sql
-- 인덱스 제거 후, 다른 순서로 인덱스 생성
DROP INDEX t_idx;

CREATE INDEX t_idx ON t(object_name, owner);

SELECT /*+ index(t t_idx) */ count(*)
  FROM t
 WHERE owner LIKE 'SYS%' AND object_name = 'ALL_OBJECTS';
```

| Rows |                          Row Source Operation                        |
|:----:|:--------------------------------------------------------------------:|
|  1   |               SORT AGGREGATE (cr=2 pr=0 pw=0 time=44 us)             |
|  1   | INDEX RANGE SCAN T_IDX (cr=2 pr=0 pw=0 time=23 us) (Object ID 55338) |

루트와 리프, 단 2개의 인덱스 블록만 읽었다. 한 건을 얻으려고 읽은 건수도 한 건일 것이므로 가장 효율적인 방식으로 Sequential 액세스를 수행했다

## 임의 접근(Random Access)

![01-sequential-access-and-radom-access](/assets/img/posts/database/sequential-access-and-radom-access/01-sequential-access-and-radom-access.jpg)
*점선 화살표*

- 레코드 하나를 읽기 위해 데이터 블록에 한 블록씩 **임의로 접근**하는 방식
- **인덱스가 쿼리의 조건을 직접적으로 만족하지 못할 때 사용됨**
  + 인덱스에 포함된 컬럼이 쿼리의 조건에 전부 포함되지 않는 경우
    * 인덱스와 대응되는 쿼리의 조건을 인덱스를 통해 찾고, 대응되지 않은 쿼리의 조건은 인덱스 스캔 후에 임의로 접근
- 인덱스 스캔에 의해 얻은 ROWID를 통해 접근
- **I/O 성능을 높이기 위해서는 임의 접근 발생량을 줄여야 함**
- 사진의 점선 화살표

```sql
-- 인덱스 제거 후, 단일 컬럼 인덱스 생성
DROP INDEX t_idx;

CREATE INDEX t_idx ON t(owner);

SELECT object_id
  FROM t
 WHERE owner = 'sys' AND object_name = 'all_objects';
```

| Rows  |                          Row Source Operation                             |
|:-----:|:-------------------------------------------------------------------------:|
|   1   |     TABLE ACCESS BY INDEX ROWID T (cr=739 pr=0 pw=0 time=38822 us)        |
| 22934 | INDEX RANGE SCAN T_IDX (cr=51 pr=0 pw=0 time=115672 us) (Object ID 55339) |

인덱스로부터 조건을 만족하는 22,934건을 읽어 그 횟수만큼 테이블을 Random 액세스하였다. 최종적으로 한 건이 선택된 것에 비해 너무 많은 Random 액세스가 발생했다. 아래는 인덱스를 변경하여 테이블 Random 액세스 발생량을 줄인 결과다.

```sql
-- 인덱스 제거 후, 복합 인덱스 생성
DROP INDEX t_idx;

CREATE INDEX t_idx ON t(owner, object_name);

SELECT object_id
  FROM t
 WHERE owner = 'sys' AND object_name = 'all_objects';
```

| Rows |                        Row Source Operation                          |
|:----:|:--------------------------------------------------------------------:|
|  1   |      TABLE ACCESS BY INDEX ROWID T (cr=4 pr=0 pw=0 time=67 us)       |
|  1   | INDEX RANGE SCAN T_IDX (cr=3 pr=0 pw=0 time=51 us) (Object ID 55340) |

인덱스로부터 1건을 출력했으므로 테이블을 1번 방문한다. 실제 발생한 테이블 Random 액세스도 1(4 - 3)번이다. 같은 쿼리를 수행했는데 인덱스 구성이 바뀌자 테이블 Random 액세스가 대폭 감소한 것이다. 지금까지의 테스트 결과가 쉽게 이해되지 않을 수도 있다. 만약 그렇다면, 세부적인 인덱스 튜닝 원리를 설명한 4장을 읽고서 다시 학습하기 바란다.

## 참고자료

- [admin, *데이터베이스 I/O 원리*, 데이터온에어, 2021-02-15](https://dataonair.or.kr/db-tech-reference/d-guide/sql/?pageid=2&mod=document&uid=360){: target="_blank" }