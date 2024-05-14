---
title: "[Oracle]23c 샘플 스키마 설치하기(Windows)"
categories: [Tools, Oracle]
tags: [Oracle, 오라클, sample shcema, 샘플 스키마]
---

# 샘플 스키마 설치하기

## 환경

- OS : Windows 11 HOME
- Oracle Database Version : 21c

## 설치

### 소스코드 다운로드

![01-oracle-homepage](/assets/img/posts/tools/oracle/install-sample-schema-on-windows/01-oracle-homepage.jpg)
*<https://docs.oracle.com/en/database/oracle/oracle-database/21/comsc/installing-sample-schemas.html#GUID-3820972A-08D7-4033-9524-1E36676594EE>{: target="_blank" } 접속 > `Copy`*

![02-download-source-code](/assets/img/posts/tools/oracle/install-sample-schema-on-windows/02-download-source-code.jpg)
*<https://github.com/oracle-samples/db-sample-schemas/releases/tag/v23.3>{: target="_blank" } 접속 > `Source code (zip)`*

![03-select-schema-folder](/assets/img/posts/tools/oracle/install-sample-schema-on-windows/03-select-schema-folder.jpg)
*다운로드한 파일 압축 해제 > `db-sample-schemas-23.3/db-sample-schemas-23.3` > 설치할 스키마의 폴더 선택*

- customer_orders
	+ 고객 주문에 관련된 데이터를 포함
	+ 일반적으로 고객 정보, 주문 정보, 주문 상태 등이 포함될 수 있음
	+ 주문 처리 및 고객 관리와 같은 영역에 대한 데이터를 제공
	+ **스키마를 설치하기 위해서는 해당 폴더 내의 `co_install.sql`{: .filepath } 실행해야 함**

- human_resources
	+ 인사 관리에 관련된 데이터를 포함
	+ 일반적으로 사원 정보, 부서 정보, 급여 정보, 근무 기록 등이 포함될 수 있음
	+ 조직의 인적 자원 관리와 관련된 데이터를 제공
	+ **스키마를 설치하기 위해서는 해당 폴더 내의 `hr_install.sql`{: .filepath } 실행해야 함**

- order_entry
	+ 주문 입력 및 처리에 관련된 데이터를 포함
	+ 주문 정보, 제품 정보, 고객 정보 등이 포함될 수 있음
	+ 주문 처리 시스템의 데이터 모델을 제공
	+ **스키마를 설치하기 위해서는 해당 폴더 내의 `oc_main.sql`{: .filepath } 실행해야 함**

- product_media
	+ 제품과 미디어 자료에 관련된 데이터를 포함
	+ 제품 정보, 이미지, 동영상, 설명 등이 포함될 수 있음
	+ 제품 관련 미디어 자료를 관리하는 데 사용될 수 있음
	+ **스키마를 설치하기 위해서는 해당 폴더 내의 `pm_main.sql`{: .filepath } 실행해야 함**

- sales_history
	+ 판매 이력에 관련된 데이터를 포함
	+ 일반적으로 판매 주문, 판매량, 매출 정보 등이 포함될 수 있음
	+ 판매 활동과 관련된 데이터를 분석하거나 보고하는 데 사용될 수 있음
	+ **스키마를 설치하기 위해서는 해당 폴더 내의 `sh_install.sql`{: .filepath } 실행해야 함**

![04-move-schema-folder](/assets/img/posts/tools/oracle/install-sample-schema-on-windows/04-move-schema-folder.jpg)
*선택한 스키마의 폴더를 `app/사용자의 홈 디렉토리 이름/product/21c/dbhomeXE/demo/schema`{: .filepath }로 이동*

### SQL 스크립트 파일 실행

```console
$ sqlplus / as sysdba
```

![05-connect-as-admin](/assets/img/posts/tools/oracle/install-sample-schema-on-windows/05-connect-as-admin.jpg)
*관리자 수준으로 접속*

```console
SQL> ALTER session SET "_ORACLE_SCRIPT"=true;
```

