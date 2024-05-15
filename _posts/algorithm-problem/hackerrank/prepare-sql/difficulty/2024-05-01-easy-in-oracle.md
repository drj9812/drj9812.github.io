---
title: "[HackerRank | Prepare SQL]Easy Difficulty(Oracle)"
categories: [Algorithm Problem, HackerRank]
tags: [Algorithm, 알고리즘, HackerRank, Prepare SQL, Oracle, 오라클, Easy]
image:
  path: /assets/img/posts/algorithm-problem/hackerrank/01-hackerrank-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: HackerRank
---

# Easy Difficulty(Oracle)

## [Revising the Select Query I](https://www.hackerrank.com/challenges/revising-the-select-query/problem?isFullScreen=true){: target="_blank" }

![01-revising-the-select-query-I](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/01-revising-the-select-query-I.jpg)

```sql
SELECT *
  FROM city
 WHERE countrycode = 'USA' AND population > 100000;
```

## [Revising the Select Query II](https://www.hackerrank.com/challenges/revising-the-select-query-2/problem?isFullScreen=true){: target="_blank" }

![02-revising-the-select-query-II](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/02-revising-the-select-query-II.jpg)

```sql
SELECT name
  FROM city
 WHERE countrycode = 'USA' AND population > 120000;
```

## [Select All](https://www.hackerrank.com/challenges/select-all-sql/problem?isFullScreen=true){: target="_blank" }

![03-select-all](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/03-select-all.jpg)

```sql
SELECT *
  FROM city;
```

## [Select By Id](https://www.hackerrank.com/challenges/select-by-id/problem?isFullScreen=true){: target="_blank" }

![04-select-by-id](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/04-select-by-id.jpg)

```sql
SELECT *
  FROM city
 WHERE id = 1661;
```

## [Japanese Cities' Attributes](https://www.hackerrank.com/challenges/japanese-cities-attributes/problem?isFullScreen=true){: target="_blank" }

![05-japanese-cities-attributes](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/05-japanese-cities-attributes.jpg)

```sql
SELECT *
  FROM city
 WHERE countrycode = 'JPN';
```

## [Japanese Cities' Names](https://www.hackerrank.com/challenges/japanese-cities-name/problem?isFullScreen=true){: target="_blank" }

![06-japanese-cities-name](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/06-japanese-cities-name.jpg)

```sql
SELECT name
  FROM city
 WHERE countrycode = 'JPN';
```

## [Weather Observation Station 1](https://www.hackerrank.com/challenges/weather-observation-station-1/problem?isFullScreen=true){: target="_blank" }

![07-weather-observation-station-1](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/07-weather-observation-station-1.jpg)

```sql
SELECT city, state
  FROM station;
```

## [Weather Observation Station 3](https://www.hackerrank.com/challenges/weather-observation-station-3/problem?isFullScreen=true){: target="_blank" }

![08-weather-observation-station-3](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/08-weather-observation-station-3.jpg)

```sql
SELECT DISTINCT(city)
  FROM station
 WHERE MOD(id, 2) = 0;
```

## [Weather Observation Station 4](https://www.hackerrank.com/challenges/weather-observation-station-4/problem?isFullScreen=true){: target="_blank" }

![09-weather-observation-station-4](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/09-weather-observation-station-4.jpg)

```sql
SELECT COUNT(city) - COUNT(DISTINCT(city))
  FROM station;
```

> record, row, entry는 모두 행을 의미한다.
{: .prompt-info }

## [Weather Observation Station 5](https://www.hackerrank.com/challenges/weather-observation-station-5/problem?isFullScreen=true){: target="_blank" }

![10-weather-observation-station-5(1)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/10-weather-observation-station-5(1).jpg)
![11-weather-observation-station-5(2)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/11-weather-observation-station-5(2).jpg)

```sql
SELECT *
  FROM (SELECT city, LENGTH(city) AS length
          FROM station
         ORDER BY length ASC, city ASC)
 WHERE ROWNUM = 1;
 
SELECT *
  FROM (SELECT city, LENGTH(city) AS length
          FROM station
         ORDER BY length DESC, city ASC)
 WHERE ROWNUM = 1;
```

`ROWNUM()` 함수 대신 `FETCH` 절을 사용하려 했는데 무슨 이유인지 `ORA-00933: SQL command not properly ended` 메시지가 나오면서 오답 처리된다. HackerRank에서 `FETCH` 절을 지원하지 않는 것 같다.

## [Weather Observation Station 6](https://www.hackerrank.com/challenges/weather-observation-station-6/problem?isFullScreen=true){: target="_blank" }

![12-weather-observation-station-6](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/12-weather-observation-station-6.jpg)

```sql
SELECT city
  FROM station
 WHERE REGEXP_LIKE(city, '^[aeiou]', 'i');
```

