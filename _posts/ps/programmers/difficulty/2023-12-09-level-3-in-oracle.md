---
title: "[프로그래머스]레벨 3 난이도 모든 문제(Oracle)"
categories: [PS, Programmers]
tags: [PS, Algorithm, 알고리즘, Programmers, 프로그래머스, SQL, Oracle, 오라클, 레벨3]
image:
  path: /assets/img/posts/ps/programmers/01-programmers-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: programmers
---

# 레벨 3 난이도 모든 문제(Oracle)

2023-12-09 기준 정답률 순

## [오랜 기간 보호한 동물(1)](https://school.programmers.co.kr/learn/courses/30/lessons/59044){: target="_blank" }

![오랜-기간-보호한-동물(1)(1)](/assets/img/posts/ps/programmers/sql-problems/오랜-기간-보호한-동물(1)(1).png)
![오랜-기간-보호한-동물(1)(2)](/assets/img/posts/ps/programmers/sql-problems/오랜-기간-보호한-동물(1)(2).png)

```sql
SELECT name, datetime
  FROM (SELECT i.name, i.datetime
          FROM animal_ins i
          LEFT JOIN animal_outs o
	    ON i.animal_id = o.animal_id
         WHERE o.animal_id IS NULL
         ORDER BY i.datetime)
 FETCH FIRST 3 ROWS ONLY;
```

아직 입양을 가지 못했다는 의미는 `animal_ins` 테이블에는 존재하지만 `animal_outs` 테이블에 존재하지 않는 동물이라는 의미이므로, 기본키인 `animal_ins`테이블의 `animal_id` 컬럼을 기준으로 `animal_outs`테이블을 `LEFT JOIN`하여 `NULL` 값이 만들어지록 한다.

## [카테고리 별 도서 판매량 집계하기](https://school.programmers.co.kr/learn/courses/30/lessons/144855){: target="_blank" }

![카테고리-별-도서-판매량-집계하기(1)](/assets/img/posts/ps/programmers/sql-problems/카테고리-별-도서-판매량-집계하기(1).png)
![카테고리-별-도서-판매량-집계하기(2)](/assets/img/posts/ps/programmers/sql-problems/카테고리-별-도서-판매량-집계하기(2).png)

```sql
SELECT b.category, SUM(bs.SALES) AS total_Sales
  FROM book b
 INNER JOIN book_sales bs
    ON b.book_id = bs.book_id
 WHERE EXTRACT(MONTH FROM bs.sales_date) = 1
 GROUP BY b.category
 ORDER BY b.category ASC;
```

## [있었는데요 없었습니다](https://school.programmers.co.kr/learn/courses/30/lessons/59043){: target="_blank" }

![있었는데요-없었습니다(1)](/assets/img/posts/ps/programmers/sql-problems/있었는데요-없었습니다(1).png)
![있었는데요-없었습니다(2)](/assets/img/posts/ps/programmers/sql-problems/있었는데요-없었습니다(2).png)

```sql
SELECT i.animal_id, i.name
  FROM animal_ins i
  LEFT JOIN animal_outs o
    ON i.animal_id = o.animal_id
 WHERE i.datetime > o.datetime
 ORDER BY i.datetime ASC;
```

## [오랜 기간 보호한 동물(2)](https://school.programmers.co.kr/learn/courses/30/lessons/59411){: target="_blank" }

![오랜-기간-보호한-동물(2)(1)](/assets/img/posts/ps/programmers/sql-problems/오랜-기간-보호한-동물(2)(1).png)
![오랜-기간-보호한-동물(2)(2)](/assets/img/posts/ps/programmers/sql-problems/오랜-기간-보호한-동물(2)(2).png)

```sql
SELECT *
  FROM 
  (SELECT i.animal_id, i.name
     FROM animal_ins i
    INNER JOIN animal_outs o
       ON i.animal_id = o.animal_id
    ORDER BY o.datetime - i.datetime DESC)
 WHERE ROWNUM <= 2;
```
```sql
SELECT *
  FROM 
  (SELECT i.animal_id, i.name
     FROM animal_ins i
    INNER JOIN animal_outs o
       ON i.animal_id = o.animal_id
    ORDER BY o.datetime - i.datetime DESC)
 FETCH FIRST 2 ROWS ONLY;
```

## [조건별로 분류하여 주문상태 출력하기](https://school.programmers.co.kr/learn/courses/30/lessons/131113){: target="_blank" }

![조건별로-분류하여-주문상태-출력하기(1)](/assets/img/posts/ps/programmers/sql-problems/조건별로-분류하여-주문상태-출력하기(1).png)
![조건별로-분류하여-주문상태-출력하기(2)](/assets/img/posts/ps/programmers/sql-problems/조건별로-분류하여-주문상태-출력하기(2).png)

```sql
SELECT order_id,
       product_id, 
       TO_CHAR(out_date, 'YYYY-MM-DD') AS out_date,
       CASE WHEN out_date <= DATE '2022-05-01' THEN '출고완료'
            WHEN out_date > DATE '2022-05-01' THEN '출고대기'
       ELSE '출고미정'
        END AS 출고여부
  FROM food_order
 ORDER BY order_id ASC;
```

