---
title: "[Spring]의존성 주입(Dependency Injection)"
categories: [Study, Spring]
tags: [Java, 자바, Spring, 스프링, Depdency Injection, DI, 의존성 주입]
---

# 의존성 주입(Dependency Injection)

## 의존성 주입(Dependency Injection, DI)이란?

- 소프트웨어 공학에서 사용되는 디자인 패턴 중 하나
- 객체 간의 의존성을 외부로부터 주입하는 방식을 의미

### 객체 의존성(Object Dependencies)이란?

```java
class A {

    public void doSomething() {
	// ...
    }
}

class B {
    private A a;

    // 생성자 주입을 이용한 의존성 주입
    public B(A a) {
        this.a = a;
    }

    public void useA() {
        a.doSomething();
    }
}

public class Main {

    public static void main(String[] args) {

        // B 객체 생성
        A a = new A();

        // B 객체 생성 및 의존성 주입
        B b = new B(a);

        // 의존성을 사용하는 메서드 호출
        b.useA();
    }
}
```



## 참고 자료

- ["[Spring] 스프링 의존성 주입(DI) 이란?", Gyun's 개발일지, 2020-07-21](https://devlog-wjdrbs96.tistory.com/165){: target="_blank" }