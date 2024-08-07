---
title: "[SQLP | 3과목]SQL 고급활용 및 튜닝 - SQL 분석 도구(2024)"
categories: [Certifications, SQLP]
tags: [Certification, 자격증, SQLP]
math: true
image:
  path: /assets/img/posts/certifications/sqld/01-kdata-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: 한국데이터산업진흥원
---

# SQL 고급활용 및 튜닝 - SQL 분석 도구(2024)

|         주요 항목           |         세부 항목       |
|:--------------------------:|:-----------------------:|
|        SQL 수행 구조        |  데이터베이스 아키텍처   |
|                            |       SQL 처리 과정      |
|                            | 데이터베이스 I/O 메커니즘 |
|      **SQL 분석 도구**      |       예상 실행계획      |
|                            |        SQL 트레이스      |
|                            |       응답 시간 분석     |
|         인덱스 튜닝	        |      인덱스 기본 원리    |
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

## 예상 실행계획

### SQL *Plus에서 실행계획 확인

### 상용 쿼리 툴에서 실행계획 확인

### 더 많은 정보 확인하기

## SQL 트레이스

### SQL 트레이스 수집 및 파일 찾기

### 리포트 생성

### 트레이스 결과 분석

## 응답 시간 분석

### 대기 이벤트

DBMS 내부에서 활동하는 수많은 프로세스 간에는 상호작용이 필요하며, 그 과정에서 다른 프로세스가 일을 마칠 때까지 기다려야만 하는 상황이 자주 발생한다. 그때마다 해당 프로세스는 자신이 일을 계속 진행할 수 있는 조건이 충족될 때까지 수면(sleep) 상태로 대기하는데, 그 기간에 정해진 간격으로(1초, 3초 등) 각 대기 유형별 상태와 시간 정보가 공유 메모리 영역에 저장된다. 대개 누적치만 저장되지만, 사용자가 원하면(10046 이벤트 트레이스를 활성화하면) 로그처럼 파일로 기록해 주기도 한다. 이러한 대기 정보를 Oracle에서는 ‘대기 이벤트(Wait Event)’라고 부르며, SQL Server에서는 ‘대기 유형(Wait Type)’이라고 부른다. 대기 이벤트가 중요한 이유는, 1990년대 후반부터 이를 기반으로 한 ‘Response Time Analysis’ 성능관리 방법론이 데이터베이스 성능 진단 분야에 일대 변혁을 가져왔기 때문이다. 세션 또는 시스템 전체에 발생하는 병목 현상과 그 원인을 찾아 문제를 해결하는 방법과 과정을 다루는 이 방법론은, 데이터베이스 서버의 응답 시간을 서비스 시간과 대기 시간의 합으로 정의하고 있다.

$$ Response Time = Service Time + Wait Time = CPU Time + Queue Time $$

서비스 시간(Service Time)은 프로세스가 정상적으로 동작하며 일을 수행한 시간을 말한다. CPU Time과 같은 의미다. 대기 시간(Wait Time)은 프로세스가 잠시 수행을 멈추고 대기한 시간을 말한다. 다른 말로 방법론은 Response Time을 위와 같이 정의하고, CPU Time과 Wait Time을 각각 break down 하면서 서버의 일량과 대기 시간을 분석해 나간다. CPU Time은 파싱 작업에 소비한 시간인지 아니면 쿼리 본연의 오퍼레이션 수행을 위해 소비한 시간인지를 분석한다. Wait Time은 각각 발생한 대기 이벤트들을 분석해 가장 시간을 많이 소비한 이벤트 중심으로 해결방안을 모색한다. Oracle 10g 기준으로 대기 이벤트 개수는 890여 개에 이르는데, 그 중 가장 자주 발생하고 성능 문제와 직결되는 것들을 일부 소개하고자 한다. 참고로 본 단락은 Oracle 중심으로만 설명하는데, SQL Server는 대기 유형이 잘 알려지지 않은 데다 아직 활용도가 낮은 편이기 때문이다.

참고로, DB를 모니터링하거나 성능 진단 업무를 담당하지 않는다면 아래 내용을 굳이 공부하지 않아도 된다. 그럼에도 여기서 소개하는 이유는, DBMS 병목이 주로 어디서 발생하는지 그리고 어떤 이벤트로써 측정되는지를 간단하게나마 보여주기 위한 것이므로 부담없이 읽어 나가기 바란다.

#### 라이브러리 캐시 부하

아래는 라이브러리 캐시에서 SQL 커서를 찾고 최적화하는 과정에 경합이 발생했음을 나타나는 대기 이벤트다.

- latch: shared pool
- latch: library cache

라이브러리 캐시와 관련해 자주 발생하는 대기 이벤트로는 아래 2가지가 있는데, 이들은 수행 중인 SQL이 참조하는 오브젝트에 다른 사용자가 DDL 문장을 수행할 때 나타난다.

- library cache lock
- library cache pin

라이브러리 캐시 관련 경합이 급증하면 심각한 동시성 저하를 초래하는데, 2절에서 이를 최소화하는 방안을 소개한다.

