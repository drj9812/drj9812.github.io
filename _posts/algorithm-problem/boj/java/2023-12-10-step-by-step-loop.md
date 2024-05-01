---
title: "[백준 | Java]단계별로 풀어보기 - 반복문 모든 문제"
categories: [Algorithm Problem, BOJ]
tags:  [Algorithm, 알고리즘, BOJ, 백준, Java, 자바, 단계별로 풀어보기, 반복문]
image:
  path: /assets/img/posts/algorithm-problem/boj/01-boj-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
---

# 단계별로 풀어보기 - 반복문 모든 문제

2023-12-10 기준

## [2739번 - 구구단](https://www.acmicpc.net/problem/2739){: target="_blank" }

![01-2739](/assets/img/posts/algorithm-problem/boj/java/step-by-step-loop/01-2739.png)

```java
import java.util.scanner;

public class Main {

    public static void main(String[] args) {

	scanner scanner = new scanner(System.in);
		
	int n = scanner.nextInt();
		
	for (int i = 1; i < 10; i++) {
	    System.out.println(n + " * " + i + " = " + n * i);
	}
    }
}
```

## [10950번 - A+B - 3](https://www.acmicpc.net/problem/10950){: target="_blank" }

![02-10950](/assets/img/posts/algorithm-problem/boj/java/step-by-step-loop/02-10950.png)

```java
import java.util.scanner;

public class Main {

    public static void main(String[] args) {

	scanner scanner = new scanner(System.in);
		
	int t = scanner.nextInt();
		
	for (int i = 0; i < t; i++) {
	    int a = scanner.nextInt();
	    int b = scanner.nextInt();

	    int result = a + b;
			
	    System.out.println(result);
	}
    }
}
```

## [8393번 - 합](https://www.acmicpc.net/problem/8393){: target="_blank" }

![03-8393](/assets/img/posts/algorithm-problem/boj/java/step-by-step-loop/03-8393.png)

```java
import java.util.scanner;

public class Main {

    public static void main(String[] args) {

	scanner scanner = new scanner(System.in);
		
	int n = scanner.nextInt();
	int sum = 0;
		
	for (int i = 1; i <= n; i++) {
	    sum += i;
	}
		
	System.out.println(sum);
    }
}
```

## [25304번 - 영수증](https://www.acmicpc.net/problem/25304){: target="_blank" }

![04-25304(1)](/assets/img/posts/algorithm-problem/boj/java/step-by-step-loop/04-25304(1).png)
![05-25304(2)](/assets/img/posts/algorithm-problem/boj/java/step-by-step-loop/05-25304(2).png)

```java
import java.util.scanner;

public class Main {

    public static void main(String[] args) {

	scanner scanner = new scanner(System.in);
		
	int x = scanner.nextInt();
	int n = scanner.nextInt();

	int sum = 0;
		
	for (int i = 0; i < n; i++) {
	    int a = scanner.nextInt();
	    int b = scanner.nextInt();
			
	    sum += a * b;
	}
		
	if (x == sum) {
	    System.out.println("Yes");

	} else {
	    System.out.println("No");
	}
    }
}
```

## [25314번 - 코딩은 체육과목 입니다](https://www.acmicpc.net/problem/25314){: target="_blank" }

![06-25314(1)](/assets/img/posts/algorithm-problem/boj/java/step-by-step-loop/06-25314(1).png)
![07-25314(2)](/assets/img/posts/algorithm-problem/boj/java/step-by-step-loop/07-25314(2).png)

```java
import java.util.scanner;

public class Main {

    public static void main(String[] args) {

	scanner scanner = new scanner(System.in);
		
	int n = scanner.nextInt();
		
	for (int i = 0; i < (n / 4); i++) {
	    System.out.print("long ");
	}
		
	System.out.println("int");
    }
}
```

## [15552번 - 빠른 A+B](https://www.acmicpc.net/problem/15552){: target="_blank" }

