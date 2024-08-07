---
title: "[프로그래머스]레벨 4 난이도 모든 문제(Oracle)"
categories: [PS, Programmers]
tags: [PS, Algorithm, 알고리즘, Programmers, 프로그래머스, SQL, Oracle, 오라클, 레벨4]
image:
  path: /assets/img/posts/ps/programmers/01-programmers-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: programmers
---

# 레벨 4 난이도 모든 문제(Oracle)

2023-12-24 기준 정답률 순

## [보호소에서 중성화한 동물](https://school.programmers.co.kr/learn/courses/30/lessons/59045){: target="_blank" }

![보호소에서-중성화한-동물(1)](/assets/img/posts/ps/programmers/sql-problems/보호소에서-중성화한-동물(1).png)
![보호소에서-중성화한-동물(2)](/assets/img/posts/ps/programmers/sql-problems/보호소에서-중성화한-동물(2).png)
![보호소에서-중성화한-동물(3)](/assets/img/posts/ps/programmers/sql-problems/보호소에서-중성화한-동물(3).png)

```sql
SELECT i.animal_id, i.animal_type, i.name
  FROM animal_ins i
 INNER JOIN animal_outs o
    ON i.animal_id = o.animal_id
 WHERE i.sex_upon_intake LIKE '%Intact%'
       AND 
       o.sex_upon_outcome NOT LIKE '%Intact%'
 ORDER BY i.animal_id;
```

## [식품분류별 가장 비싼 식품의 정보 조회하기](https://school.programmers.co.kr/learn/courses/30/lessons/131116){: target="_blank" }

![식품분류별-가장-비싼-식품의-정보-조회하기(1)](/assets/img/posts/ps/programmers/sql-problems/식품분류별-가장-비싼-식품의-정보-조회하기(1).png)
![식품분류별-가장-비싼-식품의-정보-조회하기(2)](/assets/img/posts/ps/programmers/sql-problems/식품분류별-가장-비싼-식품의-정보-조회하기(2).png)

```sql
SELECT main.product_name, main.category, sub.max_price
  FROM food_product main
 INNER JOIN (SELECT category, MAX(price) AS max_price
               FROM food_product
              WHERE category IN ('과자', '국', '김치', '식용유')
              GROUP BY category) sub
   ON main.category = sub.category
      AND
      main.price = sub.max_price
ORDER BY max_price DESC;
```

## [5월 식품들의 총매출 조회하기](https://school.programmers.co.kr/learn/courses/30/lessons/131117){: target="_blank" }

![5월-식품들의-총매출-조회하기(1)](/assets/img/posts/ps/programmers/sql-problems/5월-식품들의-총매출-조회하기(1).png)
![5월-식품들의-총매출-조회하기(2)](/assets/img/posts/ps/programmers/sql-problems/5월-식품들의-총매출-조회하기(2).png)

```sql
SELECT fd.product_id, fd.product_name, SUM(fd.price * fo.amount) AS total_sales
  FROM food_product fd
 INNER JOIN food_order fo
    ON fd.product_id = fo.product_id
 WHERE TO_CHAR(fo.produce_date, 'YYYY-MM') = '2022-05'
 GROUP BY fd.product_id, fd.product_name
 ORDER BY total_sales DESC, fd.product_id ASC;
```

`WHERE` 절을 `WHERE fo.produce_date = TO_DATE('2022-05', 'YYYY-MM')`과 같이 작성한다면, `produce_date` 컬럼이 '2022-05'로 정확히 일치하는 행만 가져오겠다는 의미이기 때문에 원하는 결과을 얻을 수 없으므로 주의해야 한다.

## [취소되지 않은 진료 예약 조회하기](https://school.programmers.co.kr/learn/courses/30/lessons/132204){: target="_blank" }

![취소되지-않은-진료-예약-조회하기(1)](/assets/img/posts/ps/programmers/sql-problems/취소되지-않은-진료-예약-조회하기(1).png)
![취소되지-않은-진료-예약-조회하기(2)](/assets/img/posts/ps/programmers/sql-problems/취소되지-않은-진료-예약-조회하기(2).png)
![취소되지-않은-진료-예약-조회하기(3)](/assets/img/posts/ps/programmers/sql-problems/취소되지-않은-진료-예약-조회하기(3).png)

