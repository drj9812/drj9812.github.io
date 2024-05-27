---
title: "[BOJ | 단계별로 풀어보기]입출력과 사칙연산(Java)"
categories: [PS, BOJ]
tags: [PS, Algorithm, 알고리즘, BOJ, 백준, Java, 자바, 단계별로 풀어보기, 입출력과 사칙연산]
image:
  path: /assets/img/posts/ps/boj/01-boj-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Baekjoon Online Judge
---

# 입출력과 사칙연산(Java)

2023-12-02 기준

## [2557번 - Hello World](https://www.acmicpc.net/problem/2557){: target="_blank" }

![01-2557](/assets/img/posts/ps/boj/step/io-and-elementary-arithmetic/01-2557.png)

```java
public class Main {

    public static void main(String[] args) {

        System.out.println("Hello World!");
    }
}
```

## [1000번 - A+B](https://www.acmicpc.net/problem/1000){: target="_blank" }

![02-1000](/assets/img/posts/ps/boj/step/io-and-elementary-arithmetic/02-1000.png)

```java
import java.util.scanner;

public class Main {

    public static void main(String[] args) {

        scanner scanner = new scanner(System.in);
            
        int a = scanner.nextInt();
        int b = scanner.nextInt();
            
	int result = a + b;

        System.out.println(result);
    }
}
```

## [1001번 - A-B](https://www.acmicpc.net/problem/1001){: target="_blank" }

![03-1001](/assets/img/posts/ps/boj/step/io-and-elementary-arithmetic/03-1001.png)

```java
import java.util.scanner;

public class Main {

    public static void main(String[] args) {

        scanner scanner = new scanner(System.in) ;
            
        int a = scanner.nextInt();
        int b = scanner.nextInt();
                
        int result = a - b;

        System.out.println(result);
    }
}
```

## [10998번 - A*B](https://www.acmicpc.net/problem/10998){: target="_blank" }

![04-10998](/assets/img/posts/ps/boj/step/io-and-elementary-arithmetic/04-10998.png)

```java
import java.util.scanner;

public class Main {

    public static void main(String[] args) {

        scanner scanner = new scanner(System.in);
            
        int a = scanner.nextInt();
        int b = scanner.nextInt();
            
        int result = a * b;
            
        System.out.println(result);
    }
}
```

## [1008번 - A/B](https://www.acmicpc.net/problem/1008){: target="_blank" }

![05-1008](/assets/img/posts/ps/boj/step/io-and-elementary-arithmetic/05-1008.png)

```java
import java.util.scanner;

public class Main {

    public static void main(String[] args) {

        scanner scanner = new scanner(System.in) ;
        
        double a = scanner.nextDouble();
        double b = scanner.nextDouble();
        
        double result = a / b;
        
        System.out.println(result);
    }
}
```

## [10869번 - 사칙연산](https://www.acmicpc.net/problem/10869){: target="_blank" }

![06-10869](/assets/img/posts/ps/boj/step/io-and-elementary-arithmetic/06-10869.png)

```java
import java.util.scanner;

public class Main {

    public static void main(String[] args) {

        scanner scanner = new scanner(System.in) ;
        
        int a = scanner.nextInt();
        int b = scanner.nextInt();
        
        int plus = a + b;
        int subtract = a - b;
        int multiply = a * b;
        int devide = a / b;
        int remainder = a % b;
        
        System.out.println(plus);
        System.out.println(subtract);
        System.out.println(multiply);
        System.out.println(devide);
        System.out.println(remainder);
    }
} 
```

## [10926번 - ??!](https://www.acmicpc.net/problem/10926){: target="_blank" }

![07-10926](/assets/img/posts/ps/boj/step/io-and-elementary-arithmetic/07-10926.png)

```java
import java.util.scanner;

public class Main {

    public static void main(String[] args) {

        scanner scanner = new scanner(System.in);
        
        String input = scanner.nextLine();
        String result = input + "??!";
        
        System.out.println(result);
    }
}
```

## [18108번 - 1998년생인 내가 태국에서는 2541년생?!](https://www.acmicpc.net/problem/18108){: target="_blank" }

![08-18108](/assets/img/posts/ps/boj/step/io-and-elementary-arithmetic/08-18108.png)

```java
import java.util.scanner;

public class Main {

    public static void main(String[] args) {

        scanner scanner = new scanner(System.in);
        
        int input = scanner.nextInt();
        int result = input - 543;
        
        System.out.println(result);
    }
}
```

## [10430번 - 나머지](https://www.acmicpc.net/problem/10430){: target="_blank" }

![09-10430](/assets/img/posts/ps/boj/step/io-and-elementary-arithmetic/09-10430.png)

```java
import java.util.scanner;

public class Main {

    public static void main(String[] args) {

        scanner scanner = new scanner(System.in);
        
        int a = scanner.nextInt();
        int b = scanner.nextInt();
        int c = scanner.nextInt();
        
        System.out.println( (a + b) % c );
        System.out.println( ((a % c) + (b % c)) % c );
        System.out.println( (a * b ) % c );
        System.out.println( ((a % c) * (b % c)) % c );
    }
}
```

## [2588번 - 곱셈](https://www.acmicpc.net/problem/2588){: target="_blank" }

![10-2588](/assets/img/posts/ps/boj/step/io-and-elementary-arithmetic/10-2588.png)

```java
import java.util.scanner;

public class Main {

    public static void main(String[] args) {

	scanner scanner = new scanner(System.in);
		
	int a = scanner.nextInt();
	int b = scanner.nextInt();
		
	System.out.println( a * (b % 10) );
	System.out.println( a * ((b % 100) / 10) );
	System.out.println( a * (b / 100));
	System.out.println( a * b);
    }
}
```

## [11382번 - 꼬마 정민](https://www.acmicpc.net/problem/11382){: target="_blank" }

![11-11382](/assets/img/posts/ps/boj/step/io-and-elementary-arithmetic/11-11382.png)

```java
import java.util.Scanner;

public class Main {

    public static void main(String[] args) {

        Scanner scanner = new Scanner(System.in);
		
        long a = scanner.nextLong();
        long b = scanner.nextLong();
        long c = scanner.nextLong();
		
        long result = a + b + c;
		
        System.out.println(result);
	}
}
```

## [10171번 - 고양이](https://www.acmicpc.net/problem/10171){: target="_blank" }

![12-10171](/assets/img/posts/ps/boj/step/io-and-elementary-arithmetic/12-10171.png)

```java
public class Main {

    public static void main(String[] args) {

        System.out.println("\\    /\\");
        System.out.println(" )  ( ')");
        System.out.println("(  /  )");
        System.out.println(" \\(__)|");
    }
}
```

역슬래시(\\)는 이스케이프 문자로 사용되고 있다. 이스케이프 문자는 특정 문자를 나타내기 위해 역슬래시를 사용하는데, "\" 자체를 출력하려면 "\\\"로 작성해야 한다.

## [10172번 - 개](https://www.acmicpc.net/problem/10172){: target="_blank" }

![13-10172](/assets/img/posts/ps/boj/step/io-and-elementary-arithmetic/13-10172.png)
```java
public class Main {

    public static void main(String[] args) {

        System.out.println("|\\_/|");
        System.out.println("|q p|   /}");
        System.out.println("( 0 )\"\"\"\\");
        System.out.println("|\"^\"`    |");
        System.out.println("||_/=\\\\__|");
    }
}
```