![08-15552(1)](/assets/img/posts/algorithm-problem/boj/java/step-by-step-loop/08-15552(1).png)
![09-15552(2)](/assets/img/posts/algorithm-problem/boj/java/step-by-step-loop/09-15552(2).png)

```java
import java.io.*;
import java.util.StringTokenizer;

public class Main {

    public static void main(String[] args) throws IOException {

	BufferedReader bfrReader = new BufferedReader(new InputStreamReader(System.in));
	BufferedWriter bfrWriter = new BufferedWriter(new OutputStreamWriter(System.out));
	StringTokenizer strTokenizer;
		
	int t = Integer.parseInt(bfrReader.readLine());
	int sum = 0;
		
	for (int i = 0; i < t; i++) {
	    strTokenizer = new StringTokenizer(bfrReader.readLine());
			
	    int a = Integer.parseInt(strTokenizer.nextToken());
	    int b = Integer.parseInt(strTokenizer.nextToken());
			
	    sum = a + b;
			
	    bfrWriter.write(sum + "\n");
	}

	bfrWriter.flush();
	bfrWriter.close();
	bfrReader.close();
    }
}
```

문제의 설명대로 자바를 이용하여 문제를 풀 때, `Scanner`를 사용하면 시간초과되어 오답처리 될 수 있기 때문에 앞으로는 `BufferedWriter`까지는 아니더라도 `BufferedReader`를 사용해야 한다. 위 문제에서는 `println()` 메서드로 출력하려 했더니 시간초과로 오답처리되서 `BufferedWriter`를 사용했다.

## [11021번 - A+B - 7](https://www.acmicpc.net/problem/11021){: target="_blank" }

![10-11021](/assets/img/posts/algorithm-problem/boj/java/step-by-step-loop/10-11021.png)

```java
import java.io.*;
import java.util.StringTokenizer;

public class Main {

    public static void main(String[] args) throws IOException {

	BufferedReader bfrReader = new BufferedReader(new InputStreamReader(System.in));
	BufferedWriter bfrWriter = new BufferedWriter(new OutputStreamWriter(System.out));
	StringTokenizer strTokenizer;
		
	int t = Integer.parseInt(bfrReader.readLine());
	int sum = 0;
		
	for (int i = 1; i <= t; i++) {
	    strTokenizer = new StringTokenizer(bfrReader.readLine());
			
	    int a = Integer.parseInt(strTokenizer.nextToken());
	    int b = Integer.parseInt(strTokenizer.nextToken());
			
	    sum = a + b;
			
	    bfrWriter.write("Case #" + i + ": " + sum + "\n");
	}

	bfrWriter.flush();
	bfrWriter.close();
	bfrReader.close();		
    }
}
```

## [11022번 - A+B - 8](https://www.acmicpc.net/problem/11022){: target="_blank" }

![11-11022](/assets/img/posts/algorithm-problem/boj/java/step-by-step-loop/11-11022.png)

```java
import java.io.*;
import java.util.StringTokenizer;

public class Main {

    public static void main(String[] args) throws IOException {

	BufferedReader bfrReader = new BufferedReader(new InputStreamReader(System.in));
	BufferedWriter bfrWriter = new BufferedWriter(new OutputStreamWriter(System.out));
	StringTokenizer strTokenizer;
		
	int t = Integer.parseInt(bfrReader.readLine());
	int sum = 0;
		
	for (int i = 1; i <= t; i++) {
	    strTokenizer = new StringTokenizer(bfrReader.readLine());
			
	    int a = Integer.parseInt(strTokenizer.nextToken());
	    int b = Integer.parseInt(strTokenizer.nextToken());
			
	    sum = a + b;
			
	    bfrWriter.write("Case #" + i + ": " + a + " + " + b + " = " + sum + "\n");
	}

	bfrWriter.flush();
	bfrWriter.close();
	bfrReader.close();
    }
}
```

## [2438번 - 별 찍기 - 1](https://www.acmicpc.net/problem/2438){: target="_blank" }

