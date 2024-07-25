---
title: "[SQLP | 3과목]SQL 고급활용 및 튜닝 - SQL 수행 구조(2024)"
categories: [Certifications, SQLP]
tags: [Certification, 자격증, SQLP]
math: true
image:
  path: /assets/img/posts/certifications/sqld/01-kdata-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: 한국데이터산업진흥원
---

# SQL 고급활용 및 튜닝 - SQL 수행 구조(2024)

|         주요 항목           |         세부 항목       |
|:--------------------------:|:-----------------------:|
|      **SQL 수행 구조**      |  데이터베이스 아키텍처   |
|                            |       SQL 처리 과정      |
|                            | 데이터베이스 I/O 메커니즘 |
|        SQL 분석 도구        |       예상 실행계획      |
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

## 데이터베이스 아키텍쳐

### 아키텍쳐 개관

- 데이터베이스
  + 디스크에 저장된 데이터 집합
  + Datafile, Redo Log File, Control File 등
- 인스턴스(Instance)
  + SGA(System Global Area) 공유 메모리 영역과 이를 액세스하는 프로세스 집합

#### Oracle 아키텍쳐

![01-oracle-architecture](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/sql-performance-structure/01-oracle-architecture.jpg)
*Oracle 아키텍쳐*

- **하나의 인스턴스는 하나의 데이터베이스에만 접근 가능**
  + 하나의 인스턴스가 여러 데이터베이스를 액세스할 수는 없음
  + **RAC(Real Application Cluster) 환경에서는 여러 인스턴스가 하나의 데이터베이스를 액세스할 수 있음**

> RAC(Real Application Clusters) 환경은 오라클의 고가용성 및 확장성 기능 중 하나로, 여러 노드에서 실행되는 인스턴스들이 동일한 데이터베이스에 접근하여 작업을 처리할 수 있도록 한다.
{: .prompt-info }

#### SQL Server 아키텍쳐

![02-sql-server-architecture](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/sql-performance-structure/02-sql-server-architecture.jpg)
*SQL Server 아키텍쳐*

- **하나의 인스턴스 당 최고 32,767개의 데이터베이스를 정의해 사용할 수 있음**
- 기본적으로 master, model, msdb, tempdb 등의 시스템 데이터베이스가 만들어지며, 여기에 사용자 데이터베이스를 추가로 생성하는 구조
- **데이터베이스 하나를 만들 때마다 주(Primary 또는 Main) Datafile과 Transaction 로그 파일이 하나씩 생성됨**
  + Datafile의 확장자는`.mdf`{: .filepath }이고, Transaction 로그 파일의 확장자는 `.ldf`{: .filepath }임
  + 저장할 데이터가 많으면 보조(Non-Primary) Datafile을 추가할 수 있으며, 확장자는 `.ndf`{: .filepath }임

### 프로세스

> *SQL Server는 Thread 기반 아키텍처이므로 프로세스 대신 Thread라는 표현을 써야 한다. SQL Server 뿐만 아니라 Oracle도 Windows 버전에선 Thread를 사용하지만, 프로세스와 일일이 구분하면서 설명하려면 복잡해지므로 특별히 쓰레드를 언급해야 할 경우가 아니라면 간단히 ‘프로세스’로 통칭하기로 한다. 잠시 후 표로써 정리해 보이겠지만, 주요 Thread의 역할은 Oracle 프로세스와 크게 다르지 않다. 프로세스는 서버 프로세스(Server Processes)와 백그라운드 프로세스(Background Processes) 집합으로 나뉜다. **서버 프로세스는 전면에 나서 사용자가 던지는 각종 명령을 처리**하고, **백그라운드 프로세스는 뒤에서 묵묵히 주어진 역할을 수행**한다.*

#### 서버 프로세스(Server Processes)

- 사용자 프로세스와 통신하면서 **사용자의 각종 명령을 처리하는 프로세스**
  + **SQL을 파싱**하고 필요시 **최적화**를 수행
  + **커서를 열어 SQL을 실행**하면서 블록을 읽고, 읽은 데이터를 정렬해서 **클라이언트가 요청한 결과집합을 만들어 네트워크를 통해 전송**
  + SQL Server에선 Worker Thread가 같은 역할을 담당
- **스스로 처리하도록 구현되지 않은 기능**은 OS, I/O 서브시스템, **백그라운드 프로세스가 대신 처리하도록 Call을 통해 요청**
  + Datafile로부터 DB 버퍼 캐시로 블록을 적재하거나 Dirty 블록을 캐시에서 밀어냄으로써 Free 블록을 확보하는 일
  + Redo 로그 버퍼를 비우는 일
- 클라이언트가 서버 프로세스와 연결하는 방식은 DBMS마다 다르지만 **Oracle**을 예로 들면, **전용 서버 방식과 공유 서버 방식, 두 가지가 있음**

##### 전용 서버(Dedicated Server) 방식

![03-dedicated-server-method](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/sql-performance-structure/03-dedicated-server-method.jpg)
*전용 서버 방식*

- 처음 **연결요청을 받는 리스너가 서버 프로세스(Window 환경에서는 Thread)를 생성**
  + **단 하나의 사용자 프로세스를 위해 전용(Dedicated) 서비스를 제공**한다는 점이 특징
- SQL을 수행할 때마다 **연결 요청을 반복하면 서버 프로세스의 생성과 해제도 반복**하게 됨
  + DBMS에 매우 큰 **부담을 주고 성능을 크게 떨어뜨림**
  + 따라서 전용 서버 방식을 사용하는 OLTP(Online Transaction Processing)성 애플리케이션에선 Connection Pooling 기법을 필수적으로 사용해야 함
    * 50개의 서버 프로세스와 연결된 50개의 사용자 프로세스를 공유해서 반복 재사용하는 방식

##### 공유 서버(Shared Server) 방식

![04-shared-server-method](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/sql-performance-structure/04-shared-server-method.jpg)
*공유 서버 방식*

- **하나의 서버 프로세스를 여러 사용자 세션이 공유**하는 방식
  + Connection Pooling 기법을 DBMS 내부에 구현
  + **미리 여러 개의 서버 프로세스를 띄어 놓고 이를 공유해서 반복 재사용**
- 공유 서버 방식으로 Oracle에 접속하면 사용자 프로세스는 서버 프로세스와 직접 통신하지 않고, **Dispatcher 프로세스를 통해 간접 통신함**
  1. 사용자 명령이 Dispatcher에게 전달되면 Dispatcher는 이를 SGA에 있는 요청 큐(Request Queue)에 등록
  2. 이후 가장 먼저 가용해진 서버 프로세스가 요청 큐에 있는 사용자 명령을 꺼내서 처리하고, 그 결과를 응답 큐(Response Queue)에 등록
  3. 응답 큐를 모니터링하던 Dispatcher가 응답 결과를 발견하면 사용자 프로세스에게 전송

#### 백그라운드 프로세스(Background Processes)

|         ORACLE         |             SQL SERVER            |                                             설명                                            |
|:----------------------:|:---------------------------------:|:-------------------------------------------------------------------------------------------:|
|  System Monitor(SMON)  | Database cleanup/shrinking thread | 장애가 발생한 시스템을 재기동할 때 인스턴스 복구를 수행하고, 임시 세그먼트와 익스텐트를 모니터링 |
|  Process Monitor(PMON) |      Open Data Services(OPS)      |                        이상이 생긴 프로세스가 사용하던 리소스를 복구                          |
| Database Writers(DBWn) |         Lazywriter thread         |                      버퍼 캐시에 있는 Dirty 버퍼를 데이터 파일에 기록                         |
|    Log Writer(LGWR)    |         Log writer thread         |                          로그 버퍼 엔트리를 Redo 로그 파일에 기록                             |
|     Archiver(ARCn)     |                N/A                |              꽉 찬 Redo 로그가 덮어 쓰여지기 전에 Archive 로그 디렉토리로 백업                 |
|     Checkpoint(CKPT)   |     Database Checkpoint thread    | 마지막 Checkpoint 시점 이후의 변경 사항을 데이터 파일에 기록하고, 완료된 시점 정보를 컨트롤 파일과 데이터 파일 헤더에 저장 |
|     Recoverer(RECO)    | Distributed Transaction Coordinator(DTC) | 분산 트랜잭션 과정에 발생한 문제를 해결 |

> Checkpoint 프로세스에 대해 좀 더 자세히 설명하면, Write Ahead Logging 방식(데이터 변경 전에 로그부터 남기는 메커니즘)을 사용하는 DBMS는 Redo 로그에 기록해 둔 버퍼 블록에 대한 변경사항 중 현재 어디까지를 데이터 파일에 기록했는지 체크포인트 정보를 관리해야 한다. 이는 버퍼 캐시와 데이터 파일이 동기화된 시점을 가리키며, 장애가 발생하면 마지막 체크포인트 이후 로그 데이터만 디스크에 기록함으로써 인스턴스를 복구할 수 있도록 하는 용도로 사용된다. 이 정보를 갱신하는 주기가 길수록 장애 발생 시 인스턴스 복구 시간도 길어진다.
{: .prompt-info }

