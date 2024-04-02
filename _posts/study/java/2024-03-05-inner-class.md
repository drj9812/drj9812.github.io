---
title: "[Java]내부(Inner) 클래스"
categories: [Study, Java]
tags: [Java, 자바, Inner 클래스]
---

# 내부(Inner) 클래스

```java
class A { // B의 외부 클래스
    // ...
    class B { // A의 내부 클래스
        // ...
    }
}
```

## 내부 클래스의 장점

```java
class A {

    int i = 100;
    B b = new B();

    Class B {

        void method() {
//	    A a = new A(); // B 클래스 내부 클래스가 아니라면 A 객체를 생성해야 함
//          System.out.println(a.i);

            System.out.println(i); // A 객체 생성없이 외부 클래스의 멤버에 접근
        }
    }
}

    Class C {

        B b = new B() // 에러. B 클래스에 접근할 수 없음
}

public class InnerTest {

    public static void main(String[] args) {

        B b = new B(); // 에러. B 클래스에 접근할 수 없음
        b.method(); // 에러. B 클래스에 접근할 수 없음
    }
}
```

- 내부 클래스에서 **외부 클래스의 멤버에 쉽게 접근**할 수 있음
	+ 객체 생성 없이 접근
- 코드의 간결화
- 캡슐화

## 내부 클래스의 종류와 특징

```java
class Outer {

    class InstanceInner { // ... } // 인스턴스 클래스
    static class StaticInner ( // ... } // 스태틱 클래스

    void Mymethod() {
        class LocalInner { // ... } // 지역 클래스
    }
}
```

- 내부 클래스의 종류와 유효범위(scope)는 변수와 동일

|           내부 클래스 종류          | 특징 |
|:-------------------------------------:|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 인스턴스 클래스(Instance Class) |  외부 클래스의 멤버 변수 선언 위치에 선언하며, 외부 클래스의 인스턴스 멤버처럼 다루어진다. 주로 외부 클래스의 인스턴스 멤버들과 관련된 작업에 사용될 목적으로 선언된다. |
|     스태틱 클래스(Static Class)    | 외부 클래스의 멤버 변수 선언 위치에 선언하며, 외부 클래스의 `static` 멤버처럼 다루어진다. 주로 외부 클래스의 `static` 멤버, 특히 `static` 메서드에서 사용될 목적으로 선언된다. |
|      지역 클래스(Local Class)      | 외부 클래스의 메서드나 초기화 블럭 안에 선언하며, 선언된 영역 내부에서만 사용될 수 있다.                                                                                                                  |
|  익명 클래스(Anonymous Class) | **클래스의 선언과 객체의 생성을 동시**에 하는 이름 없는 클래스(**일회용**)                                                                                                                                      |

## 내부 클래스의 제어자와 접근성

```java
class Outer {
    private class InstanceInner { // ... } // private 인스턴스 클래스
    protected static class StaticInner ( // ... } // protected 스태틱 클래스

    void Mymethod() {
        class LocalInner { // ...} // (default) 지역 클래스
    }
}
```

- 내부 클래스의 제어자는 변수에 선언 가능한 제어자와 동일
	+ 일반 클래스는 `public`, `(default)` 접근 제어자만 선언 가능

## 예시

```java
class Ex7_12 {

    class InstanceInner {

	int iv = 100;
//	static int cv = 100; // 에러. static 변수를 선언할 수 없다.
	final static int CONST = 100; // final static은 상수이므로 허용
    }

   static class StaticInner {

	int iv = 200;
	static int cv = 200; // static 클래스만 static 멤버를 정의할 수 있다.
    } 

    void myMethod() {
	class LocalInner {

	    int iv = 300;
//	    static int cv = 300; // 에러. static 변수를 선언할 수 없다.
	    final static int CONST = 300; // final static은 상수이므로 허용
	}

        int i = LocalInner.CONST; // OK
    }

    public static void main(String args[]) {

	System.out.println(InstanceInner.CONST); // 100
	System.out.println(StaticInner.cv); // 200
//      System.out.println(LocalInner.CONST); // 에러. 지역 내부 클래스는 메서드 내에서만 사용 가능
    }
}
```

- 내부 클래스에서 `static` 멤버는 `static` 내부 클래스만 사용할 수 있음
- `static` 내부 클래스에서 객체 생성없이 외부 클래스의 인스턴스 멤버를 사용할 수 없음

```java
class Ex7_13 {

    class InstanceInner {}
    static class StaticInner {}

    // 인스턴스멤버 간에는 서로 직접 접근이 가능하다.
    InstanceInner iv = new InstanceInner();

    // static 멤버 간에는 서로 직접 접근이 가능하다.
    static StaticInner cv = new StaticInner();

//  static StaticInner cv = new InstanceInner(); // 에러. 스태틱 클래스는 인스턴스 클래스에 접근할 수 없음

    static void staticMethod() {
    // static멤버는 인스턴스멤버에 직접 접근할 수 없다.
//  InstanceInner obj1 = new InstanceInner();

    StaticInner obj2 = new StaticInner();

    // 굳이 접근하려면 아래와 같이 객체를 생성해야 한다.
    // 인스턴스클래스는 외부 클래스를 먼저 생성해야만 생성할 수 있다.
    Ex7_13 outer = new Ex7_13();
    InstanceInner obj1 = outer.new InstanceInner();
    }

    void instanceMethod() {
        // 인스턴스메서드에서는 인스턴스멤버와 static멤버 모두 접근 가능하다.
        InstanceInner obj1 = new InstanceInner();
        StaticInner obj2 = new StaticInner();

        // 메서드 내에 지역적으로 선언된 내부 클래스는 외부에서 접근할 수 없다.
//      LocalInner lv = new LocalInner();
    }

    void myMethod() {
        class LocalInner {}
        LocalInner lv = new LocalInner();
    }
}
```
```java
class Outer {

    private int outerIv = 0;
    static int outerCv = 0;

    class InstanceInner {

        int iiv = outerIv ; // 외부 클래스의 private 멤버도 접근 가능하다.
        int iiv2 = outerCv;
    }

    static class StaticInner {

//      int siv = outerIv; // 스태틱 클래스는 외부 클래스의 인스턴스 멤버에 접근할 수 없다.
        static int scv = outerCv;
    }

    void myMethod() {
        int lv = 0;
        final int LV = 0; // JDK 1.8부터 final 생략 가능

        class LocalInner {

            int liv = outerIv;
            int liv2 = outerCv;

            // 외부 클래스의 지역 변수는 final이 붙은 변수(상수)만 접근 가능
//          int liv3= lv; // 에러(JDK 1.8부터 에러 아님)
            int liv4 = Lv; // OK
        }
    }
}
```

