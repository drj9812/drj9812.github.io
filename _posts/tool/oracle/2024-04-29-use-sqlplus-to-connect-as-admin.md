---
title: "[Oracle | SQL*Plus]관리자 수준으로 접속하기"
categories: [Tool, Oracle]
tags: [Oracle, 오라클, SQL*Plus, admin, 관리자]
---

# 관리자 수준으로 접속하기

## SQL*Plus를 sys계정으로 시작

![01-connect-as-admin(1)](/assets/img/posts/tool/oracle/use-sqlplus-to-connect-as-admin/01-connect-as-admin(1).jpg)

```console
$ sqlplus / as sysdba
```

## SQL*Plus를 시작하고, sys 계정으로 접속

![02-connect-as-admin(2)](/assets/img/posts/tool/oracle/use-sqlplus-to-connect-as-admin/02-connect-as-admin(2).jpg)

```console
$ sqlplus
$ / as sysdba
```

## SQL*Plus를 시작하고, sys 계정으로 접속

![03-connect-as-admin(3)](/assets/img/posts/tool/oracle/use-sqlplus-to-connect-as-admin/03-connect-as-admin(3).jpg)

```console
$ sqlplus
$ sys /as sysdba
$ 비밀번호 입력
```

## SQL*Plus를 로그인 없이 접속하고, sys 계정으로 접속(비밀번호 입력)

![04-connect-as-admin(4)](/assets/img/posts/tool/oracle/use-sqlplus-to-connect-as-admin/04-connect-as-admin(4).jpg)

```console
$ sqlplus /nolog
SQL> conn sys/비밀번호 입력 as sysdba
```

## 참고자료

- ["[Oracle] sqlplus 에 sys 접속하고, 오라클 시작하기, 종료하기", TI이야기, 2020-01-19](https://withthisclue.tistory.com/entry/Oracle-Sqlplus%EB%A1%9C-%EC%98%A4%EB%9D%BC%ED%81%B4%EC%84%9C%EB%B2%84-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0-%EC%A2%85%EB%A3%8C%ED%95%98%EA%B8%B0){: target="_blank" }