### 파일 구조

#### 데이터 파일

![05-data-file-structure](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/sql-performance-structure/05-data-file-structure.jpg)
*데이터 파일 구조*

> Oracle과 SQL Server 모두 물리적으로는 데이터 파일에 데이터를 저장하고 관리한다. 공간을 할당하고 관리하기 위한 논리적인 구조도 크게 다르지 않지만 약간의 차이는 있다.
{: .prompt-info }

##### 블록(Block), 페이지(Page)

![block](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/sql-performance-structure/block.jpg)

- 대부분 DBMS에서 **I/O는 블록 단위**로 이루어짐
  + **하나의 레코드에서 하나의 칼럼만을 읽으려 할 때도 레코드가 속한 블록 전체를 읽게 됨**
  + Oracle은 블록(Block)이라는 용어를 사용하고, DB2, SQL Server는 페이지(Page)라는 용어를 사용함
- 하나의 블록은 하나의 테이블이 독점함
  + 한 익스텐트에 담긴 블록은 모두 같은 테이블
- <font color="red">SQL 성능을 좌우하는 가장 중요한 성능지표는 액세스하는 블록 개수이며, 옵티마이저의 판단에 가장 큰 영향을 미치는 것도 액세스해야 할 블록 개수임</font>
  + 옵티마이저가 인덱스를 이용해 테이블을 액세스할지 아니면 Full Table Scan 할지를 결정하는 데 있어 가장 중요한 판단 기준은 읽어야 할 레코드 수가 아니라 읽어야 하는 블록 개수임
- Oracle은 2KB, 4KB, 8KB, 16KB, 32KB, 64KB의 다양한 블록 크기를 사용
  + default 8KB
  + SQL Server에선 8KB 단일 크기의 페이지 사용

> 모든 데이터 블록은 디스크 상에서 몇 번 Datafile의 몇 번째 블록를 나타내는 자신만의 고유 주소값인 DBA(Data Block Address)를 갖는다. 데이터를 읽고 쓰는 단위가 블록이므로 데이터를 읽으려면 먼저 DBA부터 확인해야 한다. 인덱스를 이용해 테이블 레코드를 읽을 때는 인덱스 ROWID를 이용한다. ROWID는 DBA + 로우 번호(블록 내 순번)로 구성되므로 이를 분해하면 읽어야 할 테이블 레코드가 저장된 DBA를 알 수 있다. 테이블을 스캔할 때는 테이블 세그먼트 헤더에 저장된 익스텐트 맵을 이용한다. 익스텐트 맵을 통해 각 익스텐트의 첫 번째 블록 DBA를 알 수 있다. 익스텐트는 연속된 블록 집합이므로 테이블을 스캔할 때는 첫 번째 블록 뒤에 연속해서 저장된 블록을 읽으면 된다.
{: .prompt-info }

##### 익스텐트(Extent)

![extent](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/sql-performance-structure/extent.jpg)

- 연속된 블록들의 집합
- **테이블 스페이스로부터 공간을 할당받는 단위**
  + 테이블이나 인덱스에 데이터를 입력하다가 공간이 부족해지면 해당 오브젝트가 속한 테이블 스페이스(물리적으로는 데이터 파일)로부터 정해진 익스텐트 크기의 연속된 블록을 할당받음
    * 블록 크기가 8KB인 상태에서 64KB 단위로 익스텐트를 할당받도록 정의했다면, 공간이 부족할 때마다 테이블 스페이스로부터 8개의 연속된 블록을 찾아(찾지 못하면 새로 생성) 세그먼트에 할당함
- **익스텐트 내 블록은 논리적으로 인접하지만, 익스텐트끼리 서로 인접하지는 않을 수 있음**
  + 파일 경합을 줄이기 위해 DBMS가 데이터를 가능한 여러 Datafile로 분산 저장
- **Oracle은 다양한 크기의 익스텐트를 사용**하지만, **SQL Server에선 8개 페이지의 익스텐트만을 사용**
  + SQL Server의 페이지 크기는 8KB로 고정됐으므로 익스텐트는 항상 64KB
- Oracle은 단일 오브젝트가 사용
  + SQL Server에서는 여러 오브젝트가 같이 사용할 수도 있음

###### SQL Server의 익스텐트

- 균일(Uniform) 익스텐트
  + 64KB 이상의 공간을 필요로 하는 테이블이나 인덱스를 위해 사용
  + 8개 페이지 단위로 할당된 익스텐트를 단일 오브젝트가 모두 사용
- 혼합(Mixed) 익스텐트
  + 한 익스텐트에 할당된 8개 페이지를 여러 오브젝트가 나누어 사용하는 형태
  + 모든 테이블이 처음에는 혼합 익스텐트로 시작하지만 64KB를 넘으면서 2번째부터 균일 익스텐트를 사용하게 됨

##### 세그먼트(Segment), Heap/Index

```sql
SELECT segment_type, tablespace_name, extent_id, file_id, block_id, blocks
  FROM dba_extents
 WHERE owner = user
       AND
       segment_name = 'MY_SEGMENT'
 ORDER BY extent_id;
```

| segment_type | tablespace_name | extent_id | file_id | block_id | blocks |
|:------------:|:---------------:|:---------:|:-------:|:--------:|:------:|
|     TABLE    |      USERS      |     0     |    1    |     1    |   4    |
|     TABLE    |      USERS      |     1     |    1    |     9    |   4    |
|     TABLE    |      USERS      |     2     |    2    |     1    |   4    |
|     TABLE    |      USERS      |     3     |    2    |     5    |   4    |
|     TABLE    |      USERS      |     4     |    2    |    13    |   4    |
|     TABLE    |      USERS      |     5     |    3    |     1    |   4    |
|     TABLE    |      USERS      |     6     |    4    |     9    |   4    |
|     TABLE    |      USERS      |     7     |    5    |     5    |   4    |
|     TABLE    |      USERS      |     8     |    5    |    17    |   4    |

- **테이블, 인덱스, Undo처럼 저장공간을 필요로 하는 데이터베이스 오브젝트**
  + 저장공간을 필요로 한다는 것은 한 개 이상의 익스텐트를 사용함을 뜻함
- 파티션 테이블(또는 인덱스)을 만들면 내부적으로 여러 개의 세그먼트가 생성됨(1:M 관계)
  + **다른 오브젝트는 세그먼트와 1:1 관계를 갖음**
    * 하나의 테이블을 생성하면 내부적으로 하나의 테이블 세그먼트가 생성되고, 하나의 인덱스를 생성하면 내부적으로 하나의 세그먼트가 생성됨
- 하나의 세그먼트는 자신이 속한 테이블 스페이스 내 **여러 Datafile에 걸쳐 저장**될 수 있음
  + Datafile이 동일한 테이블 스페이스 내에 위치할 때 가능
  + **세그먼트에 할당된 익스텐트가 여러 데이터 파일에 흩어져 저장되는 것**이며, 그래야 디스크 경합을 줄이고 I/O 분산 효과를 얻을 수 있음
- LOB(Large Obejct) 컬럼은 그 자체가 하나의 세그먼트를 구성하므로 자신이 속한 테이블과 다른 별도 공간에 값을 저장함

##### 테이블 스페이스, 파일 그룹(File Group)

![tablespace](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/sql-performance-structure/tablespace.jpg)

- **세그먼트를 담는 컨테이너**로서, 여러 Datafile로 구성됨
- 사용자는 세그먼트를 위한 테이블 스페이스를 지정할 뿐, **실제 값을 저장할 Datafile을 선택하고 익스텐트를 할당하는 것은 DBMS의 몫**
  + 데이터는 물리적으로 Datafile에 저장되지만, 사용자가 Datafile을 직접 선택하진 않음
- 각 세그먼트는 정확히 한 테이블 스페이스에만 속하지만, 한 테이블 스페이스에는 여러 세그먼트가 존재할 수 있음
  + 한 테이블 스페이스가 여러 Datafile로 구성되기 때문
- 특정 세그먼트에 할당된 모든 익스텐트는 해당 세그먼트와 관련된 테이블 스페이스 내에서만 존재
  + 한 세그먼트가 여러 테이블 스페이스에 걸쳐 저장될 수는 없음

#### 임시 데이터 파일

```sql
CREATE temporary tablespace big_temp tempfile '/usr/local/oracle/oradata/ora10g/big_temp.dbf' SIZE 2000m;
 ALTER USER scott temporary tablespace big_temp;
```

