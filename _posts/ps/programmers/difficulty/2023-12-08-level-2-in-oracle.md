---
title: "[프로그래머스]레벨 2 난이도 모든 문제(Oracle)"
categories: [PS, Programmers]
tags: [PS, Algorithm, 알고리즘, Programmers, 프로그래머스, SQL, Oracle, 오라클, 레벨2]
image:
  path: /assets/img/posts/ps/programmers/01-programmers-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: programmers
---

# 레벨 2 난이도 모든 문제(Oracle)

2023-12-08 기준 정답률 순

## [동물 수 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/59406){: target="_blank" }

![동물-수-구하기](/assets/img/posts/ps/programmers/sql-problems/동물-수-구하기.png)

```sql
SELECT COUNT(*)
  FROM animal_ins;
```
## [최솟값 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/59038){: target="_blank" }

![최솟값-구하기](/assets/img/posts/ps/programmers/sql-problems/최솟값-구하기.png)

```sql
SELECT MIN(datetime) AS 시간
  FROM animal_ins;
```

보통 집계함수는 `GROUP BY`절과 함께 쓰는 것이 일반적이나, 위 쿼리에서는 전체 테이블에서 집계 함수를 적용하여 하나의 결과를 얻기 때문에 `GROUP BY` 절이 필요하지 않다.

## [중복 제거하기](https://school.programmers.co.kr/learn/courses/30/lessons/59408){: target="_blank" }

![중복-제거하기](/assets/img/posts/ps/programmers/sql-problems/중복-제거하기.png)

```sql
SELECT COUNT(DISTINCT name) AS count 
  FROM animal_ins;
```

`DISTINCT` 키워드는 중복된 값을 제거하고 고유한 값만을 반환하는데 이때 `NULL` 값은 하나의 값으로 간주된다. 즉, `NULL` 값은 중복되지 않는 유일한 값으로 처리된다. 따라서 여러 행에 `NULL`이 여러 번 나타나더라도 `DISTINCT`를 사용하면 하나의 `NULL` 값만이 결과에 나타난다.

하지만 `COUNT()` 함수를 `COUNT(column_name)`와 같이 사용한다면 `NULL` 값을 제외하고 계산하기 때문에, 위 쿼리의 `COUNT(DISTINCT name)`는 `DISTINCT`가 반환한 하나의 `NULL`을 제외하고 계산한다.

> 만약 `NULL`을 포함한 값을 카운팅하고 싶다면 `COUNT(*)`를 사용하면 된다.
{: .prompt-tip}

## [동명 동물 수 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/59041){: target="_blank" }

![동명-동물-수-찾기(1)](/assets/img/posts/ps/programmers/sql-problems/동명-동물-수-찾기(1).png)
![동명-동물-수-찾기(2)](/assets/img/posts/ps/programmers/sql-problems/동명-동물-수-찾기(2).png)

```sql
SELECT name, COUNT(name) AS count
  FROM animal_ins
 GROUP BY name
HAVING COUNT(name) >= 2
 ORDER BY name ASC;
```

## [이름에 el이 들어가는 동물 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/59047){: target="_blank" }

![이름에-el이-들어가는-동물-찾기(1)](/assets/img/posts/ps/programmers/sql-problems/이름에-el이-들어가는-동물-찾기(1).png)
![이름에-el이-들어가는-동물-찾기(2)](/assets/img/posts/ps/programmers/sql-problems/이름에-el이-들어가는-동물-찾기(2).png)

```sql
SELECT animal_id, name
  FROM animal_ins
 WHERE animal_type = 'Dog'
       AND
       LOWER(name) LIKE '%el%'
 ORDER BY name ASC;
```

`LOWER(name) LIKE '%el%'`는 소문자 변환한 name 컬럼의 값들 중에 'el'이 포함된 값이라는 의미다.

## [NULL 처리하기](https://school.programmers.co.kr/learn/courses/30/lessons/59410){: target="_blank" }

![NULL-처리하기(1)](/assets/img/posts/ps/programmers/sql-problems/NULL-처리하기(1).png)
![NULL-처리하기(2)](/assets/img/posts/ps/programmers/sql-problems/NULL-처리하기(2).png)