12c 버전부터는 컨테이너 데이터베이스로 설치할 때 일반적인 유저 생성 규칙이 변경되었다. 이전에는 "c##"을 붙이지 않고도 일반적인 이름으로 유저를 생성할 수 있었지만, 12c 이후부터는 컨테이너 데이터베이스에 연결된 플러그러블 데이터베이스(PDB)를 사용할 때 "c##"을 붙여야 한다. 그렇지 않으면 "올바르지 않은 이름"과 같은 에러가 발생할 수 있다.

`ALTER session SET "_ORACLE_SCRIPT"=true;` 문장은 이러한 규칙을 우회하기 위한 것으로, 이를 통해 "c##"을 붙이지 않고도 유저를 생성할 수 있다. 하지만 이는 보안상 권장되는 방법은 아니며, 관리자나 DBA가 적절한 보안 정책을 준수하도록 하는 것이 바람직하다.

![06-change-session](/assets/img/posts/tools/oracle/install-sample-schema-on-windows/06-change-session.jpg)

```console
SQL> @?/demo/schema/human_resources/실행할 SQL 스크립트 파일

Enter a password for the user HR: 비밀번호 입력

Enter a tablespace for HR [USERS]: 테이블스페이스 입력

Do you want to overwrite the schema, if it already exists? [YES|no]: YES 또는 no 입력
```

![07-run-hr_install.sql](/assets/img/posts/tools/oracle/install-sample-schema-on-windows/07-run-hr_install.sql.jpg)

- `@?/demo/schema/human_resources/hr_install.sql`
	+ `@`: SQL 명령줄에서 외부 스크립트를 실행하기 위한 명령어
	+ `?`: 현재 Oracle 환경의 설정된 디렉토리(오라클 환경 변수에 정의된 디렉토리)
	+ `/demo/schema/human_resources/hr_install.sql`: 실행할 SQL 스크립트의 경로
- Enter a password for the user HR
	+ HR 사용자 계정의 비밀번호를 입력
- Enter a tablespace for HR [USERS]
	+ HR 사용자의 테이블스페이스 입력
	+ 사용자 데이터를 저장하는 기본 테이블스페이스가 제안됨
		* USERS
- Do you want to overwrite the schema, if it already exists? [YES\|no]
	+ 이미 해당 스키마가 존재하는 경우 스키마를 덮어쓸 것인지 여부를 묻는 메시지

> `@?/demo/schema/human_resources/실행할 SQL 스크립트 파일` 이후에 나오는 메시지는 버전, 실행할 SQL 스크립트 파일에 따라 다를 수 있다.
{: .prompt-info }

![08-installation](/assets/img/posts/tools/oracle/install-sample-schema-on-windows/08-installation.jpg)
*설치 완료*

### 접속

![09-succed-test](/assets/img/posts/tools/oracle/install-sample-schema-on-windows/09-succed-test.jpg)
*테스트 성공*

- 스키마별 접속 재원의 사용자 이름
	+ customer_orders: co
	+ human_resources: hr
	+ order_entry: oe
	+ product_media: pm
	+ sales_history: sh

> `ALTER USER 사용자 이름 ACCOUNT UNLOCK IDENTIFIED BY 비밀번호 입력;` 명령어를 통해 비밀번호를 변경할 수 있다.
{: .prompt-info }

![10-check-installation.jpg](/assets/img/posts/tools/oracle/install-sample-schema-on-windows/10-check-installation.jpg)

## 참고자료

- [bkjo94, "오라클(ORACLE) 샘플 스키마 설치", 빵구의 개발 메꾸기, 2022-04-30](https://bkjo94.tistory.com/entry/%EC%98%A4%EB%9D%BC%ED%81%B4ORACLE-%EC%83%98%ED%94%8C-%EC%8A%A4%ED%82%A4%EB%A7%88-%EC%84%A4%EC%B9%98){: target="_blank" }
- [skylex, Oracle SQL HR 샘플 스키마 설치 및 생성, 코딩 로그 스토리지, 2022-04-08](https://codedatasotrage.tistory.com/75){: target="_blank" }