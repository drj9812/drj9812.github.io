---
title: "[Java]변수의 타입"
categories: [Language, Java]
tags: [Java, 자바]
image:
  path: /assets/img/posts/language/java/01-java-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Java
---

# 변수의 타입

## 기본형 타입(Primitive Type)

|  분류  |   타입  | 할당되는 메모리 크기 |   기본값   |                  데이터의 표현 범위                     |
|:------:|:------:|:-------------------:|:----------:|:------------------------------------------------------:|
| 논리형 | boolean |       1 byte        |   false   |                    true, false                         |
| 정수형 |   byte  |       1 byte        |     0     |                     -128 ~ 127                         |
|        |  short  |       2 byte        |     0     |                 -32,768 ~ 32,767                       |
|        |   int   |       4 byte        |     0     |           -2,147,483,648 ~ 2,147,483,647               |
|        |   long  |       8 byte        |     0L    | -9,223,372,036,854,775,808 ~ 9,223,372,036,854,775,807 |
| 실수형 |  float  |       4 byte        |    0.0F   |         (3.4 X 10-38) ~ (3.4 X 1038) 의 근사값          |
|        |  double |      8 byte         |    0.0    |        (1.7 X 10-308) ~ (1.7 X 10308) 의 근사값         |
| 문자형 |   char  |       2 byte        |  '\u0000' |                     0 ~ 65,535                          |

- `null` 값을 가질 수 없음
  + 기본값이 존재함
- 변수의 선언과 동시에 메모리 생성
- 메모리의 스택(stack) 영역에 값이 저장됨
- 저장공간에 실제 자료 값을 가짐

## 참조형 타입(Reference Type)

|    타입   | 할당되는 메모리 크기 | 기본값 |
|:---------:|:-------------------:|:-----:|
|    배열   |       4 byte        |  Null |
|    열거   |       4 byte        |  Null |
|   클래스  |       4 byte        |  Null |
| 인터페이스 |       4 byte       |  NUll |


- 객체의 메모리 주소를 저장
  + 메모리의 힙(heap) 영역에 값을 저장하고, 그 참조값을 갖는 변수는 스택(stack) 영역에 저장
- `null` 값 허용
- 다형성 지원
- 상속 및 인터페이스 구현

## 참고자료

- [인파_, *JAVA 변수의 기본형 & 참조형 타입 차이 이해하기*, Inpa Dev, 2022-09-24](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EB%B3%80%EC%88%98%EC%9D%98-%EA%B8%B0%EB%B3%B8%ED%98%95-%EC%B0%B8%EC%A1%B0%ED%98%95-%ED%83%80%EC%9E%85){ :target="_blank" }