`REGEXP_LIKE()` 함수는 주어진 문자열에서 특정 패턴을 갖는 경우를 반환해주는 함수다. 두 번째 인자에 정규 표현식을 전달하면 첫 번째 인자에서 전달한 정규 표현식을 만족하는 값들을 반환한다. 세 번째 인자인 문자열 `i`는 대소문자를 구분하지 않겠다는 의미다. 대소문자를 구분하려면 세 번째 인자에 문자열 `c`를 전달하면 된다.

정규 표현식에서 `^`는 문자가 시작한다는 의미다.

## [Weather Observation Station 7](https://www.hackerrank.com/challenges/weather-observation-station-7?isFullScreen=true){: target="_blank" }

![13-weather-observation-station-7](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/13-weather-observation-station-7.jpg)

```sql
SELECT DISTINCT(city)
  FROM station
 WHERE REGEXP_LIKE(city, '[aeiou]$', 'i');
```

정규 표현식에서 `$`는 마지막 글자를 의미한다.

## [Weather Observation Station 8](https://www.hackerrank.com/challenges/weather-observation-station-8/problem?isFullScreen=true){: target="_blank" }

![14-weather-observation-station-8](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/14-weather-observation-station-8.jpg)

```sql
SELECT DISTINCT(city)
  FROM station
 WHERE REGEXP_LIKE(city, '^[aeiou].*[aeiou]$', 'i');
```

정규 표현식에서 `.`는 개행을 제외한 모든 문자를 의미하고, `i*`는 `i`가 0회 이상 반복된다는 의미이기 때문에 `.*`는 "0개 이상의 어떤 문자"를 의미한다. 

따라서 정규 표현식 `^[aeiou].*[aeiou]$`는 "어떤 모음으로 시작하는 문자열, 그리고 어떤 문자들(0개 이상), 그리고 다시 다른 모음으로 끝나는 문자열"이라는 의미가 된다. 

정규 표현식에서 "그리고"를 명시적으로 나타내는 연산자는 없기 때문에, 일반적으로 `.*`를 사용하여 "그리고"를 표현한다.

## [Weather Observation Station 9](https://www.hackerrank.com/challenges/weather-observation-station-9/problem?isFullScreen=true){: target="_blank" }

![15-weather-observation-station-9](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/15-weather-observation-station-9.jpg)

```sql
SELECT DISTINCT(city)
  FROM station
 WHERE REGEXP_LIKE(city, '^[^aeiou]', 'i');
```

정규 표현식에서 `^ab`는 a와 b를 제외한 모든 문자라는 의미다.

## [Weather Observation Station 10](https://www.hackerrank.com/challenges/weather-observation-station-10/problem?isFullScreen=true){: target="_blank" }

![16-weather-observation-station-10](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/16-weather-observation-station-10.jpg)

```sql
SELECT DISTINCT(city)
  FROM station
 WHERE REGEXP_LIKE(city, '[^aeiou]$', 'i');
```

## [Weather Observation Station 11](https://www.hackerrank.com/challenges/weather-observation-station-11/problem?isFullScreen=true){: target="_blank" }

![17-weather-observation-station-11](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/17-weather-observation-station-11.jpg)

```sql
SELECT DISTINCT(city)
  FROM station
 WHERE REGEXP_LIKE(city, '^[^aeiou]|[^aeiou]$', 'i');
```

정규 표현식에서 `i`는 "또는"이라는 의미다.

## [Weather Observation Station 12](https://www.hackerrank.com/challenges/weather-observation-station-12/problem?isFullScreen=true){: target="_blank" }

![18-weather-observation-station-12](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/18-weather-observation-station-12.jpg)

```sql
SELECT DISTINCT(city)
  FROM station
 WHERE REGEXP_LIKE(city, '^[^aeiou].*[^aeiou]$', 'i');
```

## [Higher Than 75 Marks](https://www.hackerrank.com/challenges/more-than-75-marks?isFullScreen=true){: target="_blank" }

![19-more-than-75-marks(1)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/19-more-than-75-marks(1).jpg)
![20-more-than-75-marks(2)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/20-more-than-75-marks(2).jpg)

```sql
SELECT name
  FROM students
 WHERE marks > 75
 ORDER BY SUBSTR(name, -3) ASC, id ASC;
```

`SUBSTR()` 함수는 문자열을 추출하는데, 두 번째 인자로 시작 위치를 전달받고, 세 번째 인자로 추출할 문자열의 길이를 전달받는다.

두 번째 인자로 -3을 넘겨주고, 세 번째 인자를 생략했으니 대상 문자열의 마지막부터 3개의 문자열을 추출하게 된다.

