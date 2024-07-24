---
title: "[Oracle]Consistent 모드, Current 모드"
categories: [Database, Oracle]
tags: [Oracle, 오라클, Consistent 모드, Current 모드]
image:
  path: /assets/img/posts/tools/db/oracle/01-oracle-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Oracle
---

# Consistent 모드, Current 모드

## Consitetn 모드

###### SQL Trace

|    call   |  count  |    cpu   |  elapsed |  disk  |  **query**  | current |  rows  |
|:---------:|:-------:|:--------:|:--------:|:------:|:-----------:|:-------:|:------:| 
|   parse   |    15   |   0.00   |   0.08   |    0   |    0    |      0      |    0   | 
|  execute  |    44   |   0.03   |   0.03   |    0   |    0    |      0      |    0   | 
|   fetch   |    44   |   0.01   |   0.13   |   18   |   822   |      0      |   44   | 
| **total** | **103** | **0.04** | **0.25** | **18** | **822** |    **0**    | **44** |

- 데이터 일관성을 보장하기 위한 모드
- SQL Trace에서 `query` 항목으로 집계됨
- Autotrace에서 `consistent gets` 항목으로 집계됨
- 주로 SELECT 문에서 사용됨
  + DML 문도 작업을 시작할 때 일관된 데이터 상태를 보장하기 위해 먼저 Consistent 모드로 데이터를 읽음

### 특징

#### 스냅샷 일관성

- 쿼리가 실행되는 시점의 데이터 상태를 기준으로 데이터를 읽음
  + 쿼리가 실행되는 동안 다른 트랜잭션이 데이터를 변경하더라도, 쿼리는 실행 시작 시점의 일관된 데이터 상태를 유지

#### UNDO 데이터 사용

- 쿼리가 실행되는 동안 데이터가 변경되었다면, Oracle은 UNDO 세그먼트를 사용하여 이전 데이터 상태를 재구성
  + UNDO 세그먼트를 사용하여 트랜잭션이 시작된 시점의 데이터 상태를 재구성
    * 트랜잭션의 일관성을 유지

## Current 모드

###### SQL Trace

|    call   |  count  |    cpu   |  elapsed |  disk  |  query  | **current** |  rows  |
|:---------:|:-------:|:--------:|:--------:|:------:|:-------:|:-----------:|:------:| 
|   parse   |    15   |   0.00   |   0.08   |    0   |    0    |      0      |    0   | 
|  execute  |    44   |   0.03   |   0.03   |    0   |    0    |      0      |    0   | 
|   fetch   |    44   |   0.01   |   0.13   |   18   |   822   |      0      |   44   | 
| **total** | **103** | **0.04** | **0.25** | **18** | **822** |    **0**    | **44** |

- 현재 최신 데이터를 읽고 갱신하기 위한 모드
- SQL Trace에서 `current` 항목으로 집계됨
- AUTOtrace에서 `db block gets` 항목으로 집계됨
- DML 문에서 사용됨

### 특징

#### 최신 데이터 상태

- 현재의 최신 데이터 상태를 기준으로 데이터를 읽고 변경
- 데이터 블록을 직접 읽고 수정함

#### DML 문

- `INSERT`, `UPDATE`, `DELETE`와 같은 DML 문이 데이터를 갱신할 때 사용함
- 먼저 Consistent 모드로 데이터를 읽어 일관성을 확인한 후, 실제 데이터를 갱신할 때는 Current 모드로 데이터를 다시 읽고 변경함


#### 잠금

- 데이터의 동시성을 관리하기 위해 잠금(lock)을 사용할 수 있음
- 데이터가 갱신되는 동안 다른 트랜잭션이 해당 데이터에 접근하지 못하도록 함

## 참고자료

- 조시형, *친절한 SQL 튜닝*(서울: 디비안, 2018), 560.