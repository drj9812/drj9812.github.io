---
title: "[JDBC]Statement 대신 PrepareStatment를 사용해야 이유"
tags: [Java, 자바, JDBC]
image:
  path: /assets/img/posts/language/java/01-java-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Java
---

# createStatement() 대신 prepareStatement()를 사용해야 이유

## Statement

```java
String SQLStmt = "SELECT * FROM CUSTOMER WHERE LOGIN_ID = '" + login_id + "'";
Statement st = con.createStatement();
ResultSet rs = st.executeQuery(SQLStmt);

if (rs.next()){
    // do anything
}

rs.close();
st.close();
conn.close();
```

위 코드는 사용자가 로그인할 때마다, 쿼리 문자열을 매번 새로 생성하고, 이를 `executeQuery()` 메서드를 통해 실행한다. 따라서 매번 실행 시 새로운 쿼리가 데이터베이스에 전송되어 실행된다.

```sql
SELECT * FROM CUSTOMER WHERE LOGIN_ID 'oraking'
SELECT * FROM CUSTOMER WHERE LOGIN_ID 'javaking'
SELECT * FROM CUSTOMER WHERE LOGIN_ID 'tommy'
SELECT * FROM CUSTOMER WHERE LOGIN_ID 'karajan'
...
...
...
```

라이브러리 캐시(V$SQL)를 조회해보면 위와 같은 SQL로 가득 차 있다.

## PrepareStatement

```java
String SQLStmt = "SELECT * FROM CUSTOMER WHERE LOGIN_ID = '" + login_id + "'";
PrepareStatement st = con.prepareStatement(SQLStmt);
st.setString(1, login_id);
ResultSet rs = st.executeQuery();

if (rs.next()){
    // do anything
}

rs.close();
st.close();
conn.close();
```

위 코드의 `st.setString(1, login_id)`는 `?` 자리 표시자에 실제 값을 바인딩한다. 이 과정은 쿼리 실행 전에 수행된다. `executeQuery()` 메서드는 바인딩된 값과 함께 준비된 쿼리 템플릿을 실행한다.

```sql
SELECT * FROM CUSTOMER WHERE LOGIN_ID :1
```

라이브러리 캐시를 조회해 보면, 로그인과 관련해서 위 SQL 하나만 발견된다.

## 참고자료

- 조시형, *친절한 SQL 튜닝*(서울: 디비안, 2018), 560.