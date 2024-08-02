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

## [서울에서 김서방 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/12919){: target="_blank" }

```javascript
const solution = (seoul) => '김서방은 ' + seoul.indexOf('Kim') + '에 있다';
```

## [콜라츠 추측](https://school.programmers.co.kr/learn/courses/30/lessons/12943){: target="_blank" }

```javascript
// const solution = (n) => {
//   let count = 0;
//
//   while (n !== 1) {
//     if (n % 2 === 0) {
//         n = n / 2;
//         count++;
//     } else {
//       n = n * 3 + 1;
//       count ++;
//     }
//
//     if (count === 500) {
//        return -1;
//     }
//   }
//   return count;
// };

const solution = (n) => {
  let count = 0;

  while (n !== 1 && count !== 500) {
    n % 2 === 0 ? n = n / 2 : n = n * 3 + 1;
    count++;
  }

  return count === 500 ? -1 : count;
};
```

처음엔 주석 처리된 코드처럼 풀었는데 다른 사람의 풀이를 보고 수정했다.

## [음양 더하기](https://school.programmers.co.kr/learn/courses/30/lessons76501/){: target="_blank" }

```javascript
const solution = (absolutes, signs) => {

  return absolutes.reduce((prev, curr, index) => {

    return signs[index] ? prev + curr : prev - curr;
    }, 0);
};
```

`reduce()` 함수는 세 번째 인자로 현재 처리 중인 요소의 인덱스와 네 번재 인자로 `reduce()` 함수가 호출된 배열을 인자로 받을 수 있다.

## [없는 숫자 더하기](https://school.programmers.co.kr/learn/courses/30/lessons/86051){: target="_blank" }

```javascript
const solution = (numbers) => numbers.reduce((prev, curr) => prev - curr, 45);
```

`numbers`에는 0부터 9사이의 서로 다른 숫자가 들어있으므로 0부터 9까지의 합인 45에서 `numbers`의 모든 원소합을 뺀다.

## [나누어 떨어지는 숫자 배열](https://school.programmers.co.kr/learn/courses/30/lessons/12910){: target="_blank" }

```javascript
const solution = (arr, divisor) => {
  const result = arr.filter((e) => e % divisor === 0).sort((a, b) => a - b);
    
  return result.length === 0 ? [-1] : result;
}
```

`sort()` 함수는 문자열이 기준이므로 숫자를 정렬할 때는 비교 함수를 전달하여 의도하지 않은 결과가 나오는 것을 방지해야 한다.

## [제일 작은 수 제거하기](https://school.programmers.co.kr/learn/courses/30/lessons/12935){: target="_blank" }

```javascript
// const solution = (arr) => {
//     const copyArr = [...arr];
//     // const copyArr = arr.slice();
//     // const copyArr = Array.from(arr);

//     const min = arr.sort((a, b) => a - b)[0];
//     const index = copyArr.indexOf(min);
    
//     copyArr.splice(index, 1);
//     return copyArr.length !== 0 ? copyArr : [-1];
// };

const solution = (arr) => {
  const index = arr.indexOf(Math.min(...arr));
  arr.splice(index, 1);
    
  return arr.length !== 0 ? arr : [-1];
};
```

주석 처리된 코드가 내가 제출한 코드지만 다른 사람의 풀이를 보고 수정하였다. `min()` 함수와 스프레드 연산자(`[...]`)를 활용하자.

## [핸드폰 번호 가리기](https://school.programmers.co.kr/learn/courses/30/lessons/12948){: target="_blank" }

```javascript
// const solution = (phone_number) => {
//   return phone_number.split('').map((char, i, arr) => {
//       return arr.length - 4 > i ? '*' : char;
//   }).join('');
// };

const solution = (phone_number) => Array.from(phone_number, (_, i) => {
  return phone_number.length - 4 > i ? '*' : _;    
}).join('');
```

## [가운데 글자 가져오기](https://school.programmers.co.kr/learn/courses/30/lessons/12903){: target="_blank" }

```javascript
// const solution = (s) => {
//   const arr = s.split('');
//   const mid = parseInt(arr.length / 2);

//   return arr.length % 2 !== 0 ? arr[mid] : arr.slice(mid - 1, mid + 1).join('');
// };

const solution = (s) => {
    const mid = parseInt(s.length / 2);

    return s.length % 2 !== 0 ? s[mid] : s.slice(mid - 1, mid + 1);
};
```

js에서는 문자열을 굳이 배열로 변환해줄 필요가 없다.

## [수박수박수박수박수박수?](https://school.programmers.co.kr/learn/courses/30/lessons/12922){: target="_blank" }

```javascript
const solution = (n) => Array.from({ length: n }, (_, i) => i % 2 == 0 ? '수' : '박').join('');
```

## [내적](https://school.programmers.co.kr/learn/courses/30/lessons/70128){: target="_blank" }

```javascript
const solution = (a, b) => a.map((value, index) => value * b[index]).reduce((prev, curr) => prev + curr);

// const solution = (a, b) =>  a.reduce((acc, _, i) => acc += a[i] * b[i], 0);
```

주석 처리된 코드처럼 푸는 방법도 있다.

## [약수의 개수와 덧셈](https://school.programmers.co.kr/learn/courses/30/lessons/77884){: target="_blank" }

```javascript
const solution = (left, right) => Array.from({ length: right - left + 1 }, (_, index) => left + index).reduce((prev, curr) => {
  let count = 0;

  for (let i = 1; i <= curr; i++) {
    if (curr % i === 0) {
      count++;
    }
  }

  return count % 2 === 0 ? prev + curr : prev - curr;
}, 0);
```

