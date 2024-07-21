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

## [문자열 내 p와 y의 개수](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

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

## [문자열을 정수로 바꾸기](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

## [자릿수 더하기](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

## [자연수 뒤집어 배열로 만들기](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

## [정수 내림차순으로 배치하기](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

## [정수 제곱근 판별](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

## [짝수와 홀수](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

## [평균 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

## [하샤드 수](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

## [약수의 합](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

## [두 정수 사이의 합](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

## [x만큼 간격이 있는 n개의 숫자](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

## [나머지가 1이 되는 수 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

## [서울에서 김서방 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

## [콜라츠 추측](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

## [음양 더하기](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

## [없는 숫자 더하기](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

## [나누어 떨어지는 숫자 배열](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

## [제일 작은 수 제거하기](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

## [핸드폰 번호 가리기](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }