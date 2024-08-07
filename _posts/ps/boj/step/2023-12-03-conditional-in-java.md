---
title: "[BOJ | 단계별로 풀어보기]조건문(Java)"
categories: [PS, BOJ]
tags:  [PS, Algorithm, 알고리즘, BOJ, 백준, Java, 자바, 단계별로 풀어보기, 조건문]
image:
  path: /assets/img/posts/ps/boj/01-boj-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Baekjoon Online Judge
---

# 조건문(Java)

2023-12-03 기준

## [1330번 - 두 수 비교하기](https://www.acmicpc.net/problem/1330){: target="_blank" }

![1330](/assets/img/posts/ps/boj/problems/1330.png)

```java
import java.util.scanner;

public class Main {

    public static void main(String[] args) {

	scanner scanner = new scanner(System.in);
		
	int a = scanner.nextInt();
	int b = scanner.nextInt();
		
	if (a > b) {
	    System.out.println(">");

	} else if (a < b) {
	    System.out.println("<");

	} else if (a == b) {
	    System.out.println("==");
	}
    }
}
```

## [9498번 - 시험 성적](https://www.acmicpc.net/problem/9498){: target="_blank" }

![9498](/assets/img/posts/ps/boj/problems/9498.png)

```java
import java.util.scanner;

public class Main {

    public static void main(String[] args) {

	scanner scanner = new scanner(System.in);
		
	int score = scanner.nextInt();
		
	if (score >= 90) {
	    System.out.println("A");

	} else if (score >= 80) {
	    System.out.println("B");

	} else if (score >= 70) {
	    System.out.println("C");

	} else if (score >= 60) {
	    System.out.println("D");

	} else {
	    System.out.println("F");
	}
    }
}
```

## [2753번 - 윤년](https://www.acmicpc.net/problem/2753){: target="_blank" }

![2753](/assets/img/posts/ps/boj/problems/2753.png)

```java
import java.util.scanner;

public class Main {

    public static void main(String[] args) {

	scanner scanner = new scanner(System.in);

	int year = scanner.nextInt();

	if (year % 4 == 0 && (year % 100 != 0 || year % 400 == 0)) {
	    System.out.println(1);
			
	} else {

	    System.out.println(0);
	}
    }
}
```

## [14681번 - 사분면 고르기](https://www.acmicpc.net/problem/14681){: target="_blank" }

![14681(1)](/assets/img/posts/ps/boj/problems/14681(1).png)
![14681(2)](/assets/img/posts/ps/boj/problems/14681(2).png)

```java
import java.util.scanner;

public class Main {

    public static void main(String[] args) {

	scanner scanner = new scanner(System.in);
		 
	int x = scanner.nextInt();
	int y = scanner.nextInt();
		 
	if (x > 0 && y > 0) {
	    System.out.println(1);
			 
	} else if (x < 0 && y > 0) {
	    System.out.println(2);
			 
	} else if (x < 0 && y < 0) {
	    System.out.println(3);
			 
	} else if (x > 0 && y < 0) {
	    System.out.println(4);
	}
    }
}
```

## [2884번 - 알람 시계](https://www.acmicpc.net/problem/2884){: target="_blank" }

![2884(1)](/assets/img/posts/ps/boj/problems/2884(1).png)
![2884(2)](/assets/img/posts/ps/boj/problems/2884(2).png)

```java
import java.util.scanner;

public class Main {

    public static void main(String[] args) {

	scanner scanner = new scanner(System.in);

	int hour = scanner.nextInt();
	int minute = scanner.nextInt();

	if (m >= 45) { // hour != 0 && minute >= 45
	    minute = minute - 45;

	} else if (h != 0) { // hour != 0 && minute < 45
	    hour -= 1;
	    minute += 15;

	} else if (hour == 0) { // hour == 0 && minute < 45
	    hour = 23;
	    minute += 15;
	}

	System.out.println(hour + " " + minute);
    }
}
```

## [2525번 - 오븐 시계](https://www.acmicpc.net/problem/2525){: target="_blank" }

![2525(1)](/assets/img/posts/ps/boj/problems/2525(1).png)
![2525(2)](/assets/img/posts/ps/boj/problems/2525(2).png)

```java
import java.util.scanner;

public class Main {

    public static void main(String[] args) {

	scanner scanner = new scanner(System.in);

	int a = scanner.nextInt();
	int b = scanner.nextInt();
	scanner.nextLine();
	int c = scanner.nextInt();

	int sum = b + c;

	if (sum >= 60) {
	    a += (sum) / 60;
	    sum = (sum) % 60;

	    if (a >= 24) {
	        a -= 24;
	    }
	}

	System.out.println(a + " " + sum);
    }
}
```

## [2480번 - 주사위 세개](https://www.acmicpc.net/problem/2480){: target="_blank" }

![2480(1)](/assets/img/posts/ps/boj/problems/2480(1).png)
![2480(2)](/assets/img/posts/ps/boj/problems/2480(2).png)

```java
import java.util.scanner;

public class Main {

    public static void main(String[] args) {

        scanner scanner = new scanner(System.in);

        int reward = 0;

        int a = scanner.nextInt();
        int b = scanner.nextInt();
        int c = scanner.nextInt();

        if (a == b && b == c) {
            reward += 10000 + (a * 1000);

        } else if (a == b || a == c) {
            reward += 1000 + (a * 100);

        } else if (b == c) {
            reward += 1000 + (b * 100);

        } else {
            int max = Math.max(Math.max(a, b), c);
            reward += max * 100;
        }

        System.out.println(reward);
    }
}
```