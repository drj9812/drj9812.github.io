---
title: "[백준 | Java]단계별로 풀어보기 조건문 모든 문제"
categories: [알고리즘, 백준]
tags: [알고리즘, 백준, Java, 자바, 단계별로 풀어보기, 조건문]
---

# [백준 | Java]단계별로 풀어보기 조건문 모든 문제

## 1330번 두 수 비교하기(<https://www.acmicpc.net/problem/1330>{: target="_blank" })

![1330(1)](/assets/img/posts/algorithm/java/boj-step-by-step-conditional-by-java/1330(1).png)

```java
import java.util.Scanner;

public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		
		int a = sc.nextInt();
		int b = sc.nextInt();
		
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

## 9498번 시험 성적(<https://www.acmicpc.net/problem/9498>{: target="_blank" })

![9498(1)](/assets/img/posts/algorithm/java/boj-step-by-step-conditional-by-java/9498(1).png)

```java
import java.util.Scanner;

public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		
		int score = sc.nextInt();
		
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

## 2753번 윤년(<https://www.acmicpc.net/problem/2753>{: target="_blank" })

![2753(1)](/assets/img/posts/algorithm/java/boj-step-by-step-conditional-by-java/2753(1).png)

```java
import java.util.Scanner;

public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);

		int year = sc.nextInt();

		if (year % 4 == 0 && (year % 100 != 0 || year % 400 == 0)) {
			System.out.println(1);
			
		} else {
			System.out.println(0);
		}
	}
}
```

## 14681번 사분면 고르기(<https://www.acmicpc.net/problem/14681>{: target="_blank" })

![14681(1)](/assets/img/posts/algorithm/java/boj-step-by-step-conditional-by-java/14681(1).png)
![14681(2)](/assets/img/posts/algorithm/java/boj-step-by-step-conditional-by-java/14681(1).png)

```java
import java.util.Scanner;

public class Main {
	public static void main(String[] args) {
		 Scanner sc = new Scanner(System.in);
		 
		 int x = sc.nextInt();
		 int y = sc.nextInt();
		 
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

## 2884번 알람 시계(<https://www.acmicpc.net/problem/2884>{: target="_blank" })

![2884(1)](/assets/img/posts/algorithm/java/boj-step-by-step-conditional-by-java/2884(1).png)
![2884(2)](/assets/img/posts/algorithm/java/boj-step-by-step-conditional-by-java/2884(1).png)

```java
import java.util.Scanner;

public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);

		int h = sc.nextInt();
		int m = sc.nextInt();

		if (m >= 45) { // h != 0 && m >= 45
			m = m - 45;

		} else if (h != 0) { // h != 0 && m < 45
			h -= 1;
			m += 15;

		} else if (h == 0) { // h == 0 && m < 45
			h = 23;
			m += 15;
		}
		
		System.out.println(h + " " + m);
	}
}
```

## 2525번 오븐 시계(<https://www.acmicpc.net/problem/2525>{: target="_blank" })

![2525(1)](/assets/img/posts/algorithm/java/boj-step-by-step-conditional-by-java/2525(1).png)
![2525(2)](/assets/img/posts/algorithm/java/boj-step-by-step-conditional-by-java/2525(1).png)

```java
import java.util.Scanner;

public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);

		int a = sc.nextInt();
		int b = sc.nextInt();
		sc.nextLine();
		int c = sc.nextInt();
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

## 2480번 주사위 세개(<https://www.acmicpc.net/problem/2480>{: target="_blank" })

![2480(1)](/assets/img/posts/algorithm/java/boj-step-by-step-conditional-by-java/2480(1).png)
![2480(2)](/assets/img/posts/algorithm/java/boj-step-by-step-conditional-by-java/2480(1).png)

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int reward = 0;

        int a = sc.nextInt();
        int b = sc.nextInt();
        int c = sc.nextInt();

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