```sql
SELECT a.apnt_no, p.pt_name, a.pt_no, a.mcdp_cd, d.dr_name, a.apnt_ymd
  FROM patient p
 INNER JOIN appointment a
    ON p.pt_no = a.pt_no
 INNER JOIN doctor d
    ON a.mddr_id = d.dr_id
 WHERE TO_CHAR(a.apnt_ymd, 'YYYY-MM-DD') = '2022-04-13'
       AND
       a.apnt_cncl_yn = 'N'
       AND
       a.mcdp_cd = 'CS'
ORDER BY a.apnt_ymd ASC;
```

## [년, 월, 성별 별 상품 구매 회원 수 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/131532){: target="_blank" }

![년,-월,-성별-별-상품-구매-회원-수-구하기(1)](/assets/img/posts/ps/programmers/sql-problems/년,-월,-성별-별-상품-구매-회원-수-구하기(1).png)
![년,-월,-성별-별-상품-구매-회원-수-구하기(2)](/assets/img/posts/ps/programmers/sql-problems/년,-월,-성별-별-상품-구매-회원-수-구하기(2).png)

```sql
SELECT EXTRACT(YEAR FROM s.sales_date) AS year,
       EXTRACT(MONTH FROM s.sales_date) AS month,
       i.gender,
       COUNT(DISTINCT(i.user_id)) AS users
  FROM user_info i
 INNER JOIN online_sale s
    ON i.user_id = s.user_id
 WHERE i.gender IS NOT NULL
 GROUP BY EXTRACT(YEAR FROM s.sales_date), EXTRACT(MONTH FROM s.sales_date), i.gender
 ORDER BY year ASC, month ASC, i.gender ASC;
```

## [서울에 위치한 식당 목록 출력하기](https://school.programmers.co.kr/learn/courses/30/lessons/131118){: target="_blank" }

![서울에-위치한-식당-목록-출력하기(1)](/assets/img/posts/ps/programmers/sql-problems/서울에-위치한-식당-목록-출력하기(1).png)
![서울에-위치한-식당-목록-출력하기(2)](/assets/img/posts/ps/programmers/sql-problems/서울에-위치한-식당-목록-출력하기(2).png)

```sql
SELECT i.rest_id,
       i.rest_name,
       i.food_type,
       i.favorites,
       i.address,
       ROUND(AVG(r.review_score), 2) AS score
  FROM rest_info i
 INNER JOIN rest_review r
    ON i.rest_id = r.rest_id
 WHERE i.address LIKE '서울%'
 GROUP BY i.rest_id, i.rest_name, i.food_type, i.favorites, i.address
 ORDER BY score DESC, i.favorites DESC;
```

## [우유와 요거트가 담긴 장바구니](https://school.programmers.co.kr/learn/courses/30/lessons/62284){: target="_blank" }

![우유와-요거트가-담긴-장바구니(1)](/assets/img/posts/ps/programmers/sql-problems/우유와-요거트가-담긴-장바구니(1).png)
![우유와-요거트가-담긴-장바구니(2)](/assets/img/posts/ps/programmers/sql-problems/우유와-요거트가-담긴-장바구니(2).png)

```sql
SELECT cart_id
  FROM cart_products
 WHERE name IN ('Milk', 'Yogurt')
 GROUP BY cart_id
HAVING COUNT(DISTINCT name) = 2
 ORDER BY cart_id ASC;
```

## [저자 별 카테고리 별 매출액 집계하기](https://school.programmers.co.kr/learn/courses/30/lessons/59044){: target="_blank" }

![저자-별-카테고리-별-매출액-집계하기(1)](/assets/img/posts/ps/programmers/sql-problems/저자-별-카테고리-별-매출액-집계하기(1).png)
![저자-별-카테고리-별-매출액-집계하기(2)](/assets/img/posts/ps/programmers/sql-problems/저자-별-카테고리-별-매출액-집계하기(2).png)
![저자-별-카테고리-별-매출액-집계하기(3)](/assets/img/posts/ps/programmers/sql-problems/저자-별-카테고리-별-매출액-집계하기(3).png)