## [문자열 내림차순으로 배치하기](https://school.programmers.co.kr/learn/courses/30/lessons/12917){: target="_blank" }

```javascript
const solution = (s) => s.split('').sort().reverse().join('');
```

## [부족한 금액 계산하기](https://school.programmers.co.kr/learn/courses/30/lessons/82612){: target="_blank" }

```javascript
const solution = (price, money, count) => {
  const cost = Array.from({ length: count }, (_, i) => price * (i + 1)).reduce((prev, curr) => prev + curr);
    
  return cost - money > 0 ? cost - money : 0;
}
```

## [문자열 다루기 기본](https://school.programmers.co.kr/learn/courses/30/lessons/12918){: target="_blank" }

```javascript
const solution = (s) => {
  if (s.length !== 4 && s.length !== 6) {
    return false;
  }

  return /^[0-9]+$/.test(s) ? true : false;
};
```

정규 표현식에서 `^`는 문자열의 시작을 의미한다. `[0-9]`는 숫자 하나를 의미한다. 대괄호 안에 있는 0-9는 0부터 9까지의 모든 숫자 중 하나를 나타낸다. `+`는 바로 앞의 패턴이 한 번 이상 반복될 수 있음을 나타낸다. 즉, `[0-9]` 패턴이 하나 이상 이어져야 한다는 의미다. `$`는 문자열의 끝을 의미한다.

## [행렬의 덧셈](https://school.programmers.co.kr/learn/courses/30/lessons/12950){: target="_blank" }

```javascript
const solution = (arr1, arr2) => {
  const result = [];

  for (let i = 0; i < arr1.length; i++) {
    result[i] = [];

    for (let j = 0; j < arr1[i].length; j++) {
      result[i][j] = arr1[i][j] + arr2[i][j];
    }
  }

  return result;
};
```

## [직사각형 별찍기](https://school.programmers.co.kr/learn/courses/30/lessons/12969){: target="_blank" }

```javascript
process.stdin.setEncoding('utf8');
process.stdin.on('data', data => {
  const n = data.split(' ');
  const a = Number(n[0]), b = Number(n[1]);

  let star = '';

  for (let i = 0; i < b; i++) {
    for (let j = 0; j < a; j++) {
      star += '*';
    }
      star += '\n';
  }
  // console.log(star);
  process.stdout.write(star);
});
```

## [최대공약수와 최소공배수](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [같은 숫자는 싫어](https://school.programmers.co.kr/learn/courses/30/lessons/12906){: target="_blank" }

```javascript
const solution = (arr) => arr.filter((value, index) => value !== arr[index + 1]);
```

## [예산](https://school.programmers.co.kr/learn/courses/30/lessons/12982){: target="_blank" }

```javascript
const solution = (d, budget) => {
  d.sort((a, b) => a - b);

  return d.reduce((count, curr) => {
    if (budget >= curr) {
      budget -= curr;
      count++;
    }

    return count;
  }, 0);
};
```

## [3진법 뒤집기](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [크기가 작은 부분 문자열](https://school.programmers.co.kr/learn/courses/30/lessons/147355){: target="_blank" }

```javascript
const solution = (t, p) => t.split('').reduce((count, curr, index) => {
  const arr = t.split('').slice(index, index + p.length);

  if (arr.length !== p.length) {
    return count;
  }

  Number(arr.join('')) <= Number(p) ? count++ : count;

  return count;
}, 0);
```

## [이상한 문자 만들기](https://school.programmers.co.kr/learn/courses/30/lessons/12930){: target="_blank" }

```javascript
const solution = (s) => s.toLowerCase().split(' ').map((e) => {
  return e.split('').map((value, index) => {
    return index % 2 === 0 ? value.toUpperCase() : value;
  }).join(''); 
}).join(' ');
```

## [삼총사](https://school.programmers.co.kr/learn/courses/30/lessons/131705){: target="_blank" }

```javascript
const solution = (number) => {
  let sum = 0;
  let count = 0;

  for (let i = 0; i < number.length; i++) {
    for (let j = i + 1; j < number.length; j++) {
      for (let k = j + 1; k < number.length; k++) {

        if (number[i] + number[j] + number[k] === 0) {
          count++;
        }
      }
    }
  }
  return count;
};
```

## [최소직사각형](https://school.programmers.co.kr/learn/courses/30/lessons/86491){: target="_blank" }

```javascript
const solution = (sizes) => {
  const walletSize = [0, 0];

  sizes.forEach(size => {
    size.sort((a, b) => b - a);

    if (size[0] > walletSize[0]) walletSize[0] = size[0];
    if (size[1] > walletSize[1]) walletSize[1] = size[1];
  });

  return walletSize[0] * walletSize[1];
};
```

못 풀겠어서 GPT한테 물어보고 조금 수정한 코드다. 가로 방향이든 세로 방향이든 한 쪽으로 맞춰놓고 최댓값을 구하는 것이 핵심이다.

## [시저 암호](https://school.programmers.co.kr/learn/courses/30/lessons/12926){: target="_blank" }

```javascript
const solution = (s, n) => {
  return s.split('').map((value) => {
    if (value === ' ') {
      return value;
    } else {

      const charCode = value.charCodeAt(0);

      if (charCode >= 65 && charCode <= 90) { // 대문자 범위(65 ~ 90)
        return String.fromCharCode((charCode - 65 + n) % 26 + 65);

      } else { // 소문자 범위(97 ~ 122)
        return String.fromCharCode((charCode - 97 + n) % 26 + 97);
      }
    }
 }).join('');
};
```

## [가장 가까운 같은 글자](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [K번째수](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

## [](https://school.programmers.co.kr/learn/courses/30/lessons/){: target="_blank" }

```javascript

```