## [Employee Names](https://www.hackerrank.com/challenges/name-of-employees/problem?isFullScreen=true){: target="_blank" }

![21-name-of-employees(1)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/21-name-of-employees(1).jpg)
![22-name-of-employees(2)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/22-name-of-employees(2).jpg)

```sql
SELECT name
  FROM employee
 ORDER BY name ASC;
```

## [Employee Salaries](https://www.hackerrank.com/challenges/salary-of-employees/problem?isFullScreen=true){: target="_blank" }

![23-salary-of-emplyees(1)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/23-salary-of-emplyees(1).jpg)
![24-salary-of-emplyees(2)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/24-salary-of-emplyees(2).jpg)
![25-salary-of-emplyees(3)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/25-salary-of-emplyees(3).jpg)

```sql
SELECT name
  FROM employee
 WHERE salary > 2000 AND months < 10
 ORDER BY employee_id ASC;
```

## [Type of Triangle](https://www.hackerrank.com/challenges/what-type-of-triangle/problem?isFullScreen=true){: target="_blank" }

![26-what-type-of-triangle(1)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/26-what-type-of-triangle(1).jpg)
![27-what-type-of-triangle(2)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/27-what-type-of-triangle(2).jpg)
![28-what-type-of-triangle(3)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/28-what-type-of-triangle(3).jpg)

```sql
SELECT CASE WHEN a + b <= c OR a + c <= b OR b + c <= a THEN 'Not A Triangle'
            WHEN a = b AND b = c THEN 'Equilateral'
            WHEN a = b AND b != c OR a = c AND c != b OR b = c AND c != a THEN 'Isosceles'
            ELSE 'Scalene'
        END AS type
  FROM triangles;
```

## [Revising Aggregations - The Count Function](https://www.hackerrank.com/challenges/revising-aggregations-the-count-function/problem?isFullScreen=true){: target="_blank" }

![29-revising-aggregations-the-count-function](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/29-revising-aggregations-the-count-function.jpg)

```sql
SELECT COUNT(id)
  FROM city
 WHERE population > 100000;
```

## [Revising Aggregations - The Sum Function](https://www.hackerrank.com/challenges/revising-aggregations-sum/problem?isFullScreen=true){: target="_blank" }

![30-revising-aggregations-sum](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/30-revising-aggregations-sum.jpg)

```sql
SELECT SUM(population)
  FROM city
 WHERE district = 'California';
```

## [Revising Aggregations - Averages](https://www.hackerrank.com/challenges/revising-aggregations-the-average-function/problem?isFullScreen=true){: target="_blank" }

![31-revising-aggregations-the-average-function](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/31-revising-aggregations-the-average-function.jpg)

```sql
SELECT AVG(population)
  FROM city
 WHERE district = 'California';
```

## [Average Population](https://www.hackerrank.com/challenges/average-population/problem?isFullScreen=true){: target="_blank" }

![32-average-population](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/32-average-population.jpg)

```sql
SELECT FLOOR(AVG(population))
  FROM city;
```

> round off(또는 round)는 반올림, round up은 올림, round down은 내림이라는 뜻이다.
{: .prompt-info }

## [Japan Population](https://www.hackerrank.com/challenges/japan-population/problem?isFullScreen=true){: target="_blank" }

![33-japan-population](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/33-japan-population.jpg)

```sql
SELECT SUM(population)
  FROM city
 WHERE countrycode = 'JPN';
```

## [Population Density Difference](https://www.hackerrank.com/challenges/population-density-difference?isFullScreen=true){: target="_blank" }

![34-population-density-difference](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/34-population-density-difference.jpg)

```sql
SELECT MAX(population) - MIN(population)
  FROM city;
```

## [The Blunder](https://www.hackerrank.com/challenges/the-blunder?isFullScreen=true){: target="_blank" }

![35-the-blunder(1)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/35-the-blunder(1).jpg)
![36-the-blunder(2)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/36-the-blunder(2).jpg)
![37-the-blunder(3)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/37-the-blunder(3).jpg)

```sql
SELECT CEIL(AVG(salary) - AVG(TO_NUMBER(REPLACE(TO_CHAR(salary), '0', ''))))
  FROM employees;
```

대부분의 DB에서 문자열과 숫자 간의 산술 연산을 수행할 때 자동으로 형변환을 수행하기 때문에 굳이 형변환을 할 필요는 없다.

## [Top Earners](https://www.hackerrank.com/challenges/earnings-of-employees?isFullScreen=true){: target="_blank" }

![38-earnings-of-employees(1)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/38-earnings-of-employees(1).jpg)
![39-earnings-of-employees(2)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/39-earnings-of-employees(2).jpg)
![40-earnings-of-employees(3)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/40-earnings-of-employees(3).jpg)

