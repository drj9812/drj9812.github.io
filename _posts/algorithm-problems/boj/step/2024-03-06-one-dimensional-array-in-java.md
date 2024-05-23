---
title: "[BOJ | 단계별로 풀어보기]1차원 배열(Java)"
categories: [Algorithm Problems, BOJ]
tags:  [Algorithm, 알고리즘, BOJ, 백준, Java, 자바, 단계별로 풀어보기, 1차원 배열]
image:
  path: /assets/img/posts/algorithm-problems/boj/01-boj-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Baekjoon Online Judge
---

# 1차원 배열(Java)

2024-03-06 기준

## [10807번 - 개수 세기](https://www.acmicpc.net/problem/10807){: target="_blank" }

![01-10807](/assets/img/posts/algorithm-problems/boj/step/one-dimensional-array/01-10807.jpg)

```java
import java.io.*;
import java.util.StringTokenizer;

public class Main {

    public static void main(String[] args) throws IOException {

        BufferedReader bfrReader = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer strTokenizer;

        int count = 0;
        int n = Integer.parseInt(bfrReader.readLine());
        int[] numbers = new int[n];
		
        strTokenizer = new StringTokenizer(bfrReader.readLine());
        int target = Integer.parseInt(bfrReader.readLine());
		
        for(int i = 0; i < numbers.length; i++) {
    	    numbers[i] = Integer.parseInt(strTokenizer.nextToken());
			
	    if (numbers[i] == target) {
	        count++;
	    }
        }
		
    System.out.println(count);
    }
}
```

## [10871번 - X보다 작은 수](https://www.acmicpc.net/problem/10871){: target="_blank" }

![02-10871](/assets/img/posts/algorithm-problems/boj/step/one-dimensional-array/02-10871.jpg)

```java
import java.io.*;
import java.util.StringTokenizer;

public class Main {

    public static void main(String[] args) throws IOException {

	BufferedReader bfrReader = new BufferedReader(new InputStreamReader(System.in));
	StringBuilder strBuilder = new StringBuilder();
	StringTokenizer strTokenizer = new StringTokenizer(bfrReader.readLine());
		
	int n = Integer.parseInt(strTokenizer.nextToken());
	int[] numbers = new int[n];

	int x = Integer.parseInt(strTokenizer.nextToken());
	strTokenizer = new StringTokenizer(bfrReader.readLine());
		
	for (int i = 0; i < n; i++) {
	    numbers[i] = Integer.parseInt(strTokenizer.nextToken());
		
	    if (numbers[i] < x) {
		strBuilder.append(numbers[i] + " ");
	    }
	}
		
	System.out.println(strBuilder.toString());
    }
}
```

## [10818번 - 최소, 최대](https://www.acmicpc.net/problem/10818){: target="_blank" }

![03-10818](/assets/img/posts/algorithm-problems/boj/step/one-dimensional-array/03-10818.jpg)

```java
import java.io.*;
import java.util.StringTokenizer;

public class Main {

    public static void main(String[] args) throws IOException {

	BufferedReader bfrReader = new BufferedReader(new InputStreamReader(System.in));
	StringTokenizer strTokenizer;
		
	int n = Integer.parseInt(bfrReader.readLine());
	strTokenizer = new StringTokenizer(bfrReader.readLine());
		
	int max = Integer.parseInt(strTokenizer.nextToken());
	int min = max;
		
	for (int i = 0; i < n - 1; i++) {
	    int number = Integer.parseInt(strTokenizer.nextToken());
			
	    if (max < number) {
		max = number;

	    } else if (min > number) {
		min = number;
	    }
	}
		
	System.out.println(min + " " + max);
    }
}
```

## [2562번 - 최댓값](https://www.acmicpc.net/problem/2562){: target="_blank" }

![04-2562](/assets/img/posts/algorithm-problems/boj/step/one-dimensional-array/04-2562.jpg)

```java
import java.io.*;

public class Main {

    public static void main(String[] args) throws IOException {

	BufferedReader bfrReader = new BufferedReader(new InputStreamReader(System.in));

        int indexOfMax = 1;
	int max = Integer.parseInt(bfrReader.readLine());
		
	for (int i = 2; i < 10; i++) {
	    int num = Integer.parseInt(bfrReader.readLine());
			
	    if (num > max) {
		max = num;
		indexOfMax = i;
	    }
	}
		
	System.out.println(max);
	System.out.println(indexOfMax);
    }
}
```