![12-2438](/assets/img/posts/algorithm-problem/boj/java/step-by-step-loop/12-2438.png)

```java
import java.io.*;

public class Main {

    public static void main(String[] args) throws IOException {
	BufferedReader bfrReader = new BufferedReader(new InputStreamReader(System.in));
	BufferedWriter bfrWriter = new BufferedWriter(new OutputStreamWriter(System.out));
		
	int n = Integer.parseInt(bfrReader.readLine());
		
	for (int i = 1; i <= n; i++) {
	    for (int j = 0; j < i; j++) {

		bfrWriter.write("*");
	    }

	    bfrWriter.write("\n");
	}

	bfrWriter.flush();
	bfrWriter.close();
	bfrReader.close();
    }
}
```

## [2439번 - 별 찍기 - 2](https://www.acmicpc.net/problem/2439){: target="_blank" }

![13-2439](/assets/img/posts/algorithm-problem/boj/java/step-by-step-loop/13-2439.png)

```java
import java.io.*;

public class Main {

    public static void main(String[] args) throws IOException {

	BufferedReader bfrReader = new BufferedReader(new InputStreamReader(System.in));
	BufferedWriter bfrWriter = new BufferedWriter(new OutputStreamWriter(System.out));

	int n = Integer.parseInt(bfrReader.readLine());

	for (int i = 0; i < n; i++) {
	    int spaceCount = n - i - 1;
	    int starCount = i + 1;

	        for (int j = 0; j < spaceCount; j++) {
		    bfrWriter.write(" ");

		}
			
		for (int k = 0; k < starCount; k++) {
		    bfrWriter.write("*");

		}
			
	    bfrWriter.write("\n");
	}

	bfrWriter.flush();
	bfrWriter.close();
	bfrReader.close();
    }
}
```

## [10952번 - A+B - 5](https://www.acmicpc.net/problem/10952){: target="_blank" }

![14-10952](/assets/img/posts/algorithm-problem/boj/java/step-by-step-loop/14-10952.png)

```java
import java.io.*;
import java.util.StringTokenizer;

public class Main {

    public static void main(String[] args) throws IOException {

	BufferedReader bfrReader = new BufferedReader(new InputStreamReader(System.in));
	BufferedWriter bfrWriter = new BufferedWriter(new OutputStreamWriter(System.out));
	StringTokenizer strTokenizer;

	int sum = 0;

	while (true) {
	    strTokenizer = new StringTokenizer(bfrReader.readLine());

	    int a = Integer.parseInt(strTokenizer.nextToken());
	    int b = Integer.parseInt(strTokenizer.nextToken());

	    sum = a + b;

	    if (a == 0 && b == 0) {
		break;
	    }
			
	    bfrWriter.write(sum + "\n");
	}

	bfrWriter.flush();
	bfrWriter.close();
	bfrReader.close();
    }
}
```

## [10951번 - A+B - 4](https://www.acmicpc.net/problem/10951){: target="_blank" }

![15-10951](/assets/img/posts/algorithm-problem/boj/java/step-by-step-loop/15-10951.png)

```java
import java.io.*;
import java.util.StringTokenizer;

public class Main {

    public static void main(String[] args) throws IOException {

	BufferedReader bfrReader = new BufferedReader(new InputStreamReader(System.in));
	BufferedWriter bfrWriter = new BufferedWriter(new OutputStreamWriter(System.out));
	StringTokenizer strTokenizer;

	String input;
	int sum = 0;

	while ((input = bfrReader.readLine()) != null) {
	    strTokenizer = new StringTokenizer(input);

	    int a = Integer.parseInt(strTokenizer.nextToken());
	    int b = Integer.parseInt(strTokenizer.nextToken());

	    sum = a + b;

	    bfrWriter.write(sum + "\n");
	}

	bfrReader.close();
	bfrWriter.flush();
	bfrWriter.close();
    }
}
```