```sql
SELECT MAX(salary * MONTHS) || ' ' || COUNT(employee_id)
  FROM employee
 WHERE months * salary = (SELECT MAX(salary * months)
                            FROM employee);
```

## [Weather Observation Station 2](https://www.hackerrank.com/challenges/weather-observation-station-2/problem?isFullScreen=true){: target="_blank" }

![41-weather-observation-station-2](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/41-weather-observation-station-2.jpg)

```sql
SELECT ROUND(SUM(lat_n), 2), ROUND(SUM(long_w), 2)
  FROM station;
```

## [Weather Observation Station 13](https://www.hackerrank.com/challenges/weather-observation-station-13/problem?isFullScreen=true){: target="_blank" }

![42-weather-observation-station-13](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/42-weather-observation-station-13.jpg)

```sql
SELECT TRUNC(SUM(lat_n), 4)
  FROM station
 WHERE lat_n > 38.7880 AND lat_n < 137.2345;
```

## [Weather Observation Station 14](https://www.hackerrank.com/challenges/weather-observation-station-14/problem?isFullScreen=true){: target="_blank" }

![43-weather-observation-station-14](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/43-weather-observation-station-14.jpg)

```sql
SELECT TRUNC(MAX(lat_n), 4)
  FROM station
 WHERE lat_n < 137.2345;
```

## [Weather Observation Station 15](https://www.hackerrank.com/challenges/weather-observation-station-15/problem?isFullScreen=true){: target="_blank" }

![44-weather-observation-station-15](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/44-weather-observation-station-15.jpg)

```sql
SELECT ROUND(long_w, 4)
  FROM station
 WHERE lat_n = (SELECT MAX(lat_n)
                  FROM station
                 WHERE lat_n < 137.2345);
```

## [Weather Observation Station 16](https://www.hackerrank.com/challenges/weather-observation-station-16/problem?isFullScreen=true){: target="_blank" }

![45-weather-observation-station-16](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/45-weather-observation-station-16.jpg)

```sql
SELECT ROUND(MIN(lat_n), 4)
  FROM station
 WHERE lat_n > 38.7780;
```

## [Weather Observation Station 17](https://www.hackerrank.com/challenges/weather-observation-station-17/problem?isFullScreen=true){: target="_blank" }

![46-weather-observation-station-17](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/46-weather-observation-station-17.jpg)

```sql
SELECT ROUND(long_w, 4)
  FROM station
 WHERE lat_n = (SELECT MIN(lat_n)
                  FROM station
                 WHERE lat_n > 38.7780);
```

## [Population Census](https://www.hackerrank.com/challenges/asian-population/problem?isFullScreen=true){: target="_blank" }

![47-asian-population(1)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/47-asian-population(1).jpg)
![48-asian-population(2)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/48-asian-population(2).jpg)

```sql
SELECT SUM(city.population)
  FROM city city
 INNER JOIN country country
    ON city.countrycode = country.code
 WHERE country.continent = 'Asia';
```

## [African Cities](https://www.hackerrank.com/challenges/african-cities/problem?isFullScreen=true){: target="_blank" }

![49-african-cities(1)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/49-african-cities(1).jpg)
![50-african-cities(2)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/50-african-cities(2).jpg)

```sql
SELECT city.name
  FROM city city
 INNER JOIN country country
    ON city.countrycode = country.code
 WHERE country.continent = 'Africa';
```

## [Average Population of Each Continent](https://www.hackerrank.com/challenges/average-population-of-each-continent/problem?isFullScreen=true){: target="_blank" }

![51-average-population-of-each-continent(1)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/51-average-population-of-each-continent(1).jpg)
![52-average-population-of-each-continent(2)](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/52-average-population-of-each-continent(2).jpg)

```sql
SELECT country.continent, FLOOR(AVG(city.population))
  FROM city city
 INNER JOIN country country
    ON city.countrycode = country.code
 GROUP BY country.continent;
```

## [Draw The Triangle 2](https://www.hackerrank.com/challenges/draw-the-triangle-1/problem?isFullScreen=true){: target="_blank" }

![53-draw-the-triangle-1](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/53-draw-the-triangle-1.jpg)

```sql
 SELECT LPAD('* ', (21 - LEVEL) * 2, '* ')
   FROM dual
CONNECT BY LEVEL <= 20;
```

## [Draw The Triangle 2](https://www.hackerrank.com/challenges/draw-the-triangle-2/problem?isFullScreen=true){: target="_blank" }

![54-draw-the-triangle-2](/assets/img/posts/algorithm-problem/hackerrank/prepare-sql/difficulty/easy/54-draw-the-triangle-2.jpg)

```sql
 SELECT LPAD('* ', LEVEL * 2, '* ')
   FROM dual
CONNECT BY LEVEL <= 20;
```
