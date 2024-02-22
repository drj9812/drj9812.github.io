---
title: "[프로그래머스/오라클(ORACLE)]레벨1 모든 문제"
categories: [알고리즘, 프로그래머스]
tags: [알고리즘, 프로그래머스, SQL, 오라클, Oracle, 레벨1]
---

# [프로그래머스/오라클(ORACLE)]레벨1 모든 문제

2023-12-07 기준 정답률 순입니다.

## 동물의 아이디와 이름(<https://school.programmers.co.kr/learn/courses/30/lessons/59403>{: target="_blank" })

![동물의 아이디와 이름(1)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/동물의-아이디와-이름(1).png)

```sql
SELECT animal_id, name
  FROM animal_ins
 ORDER BY animal_id ASC;
```

## 여러 기준으로 정렬하기(<https://school.programmers.co.kr/learn/courses/30/lessons/59404>{: target="_blank" })

![여러 기준으로 정렬하기(1)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/여러-기준으로-정렬하기(1).png)
![여러 기준으로 정렬하기(2)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/여러-기준으로-정렬하기(2).png)

```sql
SELECT animal_id, name, datetime
  FROM animal_ins
 ORDER BY name, datetime DESC;
```

## 역순 정렬하기(<https://school.programmers.co.kr/learn/courses/30/lessons/59035>{: target="_blank" })

![역순 정렬하기(1)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/역순-정렬하기(1).png)

```sql
SELECT name, datetime
  FROM animal_ins
 ORDER BY animal_id DESC;
```

## 어린 동물 찾기(<https://school.programmers.co.kr/learn/courses/30/lessons/59037>{: target="_blank" })

![어린 동물 찾기(1)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/어린-동물-찾기(1).png)
![어린 동물 찾기(2)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/어린-동물-찾기(2).png)

```sql
SELECT animal_id, name
  FROM animal_ins
 WHERE intake_condition != 'Aged'
 ORDER BY animal_id ASC;
```

## 상위 n개 레코드(<https://school.programmers.co.kr/learn/courses/30/lessons/59405>{: target="_blank" })

![상위 n개 레코드(1)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/상위-n개-레코드(1).png)
![상위 n개 레코드(2)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/상위-n개-레코드(2).png)

```sql
SELECT name
  FROM animal_ins
 WHERE datetime = (SELECT MIN(datetime)
                     FROM animal_ins);
```
보통 집계함수는 `GROUP BY`절과 함께 쓰는 것이 일반적이나, 위 서브쿼리(`SELECT MIN(datetime) FROM animal_ins`)에서는 전체 테이블에서 집계 함수를 적용하여 하나의 결과를 얻기 때문에 `GROUP BY` 절이 필요하지 않습니다.

## 아픈 동물 찾기(<https://school.programmers.co.kr/learn/courses/30/lessons/59036>{: target="_blank" })

![아픈 동물 찾기(1)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/아픈-동물-찾기(1).png)
![아픈 동물 찾기(2)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/아픈-동물-찾기(2).png)

```sql
SELECT animal_id, name
  FROM animal_ins
 WHERE intake_condition = 'Sick'
 ORDER BY animal_id ASC;
```

## 이름이 있는 동물의 아이디(<https://school.programmers.co.kr/learn/courses/30/lessons/59407>{: target="_blank" })

![이름이 있는 동물의 아이디(1)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/이름이-있는-동물의-아이디(1).png)
![이름이 있는 동물의 아이디(2)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/이름이-있는-동물의-아이디(2).png)


```sql
SELECT animal_id 
  FROM animal_ins
 WHERE name IS NOT NULL
 ORDER BY animal_id ASC;
```

## 나이 정보가 없는 회원 수 구하기(<https://school.programmers.co.kr/learn/courses/30/lessons/131528>{: target="_blank" })

![나이 정보가 없는 회원 수 구하기(1)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/나이-정보가-없는-회원-수-구하기(1).png)

