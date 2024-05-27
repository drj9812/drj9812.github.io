---
title: "[Design Pattern]싱글톤(Singleton)"
categories: [Software Architecture, Design Patterns]
tags: [Software Architecture, Design Pattern, 디자인 패턴, Singleton, 싱글톤]
---

# 싱글톤(Singleton)

## 싱글톤(Singleton)이란?

- 소프트웨어 디자인에서 사용되는 디자인 패턴 중 하나
- **특정 클래스의 인스턴스가 오직 하나만 생성되도록 보장하는 패턴**
	+ 해당 클래스의 인스턴스를 전역으로 사용하고자 할 때 유용

## 예시

```java
public class Singleton {

    private String name = "싱글톤";
    
    public String getName() {
        return this.name;
    }
    
    public void setName(String name) {
        this.name = name;
    }
    
    private static Singleton instance = null;
    
    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {

            instance = new Singleton();
        }
        return instance;
    }
}
```

```java
public class SingletonTest {

    public static void main(String[] args) {

        Singleton singleton1 = Singleton.getInstance();
        System.out.println(singleton1.getName()); // 싱글톤

        Singleton singleton2 = Singleton.getInstance();
        System.out.println(singleton2.getName()); // 싱글톤
	
	singleton2.setName("싱글톤 이름 변경");
        System.out.println(singleton1.getName()); // 싱글톤 이름 변경
        System.out.println(singleton2.getName()); // 싱글톤 이름 변경
    }
}
```

## 장점

- 여러 곳에서 객체를 생성하더라도 항상 동일한 객체에 접근할 수 있으므로 **인스턴스의 유일성이 보장**됨
- 싱글톤 패턴으로 생성된 인스턴스는 **전역적으로 접근**할 수 있으므로, 다른 클래스에서 이 인스턴스에 접근하여 상태를 공유할 수 있음
	+ 여러 클래스에서 동일한 필드 값을 사용하고자 할 때, 해당 필드 값을 변경하면 **모든 곳에서 변경된 값을 참조**할 수 있음
- 생성자가 `private`으로 선언되어 있어 외부에서 직접적으로 인스턴스를 생성할 수 없으므로, 싱글톤 클래스의 **인스턴스 생성을 제어**할 수 있음
- `synchronized` 키워드를 사용하면 multi-thread 환경에서의 **thread 안전성**을 보장됨
	+ 여러 thread가 동시에 안전하게 싱글톤 인스턴스를 생성하고 반환할 수 있음
	+ 프로그램이 실행될 때 필요한 시점에만 인스턴스를 생성하므로 **자원을 효율적으로 활용**할 수 있음

## 단점

- **인스턴스를 전역적으로 접근**할 수 있게 하므로, 프로그램의 여러 부분에서 이를 **남용**할 수 있음
- 다른 클래스에서 사용되는 싱글톤 인스턴스에 의존하는 경우에는 목(mock) 객체를 사용하여 **테스트를 수행하기 어려울 수 있음**
	+ 싱글톤 인스턴스는 자원을 공유하고 있기 때문에 테스트 시행 시 목 객체가 아닌 실제 싱글톤 인스턴스에 접근하게 됨
- multi-thread 환경에서 싱글톤 인스턴스의 안정성을 보장하기 위해 고려되는 **동기화(`synchronized`) 방법이 성능 저하를 초래**할 수 있음
- 싱글톤 패턴을 사용하면 해당 클래스와 다른 클래스 간의 **의존성이 증가**할 수 있음
	+ 유연성과 재사용성이 저하될 수 있음
- **OOP의 SOLID 원칙을 위배**할 수 있음
	+ 단일 책임 원칙(Single Responsibility Principle)
		* 싱글톤 클래스는 해당 클래스의 인스턴스 생성과 관리 뿐만 아니라 다른 역할도 수행하기 때문에, 이를 분리하거나 재사용하기 어려울 수 있음
	+ 의존성 역전 원칙 (Dependency Inversion Principle)
	+ 개방 폐쇄 원칙 (Open Closed Principle)
- 코드의 유연성이 떨어짐
	+ 자식 클래스를 만들 수 없음
	+ 내부 상태를 변경하기 어려움

## 구현 방법

### 공통 특성

- `private static` 필드
	+ `private`: 필드의 값을 변경하는 것은 클래스 내부에서만 가능하도록 하기 위함
	+ `static`: 클래스 로드 시점에 메서드 영역의 메모리에 미리 변수를 생성하기 위함
- `private` 생성자
	+ 외부의 인스턴스 생성을 제어하기 위함
- `public static` 메서드
	+ `public`: 외부 접근을 허용하기 위함
	+ `static`: 싱글톤 객체가 생성되기 전에 호출할 수 있도록 하기 위함

### Eager Initialization

```java
public class Singleton {
    
    private static final Singleton instance = new Singleton();
    
    private Singleton() {}
 
    public static Singleton getInstance() {
        return instance;
    }
}
```