```sql
SELECT a.author_id, a.author_name, b.category, SUM(s.sales * b.price) AS total_sales
  FROM book b
 INNER JOIN author a
    ON b.author_id = a.author_id
 INNER JOIN book_sales s
    ON b.book_id = s.book_id
 WHERE TO_CHAR(s.sales_date, 'YYYY-MM') = '2022-01'
 GROUP BY a.author_id, a.author_name, b.category
 ORDER BY a.author_id ASC, b.category DESC;
```

## [주문량이 많은 아이스크림들 조회하기](https://school.programmers.co.kr/learn/courses/30/lessons/133027){: target="_blank" }

![주문량이-많은-아이스크림들-조회하기(1)](/assets/img/posts/ps/programmers/sql-problems/주문량이-많은-아이스크림들-조회하기(1).png)
![주문량이-많은-아이스크림들-조회하기(2)](/assets/img/posts/ps/programmers/sql-problems/주문량이-많은-아이스크림들-조회하기(2).png)
![주문량이-많은-아이스크림들-조회하기(3)](/assets/img/posts/ps/programmers/sql-problems/주문량이-많은-아이스크림들-조회하기(3).png)

```sql
SELECT flavor
  FROM (SELECT flavor, total_order
          FROM first_half
         UNION ALL
        SELECT flavor, total_order
          FROM july) combined
 GROUP BY flavor
 ORDER BY SUM(total_order) DESC
 FETCH FIRST 3 ROWS ONLY;
```

![주문량이-많은-아이스크림들-조회하기(4)](/assets/img/posts/ps/programmers/sql-problems/주문량이-많은-아이스크림들-조회하기(4).png)

7월에는 아이스크림 주문량이 많아 같은 아이스크림에 대하여 서로 다른 두 공장에서 아이스크림 가게로 출하를 진행하는 경우가 있다. 이 경우 "같은 맛의 아이스크림이라도 다른 출하 번호를 갖게 됩니다." 라고 문제에 명시되어 있다. 실제로 위 사진과 같이 `july` 테이블의 데이터를 가져와보면 `flavor`가 "strawberry"인 `shipment_id`가 2개 있다. 따라서 `INNER JOIN`대신 `UNION ALL`을 사용했다. `UNION ALL`은 중복을 제거하지 않고 `UNION`은 중복을 제거한다.

## [그룹별 조건에 맞는 식당 목록 출력하기](https://school.programmers.co.kr/learn/courses/30/lessons/131124){: target="_blank" }

![그룹별-조건에-맞는-식당-목록-출력하기(1)](/assets/img/posts/ps/programmers/sql-problems/그룹별-조건에-맞는-식당-목록-출력하기(1).png)
![그룹별-조건에-맞는-식당-목록-출력하기(2)](/assets/img/posts/ps/programmers/sql-problems/그룹별-조건에-맞는-식당-목록-출력하기(2).png)

```sql
SELECT p.member_name, r.review_text, TO_CHAR(r.review_date, 'YYYY-MM-DD') AS review_date
  FROM member_profile p
 INNER JOIN rest_review r
    ON p.member_id = r.member_id
 WHERE p.member_id IN (SELECT member_id
                         FROM (SELECT member_id,
                                      DENSE_RANK() OVER(ORDER BY count(member_id) DESC) AS count
                                 FROM rest_review
                                GROUP BY member_id)
                        WHERE count = 1)
 ORDER BY review_date ASC, review_text ASC;
```

`member_id`별 리뷰 개수를 카운팅하기 위해 `member_id`를 그룹화하고, `DENSE_RANK()` 함수를 사용하여 순위 값을 구한다. `RANK()` 함수를 사용해도 문제는 없지만 좀 더 직관적인 `DENSE_RANK() `함수를 사용했다. `RANK()` 함수와 `DENSE_RANK()` 함수에 대한 차이는 아래의 표를 보면 이해하기 쉽다.

| 값 | RANK() | DENSE_RANK() |
|:--:|:------:|:------------:|
| 10 |   1    |      1       |
| 20 |   2    |      2       |
| 20 |   2    |      2       |
| 30 |   4    |      3       |
| 40 |   5    |      4       |


## [오프라인/온라인 판매 데이터 통합하기](https://school.programmers.co.kr/learn/courses/30/lessons/131537){: target="_blank" }