```sql
SELECT animal_type, NVL(name, 'No name'), sex_upon_intake
  FROM animal_ins
 ORDER BY animal_id ASC;
```
```sql
SELECT animal_type, NVL2(name, name, 'No name'), sex_upon_intake
  FROM animal_ins
 ORDER BY animal_id ASC;
```

## [DATETIME에서 DATE로 형 변환](https://school.programmers.co.kr/learn/courses/30/lessons/59414){: target="_blank" }

![DATETIME에서-DATE로-형-변환(1)](/assets/img/posts/ps/programmers/sql-problems/DATETIME에서-DATE로-형-변환(1).png)
![DATETIME에서-DATE로-형-변환(2)](/assets/img/posts/ps/programmers/sql-problems/DATETIME에서-DATE로-형-변환(2).png)

```sql
SELECT animal_id, name, TO_CHAR(datetime, 'YYYY-MM-DD') AS 날짜
  FROM animal_ins
 ORDER BY animal_id ASC;
```

## [가격이 제일 비싼 식품의 정보 출력하기](https://school.programmers.co.kr/learn/courses/30/lessons/131115){: target="_blank" }

![가격이-제일-비싼-식품의-정보-출력하기](/assets/img/posts/ps/programmers/sql-problems/가격이-제일-비싼-식품의-정보-출력하기.png)

```sql
SELECT product_id, product_name, product_cd, category, price
  FROM food_product
 WHERE price IN (SELECT MAX(price)
                   FROM food_product);
```

## [중성화 여부 파악하기](https://school.programmers.co.kr/learn/courses/30/lessons/59409){: target="_blank" }

![중성화-여부-파악하기(1)](/assets/img/posts/ps/programmers/sql-problems/중성화-여부-파악하기(1).png)
![중성화-여부-파악하기(2)](/assets/img/posts/ps/programmers/sql-problems/중성화-여부-파악하기(2).png)

```sql
SELECT animal_id,
       name,
       CASE WHEN sex_upon_intake LIKE 'Intact%' THEN 'X'
       ELSE 'O'
        END AS 중성화
  FROM animal_ins
 ORDER BY animal_id ASC;
```

## [카테고리 별 상품 개수 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/131529){: target="_blank" }

![카테고리-별-상품-개수-구하기(1)](/assets/img/posts/ps/programmers/sql-problems/카테고리-별-상품-개수-구하기(1).png)
![카테고리-별-상품-개수-구하기(2)](/assets/img/posts/ps/programmers/sql-problems/카테고리-별-상품-개수-구하기(2).png)

```sql
SELECT SUBSTR(product_code, 1, 2) AS category, COUNT(product_id) AS products
  FROM product
 GROUP BY SUBSTR(product_code, 1, 2)
 ORDER BY category ASC;
```
`SUBSTR()` 함수는 문자열의 일부분을 추출하는 데 사용되는 함수다. 주로 특정 문자열에서 시작 위치부터 일정 길이만큼 또는 끝까지의 문자열을 반환하는 데에 활용된다. 세 번째 인자를 지정하지 않으면 두 번째 인자에 들어간 값(시작 위치)부터 문자열의 끝까지를 반환한다.

## [고양이와 개는 몇 마리 있을까](https://school.programmers.co.kr/learn/courses/30/lessons/59040){: target="_blank" }

![고양이와-개는-몇-마리-있을까](/assets/img/posts/ps/programmers/sql-problems/고양이와-개는-몇-마리-있을까.png)

```sql
SELECT animal_type, COUNT(*) AS count
  FROM animal_ins
 GROUP BY animal_type
HAVING animal_type IN ('Cat', 'Dog')
 ORDER BY animal_type ASC;
```

`HAVING` 절에서 `IN` 대신 `BETWEEN`을 사용하면 알파벳 순서를 기준으로 문자열 'Cat'과 'Dog' 사이에 있는 값을 선택하게 되므로 `IN`을 사용한다.

