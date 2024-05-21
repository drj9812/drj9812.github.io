---
title: "[HackerRank | Prepare SQL]Medium Difficulty(Oracle)"
categories: [Algorithm Problem, HackerRank]
tags: [Algorithm, 알고리즘, HackerRank, Prepare SQL, Oracle, 오라클, Medium]
math: true
image:
  path: /assets/img/posts/algorithm-problem/hackerrank/01-hackerrank-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: HackerRank
---

# Medium Difficulty(Oracle)

## [The PADS](https://www.hackerrank.com/challenges/the-pads/problem?isFullScreen=true){: target="_blank" }

![01-the-pads(1)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/01-the-pads(1).jpg)
![02-the-pads(2)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/02-the-pads(2).jpg)
![03-the-pads(3)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/03-the-pads(3).jpg)

```sql
SELECT name || '(' || SUBSTR(occupation, 1,1) || ')'
  FROM occupations
 ORDER BY name ASC;
 
SELECT 'There are a total of ' || TO_CHAR(COUNT(occupation)) || ' ' || LOWER(occupation) || 's.'
  FROM occupations
 GROUP BY occupation
 ORDER BY COUNT(occupation) ASC, LOWER(occupation) ASC;
```

## [Occupations](https://www.hackerrank.com/challenges/occupations/problem?isFullScreen=true){: target="_blank" }

![04-occupations(1)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/04-occupations(1).jpg)
![05-occupations(2)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/05-occupations(2).jpg)
![06-occupations(3)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/06-occupations(3).jpg)

```sql
SELECT doctor, professor, singer, actor
  FROM (SELECT name,
               occupation,
               RANK() OVER(PARTITION BY occupation ORDER BY name) AS rank
          FROM occupations)
 PIVOT (MAX(name) FOR occupation IN ('Doctor' AS doctor, 'Professor' AS professor, 'Singer' AS singer, 'Actor' AS actor))
 ORDER BY rank ASC;
```

![07-occupations-query-structure](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/07-occupations-query-structure.jpg)
*쿼리 구조*

`PIVOT()` 함수의 문법은 `PIVOT(Value컬럼명 FOR Unstack컬럼명 IN (값1, 값2, ... 값n)` 이다. 이때 Value 컬럼과 Unstack 컬럼은 `FROM` 절에 명시가 되어있어야 하고, Value 컬럼과 Unstack 컬럼으로 선택되지 않은 `FROM` 절의 모든 컬럼이 Stack 컬럼이 된다.

```sql
SELECT doctor, professor, singer, actor
  FROM occupations
 PIVOT (MAX(name) FOR occupation IN ('Doctor' AS doctor, 'Professor' AS professor, 'Singer' AS singer, 'Actor' AS actor));
```

만약 위와 같이 `FROM` 절에서 서브쿼리(인라인 뷰)를 사용하지 않고 컬럼이 두개(`name`, `occupation`)밖에 존재하지 않는 `occupations` 테이블을 그대로 사용한다면, Value 컬럼과 Unstack 컬럼으로 선택되지 않은 나머지 컬럼이 존재하지 않기 때문에 Stack 컬럼이 없게 된다.

| doctor | professor |  singer  |  actor   |
|:------:|:---------:|:--------:|:--------:|
|  Priya |  Priyanka | Kristeen | Samantha |

즉, `FROM` 절에서 서브쿼리를 사용하지 않은 위 쿼리의 출력은 위와 같게 된다.

> `PIVOT()` 함수 내부에서 사용한 `MAX()` 함수 대신 다른 집계 함수를 사용해도 상관없다.
{: .prompt-info }

## [Binary Tree Nodes](https://www.hackerrank.com/challenges/binary-search-tree-1?isFullScreen=true){: target="_blank" }

![08-binary-search-tree-1(1)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/08-binary-search-tree-1(1).jpg)
![09-binary-search-tree-1(2)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/09-binary-search-tree-1(2).jpg)
![10-binary-search-tree-1(3)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/10-binary-search-tree-1(3).jpg)

```sql
SELECT n, CASE WHEN p IS NULL THEN 'Root'
               WHEN n IN (SELECT p FROM bst) THEN 'Inner'
               ELSE 'Leaf'
           END AS node_type
  FROM bst
 ORDER BY n ASC;
```

