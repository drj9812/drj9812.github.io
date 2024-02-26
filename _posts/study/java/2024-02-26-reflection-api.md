---
title: Reflection API
categories: [공부, Reflection API]
tags: [Java, 자바, Reflection API]
---

# Reflection API

## 참조

- [[Java]Class 클래스](https://drj9812.github.io/posts/class-class/){: target="_blank" }
- [[Java]JPA](https://drj9812.github.io/posts/jpa/){: target="_blank" }

## Reflection API란?

> *reflection*
>
> *1. (거울 등에 비친) 상[모습]*
>
> *2. (빛·열·소리 등의) 반사, 반향*
>
> *3. (상태·속성 등의) 반영*

Reflection API는 JVM의 클래스 로더(시스템 클래스 로더)가 로드한 클래스 파일의 정보를 거울 삼아 **런타임에 동적으로 객체를 생성하고 조작하는 자바 프로그래밍 기술**이다. 이때의 거울이 클래스 파일의 정보를 담을 수 있는 `Class` 클래스다. 즉, 클래스의 구체적인 정보를 알지 못해도 클래스 파일에 클래스에 대한 정보가 담겨있기 때문에 `Class` 클래스를 통해 해당 정보에 접근할 수 있는 것이다.

## 예시

```java
package study;

import java.lang.reflect.Method;

class Car {
    private int number;
    
    public int getNumber() {
        return number;
    }
    
    Car() {}
    
    Car(int number) {
    	this.number = number;
    }
}

public class Reflection {
    public static void main(String[] args) throws Exception {
    	Object obj = new Car();
    	int number = obj.getNumber(); // 컴파일 에러
    }
}
```

- `Object obj = new Car();`
	+ `obj` 변수는 `Car` 클래스의 인스턴스를 참조하고 있지만, 변수의 타입이 컴파일 타임에 `Object`로 선언
	+ `obj` 변수는 `Object` 타입만 알 뿐, `Car` 타입은 알지 못하므로 `Car` 클래스의 정보에 접근할 수 없음
- `int number = obj.getNumber();`
	+ `obj` 변수는 `Object` 클래스에 정의된 메서드만 사용할 수 있으므로 컴파일 에러가 발생

```java
package study;

import java.lang.reflect.Method;

class Car {
    private int number;
    
    public int getNumber() {
        return number;
    }
    
    Car() {}
    
    Car(int number) {
    	this.number = number;
    }
}

public class Reflection {
    public static void main(String[] args) throws Exception {
    	Object obj = new Car();
//    	int number = obj.getNumber(); 컴파일 에러
    	
	// Reflection API
        Class carClass = Class.forName("study.Car");
        Method getNumber = carClass.getMethod("getNumber", null);
        
        int number = (int) getNumber.invoke(obj, null);
        System.out.println(number); // 0
    }
}
```

- `Class carClass = Class.forName("study.Car");`
	+ Reflection API를 사용하여 `Class` 클래스에 대한 `Class` 객체 가져옴
	+ `study.Car`는 클래스의 패키지명을 포함한 클래스명(fully qualified name)
- `Method getNumber = carClass.getMethod("getNumber", null);`
	+ Reflection API를 사용하여 `Car` 클래스에서 getNumber() 메서드를 가져옴
	+ `getMethod()` 메서드는 가져올 메서드의 이름, 타입을 인자로 요구
- `int number = (int) getNumber.invoke(obj, null);`
	+ `invoke()` 메서드를 사용하여 `getNumber()` 메서드를 호출하고 그 결과를 반환
	+ `invoke()` 메서드는 메서드를 호출할 객체와 호출할 메서드의 인자를 인자로 요구
	+ `invoke()` 메서드의 반환 타입은 `Object`이므로 `int`타입으로 캐스팅

## 단점

### 성능 저하

- 객체의 속성이나 메서드에 접근할 때 추가적인 오버헤드가 발생하기 때문에 성능 저하가 발생할 수 있음
- 메서드를 호출할 때, 메서드의 이름을 문자열로 지정하고, 매개변수 타입과 반환 타입을 알아내는 과정이 필요하기 때문에 추가적인 계산과 검색이 필요함
- `private`나 `protected` 멤버에 접근할 때, 접근 권한을 확인하고 접근을 허용해야 하는데 이 과정에서 오버헤드가 발생할 수 있음
- Refleciton을 사용하여 동적으로 접근하는 경우, 매번 Refleciton을 통해 메서드나 속성을 찾아야 하기 때문에이전에 호출한 메서드나 속성을 사용하는 캐싱의 이점을 활용할 수 없음

### 컴파일 타입 오류 감지의 어려움

- 컴파일 타임에 발견할 수 있는 오류가 런타임에 발생

### 보안 취약점

- 접근 지시자를 무시하고 객체의 멤버에 접근할 수 있기 때문에 보안 취약점이 발생할 수 있음

## 활용

### JPA

#### Entity 매핑

```java
import java.time.LocalDateTime;
import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.Table;

@Entity
@Table(name = "user")
public class UserEntity {

    @Id // primary Key
    private String email;

    @Column(name = "created_at")
    private LocalDateTime createdAt;

    // Getter and setter methods
}
```

위의 코드에서 `UserEntity` 클래스는 JPA의 `@Entity` 어노테이션을 사용하여 `Entity` 클래스로 지정되었다. JPA는 이 `Entity` 클래스를 **동적으로 로딩하고 분석하여 매핑 정보를 설정**한다.

JPA는 리플렉션을 사용하여 클래스의 필드, 메서드, 어노테이션 등의 정보를 가져온다. 예를 들어, `@Id` 어노테이션이 붙은 필드를 식별하여 이 필드가 데이터베이스의 기본 키(primary key)에 해당함을 인식한다. 또한, `@Column` 어노테이션을 사용하여 `Entity` 클래스의 필드와 데이터베이스 테이블의 컬럼 간의 매핑을 설정한다.

#### 쿼리 처리

JPA에서는 JPQL을 사용하여 `Entity`와 관련된 쿼리를 작성할 수 있다. 이때 JPQL 쿼리는 `Entity` 클래스의 속성과 필드를 기반으로 작성된다. JPA는 리플렉션을 사용하여 `Entity` 클래스의 메타데이터를 분석하고 JPQL 쿼리를 실행할 때 필요한 정보를 동적으로 얻어온다.

#### Entity 인스턴스 생성

JPA는 DB에서 가져온 결과를 `Entity` 객체로 변환하는 과정에서 리플렉션을 사용한다. `Entity` 클래스의 기본 생성자를 호출하여 `Entity` 인스턴스를 생성하고, 이후 리플렉션을 사용하여 `Entity` 필드에 DB에서 읽은 값을 설정한다.

#### 변경 감지(Dirty Check)

JPA는 `Entity`의 상태를 관리하는데, 이때도 리플렉션을 사용한다. `Entity`의 상태 변화를 감지하고 추적하기 위해 리플렉션을 사용하여 필드 값을 읽거나 변경한다. 이를 통해 JPA는 변경된 `Entity` DB에 반영하거나 관련된 `Entity`를 적절하게 로딩한다.

### Spring

#### 의존성 주입

#### 빈 등록

### Hibernate

####  매핑

### IDE

#### 자동완성 기능

### Lombok

### Jackson