## [조건에 맞는 사용자와 총 거래금액 조회하기](https://school.programmers.co.kr/learn/courses/30/lessons/164668){: target="_blank" }

![조건에-맞는-사용자와-총-거래금액-조회하기(1)](/assets/img/posts/ps/programmers/sql-problems/조건에-맞는-사용자와-총-거래금액-조회하기(1).png)
![조건에-맞는-사용자와-총-거래금액-조회하기(2)](/assets/img/posts/ps/programmers/sql-problems/조건에-맞는-사용자와-총-거래금액-조회하기(2).png)

```sql
SELECT u.user_id, u.nickname, b.total_sales
  FROM used_goods_user u
 INNER JOIN (SELECT writer_id, SUM(price) AS total_sales
               FROM used_goods_board
              WHERE status = 'DONE'
              GROUP BY writer_id
             HAVING SUM(price) >= 700000) b
    ON u.user_id = b.writer_id
 ORDER BY total_sales ASC;
```

## [대여 기록이 존재하는 자동차 리스트 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/157341){: target="_blank" }

![대여-기록이-존재하는-자동차-리스트-구하기(1)](/assets/img/posts/ps/programmers/sql-problems/대여-기록이-존재하는-자동차-리스트-구하기(1).png)
![대여-기록이-존재하는-자동차-리스트-구하기(2)](/assets/img/posts/ps/programmers/sql-problems/대여-기록이-존재하는-자동차-리스트-구하기(2).png)

```sql
SELECT car_id
  FROM car_rental_company_car
 WHERE car_type = '세단'
       AND
       car_id IN (SELECT car_id
                   FROM car_rental_company_rental_history
                  WHERE EXTRACT(MONTH FROM start_date) = 10)
ORDER BY car_id DESC;
```

## [즐겨찾기가 가장 많은 식당 정보 출력하기](https://school.programmers.co.kr/learn/courses/30/lessons/131123){: target="_blank" }

![즐겨찾기가-가장-많은-식당-정보-출력하기(1)](/assets/img/posts/ps/programmers/sql-problems/즐겨찾기가-가장-많은-식당-정보-출력하기(1).png)
![즐겨찾기가-가장-많은-식당-정보-출력하기(2)](/assets/img/posts/ps/programmers/sql-problems/즐겨찾기가-가장-많은-식당-정보-출력하기(2).png)

```sql
SELECT food_type, rest_id, rest_name, favorites
  FROM rest_info
 WHERE (food_type, favorites) IN (SELECT food_type, MAX(favorites)
                                    FROM rest_info
                                   GROUP BY food_type)
 ORDER BY food_type DESC;
```

## [없어진 기록 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/59042){: target="_blank" }

![없어진-기록-찾기(1)](/assets/img/posts/ps/programmers/sql-problems/없어진-기록-찾기(1).png)
![없어진-기록-찾기(2)](/assets/img/posts/ps/programmers/sql-problems/없어진-기록-찾기(2).png)

```sql
SELECT o.animal_id, o.name
  FROM animal_outs o
  LEFT JOIN animal_ins i 
    ON o.animal_id = i.animal_id
 WHERE i.animal_id IS NULL
 ORDER BY o.animal_id ASC;
```

## [조건에 맞는 사용자 정보 조회하기](https://school.programmers.co.kr/learn/courses/30/lessons/164670){: target="_blank" }

![조건에-맞는-사용자-정보-조회하기(1)](/assets/img/posts/ps/programmers/sql-problems/조건에-맞는-사용자-정보-조회하기(1).png)
![조건에-맞는-사용자-정보-조회하기(2)](/assets/img/posts/ps/programmers/sql-problems/조건에-맞는-사용자-정보-조회하기(2).png)

```sql
SELECT user_id,
       nickname,
       city || ' ' || street_address1 || ' ' || street_address2 AS 전체주소,
       REGEXP_REPLACE(TO_CHAR(tlno), '(\d{3}(\d{4}(\d{4}', '\1-\2-\3') AS 전화번호
  FROM used_goods_user
 WHERE user_id IN (SELECT writer_id
                     FROM used_goods_board
                    GROUP BY writer_id
                   HAVING COUNT(writer_id) >= 3)
 ORDER BY user_id DESC;
```

정규표현식에서 `\d`는 숫자를 의미하고, `{3}`는 바로 앞의 표현식이 정확히 3번 반복되어야 함을 나타낸다. 따라서  `(\d{3}(\d{4}`는 각각 세 자리 숫자, 네 자리 숫자에 대응된다.

`\1`, `\2`, `\3`는 각각 첫 번쨰 그룹(`(\d{3}`), 두 번째 그룹(`(\d{3}`), 세 번째 그룹(`(\d{3}`)에 대한 참조를 나타낸다. 따라서 `\1-\2-\3`는 각 그룹에 대한 참조를 통해 각각 그룹의 값을 다시 가져와서 &lsquo;-&rsquo; 로 구분된 형태로 만들어준다.