위 쿼리는 스칼라 서브쿼리를 사용한 풀이다. 직관적이기는 하지만 스칼라 서브쿼리는 성능이 좋지 않다.

```sql
 SELECT n, CASE WHEN CONNECT_BY_ISLEAF = 1 THEN 'Leaf'
                WHEN LEVEL = 1 THEN 'Root'
                ELSE 'Inner'
            END AS node_type
   FROM bst
  START WITH p IS NULL
CONNECT BY PRIOR n = p
  ORDER BY n ASC;
```

위 쿼리는 계층형 질의를 사용한 풀이다. `START WITH` 절의 조건을 만족하는 값은 최상위 계층이 되고, `CONNECT BY PRIOR` 절의 조건에 따라 계층이 나뉜다. `CONNECT_BY_ISLEAF`는 계층형 질의 가상 컬럼으로써 최하위 노드가 존재한다면 1을, 존재하지 않는다면 0을 반환한다.

## [New Companies](https://www.hackerrank.com/challenges/the-company/problem?isFullScreen=true){: target="_blank" }

![11-the-company(1)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/11-the-company(1).jpg)
![12-the-company(2)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/12-the-company(2).jpg)
![13-the-company(3)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/13-the-company(3).jpg)
![14-the-company(4)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/14-the-company(4).jpg)
![15-the-company(5)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/15-the-company(5).jpg)

```sql
SELECT c.company_code,
       c.founder,
       COUNT(DISTINCT(lm.lead_manager_code)),
       COUNT(DISTINCT(sm.senior_manager_code)),
       COUNT(DISTINCT(m.manager_code)),
       COUNT(DISTINCT(e.employee_code))
  FROM company c
 INNER JOIN lead_manager lm
    ON c.company_code = lm.company_code
 INNER JOIN senior_manager sm
    ON lm.lead_manager_code = sm.lead_manager_code
 INNER JOIN manager m
    ON sm.senior_manager_code = m.senior_manager_code
 INNER JOIN employee e
    ON m.manager_code = e.manager_code
 GROUP BY c.company_code, c.founder
 ORDER BY company_code ASC;
```

## [Weather Observation Station 18](https://www.hackerrank.com/challenges/weather-observation-station-18/problem?isFullScreen=true){: target="_blank" }

![16-weather-observation-station-18](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/16-weather-observation-station-18.jpg)

```sql
SELECT ROUND(ABS(MIN(lat_n) - MAX(lat_n)) + ABS(MIN(long_w) - MAX(long_w)), 4)
  FROM station;
```

맨허튼 거리(Manhattan Distance) 공식: $ \left\vert x_1 - x_2 \right\vert + \left\vert y_1 - y_2 \right\vert $

## [Weather Observation Station 19](https://www.hackerrank.com/challenges/weather-observation-station-19/problem?isFullScreen=true){: target="_blank" }

![17-weather-observation-station-19](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/17-weather-observation-station-19.jpg)

```sql
SELECT ROUND(SQRT(POWER(MIN(lat_n) - MAX(lat_n), 2) + POWER(MIN(long_w) - MAX(long_w), 2)), 4)
  FROM station;
```

유클리디안 거리(Euclidean Distance) 공식: $ \sqrt{(x_2 - x_1)^2 + (y_2 - y_2)^2} $

## [Weather Observation Station 20](https://www.hackerrank.com/challenges/weather-observation-station-20/problem?isFullScreen=true){: target="_blank" }

![18-weather-observation-station-20](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/18-weather-observation-station-20.jpg)

```sql
SELECT ROUND(MEDIAN(lat_n) , 4)
  FROM station;
```

## [The Report](https://www.hackerrank.com/challenges/the-report/problem?isFullScreen=true){: target="_blank" }

![19-the-report(1)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/19-the-report(1).jpg)
![20-the-report(2)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/20-the-report(2).jpg)
![21-the-report(3)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/21-the-report(3).jpg)

```sql
SELECT CASE WHEN GRADE < 8 THEN 'NULL'
            ELSE s.name
        END AS name,
       g.grade, s.marks
  FROM students s
 INNER JOIN grades g
    ON s.marks BETWEEN g.min_mark AND g.max_mark
 ORDER BY g.grade DESC, s.name ASC;
```