## [입양 시각 구하기(1)](https://school.programmers.co.kr/learn/courses/30/lessons/59412){: target="_blank" }

![입양-시각-구하기(1)](/assets/img/posts/ps/programmers/sql-problems/입양-시각-구하기(1).png)
![입양-시각-구하기(2)](/assets/img/posts/ps/programmers/sql-problems/입양-시각-구하기(2).png)

```sql
SELECT TO_CHAR(datetime, 'FMHH24') AS hour, COUNT(*) AS count
  FROM animal_outs
 GROUP BY TO_CHAR(datetime, 'FMHH24')
HAVING TO_CHAR(datetime, 'FMHH24') BETWEEN 9 AND 19
 ORDER BY TO_NUMBER(hour) ASC;
```

문제의 예시 표를 보면 hour 컬럼이 2자리(01, 02, 03, ...)가 아닌 1자리(1, 2, 3, ...)로 표현되어 있기 때문에 `FM` 키워드를 사용했다. 

`FM`은 "fill mode"의 줄임말로, 숫자나 문자열을 포맷팅할 때 빈 칸을 채우지 않는 모드다. 

`HAVING` 절에서 `HAVING TO_CHAR(datetime, 'FMHH24') BETWEEN '9' AND '19'`라고 하지 않은 이유는 따옴표를 사용하여 문자열 비교를 하게된다면, 각 문자의 ASCII 코드 값을 비교하게 되는데 이때 각 문자의 첫 번째 문자부터 차례대로 비교하기 때문이다.

![입양-시각-구하기(3)](/assets/img/posts/ps/programmers/sql-problems/입양-시각-구하기(3).png)

ASCII 코드에서 문자 '9'은 57이고, 문자 '1'은 49이기 때문에 `BETWEEN 57 AND 49`가 되어버려 위와 같이 원하는 출력 값을 원할 수 없게 된다.

![입양-시각-구하기(4)](/assets/img/posts/ps/programmers/sql-problems/입양-시각-구하기(4).png)

같은 이유로 `ORDER BY` 절에서 `TO_CHAR(datetime, 'FMHH24') AS hour`와 같이 문자열을 기준으로 정렬하게 된다면 문자 '9'보다 문자 '1'이 더 크게 때문에 위와 같은 결과가 나오므로 `TO_NUMBER()` 함수를 사용하여 숫자로 변환하여 정렬하였다.

## [진료과별 총 예약 횟수 출력하기](https://school.programmers.co.kr/learn/courses/30/lessons/132202){: target="_blank" }

![진료과별-총-예약-횟수-출력하기(1)](/assets/img/posts/ps/programmers/sql-problems/진료과별-총-예약-횟수-출력하기(1).png)
![진료과별-총-예약-횟수-출력하기(2)](/assets/img/posts/ps/programmers/sql-problems/진료과별-총-예약-횟수-출력하기(2).png)

```sql
SELECT mcdp_cd AS 진료과코드, COUNT(*) AS "5월예약건수"
  FROM appointment
 WHERE TO_CHAR(apnt_ymd, 'YYYY-MM') = '2022-05'
 GROUP BY mcdp_cd
 ORDER BY "5월예약건수" ASC, 진료과코드 ASC;
```

별칭이 숫자로 시작하거나, 특수문자를 포함한다면 큰 따옴표로 묶어야 한다.

## [자동차 종류 별 특정 옵션이 포함된 자동차 수 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/151137){: target="_blank" }

![자동차-종류-별-특정-옵션이-포함된-자동차-수-구하기(1)](/assets/img/posts/ps/programmers/sql-problems/자동차-종류-별-특정-옵션이-포함된-자동차-수-구하기(1).png)
![자동차-종류-별-특정-옵션이-포함된-자동차-수-구하기(2)](/assets/img/posts/ps/programmers/sql-problems/자동차-종류-별-특정-옵션이-포함된-자동차-수-구하기(2).png)

```sql
SELECT car_type, COUNT(*) AS cars
  FROM car_rental_company_car
 WHERE options LIKE '%통풍시트%'
       OR
       options LIKE '%열선시트%'
       OR
       options LIKE '%가죽시트%'
 GROUP BY car_type
 ORDER BY car_type ASC;
```

