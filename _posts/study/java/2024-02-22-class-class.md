---
title: "[Java]Class 클래스"
categories: [Study, Java]
tags: [Java, 자바, Class 클래스, Refleciton API]
image:
  path: /assets/img/posts/study/java/01-java-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Java
---

# Class 클래스

## Class 클래스란?

자바의 모든 클래스와 인터페이스는 컴파일러에 의해 자바 파일(`.java`{: .filepath })에서 클래스 파일(`.class`{: .filepath })로 변환된다. 컴파일된 클래스 파일은 클래스와 인터페이스의 정보(클래스 이름, 필드, 메서드 등)를 유지한 채 시스템 클래스패스(system classpath)에 저장되는데, `Class` 클래스를 사용하면 JVM의 클래스 로더(시스템 클래스 로더)가 클래스 파일을 로드하고 그 결과로 클래스 파일에 담긴 정보들을 사용할 수 있게 된다. 만약 클래스 로더(시스템 클래스 로더)가 클래스 파일을 찾지 못한다면 `ClassNotFoundException` 에러가 발생한다.

> Java API 기본 클래스는 시스템 클래스 로더가 아닌 부트스트랩 클래스 로더에 의해 JVM에 미리 내장되어 있다.
{: .prompt-tip }

## 클래스 정보 가져오기

### Object.getClass()

```java
package classclass;

class Car {}

public class ClassClass {

    public static void main(String[] args) {

    Car car = new Car();
	    
    Class<?> carClass = car.getClass();
    System.out.println(carClass); // class classclass.Car
    }
}
```

### 리터럴

```java
package classclass;

class Car {}

public class ClassClass {

    public static void main(String[] args) {

    Class<?> carClass = classclass.Car.class;
    System.out.println(carClass); // class classclass.Car
    }
}
```

### Class.forName()

```java
package classclass;

class Car {}

public class ClassClass throws ClassNotFoundException {

    public static void main(String[] args) {

    Class<?> carClass = Class.forName("classclass.Car");
    System.out.println(carClass); // class classclass.Car
    }
}
```

- static 메서드
- `forName()` 메서드는 인자로 패키지명을 포함한 클래스명(fully qualified name)을 요구
- fully qualified name이 아닐 경우 `ClassNotFoundException` 예외가 발생할 수 있으므로 예외처리가 강제됨

## Class 클래스 메서드

### 클래스 정보 관련 메서드

```java
package classclass;

class Car {}

public class ClassClass {

    public static void main(String[] args) throws ClassNotFoundException {

    Class<?> carClass = Class.forName("classclass.Car");

    System.out.println(carClass.getName()); // classclass.Car
    System.out.println(carClass.getSimpleName()); // Car
    System.out.println(carClass.getPackageName()); // classclass
    }
}
```

- `getName()`: 패키지 이름을 포함한 클래스 전체 이름
- `getSimpleName()`: 패키지 이름을 제외한 간단한 이름
- `getPackageName()`: 패키지 이름


### <a id=anchor1>클래스 멤버 관련 메서드</a>

```java
package classclass;

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

        Class<?> carClass = Class.forName("classclass.Car");
    	
	System.out.println(carClass.getName()); // classclass.Car
	System.out.println(carClass.getSimpleName()); // Car
	System.out.println(carClass.getPackageName()); // classclass

	    
        for (Field field : carClass.getDeclaredFields()) {
            System.out.println(field); // private int classclass.Car.number
        }
        
        for (Method method : carClass.getDeclaredMethods()) {
            System.out.println(method); // public void classclass.Car.getNumber()
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
- `getDeclaredConstructor():` 생성자를 반환
	+ 인자를 넘기지 않는다면 기본 생성자를 반환하고, 인자를 넘긴다면 파라미터가 있는 생성자를 반환

> Class 타입의 newInstance() 메소드는 deprecated
{: .prompt-info }

### getXXX(), getDeclaredXXX() 차이

#### 접근 수준

```java
package classclass;