## [Top Competitors](https://www.hackerrank.com/challenges/full-score/problem?isFullScreen=true){: target="_blank" }

![22-full-score(1)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/22-full-score(1).jpg)
![23-full-score(2)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/23-full-score(2).jpg)
![24-full-score(3)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/24-full-score(3).jpg)
![25-full-score(4)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/25-full-score(4).jpg)
![26-full-score(5)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/26-full-score(5).jpg)

```sql
 SELECT h.hacker_id, h.name
   FROM challenges c
  INNER JOIN difficulty d
     ON c.difficulty_level = d.difficulty_level
  INNER JOIN submissions s
     ON c.challenge_id = s.challenge_id
  INNER JOIN hackers h
     ON s.hacker_id = h.hacker_id
  WHERE s.score = d.score
  GROUP BY h.hacker_id, h.name
 HAVING COUNT(*) >= 2
  ORDER BY COUNT(*) DESC, h.hacker_id ASC;
```

## [Ollivander's Inventory](https://www.hackerrank.com/challenges/harry-potter-and-wands/problem?isFullScreen=true){: target="_blank" }

![27-harry-potter-and-wands(1)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/27-harry-potter-and-wands(1).jpg)
![28-harry-potter-and-wands(2)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/28-harry-potter-and-wands(2).jpg)
![29-harry-potter-and-wands(3)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/29-harry-potter-and-wands(3).jpg)
![30-harry-potter-and-wands(4)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/30-harry-potter-and-wands(4).jpg)

```sql
SELECT w.id, p.age, w.coins_needed, w.power
  FROM wands w
 INNER JOIN wands_property p
    ON w.code = p.code
 WHERE (w.code, w.power, w.coins_needed) IN (SELECT w.code, w.power, MIN(w.coins_needed) AS min
                                               FROM wands w
                                              INNER JOIN wands_property wp
                                                 ON w.code = wp.code
                                              WHERE wp.is_evil = 0
                                              GROUP BY w.code, w.power)
 ORDER BY w.power DESC, p.age DESC;
```

가장 저렴한(`MIN(coins_needed)`) non-evil wand(`is_evil = 0`)를 구하면 된다.

## [Challenges](https://www.hackerrank.com/challenges/challenges/problem?isFullScreen=true){: target="_blank" }

![31-challenges(1)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/31-challenges(1).jpg)
![32-challenges(2)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/32-challenges(2).jpg)
![33-challenges(3)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/33-challenges(3).jpg)
![34-challenges(4)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/34-challenges(4).jpg)

```sql
WITH t AS (SELECT c.hacker_id, h.name, COUNT(c.challenge_id) AS count
             FROM hackers h
            INNER JOIN challenges c
               ON h.hacker_id = c.hacker_id
            GROUP BY c.hacker_id, h.name)
SELECT *
  FROM t
 WHERE count IN (SELECT count
                   FROM t
                  GROUP BY count
                 HAVING COUNT(count) = 1 OR count = (SELECT MAX(count)
                                                       FROM t))
 ORDER BY count DESC, hacker_id ASC;
```

hacker별 hacker가 생성한 challenge의 총 개수를 구하되, 생성한 총 개수가 동일한 hacker가 있을 경우 어떤 hacker가 생성한 challenge의 총 최대 개수보다 작으면 제외시키고 출력하는 문제다.

`WITH` 절은 SQL 쿼리에서 임시로 데이터를 정의하고 그것을 여러 번 참조할 수 있는 구문이다. 다른 이름으로는 CTE(Common Table Expression)라고도 한다. `WITH` 절은 `VIEW` 객체와 비슷하지만, `VIEW`는 데이터베이스에 저장되는 객체로서 물리적으로 존재한다면 `WITH` 절은 해당 쿼리 내에서만 일시적으로 사용되며, 해당 쿼리의 일부로서 존재한다는 차이점이 있다.

|  hacker_id  |    name    |  count  |
|:-------------:|:------------:|:---------:|
|    69471    |  Michelle  |     50    |
|    37092    |   Jennifer  |      1     |
|    97338    |     Amy    |      1     |
|     5443     |     Paul    |     26    |
|     1632     |  Michael   |     2     |
|    61093    |     Mark    |     1     |
|    84085    |   Johnny   |     29    |
|    33177    |     Jane     |     21    |
|    81506    |  Deborah  |     4      |
|    87524    |   Norma   |     30    |
|       ...       |       ...       |     ...     | 

