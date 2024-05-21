---
title: "[HackerRank | Prepare SQL]Hard Difficulty(Oracle)"
categories: [Algorithm Problem, HackerRank]
tags: [Algorithm, 알고리즘, HackerRank, Prepare SQL, Oracle, 오라클, Hard]
math: true
image:
  path: /assets/img/posts/algorithm-problem/hackerrank/01-hackerrank-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: HackerRank
---

# Hard Difficulty(Oracle)

## [Interviews](https://www.hackerrank.com/challenges/interviews/problem?isFullScreen=true){: target="_blank" }

![01-interviews(1)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/hard/01-interviews(1).jpg)
![02-interviews(2)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/hard/02-interviews(2).jpg)
![03-interviews(3)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/hard/03-interviews(3).jpg)

```sql
SELECT contests.contest_id, contests.hacker_id, contests.name,
       SUM(ss.total_submissions), SUM(ss.total_accepted_submissions),
       SUM(vs.total_views), SUM(vs.total_unique_views)
  FROM contests
 INNER JOIN colleges
    ON contests.contest_id = colleges.contest_id
 INNER JOIN challenges
    ON colleges.college_id = challenges.college_id
  LEFT JOIN (SELECT challenge_id,
                    SUM(total_views) AS total_views,
                    SUM(total_unique_views) AS total_unique_views
               FROM VIEW_STATS
              GROUP BY challenge_id
             HAVING SUM(total_views) + SUM(total_unique_views) > 0) vs
    ON challenges.challenge_id = vs.challenge_id
  LEFT JOIN (SELECT challenge_id,
                    SUM(total_submissions) AS total_submissions,
                    SUM(total_accepted_submissions) AS total_accepted_submissions
               FROM submission_stats
              GROUP BY challenge_id
             HAVING SUM(total_submissions) + SUM(total_accepted_submissions) > 0) ss
    ON challenges.challenge_id = ss.challenge_id
 GROUP BY contests.contest_id, contests.hacker_id, contests.name
 ORDER BY contests.contest_id;
```

## [15 Days of Learning SQL](https://www.hackerrank.com/challenges/15-days-of-learning-sql/problem?isFullScreen=true){: target="_blank" }

![04-15-days-of-learning-sql(1)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/hard/04-15-days-of-learning-sql(1).jpg)
![05-15-days-of-learning-sql(2)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/hard/05-15-days-of-learning-sql(2).jpg)
![06-15-days-of-learning-sql(3)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/hard/06-15-days-of-learning-sql(3).jpg)

```sql
WITH t1 AS (SELECT contest_duration.sequence_date,
                   lv,
                   hackers_dense_rank.hacker_id,
                   hackers.name,
                   hackers_dense_rank.rnk
              FROM (SELECT TO_DATE('2016-03-01', 'YYYY-MM-DD') + (LEVEL - 1) AS sequence_date,
                           LEVEL AS lv
                      FROM DUAL
                   CONNECT BY TO_DATE('2016-03-01', 'YYYY-MM-DD') + (LEVEL - 1) <= TO_DATE('2016-03-15', 'YYYY-MM-DD'))contest_duration
             INNER JOIN (SELECT submission_date,
                                hacker_id,
                                DENSE_RANK() OVER(PARTITION BY hacker_id ORDER BY submission_date) AS rnk
                           FROM submissions) hackers_dense_rank
                ON contest_duration.sequence_date = hackers_dense_rank.submission_date
             INNER JOIN hackers hackers
                ON hackers_dense_rank.hacker_id = hackers.hacker_id),
     t2 AS (SELECT sequence_date,
                   COUNT(DISTINCT hacker_id) AS every_submissions_cnt
              FROM t1
             WHERE lv = rnk
             GROUP BY sequence_date),
     t3 AS (SELECT sequence_date, 
                   hacker_id, 
                   COUNT(hacker_id) AS cnt
              FROM t1
             GROUP BY sequence_date, hacker_id),
     t4 AS (SELECT t1.sequence_date, 
                   t2.every_submissions_cnt, 
                   t1.hacker_id, 
                   t1.name, 
                   t3.cnt,
                   ROW_NUMBER() OVER (PARTITION BY t1.sequence_date ORDER BY t1.hacker_id ASC) AS rn
              FROM t1
             INNER JOIN t2
                ON t1.sequence_date = t2.sequence_date
             INNER JOIN t3
                ON t1.sequence_date = t3.sequence_date
                   AND
                   t1.hacker_id = t3.hacker_id
             INNER JOIN (SELECT sequence_date, 
                                MAX(cnt) AS max_cnt
                           FROM t3
                          GROUP BY sequence_date) sub
                ON t3.sequence_date = sub.sequence_date
                   AND
                   t3.cnt = sub.max_cnt)
SELECT sequence_date, every_submissions_cnt, hacker_id, name
  FROM t4
 WHERE rn = 1
 ORDER BY sequence_date ASC;
```