```sql
SELECT COUNT(*) AS users
  FROM user_info
 WHERE age IS NULL;
```

## 가장 비싼 상품 구하기(<https://school.programmers.co.kr/learn/courses/30/lessons/131697>{: target="_blank" })

![가장 비싼 상품 구하기(1)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/가장-비싼-상품-구하기(1).png)

```sql
SELECT MAX(price) AS max_price
  FROM product;
```

## 강원도에 위치한 생산공장 목록 출력하기(<https://school.programmers.co.kr/learn/courses/30/lessons/131112>{: target="_blank" })

![강원도에 위치한 생산공장 목록 출력하기(1)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/강원도에-위치한-생산공장-목록-출력하기(1).png)
![강원도에 위치한 생산공장 목록 출력하기(2)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/강원도에-위치한-생산공장-목록-출력하기(2).png)
```sql
SELECT factory_id, factory_name, address 
  FROM food_factory
 WHERE address LIKE '강원도%'
 ORDER BY factory_id ASC;
```

`LIKE '강원도%'`는 '강원도'로 시작하는 값을 검색하겠다는 의미입니다. 와일드카드('%')자리에는 어떠한 문자가 존재해도 상관없습니다.

## 경기도에 위치한 식품창고 목록 출력하기(<https://school.programmers.co.kr/learn/courses/30/lessons/131114>{: target="_blank" })

![경기도에 위치한 식품창고 목록 출력하기(1)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/경기도에-위치한-식품창고-목록-출력하기(1).png)
![경기도에 위치한 식품창고 목록 출력하기(2)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/경기도에-위치한-식품창고-목록-출력하기(2).png)

```sql
SELECT warehouse_id, warehouse_name, address, NVL(freezer_yn, 'N') AS freezer_yn
  FROM food_warehouse
 WHERE address LIKE '경기도%'
 ORDER BY warehouse_id ASC;
```

`NVL` 함수는 첫 번째 인수가 `NULL`인 경우 두 번째 인수를 반환합니다. 따라서 `NVL(freezer_yn, 'N')`의 의미는 `freezer_yn`의 컬럼의 값이 `NULL`인 경우 'N'을 출력하겠다는 의미입니다.

 ```sql
SELECT warehouse_id, warehouse_name, address, NVL2(freezer_yn, freezer_yn, 'N') AS freezer_yn
  FROM food_warehouse
 WHERE address LIKE '경기도%'
 ORDER BY warehouse_id ASC;
```
`NVL2` 함수를 사용한 위 쿼리에서도 동일한 결과를 얻을 수 있습니다. `NVL2` 함수는 첫 번째의 인수가 `NULL`이 아닌 경우에는 두 번째 인수를 반환하고, `NULL`인 경우 세 번째 인수를 반환하겠다는 의미입니다.

### `NVL`, `NVL2` 함수 사용 예시

```sql
SELECT NVL('Hello', 'Not NULL') AS result FROM dual;
-- 결과: 'Hello'

SELECT NVL(NULL, 'Not NULL') AS result FROM dual;
-- 결과: 'Not NULL'

SELECT NVL2('Hello', 'Not NULL', 'NULL') AS result FROM dual;
-- 결과: 'Not NULL'

SELECT NVL2(NULL, 'Not NULL', 'NULL') AS result FROM dual;
-- 결과: 'NULL'
```

## 흉부외과 또는 일반외과 의사 목록 출력하기(<https://school.programmers.co.kr/learn/courses/30/lessons/132203>{: target="_blank" })

![흉부외과 또는 일반외과 의사 목록 출력하기(1)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/흉부외과-또는-일반외과-의사-목록-출력하기(1).png)
![흉부외과 또는 일반외과 의사 목록 출력하기(2)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/흉부외과-또는-일반외과-의사-목록-출력하기(2).png)

```sql
SELECT dr_name, dr_id, mcdp_cd, TO_CHAR(hire_ymd, 'YYYY-MM-DD') AS hire_ymd
  FROM doctor
 WHERE mcdp_cd IN ('CS', 'GS')
 ORDER BY hire_ymd DESC, dr_name ASC;
```
## 이름이 없는 동물의 아이디(<https://school.programmers.co.kr/learn/courses/30/lessons/59039>{: target="_blank" })

![이름이 없는 동물의 아이디(1)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/이름이-없는-동물의-아이디(1).png)

```sql
SELECT animal_id
  FROM animal_ins
 WHERE name IS NULL
 ORDER BY animal_id;
```

## 조건에 맞는 회원수 구하기(<https://school.programmers.co.kr/learn/courses/30/lessons/131535>{: target="_blank" })

![조건에 맞는 회원수 구하기(1)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/조건에-맞는-회원수-구하기(1).png)
![조건에 맞는 회원수 구하기(2)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/조건에-맞는-회원수-구하기(2).png)

```sql
SELECT COUNT(*) AS users
  FROM user_info
 WHERE EXTRACT(YEAR FROM joined) = 2021
       AND
       age BETWEEN 20 AND 29;
```

## 12세 이하인 여자 환자 목록 출력하기(<https://school.programmers.co.kr/learn/courses/30/lessons/132201>{: target="_blank" })

![12세 이하인 여자 환자 목록 출력하기(1)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/12세-이하인-여자-환자-목록-출력하기(1).png)
![12세 이하인 여자 환자 목록 출력하기(2)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/12세-이하인-여자-환자-목록-출력하기(2).png)

```sql
SELECT pt_name, pt_no, gend_cd, age, NVL(tlno, 'NONE') AS tlno
  FROM patient
 WHERE age <= 12 AND gend_cd = 'W'
 ORDER BY age DESC, pt_name ASC;
```

## 인기있는 아이스크림(<https://school.programmers.co.kr/learn/courses/30/lessons/133024>{: target="_blank" })

![인기있는 아이스크림(1)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/인기있는-아이스크림(1).png)
![인기있는 아이스크림(2)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/인기있는-아이스크림(2).png)

```sql
SELECT flavor
  FROM first_half
 ORDER BY total_order DESC, shipment_id ASC;
```

## 조건에 맞는 도서 리스트 출력하기(<https://school.programmers.co.kr/learn/courses/30/lessons/144853>{: target="_blank" })

![조건에 맞는 도서 리스트 출력하기(1)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/조건에-맞는-도서-리스트-출력하기(1).png)
![조건에 맞는 도서 리스트 출력하기(2)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/조건에-맞는-도서-리스트-출력하기(2).png)

```sql
SELECT book_id, TO_CHAR(published_date, 'YYYY-MM-DD') AS published_date
  FROM book
 WHERE EXTRACT(YEAR FROM published_date) = 2021
       AND
       category = '인문'
 ORDER BY published_date ASC;
```

## 평균 일일 대여 요금 구하기(<https://school.programmers.co.kr/learn/courses/30/lessons/151136>{: target="_blank" })

![평균_일일_대여_요금_구하기(1)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/평균-일일-대여-요금-구하기(1).png)
![평균_일일_대여_요금_구하기(2)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/평균-일일-대여-요금-구하기(2).png)

```sql
SELECT ROUND(AVG(daily_fee)) AS average_fee
  FROM car_rental_company_car
 WHERE car_type = 'SUV';
```

## 모든 레코드 조회하기(<https://school.programmers.co.kr/learn/courses/30/lessons/59034>{: target="_blank" })

![모든 레코드 조회하기(1)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/모든-레코드-조회하기(1).png)

```sql
SELECT *
  FROM animal_ins
 ORDER BY animal_id;
```

## 과일로 만든 아이스크림 고르기(<https://school.programmers.co.kr/learn/courses/30/lessons/133025>{: target="_blank" })

