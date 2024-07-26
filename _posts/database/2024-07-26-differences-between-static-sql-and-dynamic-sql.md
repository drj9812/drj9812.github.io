---
title: "[Database]Static SQL과 Dynamic SQL의 차이"
categories: [Database]
tags: [Database, Static SQL, Dynamic SQL]
---

# Static SQL과 Dynamic SQL의 차이

## 컴파일 시점

```java
String sql = "SELECT * FROM customer WHERE LOGIN_ID = 1";

Statement st = con.createStatement();
ResultSet rs = st.executeQuery(sql);

if (rs.next()){
    // do anything
}

rs.close();
st.close();
conn.close();
```

Static SQL과 Dynamic SQL를 명확히 구별하는 기준은 컴파일 시점이다. 여기서 말하는 컴파일은 서버 단의 컴파일이 아닌 DB 단의 컴파일을 의미한다. 위 코드에서 쿼리 문자열 `sql`은 서버 단에서 static이지만 DB 단에서는 dynamic이다. Java 컴파일러는 이 문자열을 SQL 문법으로 인식하지 않으며, 런타임 시점에 DB에 전달되기 전까지 그저 문자열로 취급한다. 즉, DB 단에서 컴파일을 거쳐야 비로소 SQL 문장임을 이해할 수 있는 것이다.

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

그럼 위 코드에서의 쿼리 문자열 `sql`은 static일까 dynamic일까? login_id를 동적으로 전달하고 DB에 넘길테니 서버 단에서는 dynamic이지만, 마찬가지로 DB 입장에서는 컴파일을 거치기 전까지 그저 문자열에 불과함으로 Static SQL이다.

```sql
SELECT empno INTO v_empno FROM emp WHERE empno = v_empno;
```

PL/SQL로 예를 들자면, 위 쿼리는 Static SQL이다. 위 쿼리는 PL/SQL 블록이 컴파일될 때 이미 정해져 있다.

```sql
v_sql := 'SELECT empno FROM emp WHERE empno = :1';
EXECUTE IMMEDIATE v_sql INTO v_result USING v_empno;
```

위 쿼리는 Dynamic SQL이다. 쿼리가 실행 시점에 동적으로 생성되고 결정된다.

## 참고자료

- [admin, *SQL 파싱 부하*, 데이터온에어, 2021-02-15](https://dataonair.or.kr/db-tech-reference/d-guide/sql/?mod=document&uid=358){: target="_blank" }