---
title: "[CS]Generic은 왜 탄생했는가"
categories: [CS]
tags: [CS]
---

# Generic은 왜 탄생했는가

## 타입 안전성 강화

```java
// 제네릭을 사용하지 않은 경우
List list = new ArrayList();
list.add("Hello");
list.add(123); // 컴파일러는 타입 오류를 발견하지 못함

String str = (String) list.get(1); // 런타임에 ClassCastException 발생

// 제네릭을 사용한 경우
List<String> list = new ArrayList<>();
list.add("Hello");
// list.add(123); // 컴파일 오류 발생

String str = list.get(0); // 안전하게 String 타입으로 반환
```

- Generic을 사용하면 컴파일 시점에 타입을 검사할 수 있음
  + 런타임에서 발생할 수 있는 타입 오류를 방지할 수 있음
 예를 들어, 컬렉션 클래스에서 특정 타입의 객체만 다루도록 함으로써 타입 오류를 줄일 수 있습니다.

## 코드 재사용성 향상

```java
// 제네릭을 사용하지 않은 경우
class IntegerStack {

    private Stack<Integer> stack = new Stack<>();

    public void push(Integer value) {
        stack.push(value);
    }
    public Integer pop() {
        return stack.pop();
    }
}

class StringStack {

    private Stack<String> stack = new Stack<>();

    public void push(String value) {
        stack.push(value);
    }
    public String pop() {
        return stack.pop();
    }
}

// 제네릭을 사용한 경우
class GenericStack<T> {

    private Stack<T> stack = new Stack<>();

    public void push(T value) {
        stack.push(value);
    }
    public T pop() {
        return stack.pop();
    }
}

// 사용 예시
GenericStack<Integer> integerStack = new GenericStack<>();
integerStack.push(123);

GenericStack<String> stringStack = new GenericStack<>();
stringStack.push("Hello");
```

- 동일한 코드 베이스에서 여러 타입을 처리할 수 있음
  + 코드 중복을 줄이고, 보다 일반화된 코드를 작성 가능

## 가독성 및 유지보수성 향상

```java
// 제네릭을 사용하지 않은 경우
List list = new ArrayList();
list.add("Hello");
String str = (String) list.get(0);

// 제네릭을 사용한 경우
List<String> list = new ArrayList<>();
list.add("Hello");
String str = list.get(0);
```

- 의도를 명확히 하고 코드 가독성을 높임

## 캐스팅 불필요

```java
// 제네릭을 사용하지 않은 경우
List list = new ArrayList();
list.add("Hello");
String str = (String) list.get(0); // 타입 캐스팅 필요

// 제네릭을 사용한 경우
List<String> list = new ArrayList<>();
list.add("Hello");
String str = list.get(0); // 타입 캐스팅 불필요
```

- 불필요한 타입 캐스팅을 방지
  + Generic 없이 객체를 사용할 때는 적절한 타입으로 캐스팅해야 하지만, Generic을 사용하면 컴파일러가 자동으로 올바른 타입을 처리

## 일관된 API 제공

```java
// 제네릭을 사용하지 않은 경우
class NonGenericBox {

    private Object content;

    public void set(Object content) {
        this.content = content;
    }
    public Object get() {
        return content;
    }
}

// 제네릭을 사용한 경우
class GenericBox<T> {

    private T content;

    public void set(T content) {
        this.content = content;
    }
    public T get() {
        return content;
    }
}

// 사용 예시
GenericBox<String> stringBox = new GenericBox<>();
stringBox.set("Hello");
String content = stringBox.get(); // 명확하게 String 타입으로 반환

GenericBox<Integer> intBox = new GenericBox<>();
intBox.set(123);
Integer number = intBox.get(); // 명확하게 Integer 타입으로 반환
```

- 일관성 있는 API를 설계 가능