## [10810번 - 공 넣기](https://www.acmicpc.net/problem/10810){: target="_blank" }

![05-10810](/assets/img/posts/algorithm-problems/boj/step/one-dimensional-array/05-10810.jpg)

```java
import java.io.*;
import java.util.StringTokenizer;

public class Main {

    public static void main(String[] args) throws IOException {

	BufferedReader bfrReader = new BufferedReader(new InputStreamReader(System.in));
	StringTokenizer strTokenizer = new StringTokenizer(bfrReader.readLine());

	int n = Integer.parseInt(strTokenizer.nextToken());
	int m = Integer.parseInt(strTokenizer.nextToken());

	int[] baskets = new int[n];

	for (int i = 0; i < m; i++) {
	    strTokenizer = new StringTokenizer(bfrReader.readLine());

	    int startIdx = Integer.parseInt(strTokenizer.nextToken());
	    int endIdx = Integer.parseInt(strTokenizer.nextToken());
	    int ballNumber = Integer.parseInt(strTokenizer.nextToken());

	    for (int j = startIdx - 1; j < endIdx; j++) {
	        baskets[j] = ballNumber;
	    }
	}

	for (int i = 0; i < baskets.length; i++) {
		System.out.print(baskets[i] + " ");
	}
    }
}
```

## [10813번 - 공 바꾸기](https://www.acmicpc.net/problem/10813){: target="_blank" }

![06-10813](/assets/img/posts/algorithm-problems/boj/step/one-dimensional-array/06-10813.jpg)

```java
import java.io.*;
import java.util.StringTokenizer;
import java.util.stream.IntStream;

public class Main {

    public static void main(String[] args) throws IOException {

	BufferedReader bfrReader = new BufferedReader(new InputStreamReader(System.in));
	StringTokenizer strTokenizer = new StringTokenizer(bfrReader.readLine());
		
	int n = Integer.parseInt(strTokenizer.nextToken());
	int m = Integer.parseInt(strTokenizer.nextToken());
		
	int[] baskets = IntStream.rangeClosed(1, n).toArray();
		
	for (int k = 0; k < m; k++) {
	    strTokenizer = new StringTokenizer(bfrReader.readLine());
			
	    int i = Integer.parseInt(strTokenizer.nextToken());
	    int j = Integer.parseInt(strTokenizer.nextToken());

	    int tmp = baskets[i - 1];
	    baskets[i - 1] = baskets[j - 1];
	    baskets[j - 1] = tmp;
	}
		
	for (int k = 0; k < baskets.length; k++) {
	    System.out.print(baskets[k] + " ");
	}
    }
}
```

## [5597번 - 과제 안 내신 분..?](https://www.acmicpc.net/problem/5597){: target="_blank" }

![07-5597(1)](/assets/img/posts/algorithm-problems/boj/step/one-dimensional-array/07-5597(1).jpg)
![08-5597(2)](/assets/img/posts/algorithm-problems/boj/step/one-dimensional-array/08-5597(2).jpg)
![09-5597(3)](/assets/img/posts/algorithm-problems/boj/step/one-dimensional-array/09-5597(3).jpg)

```java
import java.io.*;

public class Main {

    public static void main(String[] args) throws IOException {

	BufferedReader bfrReader = new BufferedReader(new InputStreamReader(System.in));
		
	boolean[] submissionStatus = new boolean[30];
		
	for (int i = 0; i < 28; i++) {
	    int num = Integer.parseInt(bfrReader.readLine());
	    submissionStatus[num - 1] = true;
	}
		
	for (int i = 0; i < submissionStatus.length; i++) {

            if (submissionStatus[i] == false) {
		System.out.println(i + 1);
	    }
	}
    }
}
```

## [3052번 - 나머지](https://www.acmicpc.net/problem/3052){: target="_blank" }

![10-3052(1)](/assets/img/posts/algorithm-problems/boj/step/one-dimensional-array/10-3052(1).jpg)
![11-3052(2)](/assets/img/posts/algorithm-problems/boj/step/one-dimensional-array/11-3052(2).jpg)