- 대량의 정렬이나 Hash 작업을 수행하다가 **메모리 공간이 부족해지면 중간 결과집합을 저장**하는 용도
- 임시 데이터 파일에 저장되는 오브젝트는 말 그대로 임시로 저장했다가 자동으로 삭제됨
- Redo 정보를 생성하지 않기 때문에 나중에 파일에 문제가 생겼을 때 **복구되지 않음**
  + 백업할 필요가 없음
- Oracle에선 임시 테이블 스페이스를 여러 개 생성해 두고, 사용자마다 별도의 임시 테이블 스페이스를 지정해 줄 수 있음

> SQL Server는 단 하나의 tempdb 데이터베이스를 사용한다. tempdb는 전역 리소스로서 시스템에 연결된 모든 사용자의 임시 데이터를 여기에 저장한다.
{: .prompt-info }

#### 로그 파일

##### Online Redo 로그 파일

- **[DB 버퍼 캐시](#db-버퍼-캐시db-buffer-cache)에 가해지는 모든 변경사항(트랜잭션)을 기록하는 Oracle의 로그 파일**
  + **활성 상태의 트랜잭션 로그를 실시간으로 기록**하여 트랜잭션 복구를 위해 사용됨
    * 캐시(메모리)에 저장된 변경사항이 아직 Datafile(디스크)에 기록되지 않은 상태에서 인스턴스가 비정상 종료되면서 모든 작업내용을 모두 잃게 되는 경우
  + **대부분 DBMS는** 버퍼 블록에 대한 변경사항을 건건이 데이터 파일에 기록하기보다 우선 로그 파일에 **Append 방식으로 빠르게 기록하는 방식**을 사용
    * 변경된 메모리 버퍼 블록을 디스크 상의 데이터 블록에 기록하는 작업은 Random I/O 방식으로 이루어지기 때문에 느림
  + 버퍼 블록과 데이터 파일 간 동기화는 적절한 수단(DBWR, Checkpoint)을 이용해 나중에 배치(Batch) 방식으로 일괄 처리
    * **Fast Commit 메커니즘**
- Online Redo 로그는 최소 두 개 이상의 파일로 구성됨
  + 현재 사용 중인 파일이 꽉 차면 다음 파일로 로그 스위칭(log switching)이 발생하며, 계속 로그를 써 나가다가 모든 파일이 꽉 차면 다시 첫 번째 파일부터 재사용하는 **라운드 로빈(round-robin) 방식을 사용**

> 마지막 체크포인트 이후부터 사고 발생 직전까지 수행되었던 트랜잭션들을 Redo 로그를 이용해 재현하는 것을 캐시 복구라고 한다.
{: .prompt-info }

> Fast Commit 메커니즘은 사용자의 갱신 내용이 메모리상의 버퍼 블록에만 기록된 채 아직 디스크에 기록되지 않았더라도 Redo 로그 파일에 활용해 빠르게 커밋을 완료한다. 인스턴스 장애가 발생해도 로그 파일을 이용해 복구할 수 있어 빠르고 안전한 트랜잭션 처리가 가능하다. 이는 모든 DBMS에서 공통적으로 사용하는 메커니즘이다.
{: .prompt-info }

##### Transaction 로그 파일

- **Oracle의 Online Redo 로그와 대응되는 SQL Server의 로그 파일**
- 주(Primary 또는 Main) Datafile마다, 즉 데이터베이스마다 Transaction 로그 파일(`.ldf`{: .filepath })이 하나씩 생성됨
- Transaction 로그 파일은 내부적으로 ‘가상 로그 파일’이라 불리는 더 작은 단위의 세그먼트로 나뉘며, 이 가상 로그 파일의 개수가 너무 많아지지 않도록(조각화가 발생하지 않도록) 옵션을 지정하는 게 좋음
  + 로그 파일을 애초에 넉넉한 크기로 만들어 자동 증가가 발생하지 않도록 하거나, 어쩔 수 없이 자동 증가한다면 증가하는 단위를 크게 지정

##### Archived(=Offline) Redo 로그

- 가득 찬 **Online Redo 로그 파일의 내용을 백업한 Oracle의 로그 파일**로, 장기적인 **데이터 보존과 복구**를 위해 사용됨
- **물리적으로 디스크에 결함**이 생기는 등 Media Fail 발생 시 데이터베이스(또는 미디어) 복구를 위해 사용됨
  + Media recovery
  + SQL Server에는 Archived Redo 로그에 대응되는 개념이 없음

### 메모리 구조

#### 시스템 공유 메모리 영역

![sga-component](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/sql-performance-structure/sga-component.jpg)
*SGA 구성요소*

- **여러 프로세스(또는 Thread)가 동시에 액세스할 수 있는 메모리 영역**
  + 서버 프로세스와 백그라운드 프로세스가 공통으로 액세스하는 데이터와 제어 구조를 캐싱하는 메모리 공간
  + Oracle에선 System Global Area(SGA), SQL Server에선 Memory Pool이라고 부름
- 공유 메모리를 구성하는 캐시 영역은 매우 다양하지만, 모든 DBMS가 공통적으로 사용하는 캐시 영역으로는 **DB 버퍼 캐시, 공유 풀, 로그 버퍼**가 있음
  + 공유 메모리 영역은 그 외에 Large 풀(Large Pool), 자바 풀(Java Pool) 등을 포함하고, 시스템 구조와 제어 구조를 캐싱하는 영역도 포함
- 시스템 공유 메모리 영역은 여러 프로세스에 공유되기 때문에 내부적으로 래치(Latch), 버퍼 Lock, 라이브러리 캐시 Lock/Pin 같은 액세스 직렬화 메커니즘이 사용됨

##### DB 버퍼 캐시(DB Buffer Cache)

![db-buffuer-cache](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/sql-performance-structure/db-buffuer-cache.jpg)

- **Datafile로부터 읽어 들인 데이터 블록을 담는 캐시 영역**
  + 데이터 캐시
- 인스턴스에 접속한 모든 사용자 프로세스는 서버 프로세스를 통해 DB 버퍼 캐시의 버퍼 블록을 동시에(내부적으로는 버퍼 Lock을 통해 직렬화) 액세스할 수 있음
- 일부 Direct Path Read 메커니즘이 작동하는 경우를 제외하면, **모든 블록 읽기는 버퍼 캐시를 통해 이루어짐**
  + 읽고자 하는 블록을 먼저 버퍼 캐시에서 찾아보고 없을 때 디스크에서 읽음
    * 디스크에서 읽을 때도 먼저 버퍼 캐시에 적재한 후에 읽음
- **데이터 변경**도 버퍼 캐시에 적재된 블록을 통해 이루어짐
  + 변경된 블록(Dirty 버퍼 블록)을 주기적으로 데이터 파일에 기록하는 작업은 DBWR 프로세스의 몫
- **SGA에 속하므로 같은 블록을 읽는 여러 프로세스가 동일한 블록을 읽을 때 이점을 제공**
- 메모리 I/O는 전기적 신호에 불과하기 때문에 디스크 I/O에 비교할 수 없을 정도로 빠름
  + 디스크 I/O는 물리적으로 액세스 암(Arm)이 움직이면서 헤드를 통해 이루어지기 때문에 속도가 느림
  + 디스크에서 읽은 데이터 블록을 메모리 상에 보관해 두는 기능이 모든 데이터베이스 시스템에 필수적인 이유

> 데이터 입력이나 삭제가 없어도 물리적 디스크 I/O는 SQL을 실행할 때마다 다르다. 첫 번째 실행할 때보다 두 번째 실행할 때 줄어들고, 세 번째 실행할 땐 더 줄어든다. 연속해서 실행하면 DB 버퍼 캐시에서 해당 테이블 블록의 점유율이 점점 높아지기 때문이다. 한참 후에 다시 실행하면 반대로 디스크 I/O가 늘어난다. DB 버퍼 캐시가 다른 테이블 블록을 채워지기 때문이다.
{: .prompt-info }

###### 캐시 탐색 메커니즘

![cache-searching-algorithm](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/sql-performance-structure/cache-searching-algorithm.jpg)

Direct Path I/O를 제외한 모든 블록 I/O는 메모리 버퍼 캐시를 경유한다. 구체적으로, 아래 오퍼레이션은 모두 버퍼 캐시 탐색 과정을 거친다.

- 인덱스 루트 블록을 읽을 때
  + 인덱스 루트 블록 주소는 SQL을 파싱하고 최적화하는 시점에 SQL 커서에 담긴다.
- 인덱스 루트 블록에서 얻은 주소 정보로 브랜치 블록을 읽을 때
- 인덱스 브랜치 블록에서 얻은 주소 정보로 리프 블록을 읽을 때
- 인덱스 리프 블록에서 얻은 주소 정보로 테이블 블록을 읽을 때
- 테이블 블록을 Full Scan 할 때
  + Table Full Scan 할 때는 읽어야 할 블록 목록을 익스텐트 맵(map)에서 얻는다.

버퍼 캐시 탐색 메커니즘을 설명하기에 앞서 버퍼 캐시 구조부터 살펴보자. DBMS는 버퍼 캐시를 위 사진과 같은 해시 구조로 관리한다. 위 사진은 해시 함수로 모듈러(mod) 함수를 사용하는 경우를 표현하고 있다. 실제로는 훨씬 더 정교한 알고리즘을 사용하지만, 여기서는 5로 나누었을 때의 나머지 값을 반환하는 모듈러 함수로 캐시 탐색 메커니즘을 설명하겠다.

예를 들어, 버퍼 캐시에서 20번 블록을 찾고자 한다. 블록 번호를 5로 나누면 나머지가 0이다. 이 블록이 캐싱돼 있다면 버퍼 헤더가 첫 번째 해시 체인(해시 값 = 0)에 연결돼 있을 것이므로 찾을 때 항상 첫 번째 해시 체인만 탐색하면 된다.

다른 예로, 27번 블록은 블록 번호를 5로 나누면 나머지가 2이다. 만약 이 블록이 캐싱돼 있다면 다른 버퍼 헤더가 세 번째 해시 체인(해시 값 = 2)에 있을 것이다. 위 사진을 보면, 현재 세 번째 해시 체인에는 27번 블록이 보이지 않는다. 세 번째 체인에 없다고 해서 다른 해시 체인을 탐색할 필요가 없다. 디스크로부터 읽어서 세 번째 체인에 연결해서 읽으면 된다.

버퍼 캐시에서 블록을 찾을 때 이처럼 해시 알고리즘으로 버퍼 헤더를 찾고, 거기서 얻은 포인터(Pointer)로 버퍼 블록을 액세스하는 방식을 사용한다. 해시 구조의 특징을 요약하면, 다음과 같다.

- 같은 입력 값은 항상 동일한 해시 체인(=버킷)에 연결됨
- 다른 입력 값(예를 들어, 4와 9)이 동일한 해시 체인(=버킷)에 연결될 수 있음
- 해시 체인 내에서는 정렬이 보장되지 않음

###### 메모리 공유자원에 대한 액세스 직렬화

버퍼 캐시는 SGA 구성요소이므로 버퍼 캐시에 캐싱된 버퍼 블록은 모두 공유자원이다. 공유자원은 말 그대로 모두에게 권한이 있기 때문에 누구나 접근할 수 있다. 문제는 하나의 버퍼 블록을 두 개 이상 프로세스가 '동시에' 접근하려고 할 때 발생한다. 동시에 접근하면 블록 정합성에 문제가 생길 수 있기 때문이다.

따라서 자원을 공유하는 것처럼 보여도 내부에선 한 프로세스씩 순차적으로 접근하도록 구현해야 하며, 이를 위해 직렬화(serialization) 메커니즘이 필요하다. 직렬화를 이해하기 쉽게 표현하면, '줄 세우기'다.

언제부턴가 '공유경제'란 개념이 경제기사에 자주 등장하더니 몇 년 전부터 구체적인 서비스 플랫폼이 구축되기 시작했다. 공유경제(Sharing Economy)란 여러 사람이 한 제품을 공유하면서 같이 사용하는 경제 시스템을 말한다.

한 제품을 여럿이 함께 사용한다고? 가장 대표적인 카쉐어링 서비스를 예로 들어 보자. 자동차를 여럿이 함께 사용할 수 있나? 같이 사용하는 것처럼 보이지만, 어느 한순간에는 한 사람만 자동차를 사용할 수 있다. 누군가 자동차를 사용하는 순간, 다른 사람은 줄 서서 기다려야 한다. 같이 쓰는 게 아니라 교대로 쓰는 거다. 현재 자동차 키를 손에 쥔 사람만 자동차를 사용할 수 있다. 또 다른 예로, 숙박시설 공유 서비스도 방 하나를 남과 같이 쓰는 동거의 개념이 아니라, 부대낌 없이 서로 시간을 달리하여 경제적으로 사용하는 개념이다.

'경제'를 '캐시'로 치환해 보자. 공유캐시의 특정 자원을 두 개 이상 프로세스가 같이 사용할 수 있나? 그럴 수 없다. 같이 사용하는 것처럼 보이지만, 특정 순간에는 한 프로세스만 사용할 수 있다. 그 순간 다른 프로세스는 줄 서서 기다려야 한다. 이런 줄서기가 가능하도록 지원하는 메커니즘이 래치(Latch)다.

> 캐시 버퍼 체인 래치는 대량의 데이터를 읽을 때 모든 블록에 대해 해시 체인을 탐색하는 것을 말한다. DBA(Data Block Address)를 해시 함수에 입력하고 거기서 반환된 값으로 스캔해야 할 해시 체인을 찾는다. 해시 체인을 스캔하는 동안 다른 프로세스가 체인 구조를 변경하는 일이 생기면 곤란하다. 이를 막기 위해 해시 체인 래치가 존재한다. 위 사진에서 0부터 4까지 다섯 개 체인 앞쪽에 자물쇠가 있다고 생각하면 된다. 자물쇠를 열 수 있는 키(key)를 획득한 프로세스만이 체인으로 진입할 수 있다.
{: .prompt-info }

SGA를 구성하는 서브 캐시마다 별도의 래치가 존재하는데, 버퍼 캐시에는 캐시 버퍼 체인 래치, 캐시 버퍼 LRU 체인 래치 등이 작동한다. 빠른 데이터베이스를 구현하려면 버퍼 캐시 히트율을 높여야 하지만, 캐시 I/O도 생각만큼 빠르지 않을 수 있다. 이들 래치에 의한 경합이 생길 수 있기 때문이다.

캐시 버퍼 체인뿐만 아니라 버퍼 블록 자체에도 직렬화 메커니즘이 존재한다. 바로 '버퍼 Lock'이다. 이런 직렬화 메커니즘에 의한 캐시 경합을 줄이려면, SQL 튜닝을 통해 쿼리 일량(논리적 I/O) 자체를 줄여야 한다.

> 읽고자 하는 블록을 찾았으면 캐시 버퍼 체인 래치를 곧바로 해제해야 한다. 그래야 `해당 래치가 풀리기를 기다리던 다른 프로세스들이 작업을 재개할 수 있다. 그런데 래치를 해제한 상태로 버퍼 블록 데이터를 읽고 쓰는 도중에 후행 프로세스가 하필 같은 블록에 접근해서 데이터를 읽고 쓴다면 데이터 정합성에 문제가 생길 수 있다. 이를 방지하기 위해 오라클은 버퍼 Lock을 사용한다. 캐시 버퍼 체인 래치를 해제하기 전에 버퍼 헤더에 Lock을 설정함으로써 버퍼 블록 자체에 대한 직렬화 문제를 해결하는 것이다.(같은 로우는 로우 Lock에 의해 보호될 텐데 버퍼 Lock이 왜 필요할까 싶겠지만, 로우 Lock을 설정하는 행위도 블록을 변경하는 작업이다. 로우 Lock을 설정하는 순간 다른 프로세스가 해당 블록을 읽는다면 문제가 생긴다. 그뿐만 아니라 같은 블록에서 서로 다른 로우를 동시에 읽고 쓰는 경우를 막기 위해서도 버퍼 Lock은 필요하다.)
{: .prompt-info }

###### 버퍼 블록의 상태

- Free 버퍼
  + 인스턴스 기동 후 아직 데이터가 읽히지 않아 비어 있는 상태(Clean 버퍼)이거나, 데이터가 담겼지만 데이터 파일과 서로 동기화돼 있는 상태여서 언제든지 덮어 써도 무방한 버퍼 블록
  + 데이터 파일로부터 새로운 데이터 블록을 로딩하려면 먼저 Free 버퍼를 확보해야 함
    * Free 상태인 버퍼에 변경이 발생하면 그 순간 Dirty 버퍼로 상태가 바뀜
- Dirty 버퍼
  + 버퍼에 캐시된 이후 변경이 발생했지만, 아직 디스크에 기록되지 않아 데이터 파일 블록과 동기화가 필요한 버퍼 블록
  + 이 버퍼 블록들이 다른 데이터 블록을 위해 재사용되려면 디스크에 먼저 기록되어야 함
    * 디스크에 기록되는 순간 Free 버퍼로 상태가 바뀜
- Pinned 버퍼
  + 읽기 또는 쓰기 작업이 현재 진행 중인 버퍼 블록

###### LRU(least recently used) 알고리즘

![08-lru-list](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/sql-performance-structure/08-lru-list.jpg)

- 모든 버퍼 블록 헤더를 LRU 체인에 연결해 사용빈도 순으로 위치를 옮겨가다가, Free 버퍼가 필요해질 때면 액세스 빈도가 낮은 쪽(LRU end) 데이터 블록부터 밀어내는 방식
- 모든 DBMS는 사용빈도가 높은 데이터 블록 위주로 버퍼 캐시가 구성되도록 LRU 알고리즘을 사용
  + 버퍼 캐시는 유한한 자원이므로 모든 데이터를 캐싱해 둘 수 없기 때문

##### 공유 풀(Shared Pool)

- 딕셔너리 캐시와 라이브러리 캐시로 구성되며, 버퍼 캐시처럼 LRU 알고리즘을 사용
- SQL Server에서 같은 역할을 하는 메모리 영역을 ‘프로시저 캐시(Procedure Cache)’라고 부름

###### 딕셔너리(Dictionary) 캐시

- 테이블, 인덱스 같은 오브젝트는 물론 테이블 스페이스, 데이터 파일, 세그먼트, 익스텐트, 사용자, 제약에 관한 **메타 정보를 저장**하는 곳
  + **딕셔너리에 저장된 정보들을 바탕으로 옵티마이저는 실행계획의 예상비용을 산정**
  + 저장된 정보들은 사용자가 삭제하지 않는 한 영구적으로 보관됨
  + 실행할 때 라이브러리 캐시에 적재함으로써 여러 사용자가 공유하며 재사용함
  + 로우(Row) 단위 I/O
    * 딕셔너리 캐시는 로우 캐시(Row Cache)라고도 불림
+ `주문` 테이블을 예로 들면, 입력한 주문 데이터는 데이터 파일에 저장됐다가 버퍼 캐시를 경유해 읽히지만, 테이블 메타 정보는 딕셔너리에 저장됐다가 딕셔너리 캐시를 경유해 읽힘

###### 라이브러리 캐시

- **사용자가 수행한 SQL 문과 실행계획, DB 저장형 함수/프로시저를 저장해 두는 캐시 영역**
  + 코드 캐시
- 사용자가 SQL 명령어를 통해 결과집합을 요청하면 이를 최적으로(가장 적은 리소스를 사용하면서 가장 빠르게) 수행하기 위한 처리 plan
  + 빠른 쿼리 수행을 위해 내부적으로 생성한 일종의 프로시저
- SQL 최적화 과정을 거쳐 생성됨

> 쿼리 구문을 분석해서 문법 오류 및 실행 권한 등을 체크하고, 최적화(Optimization) 과정을 거쳐 실행계획을 만들고, SQL 실행엔진이 이해할 수 있는 형태로 포맷팅하는 전 과정을 하드 파싱(Hard Parsing)이라고 한다. 특히 최적화 과정은 하드 파싱을 무겁게 만드는 가장 결정적 요인인데, 같은 SQL을 처리하려고 이런 무거운 작업을 반복 수행하는 것은 매우 비효율적이다. 이를 방지하기 위해 캐시 공간을 따로 두게 되었고, 그것이 바로 라이브러리 캐시 영역이다. 캐싱된 SQL과 그 실행계획의 재사용성을 높이는 것은 SQL 수행 성능을 높이고 DBMS 부하를 최소화하는 핵심 원리 중 한가지다.
{: .prompt-info }

##### 로그 버퍼(Log Buffer)

DB 버퍼 캐시에 가해지는 모든 변경사항을 Redo 로그 파일에 기록한다고 앞서 설명했는데, 로그 엔트리(데이터베이스의 변경 사항이나 이벤트를 기록하는 데이터 항목)도 파일에 곧바로 기록하는 것이 아니라 먼저 로그 버퍼에 기록한다. 건건이 디스크에 기록하기보다 일정량을 모았다가 기록하면 훨씬 빠르기 때문이다. 좀 더 자세히 설명하면, 서버 프로세스가 데이터 블록 버퍼에 변경을 가하기 전에 Redo 로그 버퍼에 먼저 기록해 두면 주기적으로 Log Writer(LGWR) 프로세스가 Redo 로그 파일에 기록한다. Oracle의 Redo 로그, Redo 로그 버퍼와 대비되는 개념이 SQL Server의 트랜잭션 로그, 로그 캐시다. 변경이 가해진 Dirty 버퍼를 데이터 파일에 기록하기 전에 항상 로그 버퍼를 먼저 로그 파일에 기록해야만 하는데, 그 이유는 인스턴스 장애가 발생할 때면 로그 파일에 기록된 내용을 재현해 캐시 블록을 복구하고, 최종적으로 커밋되지 않은 트랜잭션은 롤백해야 한다. 이때, 로그 파일에는 없는 변경내역이 이미 데이터 파일에 기록돼 있으면 사용자가 최종 커밋하지 않은 트랜잭션이 커밋되는 결과를 초래하기 때문이다. 정리해 보면, 버퍼 캐시 블록을 갱신하기 전에 변경사항을 먼저 로그 버퍼에 기록해야 하며, Dirty 버퍼를 디스크에 기록하기 전에 해당 로그 엔트리를 먼저 로그 파일에 기록해야 하는데, 이를 ‘Write Ahead Logging’이라고 한다. 그리고 로그 버퍼를 주기적으로 로그 파일에 기록한다고 했는데, 늦어도 커밋 시점에는 로그 파일에 기록해야 한다(Log Force at commit). 메모리상의 로그 버퍼는 언제든 유실될 가능성이 있기 때문이다. 로그를 이용한 Fast Commit이 가능한 이유는 로그를 이용해 언제든 복구 가능하기 때문이라고 설명한 것을 상기하기 바란다. 다시 말하지만, 로그 파일에 기록했음이 보장돼야 안심하고 커밋을 완료할 수 있다.

#### 프로세스 전용 메모리 영역

> *Oracle은 프로세스 기반 아키텍처이므로 서버 프로세스가 자신만의 전용 메모리 영역을 가질 수 있는데, 이를 ‘Process Global Area(PGA)’라고 부르며, 데이터를 정렬하고 세션과 커서에 관한 상태 정보를 저장하는 용도로 사용한다. Thread 기반 아키텍처를 사용하는 SQL Server는 프로세스 전용 메모리 영역을 갖지 않는다. Thread는 전용 메모리 영역을 가질 수 없고, 부모 프로세스의 메모리 영역을 사용하기 때문이다. 참고로, Windows 버전 Oracle도 Thread를 사용하지만 프로세스 기반의 Unix 버전과 같은 인터페이스를 제공하고 구조에 대한 개념과 설명도 구별하지 않는다.*

##### PGA(Process Global Area)

- 각 Oracle 서버 프로세스는 자신만의 PGA(Process/Program/Private Global Area) 메모리 영역을 할당받고, 이를 프로세스에 종속적인 고유 데이터를 저장하는 용도로 사용
- PGA는 다른 프로세스와 공유되지 않는 독립적인 메모리 공간으로서, 래치 메커니즘이 필요 없어 똑같은 개수의 블록을 읽더라도 SGA 버퍼 캐시에서 읽는 것보다 훨씬 빠름

###### User Global Area(UGA)

- 세션이 프로세스 개수보다 많아질 수 있는 구조인 [공유 서버(Shared Server) 방식](#공유-서버shared-server-방식)에서 사용되는 각 **세션을 위한 독립적인 메모리 공간**
  + [전용 서버](#전용-서버dedicated-server-방식)으로 연결할 때는 프로세스와 세션이 1:1 관계를 갖지만, 공유 서버 방식으로 연결할 때는 1:M 관계를 갖음
    * 공유 서버 방식에서는 하나의 프로세스가 여러 개의 세션을 위해 일함

> 전용 서버 방식이라고 해서 UGA를 사용하지 않는 것은 아니다. UGA는 전용 서버 방식으로 연결할 때는 PGA에 할당되고, 공유 서버 방식으로 연결할 때는 SGA에 할당된다. 구체적으로 후자는, Large Pool이 설정됐을 때는 Large Pool에, 그렇지 않을 때는 Shared Pool에 할당하는 방식이다.
{: .prompt-info }

###### Call Global Area(CGA)

- Oracle 데이터베이스의 메모리 구조에서 클라이언트와 관련된 정보를 저장하는 메모리 영역
  + 하나의 데이터베이스 Call을 넘어서 다음 Call까지 계속 참조되어야 하는 정보는 UGA에 담고, Call이 진행되는 동안에만 필요한 데이터는 CGA에 담음
- UGA와 함께 동작하여 Oracle의 클라이언트와 서버 간의 데이터 교환을 지원하는 역할
- CGA는 Parse Call, Execute Call, Fetch Call마다 매번 할당받음
  + Call이 진행되는 동안 Recursive Call이 발생하면 그 안에서도 Parse, Execute, Fetch 단계별로 CGA가 추가로 할당됨
  + CGA에 할당된 공간은 하나의 Call이 끝나자마자 해제돼 PGA로 반환됨

###### Sort Area

- Oracle 데이터베이스에서 정렬, 집합 연산, 그리고 집합 연산과 관련된 작업을 수행하기 위해 사용되는 메모리 공간
- 집합 연산이 진행되는 동안 공간이 부족해질 때마다 청크(Chunk) 단위로 조금씩 할당됨
  + 세션마다 사용할 수 있는 최대 크기를 예전에는 `sort_area_size` 파라미터로 설정하였으나, 9i부터는 새로 생긴 `workarea_size_policy` 파라미터를 `auto`(기본 값)로 설정하면 Oracle이 내부적으로 결정

> PGA 내에서 Sort Area가 할당되는 위치는 SQL문 종류와 소트 수행 단계에 따라 다르다. DML 문장은 하나의 Execute Call 내에서 모든 데이터 처리를 완료하므로 Sort Area가 CGA에 할당된다. SELECT 문장의 경우, 수행 중간 단계에 필요한 Sort Area는 CGA에 할당되고, 최종 결과집합을 출력하기 직전 단계에 필요한 Sort Area는 UGA에 할당된다. 앞에서 이미 설명한 것처럼, 쓰레드(Thread) 기반 아키텍처를 사용하는 SQL Server는 프로세스 전용 메모리 영역을 갖지 않는다. 대신, 데이터 정렬은 Memory Pool 안에 있는 버퍼 캐시에서 수행하며, 세션 관련 정보는 Memory Pool 안에 있는 Connection Context 영역에 저장한다.
{: .prompt-info }

## SQL 처리 과정

![1-2](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/sql-performance-structure/1-2.jpg)

> *오라클의 PL/SQL, SQL Serve의 T-SQL처럼 절차적(procedural) 프로그래밍 기능을 구현할 수 있는 확장 언어도 제공하지만, SQL은 기본적으로 구조적(Structured)이고 집합적(set-based)이며 선언적(declarative)인 질의 언어다.*
>
> *원하는 결과 집합을 구조적/집합적으로 선언하지만, 그 결과 집합을 만드는 과정은 절차적일 수밖에 없다. 즉, 프로시저가 필요한데, 그런 프로시저를 만들어 내는 DBMS 내부 엔진이 바로 SQL 옵티마이저다. 옵티마이저가 프로그래밍을 대신해 주는 셈이다. DBMS 내부에서 프로시저를 작성하고 컴파일해서 실행 가능한 상태로 만드는 전 과정을 'SQL 최적화'라고 한다.*

### 1. SQL 문장 수신

- 클라이언트 요청: 사용자가 SQL 쿼리를 클라이언트 애플리케이션에서 작성하고, 데이터베이스 서버에 요청을 전송
- 서버 수신: 데이터베이스 서버가 클라이언트로부터 SQL 문장을 수신

### 2. SQL 문장 파싱

- SQL이 SQL 파서(Parser)에 의해 파싱되는 단계
  + 파서는 SQL을 분석하여 **구문이 올바른지 확인**하고, 문장의 의미를 이해함
    * 문법 오류가 발견되면 오류 메시지가 반환
- 구문 트리를 생성하고, 논리적 실행 계획을 수립

### 3. SQL 문장 최적화

- 파싱된 SQL 문장은 옵티마이저에 의해 처리됨
  + 옵티마이저는 SQL 쿼리의 실행 계획을 결정하고, 가장 효율적인 방법으로 쿼리를 실행하기 위해 최적의 접근 경로를 선택
    * [딕셔너리 캐시](#딕셔너리dictionary-캐시)에 미리 수집해 둔 오브젝트 통계 및 시스템 정보를 활용
- [[SQLP \| 3과목]SQL 고급활용 및 튜닝 - SQL 옵티마이저(2024)](https://drj9812.github.io/posts/sql-optimizer/){: target="_blank" } 참고

### 4. 실행 계획 생성

- 실행 계획
  + 최적화된 SQL 쿼리는 실행 계획으로 변환됨
  + 실행 계획은 SQL 쿼리를 실제로 실행하기 위한 단계별 지침을 제공
- 계획 실행: 데이터베이스 엔진이 실행 계획을 바탕으로 실제 쿼리를 실행
- [[SQLP \| 3과목]SQL 고급활용 및 튜닝 - SQL 옵티마이저(2024)](https://drj9812.github.io/posts/sql-optimizer/){: target="_blank" } 참고

### 5. SQL 문장 실행

- 실행 엔진
  + 데이터베이스 엔진은 실행 계획을 따라 SQL 쿼리를 실행
  + 이 과정에서 필요한 데이터 검색, 업데이트, 삽입, 삭제 작업이 수행됨
- 데이터 검색: 쿼리에 필요한 데이터를 검색
- 데이터 수정: 쿼리에 따라 데이터를 삽입, 업데이트 또는 삭제

### 6. 결과 반환

- 결과 처리: 쿼리 실행 결과가 생성되면, 결과는 클라이언트 애플리케이션으로 반환
- 클라이언트 수신: 클라이언트 애플리케이션은 결과를 받아서 사용자가 이해할 수 있는 형태로 표시

### 7. 트랜잭션 처리

- 커밋 또는 롤백
  + 트랜잭션이 포함된 쿼리의 경우, 트랜잭션이 성공적으로 완료되면 커밋이 수행되어 변경 사항이 데이터베이스에 영구적으로 저장됨
  + 오류가 발생하면 롤백이 수행되어 모든 변경 사항이 취소됨

## 데이터베이스 I/O 메커니즘

### 블록 단위 I/O

- 모든 DBMS에서 I/O는 블록 단위로 이루어짐
  + 특정 레코드 하나를 읽고 싶어도 해당 블록을 통째로 읽음
- **SQL 성능을 좌우하는 가장 중요한 성능지표는 액세스하는 블록의 개수**
  + 블록의 개수가 옵티마이저의 판단에 가장 큰 영향을 미침
- 블록 단위 I/O는 **버퍼 캐시와 데이터 파일** 모두에 적용됨
  + 데이터 파일에서 DB **버퍼 캐시로 블록을 적재할 때**
  + 데이터 파일에서 블록을 직접 읽고 쓸 때
  + 버퍼 캐시에서 블록을 읽고 쓸 때
  + 버퍼 캐시에서 변경된 블록을 다시 데이터 파일에 쓸 때

### 메모리 I/O, 디스크 I/O

#### I/O 효율화 튜닝의 중요성

![disk-io](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/sql-performance-structure/disk-io.jpg)
*디스크 I/O 경합*

- 모든 DBMS는 데이터를 읽을 때 먼저 [버퍼 캐시](#db-버퍼-캐시db-buffer-cache)에서 찾아보고(메모리 I/O), 없을 경우 디스크에서 읽어와(디스크 I/O) 버퍼 캐시에 적재한 후 작업을 수행
  + **디스크 I/O가 필요할 때면 서버 프로세스는 CPU를 OS에 반환하고 I/O가 완료되기를 기다림**
    * 블로킹(Blocking) I/O
    * 디스크 I/O 경합이 심할 수록 대기 시간도 길어짐
    * **블로킹 상태에 빠진 서버 프로세스는 성능에 악영향을 미침**
  + <font color="red">모든 데이터를 메모리(버퍼 캐시)에 상주시킬 수 없으므로, 디스크 I/O를 최소화하고 버퍼 캐시의 효율을 높이는 것이 데이터베이스 I/O 튜닝의 목표</font>
- 데이터의 입력이나 삭제가 없어도 디스크 I/O는 SQL을 실행할 때 마다 다름

> 데이터의 입력이나 삭제가 없어도 디스크 I/O는 SQL을 실행할 때 마다 다르다. 첫 번째 실행할 때보다 두번째 실행할 때 줄어들고, 세 번째 실행할 땐 더 줄어든다. 연속해서 실행하면 DB 버퍼캐시에서 해당 테이블 블록의 점유율이 점점 높아지기 때문이다. 한참 후에 다시 실행하면 반대로 물리적 I/O가 늘어난다. DB 버퍼 캐시가 다른 테이블 블록으로 채워지기 때문이다.
{: .prompt-info }

##### 블로킹(Blocking) I/O 과정

![blocking-io-process](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/sql-performance-structure/blocking-io-process.jpg)
*Blocking I/O 과정*

1. I/O 요청
  - 프로세스가 디스크에서 데이터를 읽어야 할 때, 해당 프로세스는 OS에게 I/O 요청
2. CPU 반환
  - I/O 요청을 한 후, 프로세스는 더 이상 즉시 수행할 작업이 없으므로, CPU를 OS에 반환
    + 다른 프로세스들이 CPU를 사용할 수 있도록 하기 위함
3. 수면 상태
  - I/O 작업이 완료될 때까지 프로세스는 수면 상태로 전환
    + 수면 상태에 있는 동안 프로세스는 대기 큐(Wait Queue)에 놓임
4. I/O 완료 알림
  - 디스크에서 데이터 읽기가 완료되면, OS는 해당 프로세스에 알림을 보냄
5. 프로세스 재개
  - 알림을 받은 프로세스는 대기 큐에서 깨워져 다시 실행 상태로 전환되고, CPU를 할당받아 이후 작업을 계속 수행

#### 버퍼 캐시 히트율(Buffer Cache Hit Ratio)

$$ \text{BHCR} = (\frac{\text{버퍼 캐시에서 곧바로 찾은 블록 수}((\text{query} + \text{current}) - \text{disk})}{\text{총 읽은 블록 수}(\text{query} + \text{current})}) \times 100 $$

- 버퍼 캐시 효율을 측정하는 지표로서, 전체 읽은 블록 중에서 메모리 버퍼 캐시에서 찾은 비율을 나타냄
  + 물리적인 디스크 읽기를 수반하지 않고 곧바로 메모리에서 블록을 찾은 비율
    * Direct Path Read 방식 이외의 모든 블록 읽기는 버퍼 캐시를 통해 이루어짐
  + SQL 성능을 향상시키려면 디스크 I/O가 아닌 메모리 I/O를 줄이는 것이 효율적임
- 전용 서버 방식을 사용하는 OLTP(Online Transaction Processing)성 애플리케이션이라면 시스템 레벨에서 평균 99% 히트율을 달성해야 함

> 같은 블록을 반복적으로 액세스하는 형태의 SQL은 논리적인 I/O 요청이 비효율적으로 많이 발생함에도 불구하고 BCHR은 매우 높게 나타난다. 이는 BCHR이 성능지표로서 갖는 한계점이라 할 수 있다. 예를 들어 NL Join에서 작은 Inner 테이블을 반복적으로 룩업(Lookup)하는 경우가 그렇다. 작은 테이블을 반복 액세스하면 모든 블록이 메모리에서 찾아져 BCHR은 높겠지만 일량이 작지 않고, 블록을 찾는 과정에서 래치(Latch) 경합과 버퍼 Lock 경합까지 발생한다면 메모리 I/O 비용이 디스크 I/O 비용보다 커질 수 있다. 따라서 논리적으로 읽어야 할 블록 수의 절대량이 많다면 반드시 튜닝을 통해 논리적인 블록 읽기를 최소화해야 한다.
{: .prompt-warning }

##### 예시

###### SQL Trace

|    call   |  count  |    cpu   |  elapsed |  disk  |  query  | current |  rows  |
|:---------:|:-------:|:--------:|:--------:|:------:|:-------:|:-------:|:------:| 
|   parse   |    15   |   0.00   |   0.08   |    0   |    0    |    0    |    0   | 
|  execute  |    44   |   0.03   |   0.03   |    0   |    0    |    0    |    0   | 
|   fetch   |    44   |   0.01   |   0.13   |   18   |   822   |    0    |   44   | 
| **total** | **103** | **0.04** | **0.25** | **18** | **822** |  **0**  | **44** |

- `parse`
  + SQL 문을 구문 분석하는 단계
  + 15번의 파싱 호출이 발생했고, CPU 시간은 0.00초, 경과 시간은 0.08초
- `execute`
  + SQL 문을 실행하는 단계
  + 44번의 실행 호출이 발생했고, CPU 시간은 0.03초, 경과 시간도 0.03초
- `fetch`
  + 실행된 SQL 문의 결과를 가져오는 단계
  + 44번의 페치 호출이 발생했고, CPU 시간은 0.01초, 경과 시간은 0.13초
  + 디스크 I/O는 18번 발생했으며, 822개의 질의가 수행됨
  + 현재(Current) 값은 없으며, 44개의 행이 반환됨
- `total`
  + 전체 합계

> 총 읽은 블록 수(Query + Current)가 디스크로부터 읽은 블록 수를 이미 포함하므로, 총 읽은 블록 수를 840개(Disk + Query + Current)로 잘못 해석하지 않도록 주의해야 한다.
{: .prompt-warning }

#### 네트워크, 파일시스템 캐시가 I/O 효율에 미치는 영향

> *대용량 데이터를 읽고 쓰는 데 다양한 네트워크 기술(DB서버와 스토리지 간에 NAS 서버나 SAN을 사용)이 사용됨에 따라 네트워크 속도도 SQL 성능에 크게 영향을 미치고 있다. 이에 하드웨어나 DBMS 벤더는 네트워크를 통한 데이터 전송속도를 향상시키려고 노력하고 있지만, 네트워크 전송량이 많을 수 밖에 없도록 SQL을 작성한다면 결코 좋은 성능을 기대할 수 없다. 따라서 SQL을 작성할 때는 다양한 I/O 튜닝 기법을 사용해서 네트워크 전송량을 줄이려고 노력하는 것이 중요하다. RAC 같은 클러스터링(Clustering) 데이터베이스 환경에선 인스턴스 간 캐시된 블록을 공유하므로 메모리 I/O 성능에도 네트워크 속도가 지대한 영향을 미치게 되었다. 같은 양의 디스크 I/O가 발생하더라도 I/O 대기 시간이 크게 차이 날 때가 있다. 디스크 경합 때문일 수도 있고, OS에서 지원하는 파일 시스템 버퍼 캐시와 SAN 캐시 때문일 수도 있다. SAN 캐시는 크다고 문제될 것이 없지만, 파일 시스템 버퍼캐시는 최소화해야 한다. 데이터베이스 자체적으로 캐시 영역을 갖고 있으므로 이를 위한 공간을 크게 할당하는 것이 더 효과적이다. 네트워크 문제이든, 파일시스템 문제이든 I/O 성능에 관한 가장 확실하고 근본적인 해결책은 논리적인 블록 요청 횟수를 최소화하는 것이다.*

### Sequential I/O, Random I/O

![sequential-io-and-random-io.jpg](/assets/img/posts/certifications/sqlp/3-sql-advanced-utilization-and-tuning/sql-performance-structure/sequential-io-and-random-io.jpg)

- 순차 접근(Sequential Access)
  + 레코드간 **논리적 또는 물리적인 순서**를 따라 차례대로 읽어 나가는 방식
    * 사진의 ⑤번
  + ~~I/O 성능을 높이기 위해서는 순차 접근에 의한 선택 비중을 높여야 함~~
- 임의 접근(Random Access)
  + 레코드 하나를 읽기 위해 데이터 블록에 한 블록씩 **임의로 접근**하는 방식
    * 사진의 ①, ②, ③, ④, ⑥번
  + ~~I/O 성능을 높이기 위해서는 임의 접근 발생량을 줄여야 함~~

> [순차 접근(Sequential Access), 임의 접근(Random Access)](https://drj9812.github.io/posts/sequential-access-and-radom-access/){: target="_blank" } 참고
{: .prompt-tip }

### Single Block I/O, MultiBlock I/O

- Single Block I/O
  + 한번의 I/O 요청에 하나의 데이터 블록만 읽어 메모리에 적재하는 방식
  + **인덱스를 통해 테이블에 접근할 때는, 기본적으로 인덱스와 테이블 블록 모두 이 방식을 사용**
    * 인덱스 루트 블록을 읽을 때
    * 인덱스 루트 블록에서 얻은 주소 정보로 브랜치 블록을 읽을 때
    * 인덱스 브랜치 블록에서 얻은 주소 정보로 리프 블록을 읽을 때
    * 인덱스 리프 블록에서 얻은 주소 정보로 테이블 블록을 읽을 때`
  + 인덱스 스캔 시 효율적임
    * 인덱스 블록간 논리적 순서(이중 연결 리스트 구조로 연결된 순서)는 데이터 파일에 저장된 물리적인 순서와 다르기 때문임
- MultiBlock I/O
  + I/O 요청이 필요한 시점에, **인접한 블록들을 같이 읽어 메모리에 적재하는 방식**
    * 인접한 블록이란 한 익스텐트(Extent)에 속한 블록을 의미하며, 달리 말하면, 익스텐트 범위를 넘어서까지 읽지 않음
  + Table Full Scan처럼 물리적으로 저장된 순서에 따라 읽을 때는 인접한 블록들을 같이 읽는 것이 유리함

> [[Database]Single Block I/O, MultiBlock I/O](https://drj9812.github.io/posts/single-block-io-and-multiblock-io/){: target="_blank" } 참고
{:. prompt-tip }

> 물리적으로 한 익스텐트에 속한 블록들을 I/O 요청 시점에 같이 메모리에 올렸는데, 그 블록들이 논리적 순서로는 한참 뒤쪽에 위치할 수 있다. 그러면 그 블록들은 실제 사용되지 못한 채 버퍼 상에서 밀려나는 일이 발생한다. 하나의 블록을 캐싱하려면 다른 블록을 밀어내야 하는데, 이런 현상이 자주 발생한다면 앞에서 소개한 버퍼 캐시 효율만 떨어뜨리게 된다. 대량의 데이터를 MultiBlock I/O 방식으로 읽을 때 Single Block I/O 보다 성능상 유리한 이유는 I/O 요청 발생 횟수를 줄여주기 때문이다.
{: .prompt-info }

### I/O 효율화 원리

- 필요한 최소 블록만 읽도록 SQL 작성
- 최적의 옵티마이징 팩터 제공
- 필요하다면, 옵티마이저 힌트를 사용해 최적의 액세스 경로로 유도

> 논리적인 I/O 요청 횟수를 최소화하는 것이 I/O 효율화 튜닝의 핵심 원리다. I/O 때문에 시스템 성능이 낮게 측정될 때 하드웨어적인 방법을 통해 I/O 성능을 향상 시킬 수도 있다. 하지만 SQL 튜닝을 통해 I/O 발생 횟수 자체를 줄이는 것이 더 근본적이고 확실한 해결 방안이다.
{: .prompt-info }

#### 필요한 최소 블록만 읽도록 SQL 작성

데이터베이스 성능은 I/O 효율에 달렸고, 이를 달성하려면 동일한 데이터를 중복 액세스하지 않고, 필요 명령을 사용자는 최소 일량을 요구하는 형태로 논리적인 집합을 정의하고, 효율적인 처리가 가능하도록 작성하는 것이 무엇보다 중요하다. 아래는 비효율적인 중복 액세스를 없애고 필요한 최소 블록만 액세스하도록 튜닝한 사례다.

```sql
SELECT a.카드번호 , a.거래금액 전일_거래금액 , b.거래금액 주간_거래금액 , c.거래금액 전월_거래금액 , d.거래금액 연중_거래금액
  FROM (-- 전일거래실적
       SELECT 카드번호, 거래금액
         FROM 일별카드거래내역
        WHERE 거래일자 = TO_CHAR(SYSDATE - 1, 'yyyymmdd')) a,
       (-- 전주거래실적
       SELECT 카드번호, SUM(거래금액) AS 거래금액
         FROM 일별카드거래내역
        WHERE 거래일자 BETWEEN TO_CHAR(SYSDATE - 7, 'yyyymmdd')
              AND
              TO_CHAR(SYSDATE - 1, 'yyyymmdd')
        GROUP BY 카드번호) b,
       (-- 전월거래실적
       SELECT 카드번호, SUM(거래금액) AS 거래금액
         FROM 일별카드거래내역
        WHERE 거래일자 BETWEEN TO_CHAR(ADD_MONTHS(SYSDATE, -1), 'yyyymm') || '01'
              AND TO_CHAR(LAST_DAY(ADD_MONTHS(SYSDATE, -1)), 'yyyymmdd')
        GROUP BY 카드번호) c,
       ( -- 연중거래실적
       SELECT 카드번호, SUM(거래금액) AS 거래금액
         FROM 일별카드거래내역
        WHERE 거래일자 BETWEEN TO_CHAR(ADD_MONTHS(SYSDATE, -12), 'yyyymmdd')
              AND TO_CHAR(SYSDATE -1, 'yyyymmdd')
        GROUP BY 카드번호) d
 WHERE b.카드번호 (+) = a.카드번호
       AND
       c.카드번호 (+) = a.카드번호
       AND
       d.카드번호 (+) = a.카드번호;
```

위 SQL은 어제 거래가 있었던 카드에 대한 전일, 주간, 전월, 연중 거래 실적을 집계하고 있다. 논리적인 전체 집합은 과거 1년치인데, 전일, 주간, 전월 데이터를 각각 액세스한 후 조인한 것을 볼 수 있다. 전일 데이터는 총 4번을 액세스한 셈이다. SQL을 아래와 같이 작성하면 과거 1년치 데이터를 한번만 읽고 전일, 주간, 전월 결과를 구할 수 있다. 즉 논리적인 집합 재구성을 통해 액세스해야 할 데이터 양을 최소화 할 수 있다.

```sql
SELECT 카드번호,
       SUM(CASE WHEN 거래일자 = TO_CHAR(SYSDATE - 1, 'yyyymmdd') THEN 거래금액 END) AS 전일_거래금액,
       SUM(CASE WHEN 거래일자 BETWEEN TO_CHAR(SYSDATE - 7, 'yyyymmdd')
                                      AND
                                      TO_CHAR(SYSDATE - 1, 'yyyymmdd') THEN 거래금액 END) AS 주간_거래금액,
       SUM(CASE WHEN 거래일자 BETWEEN TO_CHAR(ADD_MONTHS(SYSDATE, -1), 'yyyymm') || '01'
                                      AND
                                      TO_CHAR(last_day(ADD_MONTHS(SYSDATE, -1)), 'yyyymmdd') THEN 거래금액 END) AS 전월_거래금액,
       SUM(거래금액) AS 연중_거래금액
  FROM 일별카드거래내역
 WHERE 거래일자 BETWEEN TO_CHAR(ADD_MONTHS(SYSDATE, -12), 'yyyymmdd')
       AND
       TO_CHAR(SYSDATE - 1, 'yyyymmdd')
 GROUP BY 카드번호
HAVING SUM(CASE WHEN 거래일자 = TO_CHAR(SYSDATE - 1, 'yyyymmdd') THEN 거래금액 END) > 0;
```

#### 최적의 옵티마이징 팩터 제공

옵티마이저가 블록 액세스를 최소화하면서 효율적으로 처리할 수 있도록 하려면 최적의 옵티마이징 팩터를 제공해 주어야 한다.

- 전략적인 인덱스 구성
  + 전략적인 인덱스 구성은 가장 기본적인 옵티마이징 팩터다.
- DBMS가 제공하는 기능 활용
  + 인덱스 외에도 DBMS가 제공하는 다양한 기능을 적극적으로 활용한다. 인덱스, 파티션, 클러스터, 윈도우 함수 등을 적극 활용해 옵티마이저가 최적의 선택을 할 수 있도록 한다.
- 옵티마이저 모드 설정
  + 옵티마이저 모드(전체 처리속도 최적화, 최초 응답속도 최적화)와 그 외 옵티마이저 행동에 영향을 미치는 일부 파라미터를 변경해 주는 것이 도움이 될 수 있다.
- 통계정보
  + 옵티마이저에게 정확한 정보를 제공한다.

#### 필요하다면, 옵티마이저 힌트를 사용해 최적의 액세스 경로로 유도

최적의 옵티마이징 팩터를 제공했다면 가급적 옵티마이저 판단에 맡기는 것이 바람직하지만 옵티마이저가 생각만큼 최적의 실행계획을 수립하지 못하는 경우가 종종 있다. 그럴 때는 어쩔 수 없이 힌트를 사용해야 한다. 아래는 옵티마이저 힌트를 이용해 실행계획을 제어하는 방법을 예시하고 있다.

```sql
-- Oracle
SELECT /*+ leading(d) use_nl(e) index(d dept_loc_idx) */ *
  FROM emp e, dept d
 WHERE e.deptno = d.deptno
       AND
       d.loc = 'CHICAGO';

-- SQL Server
SELECT *
  FROM dept d
  WITH (INDEX(dept_loc_idx)), emp e
 WHERE e.deptno = d.deptno
       AND
       d.loc = 'CHICAGO'
OPTION (FORCE ORDER, LOOP JOIN);
```
옵티마이저 힌트를 사용할 때는 의도한 실행계획으로 수행되는지 반드시 확인해야 한다.

CBO 기술이 고도로 발전하고 있긴 하지만 여러 가지 이유로 옵티마이저 힌트의 사용은 불가피하다. 따라서 데이터베이스 애플리케이션 개발자라면 인덱스, 조인, 옵티마이저의 기본 원리를 이해하고, 그것을 바탕으로 최적의 액세스 경로로 유도할 수 있는 능력을 필수적으로 갖추어야 한다. 3장부터 그런 원리들을 하나씩 학습하게 될 것이다.

## 참고자료

- 한국데이터산업진흥원, *SQL 자격검정 실전문제*(서울: 한국데이터산업진흥원, 2016), 279.
- [admin, *데이터베이스 아키텍쳐*, 데이터온에어, 2021-02-15](https://dataonair.or.kr/db-tech-reference/d-guide/sql/?pageid=2&mod=document&uid=357){: target="_blank" }
- [admin, *데이터베이스 I/O 원리*, 데이터온에어, 2021-02-15](https://dataonair.or.kr/db-tech-reference/d-guide/sql/?pageid=2&mod=document&uid=360){: target="_blank" }
- [admin, *옵티마이저와 실행계획*, 데이터온에어, 2021-02-15](https://dataonair.or.kr/db-tech-reference/d-guide/sql/?pageid=3&mod=document&uid=354){: target="_blank" }
- 조시형, *친절한 SQL 튜닝*(서울: 디비안, 2018), 560.
- 조시형, *국가공인 SQLP 자격검정 핵심노트 Ⅰ*(서울: 디비안, 2021), 272
- 조시형, *국가공인 SQLP 자격검정 핵심노트 Ⅱ*(서울: 디비안, 2021), 278