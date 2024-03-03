---
title: "[Java]Class 클래스"
categories: [공부, Java]
tags: [Java, 자바, Class 클래스, Refleciton API]
---

# Class 클래스

## Class 클래스란?

자바의 모든 클래스와 인터페이스는 컴파일러에 의해 자바 파일(`.java`)에서 클래스 파일(`.class`)로 변환된다. 컴파일된 클래스 파일은 클래스와 인터페이스의 정보(클래스 이름, 필드, 메서드 등)를 유지한 채 시스템 클래스패스(system classpath)에 저장되는데, `Class` 클래스를 사용하면 JVM의 클래스 로더(시스템 클래스 로더)가 클래스 파일을 로드하고 그 결과로 클래스 파일에 담긴 정보들을 사용할 수 있게 된다. 만약 클래스 로더(시스템 클래스 로더)가 클래스 파일을 찾지 못한다면  `ClassNotFoundException` 에러가 발생한다.

> Java API 기본 클래스는 시스템 클래스 로더가 아닌 부트스트랩 클래스 로더에 의해 JVM에 미리 내장되어 있다.
{: .prompt-tip }

## 클래스 정보 가져오기

### Object.getClass()

```java
package study;

class Car {}

public class ClassClass {
    public static void main(String[] args) {

    Car car = new Car();
	    
    Class<?> carClass = car.getClass();
    System.out.println(carClass); // class study.Car
    }
}
```

### 리터럴

```java
package study;

class Car {}

public class ClassClass {
    public static void main(String[] args) {

    Class<?> carClass = study.Car.class;
    System.out.println(carClass); // class study.Car
    }
}
```

### Class.forName()

```java
package study;

class Car {}

public class ClassClass throws ClassNotFoundException {
    public static void main(String[] args) {

    Class<?> carClass = Class.forName("study.Car");
    System.out.println(carClass); // class study.Car
    }
}
```

- static 메서드
- `forName()` 메서드는 인자로 패키지명을 포함한 클래스명(fully qualified name)을 요구
- fully qualified name이 아닐 경우 `ClassNotFoundException` 예외가 발생할 수 있으므로 예외처리가 강제됨

## Class 클래스 메서드

### 클래스 정보 관련 메서드

```java
package study;

class Car {}

public class ClassClass {
    public static void main(String[] args) throws ClassNotFoundException {

    Class<?> carClass = Class.forName("study.Car");

    System.out.println(carClass.getName()); // study.Car
    System.out.println(carClass.getSimpleName()); // Car
    System.out.println(carClass.getPackageName()); // study
    }
}
```

- `getName()`: 패키지 이름을 포함한 클래스 전체 이름
- `getSimpleName()`: 패키지 이름을 제외한 간단한 이름
- `getPackageName()`: 패키지 이름


### <a id=anchor1>클래스 멤버 관련 메서드</a>

```java
package study;

import java.lang.reflect.Field;
import java.lang.reflect.Method;
import java.lang.reflect.Constructor;

class Car {
    private int number;
    
    public int getNumber() {
        return number;
    }
    
    public Car() {}
    
    public Car(int number) {
    	this.number = number;
    }
}

public class ClassClass {
    public static void main(String[] args) throws Exception {
        Class<?> carClass = Class.forName("study.Car");
    	
	System.out.println(carClass.getName()); // study.Car
	System.out.println(carClass.getSimpleName()); // Car
	System.out.println(carClass.getPackageName()); // study

	    
        for (Field field : carClass.getDeclaredFields()) {
            System.out.println(field); // private int study.Car.number
        }
        
        for (Method method : carClass.getDeclaredMethods()) {
            System.out.println(method); // public void study.Car.getNumber()
        }
        
        // Car car = (Car) carClass.newInstance();
        
        Constructor<?> constructor = carClass.getDeclaredConstructor();
        Car car = (Car) constructor.newInstance(); // 기본 생성자를 통한 객체 생성
        System.out.println(car.getNumber()); // 0
    }
}
```