- 내부 클래스에서는 외부 클래스의 `private` 멤버도 접근 가능
- 외부 클래스의 지역 변수는 상수(`final`)만 접근 가능
	+ JDK 1.8 부터는 지역 변수라도 값이 바뀌지 않는다면 상수로 취급하기 때문에 외부 클래스의 지역 변수에 접근 가능

> 외부 클래스의 지역 변수보다 내부 클래스 객체가 메모리에 더 오래 유지될 수 있기 때문에 상수가 아닌 외부 클래스의 지역 변수는 내부 클래스에서 사용할 수 없다.
{: .prompt-info }

```java
class Outer2 {

    class InstanceInner {

        int iv = 100;
    }

    static class StaticInner {

        int iv = 200;
        static int cv = 300;
    }

    void myMethod() {
	class LocalInner {

	int iv = 400;
	}
    }
}

class Ex7_15 {

    public static void main(String[] args) {

    // 인스턴스 클래스의 인스턴스를 생성하려면 외부 클래스의 인스턴스를 먼저 생성해야 한다.
    Outer2 oc = new Outer2();
    Outer2.InstanceInner ii = oc.new InstanceInner();

    System.out.println("ii.iv : "+ ii.iv); // ii.iv : 100
    System.out.println("Outer2.StaticInner.cv : "+ Outer2.StaticInner.cv); // Outer2.StaticInner.cv : 300
                                     
    // 스태틱 내부 클래스의 인스턴스는 외부 클래스를 먼저 생성하지 않아도 된다.
    Outer2.StaticInner si = new Outer2.StaticInner();
    System.out.println("si.iv : "+ si.iv); // si.iv : 200
    }
}
```

```java
class Outer3 {

    int value = 10;	// Outer3.this.value

    class Inner {

	int value = 20;   // this.value

	void method1() {
	    int value = 30;
	    System.out.println("value :" + value); // value :30
	    System.out.println("this.value :" + this.value); // this.value :20
	    System.out.println("Outer3.this.value :" + Outer3.this.value); // Outer3.this.value :10
	}
    } // Inner클래스의 끝
} // Outer3클래스의 끝

class Ex7_16 {

    public static void main(String args[]) {

        Outer3 outer = new Outer3();
	Outer3.Inner inner = outer.new Inner();
	inner.method1();
    }
}
```

## 익명 클래스(Anonymous Class)

```java
new 상위클래스이름() {
    // ....
}

new 구현인터페이스이름() {
    // ...
}
```

- 이름이 없는 일회용 클래스
- 클래스 정의와 생성이 동시에 이루어짐

### 예시

```java
class Ex7_17 {

    Object iv = new Object() { void method() { // ... } }; // 익명 클래스
    static Object cv = new Object() { void method() { // ... } }; // 익명 클래스

    void myMethod() {
	Object lv = new Object() { void method() { //... } }; // 익명 클래스
    }
}
```

```java
import java.awt.*;
import java.awt.event.*;

class Ex7_18 {

    public static void main(String[] args) {

	Button b = new Button("Start");
	b.addActionListener(new EventHandler()); 
	
        // 위의 b.addActionListener(new EventHandler()); 코드를 익명 클래스를 이용해서 아래와 같이 작성 가능
	b.addActionListener(new ActionListener() { // 익명 클래스. 클래스의 정의와 생성을 동시에

	    public void actionPerformed(ActionEvent e) {
		System.out.println("ActionEvent occurred!!!");
		}
	});
    }
}

class EventHandler implements ActionListener {
    public void actionPerformed(ActionEvent e) {
        System.out.println("ActionEvent occurred!!!");
    }
}
```

## 참고자료

- [남궁성의 정석코딩, "[자바의 정석 - 기초편] ch7-42~44 내부클래스의 종류, 특징, 선언", 2020-04-23](https://www.youtube.com/watch?v=P1rDdH465Is&list=PLW2UjW795-f6xWA2_MUhEVgPauhGl3xIp&index=94){: target="_blank" }
- [남궁성의 정석코딩, "[자바의 정석 - 기초편] ch7-45~50 내부클래스의 제어자와 접근성", 2020-04-2](https://www.youtube.com/watch?v=cZJyRGX2VoM&list=PLW2UjW795-f6xWA2_MUhEVgPauhGl3xIp&index=95){: target="_blank" }
- [남궁성의 정석코딩, "[자바의 정석 - 기초편] ch7-51,52 익명 클래스", 2020-04-27](https://www.youtube.com/watch?v=jRusDJ5ca4g&list=PLW2UjW795-f6xWA2_MUhEVgPauhGl3xIp&index=96){: target="_blank" }