## [자동차 대여 기록에서 대여중 / 대여 가능 여부 구분하기](https://school.programmers.co.kr/learn/courses/30/lessons/157340){: target="_blank" }

![자동차-대여-기록에서-대여중-대여-가능-여부-구분하기(1)](/assets/img/posts/ps/programmers/sql-problems/자동차-대여-기록에서-대여중-대여-가능-여부-구분하기(1).png)
![자동차-대여-기록에서-대여중-대여-가능-여부-구분하기(2)](/assets/img/posts/ps/programmers/sql-problems/자동차-대여-기록에서-대여중-대여-가능-여부-구분하기(2).png)

```sql
SELECT car_id,
       MAX(CASE WHEN TO_DATE('2022-10-16', 'YYYY-MM-DD') BETWEEN start_date
                                                                 AND
                                                                 end_date THEN '대여중'
           ELSE '대여 가능'
            END) AS availabilty
  FROM car_rental_company_rental_history
 GROUP BY car_id
 ORDER BY car_id DESC;
```

오라클에서 한글 문자열을 `MAX()` 함수로 비교할 때, 유니코드의 사전순으로 결정되는데 이때 문자열 `대여중`이 `대여 가능`보다 더 큰 값을 가지게 된다.

따라서 그룹화된 `car_id`에 대해 문자열 `대여중`과 `대여 가능`이 중첩된 경우, `MAX()` 함수는 문제의 요구대로 문자열 `대여중`을 반환한다.

## [헤비 유저가 소유한 장소](https://school.programmers.co.kr/learn/courses/30/lessons/77487){: target="_blank" }

![헤비-유저가-소유한-장소(1)](/assets/img/posts/ps/programmers/sql-problems/헤비-유저가-소유한-장소(1).png)
![헤비-유저가-소유한-장소(2)](/assets/img/posts/ps/programmers/sql-problems/헤비-유저가-소유한-장소(2).png)

```sql
SELECT id, name, host_id
  FROM places
 WHERE host_id IN (SELECT host_id
                    FROM places
                   GROUP BY host_id
                  HAVING count(host_id) >= 2)
 ORDER BY id ASC;
```

## [조회수가 가장 많은 중고거래 게시판의 첨부파일 조회하기](https://school.programmers.co.kr/learn/courses/30/lessons/164671){: target="_blank" }

![조회수가-가장-많은-중고거래-게시판의-첨부파일-조회하기(1)](/assets/img/posts/ps/programmers/sql-problems/조회수가-가장-많은-중고거래-게시판의-첨부파일-조회하기(1).png)
![조회수가-가장-많은-중고거래-게시판의-첨부파일-조회하기(2)](/assets/img/posts/ps/programmers/sql-problems/조회수가-가장-많은-중고거래-게시판의-첨부파일-조회하기(2).png)

```sql
SELECT '/home/grep/src/' ||
        board_id ||
        '/' ||
        file_id ||
        file_name ||
        file_ext AS file_path
  FROM used_goods_file
 WHERE board_id = (SELECT board_id
                      FROM (SELECT board_id, views
                              FROM used_goods_board
                             ORDER BY views DESC
                             FETCH FIRST 1 ROWS ONLY))
 ORDER BY file_id DESC;
```
```sql
SELECT '/home/grep/src/' ||
        board_id ||
        '/' ||
        file_id ||
        file_name ||
        file_ext AS file_path
  FROM used_goods_file
 WHERE board_id = (SELECT board_id
                     FROM (SELECT board_id, RANK() OVER(ORDER BY views DESC) AS rank
                              FROM used_goods_board)
                   WHERE rank = 1)
 ORDER BY file_id DESC;
```

## [대여 횟수가 많은 자동차들의 월별 대여 횟수 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/151139){: target="_blank" }

![대여-횟수가-많은-자동차들의-월별-대여-횟수-구하기(1)](/assets/img/posts/ps/programmers/sql-problems/대여-횟수가-많은-자동차들의-월별-대여-횟수-구하기(1).png)
![대여-횟수가-많은-자동차들의-월별-대여-횟수-구하기(2)](/assets/img/posts/ps/programmers/sql-problems/대여-횟수가-많은-자동차들의-월별-대여-횟수-구하기(2).png)

```sql
SELECT EXTRACT(MONTH FROM start_date) AS month,
       car_id,
       COUNT(*) AS records
  FROM car_rental_company_rental_history
 WHERE car_id IN (SELECT car_id
                    FROM car_rental_company_rental_history
                   WHERE start_date BETWEEN TO_DATE('2022-08-01', 'YYYY-MM-DD') AND TO_DATE('2022-10-31', 'YYYY-MM-DD')
                   GROUP BY car_id
                  HAVING COUNT(*) >= 5)
       AND start_date BETWEEN TO_DATE('2022-08-01', 'YYYY-MM-DD') AND TO_DATE('2022-10-31', 'YYYY-MM-DD')
 GROUP BY EXTRACT(MONTH FROM start_date), car_id
 ORDER BY EXTRACT(MONTH FROM start_date), car_id DESC;
```