#### 데이터베이스 Call과 네트워크 부하

아래 이벤트에 의해 소모된 시간은 애플리케이션과 네트워크 구간에서 소모된 시간으로 이해하면 된다.

- `SQL*Net message from client`
- `SQL*Net message to client`
- `SQL*Net more data to client`
- `SQL*Net more data from client`

`SQL*Net message from client` 이벤트는 사실 데이터베이스 경합과는 관련이 없다. 클라이언트로부터 다음 명령이 올 때까지 Idle 상태로 기다릴 때 발생하기 때문이다. 반면, 나머지 세 개의 대기 이벤트는 실제 네트워크 부하가 원인일 수 있다. `SQL*Net message to client`와 `SQL*Net more data to client` 이벤트는 클라이언트에게 메시지를 보냈는데 메시지를 잘 받았다는 신호가 정해진 시간보다 늦게 도착하는 경우에 나타나며, 클라이언트가 너무 바쁜 경우일 수도 있다. `SQL*Net more data from client` 이벤트는 클라이언트로부터 더 받을 데이터가 있는데 지연이 발생하는 경우다. 이들 대기 이벤트를 해소하는 방안에 대해서는 3절에서 다룬다.

#### 디스크 I/O 부하

- `db file sequential read`
- `db file scattered read`
- `direct path read`
- `direct path write`
- `direct path write temp`
- `direct path read temp`
- `db file parallel read`

이들 중 특히 주목할 대기 이벤트는 `db file sequential read`와 `db file scattered read`이다. 전자는 Single Block I/O를 수행할 때 나타나는 대기 이벤트다. Single Block I/O는 말 그대로 한번의 I/O Call에 하나의 데이터 블록만 읽는 것을 말한다. 인덱스 블록을 읽을 때, 그리고 인덱스를 거쳐 테이블 블록을 액세스할 때 이 방식을 사용한다. 후자는 Multiblock I/O를 수행할 때 나타나는 대기 이벤트다. Multiblock I/O는 I/O Call이 필요한 시점에 인접한 블록들을 같이 읽어 메모리에 적재하는 것을 말한다. Table Full Scan 또는 Index Fast Full Scan 시 나타난다. 이들 대기 이벤트를 해소하는 방안에 대해서는 4절에서 다루며, 4장과 5장에서 더 자세히 다룬다.

#### 버퍼 캐시 경합

- latch: cache buffers chains
- latch: cache buffers lru chain
- buffer busy waits
- dfree buffer waits

버퍼 캐시에서 블록을 읽더라도 이들 대기 이벤트가 심하게 발생하는 순간 동시성은 현저히 저하되는데, 이들 대기 이벤트를 해소하는 방안도 디스크 I/O 부하 해소 방안과 다르지 않다. 따라서 이들 경합의 해소 원리도 4절과 더불어 4장, 5장에서 함께 다루게 된다.

#### Lock 관련 대기 이벤트

아래 ‘enq’로 시작되는 대기 이벤트는 Lock과 관련된 것으로서, 그 발생 원인과 해소 방안을 2장에서 일부 소개한다.

- enq: TM - contention
- enq: TX - row lock contention
- enq: TX - index contention
- enq: TX - allocate ITL entry
- enq: TX contention
- latch free

latch free는 특정 자원에 대한 래치를 여러 차례(2,000번 가량) 요청했지만 해당 자원이 계속 사용 중이어서 잠시 대기 상태로 빠질 때마다 발생하는 대기 이벤트다. 래치(latch)는 우리가 흔히 말하는 Lock과 조금 다르다. Lock은 사용자 데이터를 보호하는 반면, 래치는 SGA에 공유돼 있는 갖가지 자료구조를 보호할 목적으로 사용하는 가벼운 Lock이다. 래치도 일종의 Lock이지만, 큐잉(Queueing) 메커니즘을 사용하지 않는다. 따라서 특정 자원에 액세스하려는 프로세스는 래치 획득에 성공할 때까지 시도를 반복할 뿐, 우선권을 부여 받지는 못한다. 이는 가장 먼저 래치를 요구했던 프로세스가 가장 늦게 래치를 얻을 수도 있음을 뜻한다.

지금까지 소개한 것 외에 자주 발생하는 대기 이벤트로는 아래와 같은 것들이 있다.

- log file sync
- checkpoint completed
- log file switch completion
- log buffer space

## 참고자료

- 한국데이터산업진흥원, *SQL 자격검정 실전문제*(서울: 한국데이터산업진흥원, 2016), 279.
- [admin, *데이터베이스 아키텍쳐*, 데이터온에어, 2021-02-15](https://dataonair.or.kr/db-tech-reference/d-guide/sql/?pageid=2&mod=document&uid=357){: target="_blank" }
- 조시형, *친절한 SQL 튜닝*(서울: 디비안, 2018), 560.
- 조시형, *국가공인 SQLP 자격검정 핵심노트 Ⅰ*(서울: 디비안, 2021), 272
- 조시형, *국가공인 SQLP 자격검정 핵심노트 Ⅱ*(서울: 디비안, 2021), 278