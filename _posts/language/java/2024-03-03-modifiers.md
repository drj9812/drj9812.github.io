---
title: "[Java]제어자(Modifier)"
categories: [Language, Java]
tags: [Java, 자바, Modifer, 제어자, Access Modifier, 접근 제어자]
image:
  path: /assets/img/posts/language/java/01-java-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Java
---

# 제어자(Modifier)

## 제어자(Modifier)

|   제어자 종류   |                                         제어자 이름                                           |
|:---------------:|:-------------------------------------------------------------------------------------------:|
|   **제어자**    | `static`, `final`, `abstract`, `native`, `transient`, `synchronized`, `volatile`, `strictfp` |
| **접근 제어자** |                          `public`, `protected`, `(default)`, `private`                       |

- 클래스와 클래스의 멤버(멤버 변수, 메서드)에 부가적인 의미 부여

```java
public class ModifierTest {

    public static final int WIDTH = 200;

    public static void main(String[] args) {

        System.out.println("WIDTH=" + WIDTH);
    }
}
```

- 하나의 대상에 여러 제어자를 선언할 수 있음
	+ 접근 제어자는 하나만 선언할 수 있음

> 접근 제어자는 맨 앞에 선언하는 것이 관습이다.
{: .prompt-tip }

### static

|  제어자  |    대상   |                                  의미                            |
|:--------:|:--------:|:----------------------------------------------------------------:|
| `static` | 멤버 변수 |             모든 인스턴스에 공통적으로 사용되는 클래스 변수        |
|          |           |        클래스 변수는 **인스턴스를 생성하지 않고도 사용** 가능      |
|          |           |                **클래스가 메모리에 로드될 때 생성**               |
|          |  메서드   |             **인스턴스를 생성하지 않고도 호출**이 가능             |
|          |           | `static` 메서드 내에서는 **인스턴스 멤버들을 직접 사용할 수 없음** |

```java
class StaticTest {

    static int width = 200; // 클래스 변수(static 변수)
    static int height = 120; // 클래스 변수(static 변수)

    static { // 클래스 초기화 블럭
        // static 변수의 복잡한 초기화 수행
    }
    static int max(int a, int b) { // 클래스 메서드(static 메서드)
        return a > b ? a : b;
    }
}
```

### final

|  제어자 |        대상     |                 의미                |
|:-------:|:--------------:|:-----------------------------------:|
| `final` |      클래스     | 변경, 확장될 수 없는 클래스 - 상속 X |
|         |      메서드     | 변경될 수 없는 메서드 - 오버라이딩 X |
|         | 멤버, 지역 변수 |     값을 **변경할 수 없는 상수**     |

```java
final class FinalTest { // 상속받을 수 없는 클래스

    final int MAX_SIZE = 10; // 상수

    final void getMaxSize() { // 오버라이딩 할 수 없는 메서드
        final int LV = MAX_SIZE; // 값을 변경할 수 없는 지역 변수
	return MAX_SIZE;
    }
}
```

### abstract

|   제어자   |   대상  |                   의미                       |
|:----------:|:------:|:--------------------------------------------:|
| `abstract` | 클래스 |    클래스 내에서 추상 메서드가 선언된 클래스    |
|            | 메서드 | 선언부만 작성하고 구현부는 작성되지 않은 메서드 |

```java
abstract class AbstractTest { // 추상 클래스(추상 메서드를 포함한 클래스)

    abstract void move(); // 추상 메서드(구현부가 없는 메서드)
}
```

### synchronized

- multi-thread 환경에서 동기화를 보장하는 역할
	+ 여러 thread가 동시에 하나의 메서드나 코드 블럭을 실행하지 못하도록 제어하는 데 사용
- 성능 저하를 일으킬 수 있음
	+ JVM이 동기화된 영역에 대한 락(lock)을 얻고 해제하는 데 필요한 추가적인 작업을 수행하면서 오버헤드를 발생
		* 많은 thread가 동시에 접근하는 경우에는 이 오버헤드가 더욱 심각해질 수 있음
	+ 동기화 블럭 내에서 한 thread가 작업을 수행하고 있으면 다른 thread는 작업을 수행하지 못하고 대기 상태에 들어가야 하므로 전반적인 처리 시간이 느려짐
	+ 동기화를 사용하면 여러 thread 간의 경합 조건(race condition)을 방지할 수 있지만, 이는 thread들이 순차적으로 실행되도록 보장하기 위해 락을 사용해야 하므로, 동시성을 제한하는 결과를 초래할 수 있음
	+ 동기화된 블럭에서 변수에 대한 수정이 이루어지면, 다른 thread에서 해당 변수의 캐시를 갱신해야 하는데 이는 메모리에 대한 추가적인 접근을 유발하여 성능을 저하시킬 수 있음

## 접근 제어자(Access Modifier)

- `private`: **같은 클래스** 내에서만 **접근** 가능
- `(default)`: **같은 패키지** 내에서만 **접근** 가능
- `protected`: **같은 패키지** 내에서, 그리고 **다른 패키지의 자손 클래스**에서 **접근** 가능
- `public`: 접근 제한 없음

|    제어자   | 같은 클래스 | 같은 패키지 | 자손 클래스 | 전체 |
|:-----------:|:----------:|:----------:|:-----------:|:----:|
|   `public`  |     O      |      O     |      O      |  O   |
| `protected` |     O      |      O     |      O      |      |
| `(default)` |     O      |      O     |             |      |
|  `private`  |     O      |            |             |      |

> 클래스는 `public`, `(default)` 접근 제어자만 선언할 수 있다.
{: .prompt-info }

### 접근 제어자를 사용하는 이유

```java
class Time {

    // 접근 제어자를 pirvate으로 선언하여 외부에서 직접 접근하지 못하도록 한다.
    private int hour; // 0 ~ 23 사이의 값을 가져야 함
    private int minute; // 0 ~ 59 사이의 값을 가져야 함
    private int second; // 0 ~ 59 사이의 값을 가져야 함 

    public int getHour() { return hour; }

    public void setHour(int hour) {
        if(isNotValidHour(hour)) return;

        this.hour = hour;
    }

    // 매개 변수 hour가 0~23 사이의 값인지 확인하는 메서드
    // Time 클래스 내부에서만 사용하는 메서드이기 때문에 public으로 선언할 필요가 없음
    private boolean isNotValidHour(int hour) {
        return hour < 0 || hour > 23;
    }
}

public class TimeTest() {

    public static main(String[] args) {

    Time t = new Time();

    t.hour = 25; // 에러. 멤버 변수에 직접 접근

    t.setHour(23);
    System.out.println(t.hour); // 23
    }
}
```

- 외부로부터 데이터를 보호
- 외부에는 불필요한, 내부적으로만 사용되는 부분을 감추기 위함

> 접근 제어자의 범위는 좁을 수록 좋다.
{: .prompt-tip }

## 참고자료

- [남궁성의 정석코딩, "[자바의정석 - 기초편 ] ch7-17~20 제어자, static, final, abstract", 2020-03-06](https://www.youtube.com/watch?v=Hmu7YH8AXmI&list=PLW2UjW795-f6xWA2_MUhEVgPauhGl3xIp&index=77){: target="_blank" }
- [남궁성의 정석코딩, "[자바의 정석 - 기초편] ch7-21 접근제어자
", 2020-03-07](https://www.youtube.com/watch?v=Qm08p4Vk2sw&list=PLW2UjW795-f6xWA2_MUhEVgPauhGl3xIp&index=79){: target="_blank" }