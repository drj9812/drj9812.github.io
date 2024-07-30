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

- 2024-07-22 정답률 기준

## [문자열 내 p와 y의 개수](https://school.programmers.co.kr/learn/courses/30/lessons/12916){: target="_blank" }

```javascript
const solution = (s) => {
  const array = s.split('').map((str) => str.toUpperCase());

  let countP = 0;
  let countY = 0;
    
  array.forEach((e) => {
    if (e === 'P') {
      countP++;
    } else if (e === 'Y') {
      countY++;
    }
  })

  if (countP === 0 && countY === 0) {
    return true;
  }
    
  if (countP === countY) {
    return true;
  } else {
    return false;
  }
};
```

## [문자열을 정수로 바꾸기](https://school.programmers.co.kr/learn/courses/30/lessons/12925){: target="_blank" }

```javascript
// const solution = (s) => Number(s); // 소수점 무시 X
const solution = (s) => parseInt(s); // 소수점 무시
```

## [자릿수 더하기](https://school.programmers.co.kr/learn/courses/30/lessons/12931){: target="_blank" }

```javascript
const solution = (N) => (N + '').split('').reduce((prev, curr) => {
  return prev + Number(curr); 
}, 0);
```

## [자연수 뒤집어 배열로 만들기](https://school.programmers.co.kr/learn/courses/30/lessons/12932){: target="_blank" }

```javascript
const solution = (n) => (n + '').split('').reverse().map(Number);
```

## [나머지가 1이 되는 수 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/87389){: target="_blank" }

```javascript
const solution = (n) => {
  for (let i = 2; i < n; i++) {
    if (n % i === 1) {
      return i;
    }
  }
};
```

## [x만큼 간격이 있는 n개의 숫자](https://school.programmers.co.kr/learn/courses/30/lessons/12954){: target="_blank" }

```javascript
const solution = (x, n) => Array.from({ length: n }, (_, i) => x * (i + 1));
// const solution = (x, n) => Array.from(Array(n), (_, i) => x * (i + 1));

// const solution = (x, n) => Array(n).fill().map((_, i) => x * (i + 1));
```

JS에서 배열은 오브젝트다. `{ length: n }`는 n의 길이를 갖는 배열을 의미한다. `from()` 함수는 첫 번째 인자로 iterable한 객체를 받는다. 위 코드에서는 n의 길이를 갖는 배열이 전달됐다. 두 번째 인자로 함수를 넘길 수 있는데 그 함수는 인자로 각각 iterable한 객체의 현재 요소와 현재 인덱스를 전달받는다.

## [정수 내림차순으로 배치하기](https://school.programmers.co.kr/learn/courses/30/lessons/12933){: target="_blank" }

```javascript
const solution = (n) => Number((n + '').split('').sort().reverse().join(''));

// const solution = (n) => (n + '').split('').sort().reverse().reduce((prev, curr) => {
//   return Number(prev + '' + curr);
// });
```

주석 처리한 코드는 테스트 케이스 한 개를 통과하지 못해서 오답처리가 됐는데 어디서 잘못됐는지 모르겠다.

## [정수 제곱근 판별](https://school.programmers.co.kr/learn/courses/30/lessons/12934){: target="_blank" }

```javascript
const solution = (n) => {
  if (Number.isInteger(Math.sqrt(n)) === true) {
    return (Math.sqrt(n) + 1) ** 2;
  } else {
    return -1;
  }
};
```

## [짝수와 홀수](https://school.programmers.co.kr/learn/courses/30/lessons/12937){: target="_blank" }

```javascript
const solution = (num) => num % 2 ? 'Odd' : 'Even';

// const solution = (num) => num % 2 === 0 ? 'Even' : 'Odd';
```

0은 `false`고 1은 `true`다.

## [평균 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript
const solution = (arr) => arr.reduce((prev, curr) => prev + curr) / arr.length;
```

## [하샤드 수](https://school.programmers.co.kr/learn/courses/30/lessons/12947){: target="_blank" }

```javascript
// const solution = (x) => (x + '').split('').reduce((prev, curr) => {
//   return x % (Number(prev) + Number(curr)) ? false : true;
// })

const solution = (x) => {
  const sum = (x + '').split('').reduce((prev, curr) => Number(prev) + Number(curr));

  return x % sum === 0;
};
```

`reduce()` 함수를 제대로 이해하지 못해서 주석된 코드와 같이 사용하는 것을 주의하자.

## [약수의 합](https://school.programmers.co.kr/learn/courses/30/lessons/12928){: target="_blank" }

```javascript
const solution = (n) => {
  let sum = 0;

  for (let i = 0; i <= n; i++) {

    if (n % i === 0) {
      sum += i;
    }
  }
  return sum;
}
```

## [두 정수 사이의 합](https://school.programmers.co.kr/learn/courses/30/lessons/12912){: target="_blank" }

```javascript
const solution = (a, b) => {
  let sum = 0;

  for (let i = Math.min(a, b); i <= Math.max(a, b); i++) {
    sum += i;
  }

  return sum;
};
```

## [x만큼 간격이 있는 n개의 숫자](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [나머지가 1이 되는 수 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [서울에서 김서방 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [콜라츠 추측](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [음양 더하기](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [없는 숫자 더하기](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [나누어 떨어지는 숫자 배열](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [제일 작은 수 제거하기](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [핸드폰 번호 가리기](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```
