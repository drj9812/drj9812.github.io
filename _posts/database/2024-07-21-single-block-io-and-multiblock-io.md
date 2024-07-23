---
title: "[Database]Single Block I/O, MultiBlock I/O"
categories: [Database]
tags: [Database, 데이터베이스, DB, index, 인덱스, Single Block I/O, MultiBlock I/O]
---

# Single Block I/O, MultiBlock I/O

```sql
CREATE TABLE t
    AS
SELECT *
FROM all_objects;

ALTER TABLE t ADD CONSTRAINT t_pk PRIMARY KEY(object_id);

SELECT /*+ index(t) */ count(*)
  FROM t
 WHERE object_id > 0 
```

|    Call   | Count |    Cpu   |  Elapsed |  Disk  | Query  | Current | Rows  |
|:---------:|:-----:|:--------:|:--------:|:------:|:------:|:-------:|:-----:| 
|   Parse   |   1   |   0.00   |   0.00   |    0   |    0   |    0    |   0   | 
|  Execute  |   1   |   0.00   |   0.00   |    0   |    0   |    0    |   0   | 
|   Fetch   |   2   |   0.26   |   0.25   |   64   |   65   |    0    |   1   | 
| **Total** | **4** | **0.26** | **0.25** | **64** | **65** |  **0**  | **1** |

|  Rows |                  Row Source Operation                     |
|:-----:|:---------------------------------------------------------:|
|   1   |      SORT AGGREGATE (cr=65 r=64 w=0 time=256400 us)       |
| 31192 | INDEX FAST FULL SCAN T_PK (cr=65 r=64 w=0 time=134613 us) |

Elapsed times include waiting on following events:

|       Event waited on       | Times Max. | Wait | Total Waited |
|:---------------------------:|:----------:|:----:|:------------:| 
|  SQL*Net message to client  |     2      | 0.00 |      0.00    |
|    db file scattered read   |    64      | 0.00 |      0.00    |
| SQL*Net message from client |     2      | 0.05 |      0.05    |

위 실행 결과를 보면 64개 인덱스 블록을 디스크에서 읽으면서 64번의 I/O Call(db file sequential read 대기 이벤트)이 발생했다. 아래는 같은 양의 인덱스 블록을 MultiBlock I/O 방식으로 수행한 결과다.

```sql
-- 디스크 I/O가 발생하도록 버퍼 캐시
Flushing ALTER system flush buffer_cache;

-- Multiblock I/O 방식으로 인덱스 스캔
SELECT /*+ index_ffs(t) */ count(*)
  FROM t
 WHERE object_id > 0
```

|    Call   | Count |    Cpu   |  Elapsed |  Disk  | Query  | Current | Rows  |
|:---------:|:-----:|:--------:|:--------:|:------:|:------:|:-------:|:-----:| 
|   Parse   |   1   |   0.00   |   0.00   |    0   |    0   |    0    |   0   | 
|  Execute  |   1   |   0.00   |   0.00   |    0   |    0   |    0    |   0   | 
|   Fetch   |   2   |   0.26   |   0.26   |   64   |   69   |    0    |   1   | 
| **Total** | **4** | **0.26** | **0.26** | **64** | **69** |  **0**  | **1** |

|  Rows |                  Row Source Operation                     |
|:-----:|:---------------------------------------------------------:|
|   1   |      SORT AGGREGATE (cr=69 r=64 w=0 time=267453 us)       |
| 31192 | INDEX FAST FULL SCAN T_PK (cr=69 r=64 w=0 time=143781 us) |

Elapsed times include waiting on following events:

|       Event waited on       | Times Max. | Wait | Total Waited |
|:---------------------------:|:----------:|:----:|:------------:| 
|  SQL*Net message to client  |     2      | 0.00 |      0.00    |
|    db file scattered read   |     9      | 0.00 |      0.00    |
| SQL*Net message from client |     2      | 0.35 |      0.36    |

똑같이 64개 블록을 디스크에서 읽었는데, I/O Call이 9번(db file scattered read 대기 이벤트)에 그쳤다. 참고로, 위 테스트는 Oracle 9i에서 수행한 것이다. Oracle 10g부터는 Index Range Scan 또는 Index Full Scan 일 때도 Multiblock I/O 방식으로 읽는 경우가 있는데, 위처럼 테이블 액세스 없이 인덱스만 읽고 처리할 때가 그렇다. 인덱스를 스캔하면서 테이블을 Random 액세스할 때는 9i 이전과 동일하게 테이블과 인덱스 블록을 모두 Single Block I/O 방식으로 읽는다. Single Block I/O 방식으로 읽은 블록들은 LRU 리스트 상 MRU 쪽(end)으로 위치하므로 한번 적재되면 버퍼 캐시에 비교적 오래 머문다. 반대로 MultiBlock I/O 방식으로 읽은 블록들은 LRU 리스트 상 LRU 쪽(end)으로 연결되므로 적재된 지 얼마 지나지 않아 1순위로 버퍼캐시에서 밀려난다.

## Single I/O

- 한번의 I/O 호출에 하나의 데이터 블록만 읽어 메모리에 적재하는 방식
- **인덱스를 통해 테이블에 접근할 때는, 기본적으로 인덱스와 테이블 블록 모두 이 방식을 사용**
  + 인덱스 루트 블록을 읽을 때
  + 인덱스 루트 블록에서 얻은 주소 정보로 브랜치 블록을 읽을 때
  + 인덱스 브랜치 블록에서 얻은 주소 정보로 리프 블록을 읽을 때
  + 인덱스 리프 블록에서 얻은 주소 정보로 테이블 블록을 읽을 때`
- 인덱스 스캔 시 효율적임
  + 인덱스 블록간 논리적 순서(이중 연결 리스트 구조로 연결된 순서)는 데이터 파일에 저장된 물리적인 순서와 다르기 때문임
- `db file sequential read` 대기 이벤트 발생

## MultiBlock I/O

- 캐시에서 찾지 못한 특정 블록을 읽으려고 I/O 호출할 때 디스크 상에 그 블록과 인전합 블록들을 한꺼번에 읽어 캐시에 미리 적재하는 기능
  + 인접한 블록이란 한 익스텐트(Extent)에 속한 블록을 의미하며, 달리 말하면, 익스텐트 범위를 넘어서까지 읽지 않음
- Multiblock I/O 단위는 `db_File_multiblock_read_Count` 파라미터에 의해 결정됨
- Table Full Scan처럼 물리적으로 저장된 순서에 따라 읽을 때는 인접한 블록들을 같이 읽는 것이 유리함
- `db file scattered read` 대기 이벤트 발생

## 참고자료

- [admin, *데이터베이스 I/O 원리*, 데이터온에어, 2021-02-15](https://dataonair.or.kr/db-tech-reference/d-guide/sql/?pageid=2&mod=document&uid=360){: target="_blank" }