위 테이블은 `WITH` 절에서 정의한 hacker별 생성한 challenge의 총 개수(`COUNT(count) AS count`)를 출력하는 테이블 `t`다. 

|  count  |  COUNT(count)  |
|:---------:|:-------------------:|
|     1     |         126          |
|    30     |          1            |
|    25     |          1            |
|    22     |          1            |
|    34     |          1            |
|    42     |          1            |
|    29     |          1            |
|     6      |          5            |
|    28     |          1            |
|    26     |          1            |
|     ...     |          ...            |

위 테이블은 메인 쿼리의 `WHERE` 절에서 사용된 서브쿼리를 `HAVING` 절로 필터링하지 않았을 때(`SELECT count, COUNT(count) FROM t GROUP BY count`)의 출력 결과인데, count별 개수를 출력하고 있다. 즉, 위 테이블의 첫 번째 행의 의미는 어떤 hacker가 생성한 challenge의 총 개수가 1개인 경우가 총 126건 있다는 것이다.

따라서 서브쿼리의 `HAVING COUNT(count) = 1`은 어떤 hacker가 생성한 challenge의 총 개수(`COUNT(count) AS count`)가 유일하다는 의미가 되고, `OR` 연산자를 사용해서 이 조건을 만족하지 않는 경우(어떤 hacker가 생성한 challenge의 총 개수가 유일하지 않는 경우), `HAVING count = (SELECT MAX(count) FROM t))` 조건을 만족하도록 하여 생성한 challenge의 총 개수가 최대인 경우만 출력함으로써 문제의 요구(어떤 hacker가 생성한 challenge의 총 최대 개수보다 작으면 제외)를 만족한다.

## [Contest Leaderboard](https://www.hackerrank.com/challenges/contest-leaderboard/problem?isFullScreen=true){: target="_blank" }

![35-contest-leaderboard(1)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/35-contest-leaderboard(1).jpg)
![36-contest-leaderboard(2)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/36-contest-leaderboard(2).jpg)
![37-contest-leaderboard(3)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/37-contest-leaderboard(3).jpg)

```sql
SELECT h.hacker_id, h.name, SUM(sub.max) AS total_score
  FROM hackers h
 INNER JOIN (SELECT h.hacker_id, s.challenge_id, MAX(s.score) AS max
               FROM hackers h
              INNER JOIN submissions s
                 ON h.hacker_id = s.hacker_id
              WHERE s.score != 0
              GROUP BY h.hacker_id, s.challenge_id) sub
    ON h.hacker_id = sub.hacker_id
 GROUP BY h.hacker_id, h.name
 ORDER BY total_score DESC, hacker_id ASC;
```

challenge에 대한 hacker별 최대 `score`의 합을 구하는 문제다. `score`가 0인 경우를 제외한다.

## [SQL Project Planning](https://www.hackerrank.com/challenges/sql-projects/problem?isFullScreen=true){: target="_blank" }

![38-sql-projects(1)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/38-sql-projects(1).jpg)
![39-sql-projects(2)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/39-sql-projects(2).jpg)
![40-sql-projects(3)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/40-sql-projects(3).jpg)

```sql
SELECT MIN(start_date) AS min, MAX(end_date) AS max
  FROM (WITH project_dates AS (SELECT start_date, end_date,
                                      LAG(start_date, 1, start_date) OVER(ORDER BY start_date) AS prev_start_date
                                 FROM projects)
        SELECT start_date, end_date,
               SUM(CASE WHEN start_date - prev_start_date = 1 THEN 0
                        ELSE 1
                    END) OVER(ORDER BY start_date) AS project_id
  FROM project_dates)
 GROUP BY project_id
 ORDER BY max - min ASC, min ASC;
```

![41-sql-projects-query-structure](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/41-sql-projects-query-structure.jpg)
*쿼리 구조*

