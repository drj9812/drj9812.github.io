---
title: "[JavaScript]클래스(Class)와 오브젝트(Object)"
categories: [Language, JavaScript]
tags: [JavaScript]
image:
  path: /assets/img/posts/language/javascript/01-javascript-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: JavaScript
---

# 클래스(Class)와 오브젝트(Object)

- [[Java]OOP(Object-Oriented Programming)](https://drj9812.github.io/posts/oop){: target="_blank" } 참조

## 클래스(Class)

```javascript
// ES6 이전
// function Obj() {}
// Obj.prototype.method = function () {}

// ES6 이후
class Obj {
    method:

    function () {}
}
```

- ES6에 도입됨
  + 클래스가 도입되기 전에는 바로 오브젝트를 생성해서 사용
- 기존에 존재하던 프로토타입을 기반으로 문법만 추가됨
  + Syntactic sugar

### 선언

```javascript
class Person {
  // constructor
  constructor(name, age) {
    // fields
    this.name = name;
    this.age = age;
  }

  // methods
  speak() {
    console.log(`${ this.name }: hello!`);
  }
}

const ellie = new Person('ellie', 20);
console.log(ellie.name); // ellie
console.log(ellie.age); // 20
ellie.speak(); // ellie: hello!
```

- 프로퍼티를 선언할 때 `var`, `const`, `let` 키워드를 사용하지 않음

### Getter와 Setter

```javascript
class User {
  constructor(firstName, lastName, age) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.age = age;
  }

  // getter
  get age() {
    // return this.age;
    return this._age;
  }

  // setter
  set age(value) {
    // return this.age = value;
    this._age = value < 0 ? 0 : value;
  }
}
```

- getter를 정의하는 순간 `this.age`는 getter를 호출하고 setter를 정의하는 순간 `= age;`는 setter를 호출
  + setter를 `set age(value) { return this.age = value; }`와 같이 정의한다면 `return this.age = value;`에서 `this.age = value;`는 getter와 setter를 재귀 호출하므로 getter와 setter 안에서 쓰이는 변수의 이름을 `_age`와 같이 변경해주어야 함

### 필드(Fields)

#### 생성자 내부 선언 및 초기화

```javascript
class User {

  // 생성자 내부에서 필드 선언 및 초기화
  constructor() {
    this.publicField = 0; // 암묵적으로 public 필드 선언 및 초기화
    this.#privateField = 0; // 프라이빗 필드는 암묵적으로 선언되지 않음
  }

  // 프라이빗 필드 명시적으로 선언
  #privateField;
}
```

- ES2022 이전
- `public` 필드는 생성자 안에서 `this` 키워드를 사용하여 필드를 초기화하면 자동으로 추가됨
- `private` 필드는 클래스 바디 내부에서 명시적으로 선언해야 함

#### 클래스 바디 내부 선언 및 초기화

```javascript
class User {

  // constructor() {
  //   this.publicField = 0; // 암묵적으로 public 필드 선언 및 초기화
  //   this.#privateField = 0; // 프라이빗 필드는 암묵적으로 선언되지 않음
  // }

  // // 프라이빗 필드 명시적으로 선언
  // #privateField;

  // 클래스 바디 내부에서 필드 선언 및 초기화
  publicField = 0;
  #privateField = 0;
}
```

- ES2022 이후

### static

```javascript
class Article {
  static publisher = 'Dream Coding';

  constructor(articleNumber) {
    this.articleNumber = articleNumber;
  }

  static printPublisher() {
    console.log(Article.publisher);
  }
}

const article1 = new Article(1);

console.log(article1.publisher); // undefined

console.log(Article.publisher); // Dream Coding
Article.printPublisher(); // Dream Coding
```

- `static` 멤버는 오브젝트가 아닌 클래스 자체에 할당됨

### 상속과 다형성

```javascript
class Shape {
  constructor(width, height, color) {
    this.width = width;
    this.height = height;
    this.color = color;
  }

  draw() {
    console.log(`drawing ${ this.color } color!`);
  }

  getArea() {
    return this.width * this.height;
  }
}

class Rectangle extends Shape { }

class Triangle extends Shape {
  /**
   * @override
   */
  getArea() {
    return (this.width * this.height) / 2;
  }
}

const rectangle = new Rectangle(20, 20, 'blue');
rectangle.draw(); // drawing blue color!
console.log(rectangle.getArea()); // 400

const triangle = new Triangle(20, 20, 'red');
triangle.draw(); // drawing red color!
console.log(triangle.getArea()); // 200

console.log(rectangle instanceof Rectangle); // true
console.log(triangle instanceof Rectangle); // false
console.log(triangle instanceof Triangle); // true
console.log(triangle instanceof Shape); // true
console.log(triangle instanceof Object); // true
```

## 오브젝트(Object)

### 선언

#### Object constructor syntax

```javascript
const person = new Person();
```

- 클래스의 생성자를 이용하여 생성

#### Object literal syntax

```javascript
const person = { name: 'ellie', age: 4 };

person.hasJob = true;
console.log(person.hasJob); // true

delete person.hasJob;
console.log(person.hasJob); // undefined
```

- 클래스가 없어도 생성할 수 있음
- 오브젝트를 정의한 후에 프로퍼티를 추가, 삭제할 수 있음
  + JS는 런타임에 타입이 결정되는 언어

### Computed properties

```javascript
const person = { name: 'ellie', age: 20 };

console.log(person.name); // ellie

// Computed properties
console.log(person[name]) // undefined
console.log(person['name']); // ellie

// Computed properties를 이용한 프로퍼티 추가
person['hashJob'] = true;
console.log(person['hashJob']); // true
```

- Key는 항상 string 타입

```javascript
const person = { name: 'ellie', age: 20 };

function printValue(obj, key) {
  // console.log(obj.key); // undefined
  console.log(obj[key]); // ellie
}

printValue(person, 'name'); 
```

- 동적으로 Key를 받아와야 할 때 사용

### Property value shorthand

```javascript
const person1 = { name: 'bob', age: 1 };
const person2 = { name: 'steve', age: 2 };
const person3 = { name: 'dave'. age: 3 };
const person4 = makePerson('ellie', 4);

console.log(person4); // {name: "ellie", age: 4}

function makePerson(name, age) {
  return {
    // name: name,
    // age: age,

    // Property value shorthand
    name,
    age,
  }
}
```

### Constructor Function

```javascript
// function makePerson(name, age) {
//   return {
//     name,
//     age,
//   }
// }

const person = new Person('ellie', 4);

function Person(name, age) {
  // this = {};
  this.name = name;
  this.age = age;
  // return this; 
}
```

- 다른 계산을 하지 않고 순수하게 오브젝트를 생성하는 함수
- 대문자로 시작하도로 작명
- 클래스의 생성자와 같은 문법으로 작성

### in 연산자(Operator)

```javascript
const person = { name: 'ellie', age: 20 };

console.log('name' in person); // true
console.log('age' in person); // true

console.log('random' in person); // false
console.log(person.random); // undefined
```

- 오브젝트에 해당하는 Key가 있는지 확인하는 연산자

### for... in 문

```javascript
const person = { name: 'ellie', age: 20 };

for (key in person) {
  console.log(key); // name
                    // age
}
```

- 객체의 모든 열거 가능한 속성을 반복(iterate)

### Fun cloning

```javascript
const person1 = { name: 'ellie', age: 20 };
const person2 = person1
person2.name = 'coder';

console.log(person); // {name: "coder", age: 20}

// old way
const person3 = {};

for (key in person1) [
  person3[key] = person1[key];
]

console.log(person3); // {name: "coder", age: 20}

// new way
const person4 = Object.assgin(person4, person);
console.log(person4); // {name: "codeR", age: 20}
```

- `Object`의 `assign()` 메서드 활용
  + JS의 모든 오브젝트는 `Object`를 상속함

## 참고자료

- [엘리, *자바스크립트 6. 클래스와 오브젝트의 차이점(class vs object), 객체지향 언어 클래스 정리 \| 프론트엔드 개발자 입문편 (JavaScript ES6)*, 드림코딩, 2020-04-27](https://www.youtube.com/watch?v=_DLhUBWsRtw&list=PLv2d7VI9OotTVOL4QmPfvJWPJvkmv6h-2&index=6){: target="_blank" }