![오프라인-온라인-판매-데이터-통합하기(1)](/assets/img/posts/ps/programmers/sql-problems/오프라인-온라인-판매-데이터-통합하기(1).png)
![오프라인-온라인-판매-데이터-통합하기(2)](/assets/img/posts/ps/programmers/sql-problems/오프라인-온라인-판매-데이터-통합하기(2).png)

```sql
(SELECT TO_CHAR(sales_date, 'YYYY-MM-DD') AS sales_date, product_id, user_id, sales_amount
   FROM online_sale
  WHERE TO_CHAR(sales_date, 'YYYY-MM') = '2022-03'
  UNION ALL
 SELECT TO_CHAR(sales_date, 'YYYY-MM-DD'), product_id, NULL, sales_amount
   FROM offline_sale
  WHERE TO_CHAR(sales_date, 'YYYY-MM') = '2022-03')
  ORDER BY sales_date ASC, product_id ASC, user_id ASC;
```

`UNION` 또는 `UNION ALL`을 사용할 때는 `SELECT` 문의 대응하는 컬럼의 데이터 유형과 컬럼의 개수가 일치해야 한다. 따라서 `online_sale` 테이블과 `offline_sale` 테이블의 `sales_date` 컬럼과 동일하게 `TO_CHAR()` 함수를 사용하여 문자열로 변환시켜준다.

마찬가지로 두 테이블의 컬럼 개수를 일치시켜주기 위해 `offline_sale` 테이블에서 `NULL`을 `SELECT` 하는데 이때, `NULL`과 `online_sale` 테이블의 `user_id` 컬럼의 데이터 형식이 일치하지 않지 않느냐는 의문이 생길 수 있다. 확인해 본 결과 오라클에서는 `NULL`이 특수한 값으로 간주되어 어떤 데이터 형식과도 호환이 될 수 있다고 한다. 잘 생각해보면 당연한 게 우리가 테이블에 `NULL`을 `INSERT`할 때 데이터 형식을 신경쓰지는 않았다. 물론 `NOT NULL` 제약조건이 있으면 얘기가 달라진다.

## [입양 시각 구하기(2)](https://school.programmers.co.kr/learn/courses/30/lessons/59413){: target="_blank" }

![입양-시각-구하기(2)(1)](/assets/img/posts/ps/programmers/sql-problems/입양-시각-구하기(2)(1).png)
![입양-시각-구하기(2)(2)](/assets/img/posts/ps/programmers/sql-problems/입양-시각-구하기(2)(2).png)

```sql
SELECT d.hour, COUNT(o.animal_id) AS count
  FROM (SELECT LEVEL - 1 AS hour
          FROM dual
       CONNECT BY LEVEL <= 24) d
  LEFT JOIN animal_outs o
    ON d.hour = TO_CHAR(o.datetime, 'FMHH24')
 GROUP BY d.hour
 ORDER BY d.hour ASC;
```

문제 예시대로 0부터 23에 해당하는 가상의 테이블을 생성하고 각각의 숫자에 대응되도록 `JOIN`하되, 대응하는 숫자가 없는 경우도 고려해줘야 되기 때문에 `LEFT JOIN`을 사용한다.

`LEFT JOIN`을 사용함으로써 발생한 `NULL`은 카운팅되지 않아야하기 때문에 `COUNT(*)`대신 `COUNT(column_name)`을 사용했다. `COUNT(*)`는 `NULL`을 계산하는 반면, `COUNT(coulmn_name)`은 `NULL`을 계산하지 않는다.

## [특정 기간동안 대여 가능한 자동차들의 대여비용 구하기(1)](https://school.programmers.co.kr/learn/courses/30/lessons/157339){: target="_blank" }

![특정-기간동안-대여-가능한-자동차들의-대여비용-구하기(1)](/assets/img/posts/ps/programmers/sql-problems/특정-기간동안-대여-가능한-자동차들의-대여비용-구하기(1).png)
![특정-기간동안-대여-가능한-자동차들의-대여비용-구하기(2)](/assets/img/posts/ps/programmers/sql-problems/특정-기간동안-대여-가능한-자동차들의-대여비용-구하기(2).png)
![특정-기간동안-대여-가능한-자동차들의-대여비용-구하기(3)](/assets/img/posts/ps/programmers/sql-problems/특정-기간동안-대여-가능한-자동차들의-대여비용-구하기(3).png)