`start_date`와 `end_date`의 차이가 하루라면 하나의 프로젝트로 보고, 해당 프로젝트가 시작된 날짜와 종료된 날짜를 출력하는 문제다. 이때 프로젝트의 시작된 날짜와 종료된 날짜의 차이를 기준으로 오름차순 정렬하고, 그 일수가 같다면 시작된 날짜를 기준으로 오름차순 정렬한다.

```sql
WITH project_dates AS (SELECT start_date, end_date,
                              LAG(start_date, 1, start_date) OVER(ORDER BY start_date) AS prev_start_date
                         FROM projects)
SELECT start_date, end_date, prev_start_date,
       SUM(CASE WHEN start_date - prev_start_date = 1 THEN 0
                ELSE 1
            END) OVER(ORDER BY start_date) AS project_id
FROM project_dates;
```

|    start_date   |    end_date   |  prev_start_date  |  project_id  |
|:----------------:|:---------------:|:--------------------:|:--------------:|
|  2015-10-01  |  2015-10-02  |    2015-10-01    |        1        |
|  2015-10-02  |  2015-10-03  |    2015-10-01    |        1        |
|  2015-10-03  |  2015-10-04  |    2015-10-02    |        1        |
|  2015-10-04  |  2015-10-05  |    2015-10-03    |        1        |
|  2015-10-11  |  2015-10-12  |    2015-10-04    |        2        |
|  2015-10-12  |  2015-10-13  |    2015-10-11    |        2        |
|  2015-10-15  |  2015-10-16  |    2015-10-12    |        3        |
|  2015-10-17  |  2015-10-18  |    2015-10-15    |        4        |
|  2015-10-19  |  2015-10-20  |    2015-10-17    |        5        |
|  2015-10-21  |  2015-10-22  |    2015-10-19    |        6        |
|  2015-10-25  |  2015-10-26  |    2015-10-21    |        7        |
|  2015-10-26  |  2015-10-27  |    2015-10-25    |        7        |
|  2015-10-27  |  2015-10-28  |    2015-10-26    |        7        |
|  2015-10-28  |  2015-10-29  |    2015-10-27    |        7        |
|  2015-10-29  |  2015-10-30  |    2015-10-28    |        7        |
|  2015-10-30  |  2015-10-31  |    2015-10-29    |        7        |
|  2015-11-01  |  2015-11-02  |    2015-10-30    |        8        |
|  2015-11-04  |  2015-11-05  |    2015-11-01    |        9        |
|  2015-11-05  |  2015-11-06  |    2015-11-04    |        9        |
|  2015-11-06  |  2015-11-07  |    2015-11-05    |        9        |
|  2015-11-07  |  2015-11-08  |    2015-11-06    |        9        |
|  2015-11-11  |  2015-11-12  |    2015-11-07    |       10       |
|  2015-11-12  |  2015-11-13  |    2015-11-11    |       10       |
|  2015-11-17  |  2015-11-18  |    2015-11-12    |       11       |

위 쿼리와 테이블은 서브쿼리(인라인 뷰)와 그 출력 결과다. 설명을 위해 `WITH` 절의 `project_dates` 테이블을 `FROM` 절로 갖는 `SELECT` 절에 `project_dates` 테이블의 `prev_start_date`를 추가했다.

`WITH` 절에서 `LAG()` 함수를 사용하여 이전 행의 `start_date` 값을 함께 출력한다. `LAG()` 함수의 세 번째 인자는 이전 행의 `start_date`가 `NULL` 일 때 전달한 세 번째 인자 값을 반환한다.

이후 프로젝트를 식별하기 위해 `WITH` 절의 `project_dates` 테이블에서 누적합(`project_id`)을 구한다. `SUM()` 함수와 `OVER()` 함수를 함께 사용하면 누적합을 구할 수 있다.

누적합(`project_id`)으로 식별된 프로젝트의 프로젝트가 시작된 날짜와 종료된 날짜를 한 행에 출력해야 하기 때문에 누적합을 구한 출력을 서브쿼리(인라인 뷰)로 두고 누적합(`project_id`)을 그룹핑한다. 이때 주의해야 할 것은 두 번째 정렬 조건에 `min ASC` 대신 `start_date ASC` 을 명시하면 에러가 난다. `GROUP BY` 절을 사용하는 경우 `GROUP BY` 절에 명시된 컬럼이나, 그룹함수에 사용된 컬럼만 명시할 수 있다.

