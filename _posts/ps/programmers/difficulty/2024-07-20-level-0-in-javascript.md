---
title: "[프로그래머스]레벨 0 난이도 모든 문제(JavaScript)"
categories: [PS, Programmers]
tags: [PS, Algorithm, 알고리즘, Programmers, 프로그래머스, JavaScript, 레벨 0]
image:
  path: /assets/img/posts/ps/programmers/01-programmers-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: programmers
---

# 레벨 0 난이도 모든 문제(JavaScript)

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

## [순서쌍의 개수](https://school.programmers.co.kr/learn/courses/30/lessons/120836){: target="_blank" }

```javascript
const solution = (n) => {
  let pairCount = 0;

  if (n === 1) {
    pairCount = 1;
  } else {
    pairCount = 2;
  }

  for (let i = 2; i < n; i++) {
    if (n % i === 0) {
      pairCount++;
    }
  }

  return pairCount;
}
```

## [개미 군단](https://school.programmers.co.kr/learn/courses/30/lessons/120837){: target="_blank" }

```javascript
const solution = (hp) => {
  let count = 0;

  count += Math.floor(hp / 5);
  hp %= 5;

  count += Math.floor(hp / 3);
  hp %= 3;

  count += hp;

  return count;
}
```

## [편지](https://school.programmers.co.kr/learn/courses/30/lessons/120898){: target="_balnk" }

```javascript
const solution = (message) => message.length * 2;
```

## [세균 증식](https://school.programmers.co.kr/learn/courses/30/lessons/120910){: target="_blank" }

```javascript
const solution = (n, t) => n * 2 ** t;
```

## [특정 문자 제거하기](https://school.programmers.co.kr/learn/courses/30/120826){: target="_blank" }

```javascript
// const solution = (my_string, letter) => {
//     const arr = my_string.split('');
    
//     return arr.reduce((prev, curr) => {
//         if (curr !== letter) {
//             prev.push(curr);
//         }
//         return prev;
//     }, []).join('');
// };

const solution = (my_string, letter) => my_string.split(letter).join('');
```

주석처럼 풀었는데 그냥 `split()` 함수를 사용하면 된다.

## [배열의 유사도](https://school.programmers.co.kr/learn/courses/30/lessons/120903){: target="_blank" }

```javascript
const solution = (s1, s2) => s1.filter((e) => s2.includes(e)).length;
```

## [모음 제거](https://school.programmers.co.kr/learn/courses/30/lessons/120849){: target="_blank" }

```javascript
const solution = (my_string) => my_string.split('').filter((e) => {
  return e !== 'a' && e !== 'e' && e !== 'i' && e !== 'o' && e !== 'u';
}).join('');

// const solution = (my_string) => my_string.replace(/[aeiou]/g, '');

// const solution = (my_string) => {
//   const vowels = ['a', 'e', 'i', 'o', 'u'];
//   return my_string.split('').filter((e) => !vowels.includes(e)).join('');
// }
```

주석 처리된 코드처럼 푸는 게 더 깔끔한 것 같다.

## [숨어있는 숫자의 덧셈 (1)](https://school.programmers.co.kr/learn/courses/30/lessons/120851){: target="_blank" }

```javascript
const solution = (my_string) => {

  return my_string.split('').reduce((prev, curr) => {
    let num = Number(curr);

    return Number.isInteger(num) ? prev + num : prev;
  }, 0);
};
```

## [암호 해독](https://school.programmers.co.kr/learn/courses/30/lessons/120892){: target="_blank" }

```javascript
const solution = (cipher, code) => {
  const arr = cipher.split('');
  let index = 0;

  return arr.reduce((prev, curr) => {
    index++;

    return index % code === 0 ? prev + curr : prev 
    }, '');
};
```

## [대문자와 소문자](https://school.programmers.co.kr/learn/courses/30/lessons/120893){: target="_blank" }

```javascript
const solution = (my_string) => {

  return my_string.split('').map((e) => {

    return e === e.toUpperCase() ? e.toLowerCase() : e.toUpperCase();
  }).join('');
};
```

## [짝수 홀수 개수](https://school.programmers.co.kr/learn/courses/30/lessons/120824){: target="_blank" }