## [상품 별 오프라인 매출 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/131533){: target="_blank" }

![상품-별-오프라인-매출-구하기(1)](/assets/img/posts/ps/programmers/sql-problems/상품-별-오프라인-매출-구하기(1).png)
![상품-별-오프라인-매출-구하기(2)](/assets/img/posts/ps/programmers/sql-problems/상품-별-오프라인-매출-구하기(2).png)

```sql
SELECT p.product_code, SUM(p.price * o.sales_amount) AS sales
  FROM product p
 INNER JOIN offline_sale o
    ON p.product_id = o.product_id
 GROUP BY p.product_code
 ORDER BY sales DESC, p.product_code ASC;
```

## [조건에 맞는 도서와 저자 리스트 출력하기](https://school.programmers.co.kr/learn/courses/30/lessons/144854){: target="_blank" }

![조건에-맞는-도서와-저자-리스트-출력하기(1)](/assets/img/posts/ps/programmers/sql-problems/조건에-맞는-도서와-저자-리스트-출력하기(1).png)
![조건에-맞는-도서와-저자-리스트-출력하기(2)](/assets/img/posts/ps/programmers/sql-problems/조건에-맞는-도서와-저자-리스트-출력하기(2).png)

```sql
SELECT b.book_id, a.author_name, TO_CHAR(b.published_date, 'YYYY-MM-DD') AS published_date
  FROM book b
 INNER JOIN author a
    ON b.author_id = a.author_id
 WHERE b.category = '경제'
 ORDER BY published_date ASC;
```

## [성분으로 구분한 아이스크림 총 주문량](https://school.programmers.co.kr/learn/courses/30/lessons/133026){: target="_blank" }

![성분으로-구분한-아이스크림-총-주문량(1)](/assets/img/posts/ps/programmers/sql-problems/성분으로-구분한-아이스크림-총-주문량(1).png)
![성분으로-구분한-아이스크림-총-주문량(2)](/assets/img/posts/ps/programmers/sql-problems/성분으로-구분한-아이스크림-총-주문량(2).png)
![성분으로-구분한-아이스크림-총-주문량(3)](/assets/img/posts/ps/programmers/sql-problems/성분으로-구분한-아이스크림-총-주문량(3).png)

```sql
SELECT i.ingredient_type, SUM(f.total_order) AS total_order
  FROM first_half f
 INNER JOIN icecream_info i
    ON f.flavor = i.flavor
 GROUP BY i.ingredient_type
 ORDER BY total_order ASC;
```

언더스코어(`_`)는 Oracle에서 특수문자로 간주되지 않는다. 언더스코어는 일반적으로 문자로 취급되며, 특수문자로 감지되지 않다. 따라서 언더스코어를 사용할 때에는 별도의 따옴표로 묶어줄 필요가 없다.

## [루시와 엘라 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/59046){: target="_blank" }

![루시와-엘라-찾기(1)](/assets/img/posts/ps/programmers/sql-problems/루시와-엘라-찾기(1).png)
![루시와-엘라-찾기(2)](/assets/img/posts/ps/programmers/sql-problems/루시와-엘라-찾기(2).png)

```sql
SELECT animal_id, name, sex_upon_intake
  FROM animal_ins
 WHERE name IN ('Lucy', 'Ella', 'Pickle', 'Sabrina', 'Mitty')
 ORDER BY animal_id ASC;
```

## [가격대 별 상품 개수 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/131530){: target="_blank" }

![가격대-별-상품-개수-구하기(1)](/assets/img/posts/ps/programmers/sql-problems/가격대-별-상품-개수-구하기(1).png)
![가격대-별-상품-개수-구하기(2)](/assets/img/posts/ps/programmers/sql-problems/가격대-별-상품-개수-구하기(2).png)

```sql
SELECT TRUNC(price, -4) AS "price_group", COUNT(*) AS products
  FROM product
 GROUP BY TRUNC(price, -4)
 ORDER BY "price_group" ASC;
```