```sql
WITH cte_cnt AS
    (SELECT task_id, start_date, end_date,
            CASE WHEN start_date <> LAG(end_date) OVER(ORDER BY start_date) THEN 1 
                 ELSE 0 
             END AS cnt
            FROM projects),
     cte_grp AS
    (SELECT start_date, end_date, SUM(cnt) OVER(ORDER BY start_date) AS grp
       FROM cte_cnt)
SELECT MIN(start_date) AS start_date, MAX(end_date) AS end_date
  FROM cte_grp
 GROUP BY grp
 ORDER BY end_date - start_date ASC, start_date ASC;
```

위 쿼리처럼 풀 수도 있다.

## [Placements](https://www.hackerrank.com/challenges/placements/problem?isFullScreen=true){: target="_blank" }

![42-placements(1)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/42-placements(1).jpg)
![43-placements(2)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/43-placements(2).jpg)
![44-placements(3)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/44-placements(3).jpg)

```sql
SELECT name
  FROM (SELECT s.name, p.salary AS student_salary, f.friend_id, (SELECT salary
                                                                   FROM packages
                                                                  WHERE id = f.friend_id) AS friend_salary
          FROM students s
 INNER JOIN friends f
    ON s.id = f.id
 INNER JOIN packages p
    ON s.id = p.id)
 WHERE student_salary < friend_salary
 ORDER BY friend_salary ASC;
```

친구의 `salary`가 자신의 `salary`보다 높은 학생을 친구의 `salary`를 기준으로 오름차순 출력하는 문제다.

## [Symmetric Pairs](https://www.hackerrank.com/challenges/symmetric-pairs/problem?isFullScreen=true){: target="_blank" }

![45-symmetric-pairs(1)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/45-symmetric-pairs(1).jpg)
![46-symmetric-pairs(2)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/46-symmetric-pairs(2).jpg)

```sql
SELECT f1.*
  FROM functions f1
 INNER JOIN functions f2
    ON f1.x = f2.y AND f2.x= f1.y
 GROUP BY f1.x, f1.y
HAVING COUNT(*) > 1 OR f1.x < f1.y
 ORDER BY f1.x ASC;
```
두 쌍 $ (X_1, Y_1) $, $ (X_2, Y_2) $이 있을 때, $ X_1 = Y_2 $ and $ X_2 = Y_1 $를 만족하는 대칭 쌍을 $ X $를 기준으로 오름차순 출력하는 문제다. 주의해야 할 점은 대칭쌍은 최소 두 개의 행이 존재해야한다는 것이다. 예를 들어, $ X_1 $와 $ Y_1 $가 각각 1인 행이 한 개만 존재한다면 이 쌍은 대칭 쌍이 아니다.

## [Print Prime Numbers](https://www.hackerrank.com/challenges/print-prime-numbers/problem?isFullScreen=true){: target="_blank" }

![47-print-prime-numbers](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/medium/47-print-prime-numbers.jpg)

```sql
WITH t AS (SELECT  a.num
             FROM (SELECT LEVEL AS num
                     FROM dual
                  CONNECT BY LEVEL <= 1000) a
            CROSS JOIN (SELECT LEVEL AS num
                          FROM dual
                       CONNECT BY LEVEL <= 1000) b
            WHERE a.num != 1 AND b.num != 1 AND MOD(a.num, b.num) = 0
            GROUP BY a.num
           HAVING COUNT(a.num) = 1
            ORDER BY a.num ASC)
SELECT LISTAGG(num, '&') WITHIN GROUP (ORDER BY num) AS PRIME_NUM
  FROM t;
```

앰퍼샌드(&) 기호를 구분자를 사용해서 1000이하의 소수를 일렬로 오름차순 출력하는 문제다.

```sql
SELECT  a.num
  FROM (SELECT LEVEL AS num
          FROM dual
       CONNECT BY LEVEL <= 3) a
 CROSS JOIN (SELECT LEVEL AS num
               FROM dual
            CONNECT BY LEVEL <= 2) b
```

|   a.num   |
|:---------:|
|     1     |
|     1     |
|     2     |
|     2     |
|     3     |
|     3     |

