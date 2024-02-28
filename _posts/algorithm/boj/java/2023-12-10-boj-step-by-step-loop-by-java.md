---
title: "[백준 | Java]단계별로 풀어보기 반복문 모든 문제"
categories: [알고리즘, 백준]
tags: [알고리즘, 백준, Java, 자바, 단계별로 풀어보기, 반복문]
---

# 백준 단계별로 풀어보기 반복문 모든 문제

## 2739번 구구단(<https://www.acmicpc.net/problem/2739>{: target="_blank" })

![2739(1)](/assets/img/posts/algorithm/boj/java/step-by-step-loop/2739(1).png)

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
	Scanner sc = new Scanner(System.in);
		
	int n = sc.nextInt();
		
	for (int i = 1; i < 10; i++) {
		System.out.println(n + " * " + i + " = " + n * i);
	}
    }
}
```

## 10950번 A+B - 3(<https://www.acmicpc.net/problem/10950>{: target="_blank" })

![10950(1)](/assets/img/posts/algorithm/boj/java/step-by-step-loop/10950(1).png)

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
	Scanner sc = new Scanner(System.in);
		
	int t = sc.nextInt();
		
	for (int i = 0; i < t; i++) {
		int a = sc.nextInt();
		int b = sc.nextInt();
		int result = a + b;
			
		System.out.println(result);
	}
    }
}
```

## 8393번 합(<https://www.acmicpc.net/problem/8393>{: target="_blank" })

![8393(1)](/assets/img/posts/algorithm/boj/java/step-by-step-loop/8393(1).png)

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
	Scanner sc = new Scanner(System.in);
		
	int n = sc.nextInt();
	int sum = 0;
		
	for (int i = 1; i <= n; i++) {
		sum += i;
	}
		
	System.out.println(sum);
    }
}
```

## 25304번 영수증(<https://www.acmicpc.net/problem/25304>{: target="_blank" })

![25304(1)](/assets/img/posts/algorithm/boj/java/step-by-step-loop/25304(1).png)
![25304(2)](/assets/img/posts/algorithm/boj/java/step-by-step-loop/25304(2).png)

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
	Scanner sc = new Scanner(System.in);
		
	int X = sc.nextInt();
	int n = sc.nextInt();
	int sum = 0;
		
	for (int i = 0; i < n; i++) {
		int a = sc.nextInt();
		int b = sc.nextInt();
			
		sum += a * b;
	}
		
	if (X == sum) {
		System.out.println("Yes");

	} else {
		System.out.println("No");
	}
    }
}
```

## 25314번 코딩은 체육과목 입니다(<https://www.acmicpc.net/problem/25314>{: target="_blank" })

![25314(1)](/assets/img/posts/algorithm/boj/java/step-by-step-loop/25314(1).png)
![25314(2)](/assets/img/posts/algorithm/boj/java/step-by-step-loop/25314(2).png)

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
	Scanner sc = new Scanner(System.in);
		
	int n = sc.nextInt();
		
	for (int i = 0; i < (n / 4); i++) {
		System.out.print("long ");
	}
		
	System.out.println("int");
    }
}
```

## 15552번 빠른 A+B(<https://www.acmicpc.net/problem/15552>{: target="_blank" })

![15552(1)](/assets/img/posts/algorithm/boj/java/step-by-step-loop/15552(1).png)
![15552(2)](/assets/img/posts/algorithm/boj/java/step-by-step-loop/15552(2).png)

```java
import java.io.*;
import java.util.StringTokenizer;

public class Main {
    public static void main(String[] args) throws IOException {
	BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
	BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
	StringTokenizer tkn;
		
	int t = Integer.parseInt(br.readLine());
	int sum = 0;
		
	for (int i = 0; i < t; i++) {
		tkn = new StringTokenizer(br.readLine());
			
		int a = Integer.parseInt(tkn.nextToken());
		int b = Integer.parseInt(tkn.nextToken());
			
		sum = a + b;
			
		bw.write(sum + "\n");
	}

	bw.flush();
	bw.close();
	br.close();
    }
}
```

문제의 설명대로 자바(Java)를 이용하여 문제를 풀 때, `Scanner`를 사용하면 시간초과되어 오답처리 될 수 있기 때문에 앞으로는 `BufferedWriter`까지는 아니더라도 `BufferedReader`를 사용하는 것을 권장합니다. 위 문제에서는 `println()`메서드로 출력하려했더니 시간초과되서 `BufferedWriter`를 사용했습니다.

## 11021번 A+B - 7(<https://www.acmicpc.net/problem/11021>{: target="_blank" })

![11021(1)](/assets/img/posts/algorithm/boj/java/step-by-step-loop/11021(1).png)

```java
import java.io.*;
import java.util.StringTokenizer;