`TRUNC()` 함수는 두 번째 인자로 받는 숫자를 지정된 자릿수로 절사하여 반환하는 함수다. 두 번째 인자가 양수라면 소수점 아래 자리수를 절사하고, 음수라면 소수점 왼쪽 자리수를 절사한다. 위 쿼리에선 천의 자리수를 절사하여 문제의 요구대로 만원 단위로 반환되었다. 만약 두 번째 인자를 넣지 않는다면 모든 소수점 아래 자리수를 절사한다. 즉, `TRUNC(3.14)`는 3을 반환한다.

## [3월에 태어난 여성 회원 목록 출력하기](https://school.programmers.co.kr/learn/courses/30/lessons/131120){: target="_blank" }

![3월에-태어난-여성-회원-목록-출력하기(1)](/assets/img/posts/ps/programmers/sql-problems/3월에-태어난-여성-회원-목록-출력하기(1).png)
![3월에-태어난-여성-회원-목록-출력하기(2)](/assets/img/posts/ps/programmers/sql-problems/3월에-태어난-여성-회원-목록-출력하기(2).png)

```sql
SELECT member_id, member_name, gender, TO_CHAR(date_of_birth, 'YYYY-MM-DD') AS date_of_birth
  FROM member_profile
 WHERE EXTRACT(MONTH FROM date_of_birth) = 03
       AND
       gender = 'W'
       AND
       tlno IS NOT NULL
 ORDER BY member_id ASC;
```

## [재구매가 일어난 상품과 회원 리스트 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/131536){: target="_blank" }

![재구매가-일어난-상품과-회원-리스트-구하기(1)](/assets/img/posts/ps/programmers/sql-problems/재구매가-일어난-상품과-회원-리스트-구하기(1).png)
![재구매가-일어난-상품과-회원-리스트-구하기(2)](/assets/img/posts/ps/programmers/sql-problems/재구매가-일어난-상품과-회원-리스트-구하기(2).png)

```sql
SELECT user_id, product_id
  FROM online_sale
 GROUP BY user_id, product_id
HAVING COUNT(product_id) >= 2
 ORDER BY user_id ASC, product_id DESC;
```

## [조건에 부합하는 중고거래 상태 조회하기](https://school.programmers.co.kr/learn/courses/30/lessons/164672){: target="_blank" }

![조건에-부합하는-중고거래-상태-조회하기(1)](/assets/img/posts/ps/programmers/sql-problems/조건에-부합하는-중고거래-상태-조회하기(1).png)
![조건에-부합하는-중고거래-상태-조회하기(2)](/assets/img/posts/ps/programmers/sql-problems/조건에-부합하는-중고거래-상태-조회하기(2).png)

```sql
SELECT board_id, writer_id, title, price,
       CASE WHEN status = 'SALE' THEN '판매중'
            WHEN status = 'RESERVED' THEN '예약중'
            WHEN status = 'DONE' THEN '거래완료'
        END AS status
  FROM used_goods_board
 WHERE TO_CHAR(created_date, 'YYYY-MM-DD') = '2022-10-05'
 ORDER BY board_id DESC;
```

## [자동차 평균 대여 기간 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/157342){: target="_blank" }

![자동차-평균-대여-기간-구하기(1)](/assets/img/posts/ps/programmers/sql-problems/자동차-평균-대여-기간-구하기(1).png)
![자동차-평균-대여-기간-구하기(2)](/assets/img/posts/ps/programmers/sql-problems/자동차-평균-대여-기간-구하기(2).png)

```sql
SELECT car_id, ROUND(AVG(end_date - start_date + 1), 1) AS average_duration
  FROM car_rental_company_rental_history
 GROUP BY car_id
HAVING AVG(end_date - start_date + 1) >= 7
 ORDER BY average_duration DESC, car_id DESC;
```

집계함수는 `WHERE` 절에서 사용할 수 없으며 `SELECT` 절이나 `HAVING` 절에서 사용된다.

`ROUND()` 함수의 두 번째 인자에 1을 넣으면, 소수점 두 번째 자리를 반올림한다.