- `getDeclaredFields()`: 필드의 접근 제어자, 타입, 패키지 이름을 포함한 필드의 이름을 배열로 반환
- `getDeclaredMethods()`: 메서드의 접근 제어자, 반환 타입, 패키지 이름을 포함한 메서드의 이름을 배열로 반환
- `getDeclaredConstructor():` 기본 생성자를 반환

> `getDeclaredXXX()` 메서드들은 접근 제어자를 무시한다. 위 코드에서 `number`가 `private`로 선언됐음에도 불구하고 접근할 수 있었던 이유다. 반면에, `getFields()`, `getMethods()`, `getConstructor()`와 같은 `getXXX()` 메서드들은 `public`으로 선언된 멤버에게만 접근할 수 있다.
{: .prompt-info }

> Class 타입의 newInstance() 메소드는 deprecated
{: .prompt-info }

## Reflection API

### Reflection API란?

> *reflection*
>
> *1. (거울 등에 비친) 상[모습]*
>
> *2. (빛·열·소리 등의) 반사, 반향*
>
> *3. (상태·속성 등의) 반영*

Reflection API는 JVM의 클래스 로더(시스템 클래스 로더)가 로드한 클래스 파일의 정보를 거울 삼아 **런타임에 동적으로 객체를 생성하고 조작하는 자바 프로그래밍 기술**이다. 이때의 거울이 클래스 파일의 정보를 담을 수 있는 `Class` 클래스다. 즉, 클래스의 구체적인 정보를 알지 못해도 클래스 파일에 클래스에 대한 정보가 담겨있기 때문에 `Class` 클래스를 통해 해당 정보에 접근할 수 있는 것이다.

```java
package study;

import java.lang.reflect.Field;
import java.lang.reflect.Method;
import java.lang.reflect.Constructor;

class Car {
    private int number;
    
    public int getNumber() {
        return number;
    }
    
    public Car() {}
    
    public Car(int number) {
    	this.number = number;
    }
}

public class ClassClass {
    public static void main(String[] args) throws Exception {
        Class<?> carClass = Class.forName("study.Car");
    	
	System.out.println(carClass.getName()); // study.Car
	System.out.println(carClass.getSimpleName()); // Car
	System.out.println(carClass.getPackageName()); // study

	    
        for (Field field : carClass.getDeclaredFields()) {
            System.out.println(field); // private int study.Car.number
        }
        
        for (Method method : carClass.getDeclaredMethods()) {
            System.out.println(method); // public void study.Car.getNumber()
        }
        
        // Car car = (Car) carClass.newInstance();
        
        Constructor<?> constructor = carClass.getDeclaredConstructor();
        Car car = (Car) constructor.newInstance(); // 기본 생성자를 통한 객체 생성
        System.out.println(car.getNumber()); // 0
    }
}
```

[클래스 멤버 관련 메서드](#anchor1)에서 사용한 위 코드에서 이미 Reflection API가 사용되었다. `import`문을 보면 알 수 있듯이, `Field` 클래스, `Method` 클래스, `Constructor` 클래스가 `reflect` 패키지에 속한다. Reflection API가 이 `reflect` 패키지를 사용하는 것이다.

 <div style="display: flex;">
    <figure style="flex: 1; margin-left: 10px;">
        <img src="/assets/img/posts/study/java/class-class/01-field-class.png" style="width: 100%;" alt="01-field-class">
        <figcaption>Field.class</figcaption>
    </figure>
    <figure style="flex: 1; margin-right: 10px;">
        <img src="/assets/img/posts/study/java/class-class/02-getdeclaredfields().png" style="width: 100%;" alt="02-getdeclaredfields()">
        <figcaption>Class.class</figcaption>
    </figure>
</div>

뿐만 아니라, 클래스의 멤버들을 가져올 때 사용되어지는 `getDeclaredFields()`와 같은 메서드들은 `Class` 클래스에 속하지만, 역시 `Class` 클래스 내부에서 `reflect` 패키지를 사용하고 있다.

> [[Java]Reflection API](https://drj9812.github.io/posts/reflection-api/){: target="_blank" } 참조
{: .prompt-tip }