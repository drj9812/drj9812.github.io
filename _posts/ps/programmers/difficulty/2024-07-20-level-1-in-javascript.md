---
title: "[프로그래머스]레벨 1 난이도 모든 문제(JavaScript)"
categories: [PS, Programmers]
tags: [PS, Algorithm, 알고리즘, Programmers, 프로그래머스, JavaScript, 레벨 1]
image:
  path: /assets/img/posts/ps/programmers/01-programmers-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: programmers
---

# 레벨 1 난이도 모든 문제(JavaScript)

- 2024-07-20 정답률 기준

## [몫 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/120805){: target="_blank" }

```javascript
const solution = (num1, num2) => Math.floor(num1 / num2);
```

## [두 수의 차](https://school.programmers.co.kr/learn/courses/30/lessons/120807){: target="_blank" }

```javascript
const solution = (num1, num2) => num1 - num2;
```

## [숫자 비교하기](https://school.programmers.co.kr/learn/courses/30/lessons/120807){: target="_blank" }

```javascript
const solution = (num1, num2) => num1 === num2 ? 1 : -1
```
## [나이 출력](https://school.programmers.co.kr/learn/courses/30/lessons/120820){: target="_blank" }

```javascript
const solution = (age) => 2022 - age + 1;
```

## [나머지 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/120810){: target="_blank" }

```javascript
const solution = (num1, num2) => num1 % num2;
```

## [두 수의 곱](https://school.programmers.co.kr/learn/courses/30/lessons/120804){: target="_blank" }

```javascript
const solution = (num1, num2) => num1 * num2;
```

## [두 수의 나눗셈](https://school.programmers.co.kr/learn/courses/30/lessons/120806){: target="_blank" }


```javascript
const solution = (num1, num2) => Math.trunc((num1 / num2) * 1000);
```
## [배열의 평균값](https://school.programmers.co.kr/learn/courses/30/lessons/120817){: target="_blank" }


```javascript
const solution = (numbers) => {
  const sum = numbers.reduce((prev, curr) => prev + curr, 0);

  return sum / numbers.length;
}
```

## [각도기](https://school.programmers.co.kr/learn/courses/30/lessons/120829){: target="_blank" }

```javascript
const solution = (angle) => {
  if (angle > 90 && angle < 180) {
    return 3;

  } else if (angle == 90) {
    return 2;  

  } else if (angle > 0 && angle < 90) {
    return 1;

  } else {
    return 4;
  }
}
```
## [짝수의 합](https://school.programmers.co.kr/learn/courses/30/lessons/120831){: target="_blank" }


```javascript
const solution = (n) => {
  let sum = 0;

  for (let i = n; i > 0; i--) {
    if (i % 2 == 0) {
      sum += i;
    }
  }

  return sum;
}
```

## [두 수의 합](https://school.programmers.co.kr/learn/courses/30/lessons/120802){: target="_blank" }


```javascript
const solution = (num1, num2) => num1 + num2;
```

## [중복된 숫자 개수](https://school.programmers.co.kr/learn/courses/30/lessons/120583){: target="_blank" }

```javascript
const solution = (array, n) => {
  const result = array.filter((value) => value === n);

  return result.length;
}
```

## [머쓱이보다 키 큰 사람](https://school.programmers.co.kr/learn/courses/30/lessons/120585){: target="_blank" }

```javascript
const solution = (array, height) => {
  return array.reduce((count, value) => value > height ? count +1 : +0, 0);   
}
```

## [배열 두 배 만들기](https://school.programmers.co.kr/learn/courses/30/lessons/120809){: target="_blank" }

```javascript
const solution = (numbers) => numbers.map((number) => number * 2);
```

## [중앙값 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/120811){: target="_blank" }

```javascript
const solution = (array) => array.sort((a, b) => a - b)[Math.trunc(array.length / 2)];
```

## [짝수는 싫어요](https://school.programmers.co.kr/learn/courses/30/lessons/120813){: target="_blank" }

```javascript
const solution = (n) => {
  const result = [];

  for (let i = 0; i <= n; i++) {
    if (i % 2 != 0) {
      result.push(i);
    }
  }

  return result;
}
```

## [피자 나눠 먹기 (1)](https://school.programmers.co.kr/learn/courses/30/lessons/120814){: target="_blank" }

```javascript
const solution = (n) => Math.ceil(n / 7);
```

## [옷가게 할인 받기](https://school.programmers.co.kr/learn/courses/30/lessons/120818){: target="_blank" }

```javascript
const solution = (price) => {
  if (price >= 500000) {
    return Math.trunc(price - price * 0.2);

  } else if (price >= 300000) {
    return Math.trunc(price - price * 0.1);

  } else if (price >= 100000) {
    return Math.trunc(price - price * 0.05);

  } else {
    return price;
  }
}
```

## [아이스 아메리카노](https://school.programmers.co.kr/learn/courses/30/lessons/120819){: target="_blank" }

```javascript
const solution = (money) => {
  const result = [];
  const maxCoffee = Math.trunc(money / 5500);
    
  result.push(maxCoffee);
  result.push(money - maxCoffee * 5500);
    
  return result;
}
```

## [배열 자르기](https://school.programmers.co.kr/learn/courses/30/lessons/120833){: target="_blank" }

```javascript
const solution = (numbers, num1, num2) => numbers.slice(num1, num2 + 1);
```