![과일로 만든 아이스크림 고르기(1)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/과일로-만든-아이스크림-고르기(1).png)
![과일로 만든 아이스크림 고르기(2)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/과일로-만든-아이스크림-고르기(2).png)
![과일로 만든 아이스크림 고르기(3)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/과일로-만든-아이스크림-고르기(3).png)

```sql
SELECT i.flavor
  FROM icecream_info i
 INNER JOIN first_half f
    ON i.flavor = f.flavor
 WHERE i.ingredient_type = 'fruit_based'
 GROUP BY i.flavor
HAVING SUM(f.total_order) >= 3000
 ORDER BY SUM(f.total_order) DESC;
```

## 최댓값 구하기(<https://school.programmers.co.kr/learn/courses/30/lessons/59415>{: target="_blank" })

![최댓값 구하기(1)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/최댓값-구하기(1).png)
![최댓값 구하기(2)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/최댓값-구하기(2).png)

```sql
SELECT MAX(datetime) AS 시간
  FROM animal_ins;
```

## 특정 옵션이 포함된 자동차 리스트 구하기(<https://school.programmers.co.kr/learn/courses/30/lessons/157343>{: target="_blank" })

![특정 옵션이 포함된 자동차 리스트 구하기(1)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/특정-옵션이-포함된-자동차-리스트-구하기(1).png)
![특정 옵션이 포함된 자동차 리스트 구하기(2)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/특정-옵션이-포함된-자동차-리스트-구하기(2).png)

```sql
SELECT *
  FROM car_rental_company_car
 WHERE options LIKE '%네비게이션%'
 ORDER BY car_id DESC;
```

## 자동차 대여 기록에서 장기/단기 대여 구분하기(<https://school.programmers.co.kr/learn/courses/30/lessons/151138>{: target="_blank" })

![자동차 대여 기록에서 장기/단기 대여 구분하기(1)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/자동차-대여-기록에서-장기,단기-대여-구분하기(1).png)
![자동차 대여 기록에서 장기/단기 대여 구분하기(2)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/자동차-대여-기록에서-장기,단기-대여-구분하기(2).png)

```sql
SELECT history_id, car_id, TO_CHAR(start_date, 'YYYY-MM-DD') AS start_date, TO_CHAR(end_date, 'YYYY-MM-DD') AS end_date,
       CASE WHEN end_date - start_date + 1 >= 30 THEN '장기 대여'
       ELSE '단기 대여'
        END AS rent_type
  FROM car_rental_company_rental_history
 WHERE TO_CHAR(start_date, 'YYYY-MM') = '2022-09'
 ORDER BY history_id DESC;
```

대여 시작일이 2022년 09월 01일이고, 대여 종료일이 2022년 09월 02일이라면 대여 기간은 총 2일(9월 1일, 9월 2일)이므로 기간을 계산할 떄 +1을 추가합니다.

## 조건에 부합하는 중고거래 댓글 조회하기(<https://school.programmers.co.kr/learn/courses/30/lessons/164673>{: target="_blank" })

![조건에 부합하는 중고거래 댓글 조회하기(1)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/조건에-부합하는-중고거래-댓글-조회하기(1).png)
![조건에 부합하는 중고거래 댓글 조회하기(2)](/assets/img/posts/algorithm/sql/programmers-sql-level-1-by-oracle/조건에-부합하는-중고거래-댓글-조회하기(2).png)

```sql
SELECT b.title, b.board_id, r.reply_id, r.writer_id, r.contents, TO_CHAR(r.created_date, 'YYYY-MM-DD')
  FROM used_goods_board b
 INNER JOIN used_goods_reply r
    ON b.board_id = r.board_id
 WHERE TO_CHAR(b.created_date, 'YYYY-MM') = '2022-10'
 ORDER BY r.created_date ASC, b.title ASC;
```