날짜별 매일 제출한 hacker 수, 해당 날짜에 가장 많은 제출한 `hacker_id`와 `name`을 출력하는 문제다. 처음에는 contest 기간(2016-03-01 ~ 2016-03-15) 동안 하루도 빠짐없이 제출한 hacker를 구하라는 건 줄 알았는데 그게 아니었다. 2016년 3월 1일에 제출한 hacker가 10명이고, 2016년 3월 2일에 제출한 hacker가 5명일 때 5명 중 3명이 2016년 3월 1일에 제출했다면 3을 출력하라는 의미다.

```sql
SELECT contest_duration.sequence_date,
       lv,
       hackers_dense_rank.hacker_id,
       hackers.name,
       hackers_dense_rank.rnk
  FROM (SELECT TO_DATE('2016-03-01', 'YYYY-MM-DD') + (LEVEL - 1) AS sequence_date,
               LEVEL AS lv
          FROM DUAL
       CONNECT BY TO_DATE('2016-03-01', 'YYYY-MM-DD') + (LEVEL - 1) <= TO_DATE('2016-03-15', 'YYYY-MM-DD')) contest_duration
 INNER JOIN (SELECT submission_date,
                    hacker_id,
                    DENSE_RANK() OVER(PARTITION BY hacker_id ORDER BY submission_date) AS rnk
               FROM submissions) hackers_dense_rank
    ON contest_duration.sequence_date = hackers_dense_rank.submission_date
 INNER JOIN hackers hackers
    ON hackers_dense_rank.hacker_id = hackers.hacker_id;
```

| sequence_date | lv | hacker_id |   name   | rnk |
|:-------------:|:--:|:---------:|:--------:|:---:|
|  2016-03-05   |  5 |     79    |   Rose   |  1  |
|  2016-03-01   |  1 |    433    |  Angela  |  1  |
|  2016-03-09   |  9 |    433    |  Angela  |  2  |
|  2016-03-15   | 15 |    433    |  Angela  |  3  |
|  2016-03-09   |  9 |    463    |   Frank  |  1  |
|  2016-03-13   | 13 |    463    |   Frank  |  2  |
|  2016-03-14   | 14 |    463    |   Frank  |  3  |
|  2016-03-02   |  2 |    533    |  Patrick |  1  |
|  2016-03-04   |  4 |    533    |  Patrick |  2  |
|  2016-03-04   |  4 |    533    |  Patrick |  2  |
|      ...      | ...|    ...    |    ...   | ... |    

위 쿼리와 테이블은 `WITH` 절의 `t1` 테이블과 그 출력 결과다. 인라인 뷰는 대회 기간이다. `START WITH submission_date = (SELECT MIN(submission_date) FROM submissions CONNECT BY PRIOR submission_date = submission_date - 1`와 같이 계층형 질의를 사용해서 `submissions` 테이블의 `submission_date` 컬럼을 이용할 수도 있었지만, 어떤 hacker도 제출하지 않은 날짜가 있을 수 있기 때문에 사용하지 않았다.

```sql
SELECT submission_date,
       hacker_id,
       DENSE_RANK() OVER(PARTITION BY hacker_id ORDER BY submission_date) AS rnk
       FROM submissions;
```

| submission_date | hacker_id | rnk |
|:---------------:|:---------:|:---:|
|    2016-03-05   |     79    |  1  |
|    2016-03-01   |    433    |  1  |
|    2016-03-09   |    433    |  2  |
|    2016-03-15   |    433    |  3  |
|    2016-03-09   |    463    |  1  |
|    2016-03-13   |    463    |  2  |
|    2016-03-14   |    463    |  3  |
|    2016-03-02   |    533    |  1  |
|    2016-03-04   |    533    |  2  |
|    2016-03-04   |    533    |  2  |
|       ...       |    ...    | ... |

위 쿼리와 테이블은 `t1` 테이블의 인라인 뷰와 `INNER JOIN`되는 `hackers_dense_rank` 테이블과 그 출력 결과다. `hacker_id`별로 해당 날짜에 대한 순서를 오름차순으로 순위(`rnk`)가 메겨진다. 모든 날짜에 제출한 hacker의 순위는 인라인 뷰의 `LEVEL` 컬럼과 그 값이 같을 것이다.

```sql
SELECT sequence_date,
       COUNT(DISTINCT hacker_id) AS every_submissions_cnt
  FROM t1
 WHERE lv = rnk
 GROUP BY sequence_date;
```

| sequence_date | every_submissions_cnt |
|:-------------:|:---------------------:|
|  2016-03-12   |           35          |
|  2016-03-10   |           35          |
|  2016-03-05   |           49          |
|  2016-03-09   |           35          |
|  2016-03-15   |           35          |
|  2016-03-04   |           49          |
|  2016-03-14   |           35          |
|  2016-03-13   |           35          |
|  2016-03-08   |           35          |
|  2016-03-11   |           35          |
|  2016-03-01   |          112          |
|  2016-03-03   |           51          |
|  2016-03-06   |           49          |
|  2016-03-07   |           35          |
|  2016-03-02   |           59          |

위 쿼리와 테이블은 `t2` 테이블과 그 출력 결과다. `t2` 테이블은 모든 날짜에 제출한 hacker를 조건으로 해당 날짜에 몇명의 hacker가 제출했는지 출력한다. 어떤 날짜에 한 hacker가 여러 번 제출하는 경우도 있기 때문에 `DISTINCT()` 함수를 사용한다.

```sql
SELECT sequence_date, 
       hacker_id, 
       COUNT(hacker_id) AS cnt
  FROM t1
 GROUP BY sequence_date, hacker_id;
```

위 쿼리는 `t3` 테이블이다. 해당 날짜에 hacker가 제출한 횟수를 출력한다. 해당 날짜에 가장 많이 제출한 hacker를 구하기 위한 테이블이다.

```sql
SELECT t1.sequence_date, 
       t2.every_submissions_cnt, 
       t1.hacker_id, 
       t1.name, 
       t3.cnt,
       ROW_NUMBER() OVER (PARTITION BY t1.sequence_date ORDER BY t1.hacker_id ASC) AS rn
  FROM t1
 INNER JOIN t2
    ON t1.sequence_date = t2.sequence_date
 INNER JOIN t3
    ON t1.sequence_date = t3.sequence_date
       AND
       t1.hacker_id = t3.hacker_id
 INNER JOIN (SELECT sequence_date, 
                    MAX(cnt) AS max_cnt
               FROM t3
              GROUP BY sequence_date) sub
    ON t3.sequence_date = sub.sequence_date
       AND
       t3.cnt = sub.max_cnt;
```

| sequence_date | every_submissions_cnt | hacker_id |  name  | cnt | rn |
|:-------------:|:---------------------:|:---------:|:------:|:---:|:--:|
|   2016-03-01  |          112          |   81314   | Denise |  3  |  1 |
|   2016-03-01  |          112          |   81314   | Denise |  3  |  2 |
|   2016-03-01  |          112          |   81314   | Denise |  3  |  3 |
|   2016-03-02  |           59          |   39091   |  Ruby  |  3  |  1 |
|   2016-03-02  |           59          |   39091   |  Ruby  |  3  |  2 |
|   2016-03-02  |           59          |   39091   |  Ruby  |  3  |  3 |
|   2016-03-02  |           59          |   98981   |  Rose  |  3  |  4 |
|   2016-03-02  |           59          |   98981   |  Rose  |  3  |  5 |
|   2016-03-02  |           59          |   98981   |  Rose  |  3  |  6 |
|      ...      |          ...          |    ...    |  ...   | ... | ...|

위 쿼리와 테이블은 `t4` 테이블과 그 출력 결과다. 매일 제출하면서 해당 날짜의 제출한 횟수가 최대인 hacker가 여러 명 있을 경우, 문제의 요구대로 `hacker_id`가 낮은 hacker를 구하기 위해 `ROW_NUMBER()` 함수를 사용한다.

```sql
WITH t1 AS (...),
     t2 AS (...),
     t3 AS (...),
     t4 AS (...)
SELECT sequence_date, every_submissions_cnt, hacker_id, name
  FROM t4
 WHERE rn = 1
 ORDER BY sequence_date ASC;
 ```

| sequence_date | every_submissions_cnt | hacker_id |   name    |
|:-------------:|:---------------------:|:---------:|:---------:|
|   2016-03-01  |         112           |   81314   |  Denise   |
|   2016-03-02  |          59           |   39091   |   Ruby    |
|   2016-03-03  |          51           |   18105   |    Roy    |
|   2016-03-04  |          49           |     533   |  Patrick  |
|   2016-03-05  |          49           |    7891   | Stephanie |
|   2016-03-06  |          49           |   84307   |   Evelyn  |
|   2016-03-07  |          35           |   80682   |  Deborah  |
|   2016-03-08  |          35           |   10985   |  Timothy  |
|   2016-03-09  |          35           |   31221   |   Susan   |
|   2016-03-10  |          35           |   43192   |   Bobby   |
|   2016-03-11  |          35           |    3178   |  Melissa  |
|   2016-03-12  |          35           |   54967   |  Kenneth  |
|   2016-03-13  |          35           |   30061   |   Julia   |
|   2016-03-14  |          35           |   32353   |    Rose   |
|   2016-03-15  |          35           |   27789   |   Helen   |

위 쿼리와 테이블은 `t4` 테이블에서 문제의 요구를 최종적으로 출력하는 정답 쿼리와 출력 결과다. 첫 날인 2016년 3월 1일에는 총 112명이 제출했지만, 그중 마지막 날인 2016년 3월 15일까지 제출한 hacker의 수는 총 35명임을 확인할 수 있다.