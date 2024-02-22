---
title: Class 클래스
categories: [공부, Class 클래스]
tags: [Java, 자바, Class 클래스]
---

# Class 클래스

자바의 모든 클래스와 인터페이스는 컴파일러에 의해 자바 파일(`.java`)에서 클래스 파일(`.class`)로 변환된다. 컴파일된 클래스 파일은 클래스와 인터페이스의 정보(클래스 이름, 필드, 메서드 등)를 유지한 채 시스템 클래스패스(system classpath)에 저장되는데, `Class` 클래스를 사용하면 JVM의 클래스 로더(시스템 클래스 로더)가 클래스 파일을 로드하고 그 결과로 클래스 파일에 담긴 정보들을 사용할 수 있게 된다. 만약 클래스 로더(시스템 클래스 로더)가 클래스 파일을 찾지 못한다면  `ClassNotFoundException` 에러가 발생한다.

> Java API 기본 클래스는 JVM이 실행될 때, 시스템 클래스 로더가 아닌 부트스트랩 클래스 로더에 의해 JVM에 미리 내장되어 있다.
{: .prompt-tip }

## 클래스 정보 가져오기

### Object.getClass()

```java
package study;

class Car {}

public class ClassClass {
	public static void main(String[] args) {

	    Car car = new Car();
	    
	    Class cls = car.getClass();
	    System.out.println(cls); // class study.Car
	}
}
```

### 리터럴

```java
package study;

class Car {}

public class ClassClass {
	public static void main(String[] args) {

	    Class cls = study.Car.class;
	    System.out.println(cls); // class study.Car
	}
}
```

### Class.forName()

```java
package study;

class Car {}

public class ClassClass throws ClassNotFoundException {
	public static void main(String[] args) {

	    Class cls = Class.forName("study.Car");
	    System.out.println(cls); // class study.Car
	}
}
```

- static 메서드
- `forName()` 메서드는 인자로 fully qualified name을 요구
- fully qualified name가 아닐 경우 예외가 발생할 수 있으므로 예외처리가 강제됨

## Class 클래스 메서드

### 클래스 정보 관련 메서드

```java
class Car {}

public class ClassClass {
	
	public static void main(String[] args) throws ClassNotFoundException {

	    Class cls = Class.forName("study.Car");

	    System.out.println(cls.getName()); // study.Car
	    System.out.println(cls.getSimpleName()); // Car
	    System.out.println(cls.getPackageName()); // study

	}
}
```

- `getName()`: 패키지 이름을 포함한 클래스 전체 이름
- `getSimpleName()`: 패키지 이름을 제외한 간단한 이름
- `getPackageName()`: 패키지 이름


### 클래스 멤버 관련 메서드

```java
package study;

import java.lang.reflect.Field;
import java.lang.reflect.Method;
import java.lang.reflect.Constructor;

class Car {
    private int number;
    
    public void getNumber() {
        System.out.println(number);
    }
    
    Car() {}
    
    Car(int number) {
    	this.number = number;
    }
    
}

public class ClassClass {
    public static void main(String[] args) throws Exception {

        Class cls = Class.forName("study.Car");
    	
	System.out.println(cls.getName()); // study.Car
	System.out.println(cls.getSimpleName()); // Car
	System.out.println(cls.getPackageName()); // study

	    
        for (Field field : cls.getDeclaredFields()) {
            System.out.println(field); // private int study.Car.number
        }
        
        for (Method method : cls.getDeclaredMethods()) {
            System.out.println(method); // public void study.Car.getNumber()
        }
        
        // Car car = (Car) cls.newInstance();
        
        Constructor<Car> constructor = cls.getDeclaredConstructor();
        Car instance = constructor.newInstance(); // 기본 생성자를 통한 객체 생성
        instance.getNumber(); // 0
    }
}
```

- `getDeclaredFields()`: 필드의 접근 제어자, 타입, 패키지 이름을 포함한 필드의 이름을 배열로 반환
- `getDeclaredMethods()`: 메서드의 접근 제어자, 반환 타입, 패키지 이름을 포함한 메서드의 이름을 배열로 반환
- `getDeclaredConstructor():` 기본 생성자를 반환

> `getDeclaredXXX()` 메서드들은 접근 제어자를 무시한다. 위 코드에서 `number`가 `private`로 선언됐음에도 불구하고 접근할 수 있었던 이유다. 반면에, `getFields()`, `getMethods()`, `getConstructor()`와 같은 `getXXX()` 메서드들은 `public`으로 선언된 멤버에게만 접근할 수 있다.
{: .prompt-info }

> Class 타입의 newInstance() 메소드는 deprecated
{: .prompt-tip }