public class Main {
    public static void main(String[] args) throws IOException {
	BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
	BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
	StringTokenizer tkn;
		
	int t = Integer.parseInt(br.readLine());
	int sum = 0;
		
	for (int i = 1; i <= t; i++) {
		tkn = new StringTokenizer(br.readLine());
			
		int a = Integer.parseInt(tkn.nextToken());
		int b = Integer.parseInt(tkn.nextToken());
			
		sum = a + b;
			
		bw.write("Case #" + i + ": " + sum + "\n");
	}

	bw.flush();
	bw.close();
	br.close();		
    }
}
```

## 11022번 A+B - 8(<https://www.acmicpc.net/problem/11022>{: target="_blank" })

![11022(1)](/assets/img/posts/algorithm/boj/java/step-by-step-loop/11022(1).png)

```java
import java.io.*;
import java.util.StringTokenizer;

public class Main {
    public static void main(String[] args) throws IOException {
	BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
	BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
	StringTokenizer tkn;
		
	int t = Integer.parseInt(br.readLine());
	int sum = 0;
		
	for (int i = 1; i <= t; i++) {
		tkn = new StringTokenizer(br.readLine());
			
		int a = Integer.parseInt(tkn.nextToken());
		int b = Integer.parseInt(tkn.nextToken());
			
		sum = a + b;
			
		bw.write("Case #" + i + ": " + a + " + " + b + " = " + sum + "\n");
	}

	bw.flush();
	bw.close();
	br.close();
    }
}
```

## 2438번 별 찍기 - 1(<https://www.acmicpc.net/problem/2438>{: target="_blank" })

![2438(1)](/assets/img/posts/algorithm/boj/java/step-by-step-loop/2438(1).png)

```java
import java.io.*;

public class Main {
    public static void main(String[] args) throws IOException {
	BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
	BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
		
	int n = Integer.parseInt(br.readLine());
		
	for (int i = 1; i <= n; i++) {
		for (int j = 0; j < i; j++) {
			bw.write("*");
		}
		bw.write("\n");
	}

	bw.flush();
	bw.close();
	br.close();
    }
}
```

## 2439번 별 찍기 - 2(<https://www.acmicpc.net/problem/2439>{: target="_blank" })

![2439(1)](/assets/img/posts/algorithm/boj/java/step-by-step-loop/2439(1).png)

```java
import java.io.*;

public class Main {
    public static void main(String[] args) throws IOException {
	BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
	BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

	int n = Integer.parseInt(br.readLine());

	for (int i = 0; i < n; i++) {
		int spaces = n - i - 1;
		int stars = i + 1;

		for (int j = 0; j < spaces; j++) {
			bw.write(" ");
		}
			
		for (int k = 0; k < stars; k++) {
			bw.write("*");
		}
			
		bw.write("\n");
	}

	bw.flush();
	bw.close();
	br.close();
    }
}
```

## 10952번 A+B - 5(<https://www.acmicpc.net/problem/10952>{: target="_blank" })

![10952(1)](/assets/img/posts/algorithm/boj/java/step-by-step-loop/10952(1).png)

```java
import java.io.*;
import java.util.StringTokenizer;

public class Main {
    public static void main(String[] args) throws IOException {
	BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
	BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
	StringTokenizer tkn;

	int sum = 0;

	while (true) {
		tkn = new StringTokenizer(br.readLine());

		int a = Integer.parseInt(tkn.nextToken());
		int b = Integer.parseInt(tkn.nextToken());

		sum = a + b;

		if (a == 0 && b == 0) {
			break;
		}
			
		bw.write(sum + "\n");
	}

	bw.flush();
	bw.close();
	br.close();
    }
}
```

## 10951번 A+B - 4(<https://www.acmicpc.net/problem/10951>{: target="_blank" })

![10951(1)](/assets/img/posts/algorithm/boj/java/step-by-step-loop/10951(1).png)

```java
import java.io.*;
import java.util.StringTokenizer;

public class Main {
    public static void main(String[] args) throws IOException {
	BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
	BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
	StringTokenizer tkn;

	String input;
	int sum = 0;

	while ((input = br.readLine()) != null) {
		tkn = new StringTokenizer(input);

		int a = Integer.parseInt(tkn.nextToken());
		int b = Integer.parseInt(tkn.nextToken());

		sum = a + b;

		bw.write(sum + "\n");
	}

	br.close();
	bw.flush();
	bw.close();
    }
}
```