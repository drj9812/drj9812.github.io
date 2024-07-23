---
title: "[JavaScript]함수"
categories: [Language, JavaScript]
tags: [JavaScript, Function, 함수]
image:
  path: /assets/img/posts/language/javascript/01-javascript-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: JavaScript
---

# 함수

## 선언

### 함수 선언문(Function Declarations)

```javascript
log('Hello'); // Hello

function log(message) {
  console.log(message);
}

log('Hello'); // Hello
log(1234); // 1234
```

- 호이스팅됨

### 함수 표현식(Function Expressions)

```javascript
print1() // ReferenceError: Cannot access 'print1' before initialization

const print1 = function () { // annonymous function
  console.log('print1');
}

const print1 = function print() { // named function
  console.log('print2');
}

print1() // print1
print2() // print2

const printAgain = print1;
printAgain(); // print1
```

- 호이스팅되지 않음

> named function은 디버깅이나 재귀 호출을 위해 사용된다.
{: .prompt-tip }

## Parameters

### Default Parameter

```javascript
function showMessage(message, from = 'unknown') {
  conosole.log(`${ message } by ${ from }`);
}

showMessage('Hi!'); // Hi by unknown
```

- 파라미터에 기본값 설정 가능
  + ES6에 도입됨

### Rest Parameter

```javascript
function printAll(...args) {
  // for(let i = 0; i < args.length; i++) {
  //   console.log(args[i]);
  // }

  // for (const arg of args) {
  //   console.log(arg)
  // }

  // args.forEach(arg) {
  //   console.log(arg)
  // }

  args.forEach((arg) => console.log(arg)); 
}

printAll('dream', 'coding', 'ellie');
```

- `...` 연산자를 함수의 매개변수 앞에 붙이면, 해당 매개변수는 전달된 인자의 목록을 배열로 받게 됨
  + ES6에 도입됨

## 종류

### Callback

```javascript
function greet(name, callback) {
  console.log('Hello ' + name);
  callback();
}

function sayGoodbye() {
  console.log('Goodbye!');
}

greet('Alice', sayGoodbye);
```

### Arrow Function

```javascript
// function Expression
// const simplePrint = function () {
//   console.log('simplePrint!');
// }

// Arrow Function
const simplePrint = () => console.log('simplePrint!');
const sum = (a, b) => a + b;
const simpleMultiply = (a, b) => {
  // do something more
  return a * b;
}
```

### IIFE(Immediately Invoked Function Expression)

```javascript
(function hello() {
  console.log('IIFE');
})(); // IIFE
```

- 함수가 바로 호출됨

## 참고자료

- [엘리, *자바스크립트 5. Arrow Function은 무엇인가? 함수의 선언과 표현 | 프론트엔드 개발자 입문편(JavaScript ES6)*, 드림코딩, 2020-04-22](https://www.youtube.com/watch?v=e_lU39U-5bQ&list=PLv2d7VI9OotTVOL4QmPfvJWPJvkmv6h-2&index=5){: target="_blank" }