```sql
SELECT car_id, car_type, daily_fee * 30 - (daily_fee * 30 * discount_rate / 100) AS fee
  FROM (SELECT c.car_id,
               c.car_type,
               c.daily_fee,
               d.discount_rate
          FROM car_rental_company_car c
         INNER JOIN car_rental_company_discount_plan d
            ON c.car_type = d.car_type
         WHERE c.car_id NOT IN (SELECT car_id
                                  FROM car_rental_company_rental_history
                                 WHERE end_date > DATE '2022-11-01'
                                       AND
                                       start_date < DATE '2022-12-01')
               AND
               c.car_type IN ('세단', 'SUV')
               AND
               d.duration_type = '30일 이상')
 WHERE daily_fee * 30 - (daily_fee * 30 * discount_rate / 100) >= 500000
       AND
       daily_fee * 30 - (daily_fee * 30 * discount_rate / 100) < 2000000
 ORDER BY fee DESC, car_type ASC, car_id DESC;
```

조건이 많아 쿼리가 복잡해보이지만 아래의 서브쿼리 출력값을 확인하면 이해하기 쉽다.

```sql
SELECT c.car_id,
       c.car_type,
       c.daily_fee,
       d.discount_rate
  FROM car_rental_company_car c
 INNER JOIN car_rental_company_discount_plan d
    ON c.car_type = d.car_type
 WHERE c.car_id NOT IN (SELECT car_id
                          FROM car_rental_company_rental_history
                         WHERE end_date > DATE '2022-11-01'
                               AND
                               start_date < DATE '2022-12-01')
       AND
       c.car_type IN ('세단', 'SUV')
       AND
       d.duration_type = '30일 이상';
```

| car_id | car_type | daily_fe` | discount_rate |
|:------:|:--------:|:---------:|:-------------:|    
|    3   |   세단   |   55000   |       8       |
|   23   |   세단   |   50000   |       8       |
|   24   |   세단   |  184000   |       8       |
|   25   |   세단   |  115000   |       8       |
|    4   |   SUV    |  150000   |       5       |
|   14   |   SUV    |   77000   |       5       |

## [자동차 대여 기록 별 대여 금액 구하기(1)](https://school.programmers.co.kr/learn/courses/30/lessons/151141){: target="_blank" }

![자동차-대여-기록-별-대여-금액-구하기(1)](/assets/img/posts/ps/programmers/sql-problems/자동차-대여-기록-별-대여-금액-구하기(1).png)
![자동차-대여-기록-별-대여-금액-구하기(2)](/assets/img/posts/ps/programmers/sql-problems/자동차-대여-기록-별-대여-금액-구하기(2).png)
![자동차-대여-기록-별-대여-금액-구하기(3)](/assets/img/posts/ps/programmers/sql-problems/자동차-대여-기록-별-대여-금액-구하기(3).png)

```sql
SELECT sub.history_id, sub.fee - (sub.fee * NVL(d.discount_rate, 0) / 100) AS fee
  FROM (SELECT h.history_id,
               c.daily_fee * (h.end_date - h.start_date + 1) AS fee,
               CASE WHEN h.end_date - h.start_date + 1 >= 90 THEN '90일 이상'
                    WHEN h.end_date - h.start_date + 1 >= 30 THEN '30일 이상'
                    WHEN h.end_date - h.start_date + 1 >= 7 THEN '7일 이상'
               ELSE '7일 미만'
                END AS duration_type
          FROM car_rental_company_car c
         INNER JOIN car_rental_company_rental_history h
            ON c.car_id = h.car_id
         WHERE car_type = '트럭') sub
  LEFT JOIN car_rental_company_discount_plan d
    ON sub.duration_type = d.duration_type
       AND
       d.car_type = '트럭'
 ORDER BY fee DESC, sub.history_id DESC;
