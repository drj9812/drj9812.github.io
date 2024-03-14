---
title: "[Algorithm]재귀 알고리즘(Recursive Algorithm)"
categories: [Study, Algorithm]
tags: [Algorithm, 알고리즘, Recursive Algorithm, 재귀 알고리즘, Recursion, 재귀]
---

# 재귀 알고리즘(Recursive Algorithm)

## 재귀 알고리즘(Recursive Algorithm)이란?

- **함수나 메서드가 자기 자신을 호출**하는 프로그래밍 기법
- 알고리즘이나 함수를 해결하기 위해 자기 자신을 호출하는 방법

## 장점

- 반복문에 비해 **간결한 코드**
	+ 모든 재귀 알고리즘은 반복문으로 대체할 수 있음
- 일부 문제에서 재귀 알고리즘을 사용하면 간단하게 해결할 수 있음
	+ 트리 구조, 그래프 구조

## 단점

- 매 호출마다 스택에 새로운 호출 프레임이 추가되어 메모리 사용량이 증가
- 호출 스택이 초과되는 경우 **오버플로우**가 발생할 수 있음
- 호출과 반환에 많은 시간이 소요됨
- 가독성이 떨어질 경우 이해하는 데 어려움

## 구현 예시

```java
int callRecurse() {
    recurse(3); // 123
    return 0;
}

void recurse(int n) {
    if (n == 0) {
        return;
    }

    recurse(n - 1);
    System.out.printf("%d", n);
}
```

> 재귀 호출 전의 코드와 재귀 호출 후의 코드는 실행되는 시점이 다르다. 위 코드에서 `System.out.printf("%d", n)`가 재귀 호출하는 `recurse(n - 1)`보다 위에 위치한다면 `recurse(3)`은 123이 아닌 321을 출력하게 된다.
{: .prompt-tip }

### 동작 과정

![01-recursive-algorithm-process](/assets/img/posts/study/algorithm/recursive-algorithm/01-recursive-algorithm-process.jpg)
*위 그림은 재귀 알고리즘의 동작 과정을 간단하게 도식화한 것이다.*

1. `recurse(3)` 호출
	- 호출 스택: **`recurse(3)`**
2. `recurse(3)`가 실행되고, n의 값(3)이 0이 아니므로 내부에서 `recurse(3 - 1)` 호출
	- 호출 스택: `recurse(3)` -> **`recurse(2)`**
3. `recurse(2)`가 실행되고, n의 값(2)이 0이 아니므로 내부에서 `recurse(2 - 1)` 호출
	- 호출 스택: `recurse(3)` -> `recurse(2)` -> **`recurse(1)`**
4. `recurse(1)`가 실행되고, n의 값(1)이 0이 아니므로 내부에서 `recurse(1 - 1)` 호출
	- 호출 스택: `recurse(3)` -> `recurse(2)` -> `recurse(1)` -> **`recurse(0)`**
5. `recurse(0)`가 실행되고, n의 값이 0이므로 `recurse(1)`로 `return`
	- 호출 스택: `recurse(3)` -> `recurse(2)` -> `recurse(1)` -> ~~`recurse(0)`~~
6.  `recurse(0)`를 호출한 `recurse(1)` 내부에서 1을 출력하고 `recurse(1)` 종료
	- 호출 스택: `recurse(3)` -> `recurse(2)` -> ~~`recurse(1)`~~
7. `recurse(1)`를 호출한 `recurse(2)` 내부에서 2를 출력하고 `recurse(2)` 종료
	- 호출 스택: `recurse(3)` -> ~~`recurse(2)`~~
8. `recurse(2)`를 호출한 `recurse(3)` 내부에서 3을 출력하고 `recurse(3)` 종료
	- 호출 스택: ~~`recurse(3)`~~

## 참고 자료

- [혀니C코딩, "재귀함수란? 재귀 호출(recursive call) \| 아직도 어렵다면 무조건 클릭!!!!", 2022-09-14](https://www.youtube.com/watch?v=yio6FyP1N2k){: target="_blank" }