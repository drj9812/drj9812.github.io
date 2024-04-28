---
title: "[Oracle]비밀번호 만기(ORA-28001) 해제"
categories: [Tool, Oracle]
tags: [Oracle, 오라클, ORA-28001, 비밀번호 만기]
---

# 비밀번호 만기(ORA-28001) 해제

![01-ora-28001](/assets/img/posts/tool/oracle/turn-off-password-expiration/01-ora-28001.jpg)

## ORA-28001 오류 코드란?

- DB 사용자의 암호가 만료되었음을 나타냄
- 보안 정책에 따라 암호를 주기적으로 변경해야 하는 경우, 데이터베이스 사용자가 해당 정책을 준수하지 않아 암호가 만료되면 이 오류가 발생

## 해결방법

```console
$ sqlplus / as sysdba
```

두 방법 모두, 먼저 관리자 수준으로 접속해야 한다.

### 계정 비밀번호 재설정

#### 계정 상태 확인

```console
SQL> SELECT username, expiry_date, account_status FROM dba_users;
```

![02-check-account-status(1)](/assets/img/posts/tool/oracle/turn-off-password-expiration/02-check-account-status(1).jpg)

비밀번호가 만료된 계정은 `expiry_date`와 `account_status`가 각각 만료된 날짜, EXPIRED로 표시된다.

> 비밀번호가 만료되지 않은 계정은 `expiry_date`와 `account_status`가 각각 `NULL`, OPEN으로 표시된다.
{: .prompt-info }

#### 비밀번호 만료 기간 확인

```console
SQL> SELECT resource_name, limit FROM dba_profiles WHERE profile = 'DEFAULT' AND resource_name = 'PASSWORD_LIFE_TIME';
```

![03-check-password-life-time(1)](/assets/img/posts/tool/oracle/turn-off-password-expiration/03-check-password-life-time(1).jpg)

일반적으로 180으로 설정되어 있다.

#### 비밀번호 변경

```console
SQL> ALTER user 계정 이름 IDENTIFIED BY 비밀번호 입력;
```

![04-change-password](/assets/img/posts/tool/oracle/turn-off-password-expiration/04-change-password.jpg)

`IDENTIFIED BY` 절을 이용해서 계정의 비밀번호를 변경한다.

> `IDENTIFIED`는 Oracle DB에서 사용자의 인증 방법을 지정하는 키워드다. 사용자를 생성하거나 암호를 변경할 때 사용된다. 예를 들어, `IDENTIFIED BY`는 사용자의 비밀번호를 설정할 때 사용되며, 사용자가 지정한 암호를 사용하여 인증된다. `IDENTIFIED EXTERNALLY`는 운영 체제 수준의 인증을 사용하고 데이터베이스는 외부에서 제공된 인증을 신뢰한다.
{: .prompt-info }

#### 계정 상태 확인

```console
SQL> SELECT resource_name, limit FROM dba_profiles WHERE profile = 'DEFAULT' AND resource_name = 'PASSWORD_LIFE_TIME';
```

![05-check-account-status(2)](/assets/img/posts/tool/oracle/turn-off-password-expiration/05-check-account-status(2).jpg)

비밀번호가 정상적으로 변경됐다면, `expiry_date`와 `account_status`가 각각 만료될 날짜, OPEN으로 표시된다.

### 비밀번호 만료 정책 조정

#### 비밀번호 수명 설정

```sql
ALTER profile DEFAULT limit password_life_time unlimited;
```

![06-change-password-life-time](/assets/img/posts/tool/oracle/turn-off-password-expiration/06-change-password-life-time.jpg)

비밀번호의 수명을 무제한으로 설정한다. 보안상의 이유로 unlimited 대신 유한한 값을 넣는 것을 권장한다.

#### 비밀번호 만료 기간 확인

```console
SQL> SELECT resource_name, limit FROM dba_profiles WHERE profile = 'DEFAULT' AND resource_name = 'PASSWORD_LIFE_TIME';
```

![07-check-password-life-time(2)](/assets/img/posts/tool/oracle/turn-off-password-expiration/07-check-password-life-time(2).jpg)

![08-succeed-test.jpg](/assets/img/posts/tool/oracle/turn-off-password-expiration/08-succeed-test.jpg)
*성공*

## 참고자료

- [Dev.Jeon, "[Oracle] ORA-28001: 비밀번호 만기 해결방법", 개발잔데 이제 오류를 곁들인, 2023-01-18](https://devjeon.tistory.com/10){: target="_blank" }