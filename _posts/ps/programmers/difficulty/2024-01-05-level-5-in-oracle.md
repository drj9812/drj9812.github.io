---
title: "[프로그래머스 | 모든 문제]레벨 5(Oracle)"
categories: [PS, Programmers]
tags: [PS, Algorithm, 알고리즘, Programmers, 프로그래머스, SQL, Oracle, 오라클, 레벨5]
image:
  path: /assets/img/posts/ps/programmers/01-programmers-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: programmers
---

# 레벨 5(Oracle)

2024-01-05 기준 정답률 순

## [상품을 구매한 회원 비율 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/131534){: target="_blank" }

![상품을-구매한-회원-비율-구하기(1)](/assets/img/posts/ps/programmers/sql-problems/상품을-구매한-회원-비율-구하기(1).png)
![상품을-구매한-회원-비율-구하기(2)](/assets/img/posts/ps/programmers/sql-problems/상품을-구매한-회원-비율-구하기(2).png)
![상품을-구매한-회원-비율-구하기(3)](/assets/img/posts/ps/programmers/sql-problems/상품을-구매한-회원-비율-구하기(3).png)

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