```java
import java.io.*;
import java.util.HashSet;

public class Main {

    public static void main(String[] args) throws IOException {

	BufferedReader bfrReader = new BufferedReader(new InputStreamReader(System.in));
		
	HashSet<Integer> remainders= new HashSet<>();

	for (int i = 0; i < 10; i++) {
	    remainders.add(Integer.parseInt(bfrReader.readLine()) % 42);
	}
		
        System.out.println(remainders.size());
    }
}
```
```java
import java.io.*;

public class Main {
    public static void main(String[] args) throws IOException {
	BufferedReader bfrReader = new BufferedReader(new InputStreamReader(System.in));

	boolean[] remainderCheck = new boolean[42];
	int count = 0;
		
	for (int i = 0; i < 10; i++) {
    	    remainderCheck[Integer.parseInt(bfrReader.readLine()) % 42] = true;
	}
		
	for (int i = 0; i < remainderCheck.length; i++) {

	    if (remainderCheck[i]==true) {
                count++;
	    }
	}
		
	System.out.println(count);
    }
}
```

## [10811번 - 바구니 뒤집기](https://www.acmicpc.net/problem/10811){: target="_blank" }

![12-10811](/assets/img/posts/algorithm-problems/boj/step/one-dimensional-array/12-10811.jpg)

```java
import java.io.*;
import java.util.StringTokenizer;
import java.util.stream.IntStream;

public class Main {

    public static void main(String[] args) throws IOException {

	BufferedReader bfrReader = new BufferedReader(new InputStreamReader(System.in));
	StringTokenizer strTokenizer = new StringTokenizer(bfrReader.readLine());
		
	int n = Integer.parseInt(strTokenizer.nextToken());
	int m = Integer.parseInt(strTokenizer.nextToken());
		
	int[] baskets = IntStream.rangeClosed(1, n).toArray();
		
	for (int k = 0; k < m; k++) {
	    strTokenizer = new StringTokenizer(bfrReader.readLine());
			
	    int i = Integer.parseInt(strTokenizer.nextToken()) - 1;
	    int j = Integer.parseInt(strTokenizer.nextToken()) - 1;
	    int tmp = 0;
			
	    for (int l = 0; l < j - i + 1 / 2; i++, j--) {
	        tmp = baskets[i];
	        baskets[i] = baskets[j];
	        baskets[j] = tmp;
	    }
	}
		
        for (int k = 0; k < baskets.length; k++) {

	    System.out.print(baskets[k] + " ");
	}
    }
}
```

시작 값인 `i`에 해당하는 값과 끝 값인 `j`에 해당하는 값을 서로 교체한 후, `i`는 증가하고 `j`는 감소시키면서 다시 서로 교체하는 과정을 범위의 절반(`j - i + 1 / 2`) 번만 반복하면 역순으로 정렬이 된다.

예를 들어 1부터 4를 역순으로 정렬한다고 하면 2번(1과 4, 2와 3)만 교체해주면 역순이 완성되므로 4번(1과 4, 2와 3, 3과 2, 4와 1)을 교체할 필요가 없다.

## [1546번 - 평균](https://www.acmicpc.net/problem/1546){: target="_blank" }

![13-1546(1)](/assets/img/posts/algorithm-problems/boj/step/one-dimensional-array/13-1546(1).jpg)
![14-1546(2)](/assets/img/posts/algorithm-problems/boj/step/one-dimensional-array/14-1546(2).jpg)

```java
import java.io.*;
import java.util.StringTokenizer;

public class Main {

    public static void main(String[] args) throws IOException {

	BufferedReader bfrReader = new BufferedReader(new InputStreamReader(System.in));

	int n = Integer.parseInt(bfrReader.readLine());
	StringTokenizer strTokenizer = new StringTokenizer(bfrReader.readLine());

	int[] scores = new int[n];
	int maxScore = 0;
	double sum = 0;

	for (int i = 0; i < scores.length; i++) {
	    scores[i] = Integer.parseInt(strTokenizer.nextToken());

	    if (scores[i] > maxScore) {
	        maxScore = scores[i];
	    }
	}

	for (int i = 0; i < scores.length; i++) {
	    sum += ((double) scores[i] / maxScore) * 100;
        }

	double avg = sum / scores.length;
	System.out.println(avg);
    }
}
```