```

할인된 금액을 구하기 위해선 원가가 필요하다. 원가는 일일 요금 * 대여 기간 이므로 일일 요금(`daily_fee`) 정보가 담긴 테이블인 `car_rental_company_car` 테이블과 대여 기간(`end_date - start_date + 1`) 정보가 담긴 `car_rental_comapny_rental_history` 테이블을 `JOIN`하여 서브 쿼리로 둔다. 이때 대여 기간에 따른 할인율은 `car_rental_company_discount_plan` 테이블의 `duration_type` 컬럼을 따르므로 `CASE WHEN` 문을 사용하여 동일하게 일치시켜준다. 주의할 점은 `CASE WHEN`문에서 7일 이상 조건을 90일 이상 조건보다 먼저 위치시킨다면 대여 기간이 90일 이상, 30일 이상임에도 불구하고 7일 이상으로 반환되니 조심해야 한다. 아래는 그 서브 쿼리와 서브 쿼리의 결과다.

```sql
SELECT h.history_id,
       c.daily_fee * (h.end_date - h.start_date + 1) AS fee,
       CASE WHEN h.end_date - h.start_date + 1 >= 90 THEN '90일 이상'
            WHEN h.end_date - h.start_date + 1 >= 30 THEN '30일 이상'
            WHEN h.end_date - h.start_date + 1 >= 7 THEN '7일 이상'
       ELSE '7일 미만'
        END AS duration_type
  FROM car_rental_company_car c
 INNER JOIN car_rental_company_rental_history h
    ON c.car_id = h.car_id
 WHERE car_type = '트럭';
```

| history_id |   fee   | duration_type |
|:----------:|:-------:|:-------------:|
|     722    | 3162000 |   30일 이상   |
|     558    | 4788000 |   30일 이상   |
|     653    | 4123000 |   30일 이상   |
|     710    |  532000 |    7일 미만   |
|     720    |  532000 |    7일 미만   |
|     524    |  107000 |    7일 미만   |
|     527    |  535000 |    7일 미만   |
|     546    |  214000 |    7일 미만   |
|     556    |  856000 |    7일 이상   |
|     581    |  214000 |    7일 미만   |
|     586    |  214000 |    7일 미만   |
|     591    | 1177000 |    7일 이상   |
|     618    |  214000 |    7일 미만   |
|     627    |  107000 |    7일 미만   |
|     631    |  321000 |    7일 미만   |
|     640    |  428000 |    7일 미만   |
|     673    |  321000 |    7일 미만   |
|     680    | 1605000 |    7일 이상   |
|     705    |  107000 |    7일 미만   |
|     711    |  107000 |    7일 미만   |
|     714    | 1177000 |    7일 이상   |
|     594    | 1988000 |    7일 이상   |
|     628    |  426000 |    7일 미만   |
|     641    |  426000 |    7일 미만   |
|     654    |  284000 |    7일 미만   |
|     676    |  568000 |    7일 미만   |
|     681    | 5822000 |   30일 이상   |
|     602    |  336000 |    7일 미만   |
|     701    |  504000 |    7일 미만   |
|     610    |  672000 |    7일 미만   |
|     623    |  336000 |    7일 미만   |
|     630    | 5208000 |   30일 이상   |
|     724    | 6888000 |   30일 이상   |
|     716    |  140000 |    7일 미만   |

이제 서브 쿼리의 `duration_type` 컬럼과 `car_rental_company_discount_plan` 테이블의 `duration_type` 컬럼을 일치시켰으니 `JOIN`하되, 대여 기간이 7일 미만인 경우 할인 정책이 없기 때문에 `LEFT JOIN`을 사용한다. 또한 중요한 점은 `ON` 절에 `car_rental_company_discount_plan` 테이블의 `car_type`이 '트럭'인 조건을 명시해야 한다는 것이다. `LEFT JOIN`을 사용했기 때문에 대여 기간이 7일 미만인 경우에 `NULL` 값이 생기는데 `WHERE` 절에서 조건을 명시한다면 대여 기간이 7일 미만인 데이터들은 가져오지 않는다. 아래의 쿼리와 그 출력값을 보면 이해하기 쉽다.

```sql
SELECT *
  FROM (SELECT h.history_id,
               c.daily_fee * (h.end_date - h.start_date + 1) AS fee,
               CASE WHEN h.end_date - h.start_date + 1 >= 90 THEN '90일 이상'
                    WHEN h.end_date - h.start_date + 1 >= 30 THEN '30일 이상'
                    WHEN h.end_date - h.start_date + 1 >= 7 THEN '7일 이상'
               ELSE '7일 미만'
                END AS duration_type
          FROM car_rental_company_car c
         INNER JOIN car_rental_company_rental_history h
            ON c.car_id = h.car_id
         WHERE car_type = '트럭') sub
  LEFT JOIN car_rental_company_discount_plan d
    ON sub.duration_type = d.duration_type;
