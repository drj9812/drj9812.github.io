---
title: "[CSS | SCSS]SCSS 문법"
categories: [Study, CSS]
tags: [CSS, SCSS]
image:
  path: /assets/img/posts/study/css/scss/01-sass-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: SASS
---

# SCSS 문법

## 변수(Variables)

```scss
$primary-color: #3498db;

body {
  background-color: $primary-color;
}
```

## 상속(Inheritance)

## 중첩(Nesting)

## 믹스인(Mixins)

```scss
@mixin light-syntax {
  --language-border-color: #ececec;
  --highlight-bg-color: #f6f8fa;
  --highlighter-rouge-color: #3f596f;
  --highlight-lineno-color: #9e9e9e;
  --inline-code-bg: #f6f6f7;
  --code-color: #3a3a3a;
  --code-header-text-color: #a3a3a3;
  --code-header-muted-color: #e5e5e5;
  --code-header-icon-color: #c9c8c8;
  --clipboard-checked-color: #43c743;
}
```

- 믹스인 정의

```scss
@include light-syntax;
```

- 믹스인 불러오기

## 함수(Functions)

## 조건문(Control Directives)

## 데이터 유형(Data Types)

## 연산자(Operators)

```scss
:not([속성명])
```

- `[속성명]`이 아닌 속성

## 컴파일러 설정(Compiler Options)

## 확장(Extensions)

## 모듈화(Modularization)

### @Import

```scss
@import 'colors/syntax-light';
```

- 해당 위치에 있는 파일 불러오기
- 확장자 기본값은 SCSS