- **클래스 로딩 단계에서 인스턴스를 생성**
- 인스턴스를 사용하지 않더라도 인스턴스를 생성하기 때문에 자원이 낭비됨
	+ File System, Database Connection와 같은 큰 리소스를 다룰 때 적합하지 않음
- `Exception`에 대한 핸들링을 제공하지 않음


### Static Block Initialization

```java
public class Singleton {
 
    private static Singleton instance;
    
    private Singleton() {}
    
    static {
        try {
            instance = new Singleton();

        } catch (Exception e) {
            throw new RuntimeException("Exception occured in creating singleton instance");
        }
    }
    
    public static Singleton getInstance(){
        return instance;
    }
}
```

- Eager Initialization과 유사하지만 `static` 블럭을 통해서 `Exception`에 대한 핸들링을 제공
- Eager Initialization와 마찬가지로 인스턴스를 사용하지 않더라도 인스턴스를 생성하기 때문에 자원이 낭비됨
	+ 큰 리소스를 다룰 때 적합하지 않음

### Lazy Initialization

```java
public class Singleton {

    private static Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {

            instance = new Singleton();
        }
        return instance;
    }
}
```

- 요청이 있을 때까지 인스턴스를 생성하지 않고 **필요할 때 생성**
	+ 인스턴스를 사용하지 않을 때 **자원을 절약**할 수 있음
- **multi-thread 환경에서 안정성을 보장받지 못함**
	+ 인스턴스가 생성되지 않은 시점에서 여러 thread에서 동시에 인스턴스를 생성하려 한다면 경쟁 조건(Race Condition)이 발생할 수 있음
		* 여러 thread에서 인스턴스가 생성될 수 있음(싱글톤 패턴 위반)
	+ 안정성을 보장받기 위해서는 **동기화(`synchronized`)를 고려**해야 함
- single-thread 환경이 보장됐을 때 사용

### Thread Safe Singleton

```java
public class Singleton {
    private static Singleton instance;

    private Singleton() {}

    public static synchronized Singleton getInstance() {
        if (instance == null) {

            instance = new Singleton();
        }
        return instance;
    }
}
```

- Lazy Initialization의 thread-safety 문제를 해결한 방식
- **동기화(`synchronized`)함으로써 multi-thread 환경에서 thread-safety를 보장받음**
	+ 오직 하나의 thread에서 접근 가능
	+ 여러 thread가 동시에 안전하게 싱글톤 인스턴스를 생성하고 반환할 수 있음
	+ 프로그램이 실행될 때 필요한 시점에만 인스턴스를 생성하므로 **자원을 효율적으로 활용**할 수 있음
- **`synchronized` 제어자로 인해 성능이 저하**될 수 있음
	+ [[Java]제어자(Modifier)](https://drj9812.github.io/posts/modifier/){: target="_blank" } 참조

### Double-Checked Locking

```java
public class Singleton {
    private static volatile Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {

                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

- Thread Safe Singleton을 개선한 방식
- **인스턴스 생성 코드를 동기화 블럭 내부에서만 실행**하여 인스턴스가 이미 생성되었는지 확인한 후에 인스턴스를 생성
	+ **동기화의 비용을 줄일 수 있음**

### Bill Pugh Singleton Implementaion

```java
public class Singleton {
 
    private Singleton() {}
    
    private static class SingletonHelper {
        private static final Singleton INSTANCE = new Singleton();
    }
    
    public static Singleton getInstance() {
        return SingletonHelper.INSTANCE;
    }
}
```

- Bill Pugh가 고안한 방식으로 `private inner static` Helper 클래스를 사용
- 기존의 방식들이 가지고 있던 대부분의 문제점들을 해결한 방식으로, **현재 가장 널리 쓰이는 싱글톤 구현 방식**
- 싱글톤 클래스가 로드될 때에도 로드되지 않다가 `getInstance()` 메서드가 호출됐을 때 비로소 JVM 메모리에 로드되고, 인스턴스를 생성하게 됨
- **동기화(`synchronized`)하지 않았기 때문에 성능이 저하되지 않음**

### Enum Singleton

```java
public enum EnumSingleton {
 
    INSTANCE;
    
    public static void doSomething() {
        //do something
    }
}
```

- Joshua Bloch가 고안한 방식으로 `Enum`으로 싱글톤을 구현
- 기존의 방식들이 Java의 Reflection API를 이용하여 싱글톤 패턴의 원칙을 깨트릴 수 있다는 문제점을 해결
- 클래스 로딩 단계에서 인스턴스를 생성하기 때문에 낭비가 발생할 수 있음

## 참고자료

- [Ready Kim, "[생성 패턴] 싱글톤(Singleton) 패턴을 구현하는 6가지 방법", 준비된 개발자, 2019-11-04](https://readystory.tistory.com/116){: target="_blank" }
- [2기_보스독, "싱글톤(Singleton) 패턴이란?", Tecoble, 2022-11-07](https://tecoble.techcourse.co.kr/post/2020-11-07-singleton/){: target="_blank" }