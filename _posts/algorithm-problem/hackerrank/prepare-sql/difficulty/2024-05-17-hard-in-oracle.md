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

- `contests` 테이블
   + `contests` 테이블의 hacker는 contest를 생성한 hacker
   + `name`은 중복 값(동명이인)이 있음
      * 동명이인의 `hacker_id` 유니크함
- `colleges` 테이블
   + `contest_id`는 중복 값이 있음
- `view_stats` 테이블
   + `total_views`: 총 조회수
   + `total_unique_views`: 총 조회수에서 중복 조회수 제거
- `submission_stats` 테이블 
   + `total_submission`: challenge 제출 횟수
   + `total_accepted_submission`: full score를 달성한 제출 횟수


## [15 Days of Learning SQL](https://www.hackerrank.com/challenges/15-days-of-learning-sql/problem?isFullScreen=true){: target="_blank" }

![]()

```sql

```
