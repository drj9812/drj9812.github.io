---
title: "[백준/자바(JAVA)] 단계별로 풀어보기 입출력과 사칙연산 모든 문제"
categories: [알고리즘, 백준]
tags: [알고리즘, 백준, 단계별로 풀어보기, 입출력과 사칙연산, 자바, java]
---

# [백준/자바(JAVA)]단계별로 풀어보기 입출력과 사칙연산 모든 문제

## 2557번 Hello World(<https://www.acmicpc.net/problem/2557>{: target="_blank" })

![2557(1)](/assets/img/posts/algorithm/java/boj-step-by-step-io-and-elementary-arithmetic-by-java/2557(1).png)

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Hello World!");
    }
}
```

## 1000번 A+B(<https://www.acmicpc.net/problem/1000>{: target="_blank" })

![1000(1)](/assets/img/posts/algorithm/java/boj-step-by-step-io-and-elementary-arithmetic-by-java/1000(1).png)

```java
import java.util.Scanner;

    public class Main {
        public static void main(String[] args) {
            Scanner sc = new Scanner(System.in);
            
            int a = sc.nextInt();
            int b = sc.nextInt();
            
	    int result = a + b;

            System.out.println(result);
        }
    }
```

## 1001번 A-B(<https://www.acmicpc.net/problem/1001>{: target="_blank" })

![1001(1)](/assets/img/posts/algorithm/java/boj-step-by-step-io-and-elementary-arithmetic-by-java/1001(1).png)

```java
import java.util.Scanner;

    public class Main {
        public static void main(String[] args) {
            Scanner sc = new Scanner(System.in) ;
            
                int a = sc.nextInt();
                int b = sc.nextInt();
                
		int result = a - b;

                System.out.println(result);
            }
    }
```

## 10998번 A*B(<https://www.acmicpc.net/problem/10998>{: target="_blank" })

![10998(1)](/assets/img/posts/algorithm/java/boj-step-by-step-io-and-elementary-arithmetic-by-java/10998(1).png)

```java
import java.util.Scanner;

    public class Main {
        public static void main(String[] args) {
            Scanner sc = new Scanner(System.in);
            
            int a = sc.nextInt();
            int b = sc.nextInt();
            
            int result = a * b;
            
            System.out.println(result);
        }
    }
```

## 1008번 A/B(<https://www.acmicpc.net/problem/1008>{: target="_blank" })

![1008(1)](/assets/img/posts/algorithm/java/boj-step-by-step-io-and-elementary-arithmetic-by-java/1008(1).png)

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in) ;
        
        double a = sc.nextDouble();
        double b = sc.nextDouble();
        
        double result = a / b;
        
        System.out.println(result);
    }
}
```

## 10869번 사칙연산(<https://www.acmicpc.net/problem/10869>{: target="_blank" })

![10869(1)](/assets/img/posts/algorithm/java/boj-step-by-step-io-and-elementary-arithmetic-by-java/10869(1).png)

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in) ;
        
        int a = sc.nextInt();
        int b = sc.nextInt();
        
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

## 10926번 ??!(<https://www.acmicpc.net/problem/10926>{: target="_blank" })

![10926(1)](/assets/img/posts/algorithm/java/boj-step-by-step-io-and-elementary-arithmetic-by-java/10926(1).png)

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        
        String input = sc.nextLine();
        String result = input + "??!";
        
        System.out.println(result);
    }
}
```

## 18108번 1998년생인 내가 태국에서는 2541년생?!(<https://www.acmicpc.net/problem/18108>{: target="_blank" })

![18108(1)](/assets/img/posts/algorithm/java/boj-step-by-step-io-and-elementary-arithmetic-by-java/18108(1).png)

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        
        int input = sc.nextInt();
        int result = input - 543;
        
        System.out.println(result);
    }
}
```

## 10430번 나머지(<https://www.acmicpc.net/problem/10430>{: target="_blank" })

![10430(1)](/assets/img/posts/algorithm/java/boj-step-by-step-io-and-elementary-arithmetic-by-java/10430(1).png)

```java
import java.util.Scanner;

public class P10430 {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        
        int a = sc.nextInt();
        int b = sc.nextInt();
        int c = sc.nextInt();
        
        System.out.println( (a + b) % c );
        System.out.println( ((a % c) + (b % c)) % c );
        System.out.println( (a * b ) % c );
        System.out.println( ((a % c) * (b % c)) % c );
    }
}
```

## 2588번 곱셈(<https://www.acmicpc.net/problem/2588>{: target="_blank" })

![2588(1)](/assets/img/posts/algorithm/java/boj-step-by-step-io-and-elementary-arithmetic-by-java/2588(1).png)

```java
import java.util.Scanner;

public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		
		int a = sc.nextInt();
		int b = sc.nextInt();
		
		System.out.println( a * (b % 10) );
		System.out.println( a * ((b % 100) / 10) );
		System.out.println( a * (b / 100));
		System.out.println( a * b);
	}
}
```

## 11382번 꼬마 정민(<https://www.acmicpc.net/problem/11382>{: target="_blank" })

![11382(1)](/assets/img/posts/algorithm/java/boj-step-by-step-io-and-elementary-arithmetic-by-java/11382(1).png)

```java
import java.io.*;
import java.util.StringTokenizer;

public class Main {
	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringTokenizer tkn = new StringTokenizer(br.readLine());
		
		long a = Long.parseLong(tkn.nextToken());
		long b = Long.parseLong(tkn.nextToken());
		long c = Long.parseLong(tkn.nextToken());
		
		long result = a + b + c;
		
		System.out.println(result);

		br.close();
	}
}
```

## 10171번 고양이(<https://www.acmicpc.net/problem/10171>{: target="_blank" })

![10171(1)](/assets/img/posts/algorithm/java/boj-step-by-step-io-and-elementary-arithmetic-by-java/10171(1).png)

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
역슬래시(\\)는 이스케이프 문자로 사용되고 있습니다. 이스케이프 문자는 특정 문자를 나타내기 위해 역슬래시를 사용하는데, \ 자체를 출력하려면 \\\로 작성해야 합니다.

## 10172번 개(<https://www.acmicpc.net/problem/10172>{: target="_blank" })

![10172(1)](/assets/img/posts/algorithm/java/boj-step-by-step-io-and-elementary-arithmetic-by-java/10172(1).png)
```java
public class P10172 {
    public static void main(String[] args) {
        System.out.println("|\\_/|");
        System.out.println("|q p|   /}");
        System.out.println("( 0 )\"\"\"\\");
        System.out.println("|\"^\"`    |");
        System.out.println("||_/=\\\\__|");
    }
}
```