위 쿼리와 테이블은 `WITH` 절의 `t` 테이블을 정의하는 쿼리와 그 출력 결과다. 설명을 위해 인라인 뷰인 `a` 테이블과 `b` 테이블의 `CONNECT BY` 절의 조건을 각각 `LEVEL <= 3`과 `LEVEL <= 2`로 바꾸고, `WHERE` 절 이후의 절을 전부 제거했다. 

- `a.num` = **1**
	+ `b.num` = 1
- `a.num` = **1**
	+ `b.num` = 2
- `a.num` = **2**
	+ `b.num` = 1
- `a.num` = **2**
	+ `b.num` = 2
- `a.num` = **3**
	+ `b.num` = 1
- `a.num` = **3**
	+ `b.num` = 2

1부터 3까지 총 3개의 행을 갖는 `a` 테이블과 1부터 2까지 총 2개의 행을 갖는 `b` 테이블을 `CROSS JOIN`하면 카테시안의 곱(n * m)이 발생하여 총 6(2 * 3)개의 행을 갖는 테이블이 된다. 즉, `a` 테이블의 `a.num`(1 ~ 3)에 `b` 테이블의 `b.num`(1 ~ 2)이 할당되는 형식인데, `WITH` 절에서는 `a.num`만 출력하도록 했으니 위와 같은 출력 결과가 나오게 되는 것이다.

```sql
SELECT  a.num
  FROM (SELECT LEVEL AS num
          FROM dual
       CONNECT BY LEVEL <= 1000) a
 CROSS JOIN (SELECT LEVEL AS num
               FROM dual
            CONNECT BY LEVEL <= 1000) b
 WHERE a.num != 1 AND b.num != 1 AND MOD(a.num, b.num) = 0
 GROUP BY a.num
HAVING COUNT(a.num) = 1
 ORDER BY a.num ASC)
```

다시 원래의 `WITH` 절로 돌아오자면 1은 소수가 아니기 때문에 `WHERE` 절에서 `a.num`과 `b.num`은 1이 아니라는 조건(`a.num != 1 AND b.num != 1`)을 명시함으로써 제외한다. 이때 `a.num`뿐만 아니라 `b.num`도 포함시킨 이유는 `WHERE` 절의 다른 조건인 `MOD(a.num, b.num) = 0`와 `HAVING` 절의 `COUNT(a.num) = 1` 때문이다.

- `a.num` = **2**
	+ ~~`b.num` = 1~~
		* ~~`MOD(2, 1) = 0` 만족 → `a.num`은 2 출력~~
- `a.num` = **2**
	+ `b.num` = 2
		* `MOD(2, 2) = 0` 만족 → `a.num`은 2 출력

이제 `a.num`이 1인 경우는 제외됐기 때문에 2부터 예로 들자면 `MOD(a.num, b.num) = 0` 조건의 첫 번째 인자인 `a.num`에 2가 들어가고 원래대로라면 두 번째 인자로 `b.num`의 첫 번째 값인 1부터 할당되겠지만, `a.num`과 마찬가지로 `b.num`이 1인 경우도 제외했기 때문에 2가 할당된다. 즉, `MOD(2, 2) = 0`를 만족함으로써 결국 `WHERE` 절에 명시된 모든 조건들은 만족하게 되면서 `a.num`은 총 1개의 2를 가지게 된다. 

- `a.num` = **2**
	+ `b.num` = 1
		* `MOD(2, 1) = 0` 만족 → `a.num`은 2 출력
- `a.num` = **2**
	+ `b.num` = 2
		* `MOD(2, 2) = 0` 만족 → `a.num`은 2 출력

만약 `b.num`은 1이 아니라는 조건을 명시하지 않았다면 그전에 `MOD(2, 1) = 0`을 만족하게 되어 총 2개의 2를 가지게 될텐데, 이때 `a.num`을 그룹핑 했기 때문에 `HAVING` 절의 `COUNT(a.num) = 1`을 만족하지 못하게 되어 2는 소수임에도 불구하고 출력되지 않게 되는 것이다.


> `JOIN` 연산자 이후에 hierachical_query_clause(`CONNECT BY` 절)이 와야되기 때문에 `a` 테이블의 `CONNECT BY` 절을 인라인 뷰로 두지 않으면 에러가 발생한다.
{: .prompt-info }