```javascript
// const solution = (num_list) => {
//   return [
//     num_list.filter((num) => num % 2 === 0).length,
//     num_list.filter((num) => num % 2 === 1).length,
//   ];
// }

// const solution = (num_list) => {
//   const answer = [0,0];
// 
//   for(let a of num_list){
//     answer[a % 2] += 1
//   }
// 
//   return answer;
// }

const solution = (num_list) => {
  const arr = [];
  let count = 0;

  for (let i = 0; i < num_list.length; i++) {
    if (num_list[i] % 2 === 0) {
      count++;
    }
  }

  arr.push(count);
  arr.push(num_list.length - count);
  
  return arr;
}
```

## [가장 큰 수 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/120899){: target="_blank" }

```javascript
// const solution = (array) => {
//   let max = 0;
//
//   for (e of array) {
//     if (e > max) {
//       max = e;
//     } 
//   }
//
//   return [max, array.indexOf(max)];
// };

const solution = (array) => {
  let max = Math.max(...array);

  return [max, array.indexOf(max)];
}
```

## [n의 배수 고르기](https://school.programmers.co.kr/learn/courses/30/lessons/120905){: target="_blank" }

```javascript
const solution = (n, numlist) => numlist.filter((e) => e % n === 0);
```

## [문자열을 정수로 변환하기](https://school.programmers.co.kr/learn/courses/30/lessons/181850){: target="_blank" }

```javascript
const solution = (flo) => Math.trunc(flo);
```

## [뒤에서 5등까지](https://school.programmers.co.kr/learn/courses/30/lessons/181853){: target="_blank" }

```javascript
const solution = (num_list) => num_list.sort((a, b) => a - b).splice(0, 5);
```

## [배열 비교하기](https://school.programmers.co.kr/learn/courses/30/lessons/181856){: target="_blank" }

```javascript
const solution = (arr1, arr2) => {
  const total1 = arr1.reduce((prev, curr) => prev + curr);
  const total2 = arr2.reduce((prev, curr) => prev + curr);

  if (arr1.length > arr2.length) {
    return 1;
  } else if (arr1.length < arr2.length) {
    return -1;
  } else {

    if (total1 > total2) {
      return 1;
    } else if (total1 < total2) {
      return -1;
    } else {
      return 0;
    }
  }
}
```

## [rny_string](https://school.programmers.co.kr/learn/courses/30/lessons/181863){: target="_blank" }

```javascript
const solution = (rny_string) => rny_string.split('').map((e) => e === 'm' ? 'rn' : e).join('');
```

## [문자열 바꿔서 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/181864){: target="_blank" }

```javascript
const solution = (myString, pat) => {
  const result = myString.split('').map((e) => e === 'A' ? 'B' : 'A').join('');

  return result.includes(pat) ? 1 : 0;
}
```

## [공백으로 구분하기 2](https://school.programmers.co.kr/learn/courses/30/lessons/181868){: target="_blank" }

```javascript
const solution = (my_string) =>  my_string.split(' ').filter(word => word);
```

js에서 빈 문자열은 `false`다.

## [공백으로 구분하기 1](https://school.programmers.co.kr/learn/courses/30/lessons/181869){: target="_blank" }

```javascript
const solution = (my_string) => my_string.split(' ').filter((word) => word);
```

## [특정한 문자를 대문자로 바꾸기](https://school.programmers.co.kr/learn/courses/30/lessons/181873){: target="_blank" }

```javascript
const solution = (my_string, alp) => my_string.split('').map((word) => word === alp ? word.toUpperCase() : word).join('');
```

## [A 강조하기](https://school.programmers.co.kr/learn/courses/30/lessons/181874){: target="_blank" }

```javascript
// const solution = (myString) => myString.split('').map((word) => {
//   return word === 'a' ? 'A' : word !== 'A' ? word.toLowerCase() : word;
// }).join('');

const solution = (myString) => myString.toLowerCase().replaceAll('a', 'A');
```

## [배열에서 문자열 대소문자 변환하기](https://school.programmers.co.kr/learn/courses/30/lessons/181875){: target="_blank" }

```javascript
const solution = (strArr) => strArr.map((value, index) => index % 2 !== 0 ? value.toUpperCase() : value.toLowerCase());
```

## [문자열 정수의 합](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [정수 부분](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [뒤에서 5등 위로](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [배열의 원소만큼 추가하기](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```