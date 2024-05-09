---
title: "[SQLD]ERD(Entity Relation Diagram) 표기법"
categories: [Study, Certification]
tags: [certification, 자격증, SQLD]
image:
  path: /assets/img/posts/study/certification/sqld/01-sqld-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: SQLD
---

# ERD(Entity Relation Diagram) 표기법

## Entity

- IE 표기법, Barker 표기법 모두 박스로 표현
	+ Barker 표기법의 경우 모서리가 둥근 사각형으로 표기하기도 함

## 관계

### 차수

![01-relation-notation(1)](/assets/img/posts/study/certification/sqld/erd-notation/01-relation-notation(1).jpg)

### 선택성

![02-relation-notation(2)](/assets/img/posts/study/certification/sqld/erd-notation/02-relation-notation(2).jpg)

- Barker 표기법은 자신의 Entity에 대한 표현을 반대쪽에 함
	+ 점선이 오른쪽에 있을 경우 왼쪽 실선이 선택적

### 식별/비식별

![03-relation-notation(3)](/assets/img/posts/study/certification/sqld/erd-notation/03-relation-notation(3).jpg)

- Barker 표기법은 UID Bar(`|`)로 식별 관계와 비식별 관계를 구분

## 식별자

|        관계        |            IE              |   Barker    |
|:------------------:|:-----------------------:|:------------:|
|  주 식별자(PK)  |  맨 위 네모칸 배치  |       #       |
|    일반 속성     |    밑 네모칸 배치    |  * 또는 ○  |

## 속성

|       관계        |           IE          |  Barker  |
|:-----------------:|:-------------------:|:---------:|
|    NULL 허용   |  표기하지 않음  |     ○    |
|  NULL 허용 X  |  표기하지 않음  |      *     |

## 예시

![04-ex-ie-notation](/assets/img/posts/study/certification/sqld/erd-notation/04-ex-ie-notation.jpg)
*IE 표기법*

- 선택적 관계의 경우 IE 표기법에서는 동그라미로 표현
	+ 부서 필수
	+ 사원 선택적
- 비식별 관계의 경우 IE는 점선으로 표현

![05-ex-barker-notation](/assets/img/posts/study/certification/sqld/erd-notation/05-ex-barker-notation.jpg)
*Barker 표기법*

- 한 명 이상의 직원이 각 부서에서 일할 수 있음
- 부서는 필수적
	+ 사원이 존재하면 부서도 존재해야 함
		* 사원은 반드시 부서에 소속돼야 함
- UID Bar가 없으므로 두 관계는 비식별 관계
- 부서위치는 `NULL` 허용

## 참고자료

- [홍은혜, "SQLD 완벽정리 부록편", 홍쌤의 데이터랩, 2024-03-02](https://www.youtube.com/watch?v=ovGGaIGL2Ys&list=PLbflMVhwy2jPIAzArCK90mqFlTtndFigS&index=5){: target="_blank" }
- 한국데이터산업진흥원, SQL 자격검정 실전문제(서울: 한국데이터산업진흥원, 2016), 279.
- [yunamom, Study with yuna](https://yunamom.tistory.com/){: target="_blank" }