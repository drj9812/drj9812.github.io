---
title: "[프로그래머스/오라클(ORACLE)]레벨5 모든 문제"
categories: [알고리즘, 프로그래머스]
tags: [알고리즘, 프로그래머스, sql, 오라클, ORACLE, 레벨5]
---

# [프로그래머스/오라클(ORACLE)]레벨5 모든 문제

2024-01-05 기준 정답률 순입니다.

## 상품을 구매한 회원 비율 구하기(<https://school.programmers.co.kr/learn/courses/30/lessons/131534>{: target="_blank" })

![보호소에서 중성화한 동물(1)](/assets/img/posts/algorithm/sql/programmers-sql-level-5-by-oracle/상품을-구매한-회원-비율-구하기(1).png)
![보호소에서 중성화한 동물(2)](/assets/img/posts/algorithm/sql/programmers-sql-level-5-by-oracle/상품을-구매한-회원-비율-구하기(2).png)
![보호소에서 중성화한 동물(3)](/assets/img/posts/algorithm/sql/programmers-sql-level-5-by-oracle/상품을-구매한-회원-비율-구하기(3).png)

```sql
SELECT EXTRACT(YEAR FROM sales_date) AS year,
       EXTRACT(MONTH FROM sales_date) AS month,
       COUNT(DISTINCT user_id) AS puchased_users,
       ROUND(COUNT(DISTINCT user_id) / (SELECT COUNT(user_id)
                                            FROM user_info
                                           WHERE EXTRACT(YEAR FROM joined) = 2021), 1) AS puchased_ratio
  FROM (SELECT i.user_id, s.product_id, s.sales_amount, s.sales_date
          FROM user_info i
         INNER JOIN online_sale s
            ON i.user_id = s.user_id
         WHERE EXTRACT(YEAR FROM i.joined) = 2021)
 GROUP BY EXTRACT(YEAR FROM sales_date),
          EXTRACT(MONTH FROM sales_date)
 ORDER BY year ASC, month ASC;
```