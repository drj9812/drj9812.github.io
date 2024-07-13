---
title: "[Java]Wrapper 클래스"
categories: [Language, Java]
tags: [Java, 자바, Wrapper 클래스]
image:
  path: /assets/img/posts/language/java/01-java-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Java
---

# Wrapper 클래스

## Wrapper 클래스란?

| 기본형 타입 | Wrapper 클래스 |
|:----------:|:--------------:|
|   boolean  |     Boolean    |
|    byte    |      Byte      |
|    short   |      Short     |
|     int    |     Integer    |
|    long    |      Long      |
|    float   |      Float     |
|   double   |     Double     |
|    char    |    Character   |

- 기본형 타입(primitive type)을 객체로 다룰 수 있도록 해주는 클래스

## Wrapper 클래스가 필요한 이유

### 객체 지향 프로그래밍의 일관성

Java는 객체 지향 언어로, 모든 데이터는 객체로 처리되는 것이 이상적이다. 기본형 타입은 객체가 아니기 때문에, 객체 지향 프로그래밍의 일관성을 유지하려면 객체로 변환할 필요가 있다.

### 컬렉션 프레임워크와의 호환성

```java
// 기본형 타입의 int를 ArrayList에 저장할 수 없음
// List<int> numbers = new ArrayList<int>(); // 컴파일 에러

// Wrapper 클래스를 사용하여 ArrayList에 저장 가능
List<Integer> numbers = new ArrayList<>();

// 기본형 값을 Wrapper 클래스로 변환하여 추가
numbers.add(10);  // Integer.valueOf(10)로 자동 변환됨
numbers.add(20);
numbers.add(30);
```

Java의 컬렉션 프레임워크는 객체만을 저장할 수 있다. 따라서 기본 데이터 타입을 컬렉션에 저장하려면 wrapper 클래스를 사용해야 한다. 예를 들어, `ArrayList<int>` 대신 `ArrayList<Integer>`를 사용해야 한다.

### 유틸리티 메서드 제공

```java
// 기본형 int를 Wrapper 클래스로 변환
Integer num = Integer.valueOf(123);
System.out.println(num); // 123

// Wrapper 클래스의 메서드 사용
String binaryString = Integer.toBinaryString(num); // 2진수 문자열 변환
System.out.println(binaryString); // 1111011

// 문자열을 기본형으로 변환
int parsedValue = Integer.parseInt("456");
System.out.println(parsedValue); // 456
```

Wrapper 클래스는 다양한 유틸리티 메서드를 제공한다. 예를 들어, `Integer` 클래스는 문자열을 정수로 변환하는 `parseInt()` 메서드를 제공하며, `Double` 클래스는 실수의 최대값, 최소값 등을 확인할 수 있는 메서드를 제공한다.

### 상수 정의

Wrapper 클래스는 데이터 타입의 상수를 정의한다. 예를 들어, `Integer.MAX_VALUE`는 int 타입의 최대값을 정의하며, 이를 통해 코드의 가독성을 높이고 하드코딩된 값을 피할 수 있다.

### null 값 지원

기본형 타입은 `null` 값을 가질 수 없지만, wrapper 클래스는 `null` 값을 가질 수 있다. 이로 인해 객체의 존재 여부를 체크하거나 초기화 상태를 나타내는 데 유용할 수 있다.

## 박싱(Boxing)과 언박싱(Unboxing)

```java
// 박싱
Integer integerObj = Integer.valueOf(100);

// 언박싱
int intValue = integerObj.intValue();

// 기본형 연산 후 박싱
intValue = intValue + 100;
integerObj = Integer.valueOf(intValue);
```

- Boxing: 기본형 타입의 데이터 → Wrapper 클래스 타입의 데이터
- UnBoxing: Wrapper 클래스 타입의 데이터 → 기본형 타입의 데이터

### 방식

```java
Integer integerObj1 = Integer.valueOf(100);
Integer integerObj2 = Integer.valueOf(100);

System.out.println(integerObj1 == integerObj2); // true
```

`Integer.valueOf(int i)` 메서드는 주어진 기본형 `int` 값을 `Integer` 객체로 변환한다. 이 메서드는 **캐싱(Caching)**을 사용하여 성능을 최적화한다. 자바에서는 기본형 값이 특정 범위 내에 있을 때(보통 -128부터 127까지) `Integer.valueOf()` 메서드가 같은 객체를 재사용한다. **즉, 동일한 값에 대해 Integer 객체를 새로 생성하지 않고, 기존의 객체를 반환**한다.

Java는 `Integer.valueOf()` 메서드에 대해 자동 캐싱을 제공하므로, 메모리와 성능 측면에서 더 효율적이다. 기본형 `int` 값이 캐싱 범위 내에 있는 경우, 새로운 객체를 생성하는 대신 캐시된 객체를 반환한다.

```java
Integer integerObj1 = new Integer(100);
Integer integerObj2 = new Integer(100);

System.out.println(integerObj1 == integerObj2); // false
```

`new Integer(int i)` 생성자는 새로운 `Integer` 객체를 명시적으로 생성한다. 이 방식은 항상 새로운 객체를 생성하며, Java의 오토박싱(autoboxing) 및 캐싱 기능을 우회한다.

매번 새로운 객체를 생성하기 때문에 메모리 사용이 비효율적일 수 있다. Java 9 이후부터 `new Integer(int i)`는 deprecated 되어있다. 이는 이 방식이 비효율적이기 때문에, `Integer.valueOf()` 메서드를 사용하는 것이 권장된다.

### 자동 박싱(AutoBoxing)과 자동 언박싱(AutoUnBoxing)

```java
// 자동 박싱(int → Integer)
Integer integerObj = 10; // Integer.valueOf(10);

// 자동 언박싱(Integer → int)
int intValue = integerObj; // integerObj.intValue();
```

- Java 1.5에서 도입된 기본형 타입과 Wrapper 클래스 간의 변환을 자동으로 처리하는 기능