```

| history_id |   fee   | duration_type | plan_id | car_type | duration_type | discount_rate |
|:----------:|:-------:|:-------------:|:-------:|:--------:|:-------------:|:-------------:|
|     556    |  856000 |   7일 이상    |    1     |   세단   |    7일 이상   |       5       |
|     591    | 1177000 |    7일 이상   |    1     |   세단   |    7일 이상   |       5       |
|     680    | 1605000 |    7일 이상   |    1     |   세단   |    7일 이상   |       5       |
|     714    | 1177000 |    7일 이상   |    1     |   세단   |    7일 이상   |       5       |
|     594    | 1988000 |    7일 이상   |    1     |   세단   |    7일 이상   |       5       |
|     722    | 3162000 |   30일 이상   |    2     |   세단   |   30일 이상   |       8       |
|     558    | 4788000 |   30일 이상   |    2     |   세단   |   30일 이상   |       8       |
|     653    | 4123000 |   30일 이상   |    2     |   세단   |   30일 이상   |       8       |
|     681    | 5822000 |   30일 이상   |    2     |   세단   |   30일 이상   |       8       |
|     630    | 5208000 |   30일 이상   |    2     |   세단   |   30일 이상   |       8       |
|     724    | 6888000 |   30일 이상   |    2     |   세단   |   30일 이상   |       8       |
|     556    |  856000 |    7일 이상   |    4     |    SUV   |    7일 이상   |       3       |
|     591    | 1177000 |    7일 이상   |    4     |    SUV   |    7일 이상   |       3       |
|     680    | 1605000 |    7일 이상   |    4     |    SUV   |    7일 이상   |       3       |
|     714    | 1177000 |    7일 이상   |    4     |    SUV   |    7일 이상   |       3       |
|     594    | 1988000 |    7일 이상   |    4     |    SUV   |    7일 이상   |       3       |
|     722    | 3162000 |   30일 이상   |    5     |    SUV   |   30일 이상   |       5       |
|     558    | 4788000 |   30일 이상   |    5     |    SUV   |   30일 이상   |       5       |
|     653    | 4123000 |   30일 이상   |    5     |    SUV   |   30일 이상   |       5       |
|     681    | 5822000 |   30일 이상   |    5     |    SUV   |   30일 이상   |       5       |
|     630    | 5208000 |   30일 이상   |    5     |    SUV   |   30일 이상   |       5       |
|     724    | 6888000 |   30일 이상   |    5     |    SUV   |   30일 이상   |       5       |
|     556    |  856000 |    7일 이상   |    7     |  승합차  |    7일 이상   |       5       |
|     591    | 1177000 |    7일 이상   |    7     |  승합차  |    7일 이상   |       5       |
|     680    | 1605000 |    7일 이상   |    7     |  승합차  |    7일 이상   |       5       |
|     714    | 1177000 |    7일 이상   |    7     |  승합차  |    7일 이상   |       5       |
|     594    | 1988000 |    7일 이상   |    7     |  승합차  |    7일 이상   |       5       |
|     722    | 3162000 |   30일 이상   |    8     |  승합차  |   30일 이상   |      10       |
|     558    | 4788000 |   30일 이상   |    8     |  승합차  |   30일 이상   |      10       |
|     653    | 4123000 |   30일 이상   |    8     |  승합차  |   30일 이상   |      10       |
|     681    | 5822000 |   30일 이상   |    8     |  승합차  |   30일 이상   |      10       |
|     630    | 5208000 |   30일 이상   |    8     |  승합차  |   30일 이상   |      10       |
|     724    | 6888000 |   30일 이상   |    8     |  승합차  |   30일 이상   |      10       |
|     556    |  856000 |    7일 이상   |   10     |   트럭   |    7일 이상   |       5       |
|     591    | 1177000 |    7일 이상   |   10     |   트럭   |    7일 이상   |       5       |
|     680    | 1605000 |    7일 이상   |   10     |   트럭   |    7일 이상   |       5       |
|     714    | 1177000 |    7일 이상   |   10     |   트럭   |    7일 이상   |       5       |
|     594    |  198800 |    7일 이상   |   10     |   트럭   |    7일 이상   |       5       |
|     722    | 3162000 |   30일 이상   |   11     |   트럭   |   30일 이상   |       8       |
|     558    | 4788000 |   30일 이상   |   11     |   트럭   |   30일 이상   |       8       |
|     653    | 4123000 |   30일 이상   |   11     |   트럭   |   30일 이상   |       8       |
|     681    | 5822000 |   30일 이상   |   11     |   트럭   |   30일 이상   |       8       |
|     630    | 5208000 |   30일 이상   |   11     |   트럭   |   30일 이상   |       8       |
|     724    | 6888000 |   30일 이상   |   11     |   트럭   |   30일 이상   |       8       |
|     556    |  856000 |    7일 이상   |   13     |  리무진  |    7일 이상   |       4       |
|     591    | 1177000 |    7일 이상   |   13     |  리무진  |    7일 이상   |       4       |
|     680    | 1605000 |    7일 이상   |   13     |  리무진  |    7일 이상   |       4       |
|     714    | 1177000 |    7일 이상   |   13     |  리무진  |    7일 이상   |       4       |
|     594    | 1988000 |    7일 이상   |   13     |  리무진  |    7일 이상   |       4       |
|     722    | 3162000 |   30일 이상   |   14     |  리무진  |   30일 이상   |       8       |
|     558    | 4788000 |   30일 이상   |   14     |  리무진  |   30일 이상   |       8       |
|     653    | 4123000 |   30일 이상   |   14     |  리무진  |   30일 이상   |       8       |
|     681    | 5822000 |   30일 이상   |   14     |  리무진  |   30일 이상   |       8       |
|     630    | 5208000 |   30일 이상   |   14     |  리무진  |   30일 이상   |       8       |
|     724    | 6888000 |   30일 이상   |   14     |  리무진  |   30일 이상   |       8       |
|     710    |  532000 |    7일 미만   |          |          |              |               |
|     720    |  532000 |    7일 미만   |          |          |              |               |
|     524    |  107000 |    7일 미만   |          |          |              |               |
|     527    |  535000 |    7일 미만   |          |          |              |               |
|     546    |  214000 |    7일 미만   |          |          |              |               |
|     581    |  214000 |    7일 미만   |          |          |              |               |
|     586    |  214000 |    7일 미만   |          |          |              |               |
|     618    |  214000 |    7일 미만   |          |          |              |               |
|     627    |  107000 |    7일 미만   |          |          |              |               |
|     631    |  321000 |    7일 미만   |          |          |              |               |
|     640    |  428000 |    7일 미만   |          |          |              |               |
|     673    |  321000 |    7일 미만   |          |          |              |               |
|     705    |  107000 |    7일 미만   |          |          |              |               |
|     711    |  107000 |    7일 미만   |          |          |              |               |
|     628    |  426000 |    7일 미만   |          |          |              |               |
|     641    |  426000 |    7일 미만   |          |          |              |               |
|     654    |  284000 |    7일 미만   |          |          |              |               |
|     676    |  568000 |    7일 미만   |          |          |              |               |
|     602    |  336000 |    7일 미만   |          |          |              |               |
|     701    |  504000 |    7일 미만   |          |          |              |               |
|     610    |  672000 |    7일 미만   |          |          |              |               |
|     623    |  336000 |    7일 미만   |          |          |              |               |
|     716    |  140000 |    7일 미만   |          |          |              |               |

`WHERE` 절에서 `car_type`이 '트럭'인 데이터들만 가져온다는 의미는 위 테이블에서 `car_type`이 '트럭'인 데이터들만 가져온다는 의미와 같기 때문에 대여 기간이 7일 미만이어서 `NULL` 값을 포함하는 데이터들은 포함되지 않는다. 반면 `ON` 절에서 `car_type`이 '트럭'임을 명시한다면 애초에 `JOIN`할 때 `car_type`이 '트럭'인 데이터들을 가져오기 때문에 `NULL` 값과 무관하게 의도한 데이터들을 가져올 수 있는 것이다.