public class External {

    private int privateNumber = 10;
}
```

```java
package classclass;

import java.lang.reflect.Field;

public class ClassClass {

    public static void main(String[] args) throws Exception {

        // Reflection API를 통해 외부 클래스 객체 생성
        Class<?> externalClass = Class.forName("classclass.External");

        // getDeclaredField()를 사용하여 외부 클래스의 private 필드 가져오기
        Field externalField = externalClass.getDeclaredField("privateNumber");

        // getField()를 사용하여 외부 클래스의 private 필드 가져오기
	Field externalField = externalClass.getField("privateNumber"); // `NoSuchFieldException` 에러. getXXX() 메서드는 private 멤버를 가져올 수 없음

        System.out.println(externalField); // private int classclass.External.privateNumber

        // 외부 클래스의 생성자 가져오기
        Constructor<?> constructor = externalClass.getDeclaredConstructor();
        
        // 외부 클래스의 생성자로 외부 클래스 객체 생성
        External externalInstance = (External) constructor.newInstance(); // 기본 생성자를 통한 객체 생성

	// private 필드에 접근할 수 있도록 설정
        externalField.setAccessible(true);
		
	int value = (int) externalField.get(externalInstance);
	System.out.println(value); // 10
	}
}
```

- `getDeclaredXXX()`: 접근 제어자에 상관없이 해당 클래스에서 선언된 모든 멤버를 반환
- `getXXX()`:  해당 클래스에서 `public` 접근 제어자로 선언된 멤버만을 반환

> `getDeclaredXXX()` 메서드는 접근 제어자에 상관없이 멤버의 "정보"에 접근할 수 있는 것이지 멤버의 값에 접근하는 것이 아니다. `private` 멤버의 정보가 아닌 값에 접근하기 위해서는 먼저 `setAccessible()` 메서드를 통해 `private` 멤버의 값에 접근할 수 있도록 설정해야 한다. 만약 설정하지 않는다면 `java.lang.IllegalAccessException` 에러가 발생한다.
{: .prompt-tip }

#### 상속된 필드

```java
package classclass;

public class External {

    private int privateNumber = 10;
    public int publicNumber = 20;
}

class ExternalSubclass extends External {}
```

```java
package classclass;

import java.lang.reflect.Field;

public class ClassClass {

    public static void main(String[] args) throws Exception {

        // Reflection API를 통해 외부 클래스를 상속받은 클래스 객체 생성
        Class<?> externalSubclass = Class.forName("classclass.ExternalSubclass");

	// getDeclaredField()를 사용하여 외부 클래스를 상속받은 클래스의 private 필드 가져오기
        Field externalField = externalSubclass.getDeclaredField("privateNumber"); // java.lang.NoSuchFieldException 에러. getDeclaredXXX() 메서드는 상속 받은 멤버를 가져올 수 없음
		
	// getField()를 사용하여 외부 클래스를 상속받은 클래스의 private 필드 가져오기
        Field externalField = externalSubclass.getField("privateNumber"); // NoSuchFieldException 에러. getXXX() 메서드는 private 멤버를 가져올 수 없음
		
        // getField()를 사용하여 외부 클래스를 상속받은 클래스의 public 필드 가져오기
        Field externalField = externalSubclass.getField("publicNumber"); // public int study.External.publicNumber
	}
}
```

- `getDeclaredXXX()`: 상속된 필드를 포함하지 않고, 오직 해당 클래스에서 직접 선언된 필드만을 반환
- `getXXX()`: 상속된 필드까지 모두 반환

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

클래스의 멤버들을 가져올 때 사용되는 `Class` 클래스의 `getDeclaredFields()`와 같은 메서드들도 내부적으로는 `reflect` 패키지를 사용하고 있다.

> [[Java]Reflection API](https://drj9812.github.io/posts/reflection-api/){: target="_blank" } 참조
{: .prompt-tip }