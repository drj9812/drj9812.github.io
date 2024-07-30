---
title: "[JDBC]Statement 대신 PreparedStatment를 사용해야 하는 이유"
categories: [API, JDBC]
tags: [API, Java, 자바, JDBC]
image:
  path: /assets/img/posts/language/java/01-java-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Java
---

# Statement 대신 PreparedStatment를 사용해야 하는 이유

## 라이브러리 캐시 과부화

### Statement

```java
String sql = "SELECT * FROM customer WHERE login_id = '" + login_id + "'";
Statement st = con.createStatement();
ResultSet rs = st.executeQuery(sql);

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

### PrearedStatement

```java
String sql = "SELECT * FROM customer WHERE login_id = ?";
PreparedStatement st = con.preparedStatement(sql);
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

라이브러리 캐시를 조회해 보면, 로그인과 관련해서 위 SQL 하나만 발견된다. 이 SQL에 대한 하드 파싱은 최초 한 번만 일어나고, 캐싱된 SQL은 여러 사용자가 공유하면서 재사용한다.

## SQL 인젝션

### Statement

```java
String name = "John' OR '1'='1";
String query = "SELECT * FROM users WHERE name = '" + name + "' AND age = 30";

Statement stmt = connection.createStatement();
ResultSet rs = stmt.executeQuery(query);
```

```sql
SELECT * FROM users WHERE name = 'John' OR '1'='1' AND age = 30
```

`Statement`를 사용할 경우 위와 같이 사용자의 입력값이 그대로 SQL 쿼리에 포함되기 때문에 SQL 인젝션 공격에 노출될 수 있다.

### PreparedStatement

```java
String name = "John' OR '1'='1";
String query = "SELECT * FROM users WHERE name = ? AND age = ?";

PreparedStatement pstmt = connection.prepareStatement(query);
pstmt.setString(1, name);
pstmt.setInt(2, 30);
ResultSet rs = pstmt.executeQuery();
```

```sql
SELECT * FROM users WHERE name = 'John'' OR ''1''=''1' AND age = 30
```

반면에 `PreparedStatement`는 매개변수 값을 별도의 데이터로 처리하여 SQL 구문으로 해석되지 않게 한다. 즉, `name` 변수의 값이 쿼리 내에서 단순한 문자열 데이터로 처리되기 때문에 인젝션 공격이 발생하지 않는다.

## 참고자료

- 조시형, *친절한 SQL 튜닝*(서울: 디비안, 2018), 560.