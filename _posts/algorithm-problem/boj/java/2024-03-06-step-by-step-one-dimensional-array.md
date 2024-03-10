---
title: "[백준 | Java]단계별로 풀어보기 1차원 배열 모든 문제"
categories: [Algorithm Problem, BOJ]
tags:  [Algorithm, 알고리즘, BOJ, 백준, Java, 자바, 단계별로 풀어보기, 1차원 배열]
---

# 단계별로 풀어보기 1차원 배열 모든 문제

## [10807번 - 개수 세기](https://www.acmicpc.net/problem/10807){: target="_blank" }

![10807](/assets/img/posts/algorithm-problem/boj/java/step-by-step-one-dimensional-array/10807.jpg)

```java
import java.io.*;
import java.util.StringTokenizer;

public class Main {
    public static void main(String[] args) throws IOException {
    BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    StringTokenizer stkn;
    int cnt = 0;
		
    int n = Integer.parseInt(br.readLine());
    int[] nArray = new int[n];
		
    stkn = new StringTokenizer(br.readLine());
    int target = Integer.parseInt(br.readLine());
		
    for(int i = 0; i < nArray.length; i++) {
	nArray[i] = Integer.parseInt(stkn.nextToken());
			
	if (nArray[i] == target) {
	    cnt++;
	}
    }
		
    System.out.println(cnt);
    }
}
```

## [10871번 - X보다 작은 수](https://www.acmicpc.net/problem/10871){: target="_blank" }

![10871](/assets/img/posts/algorithm-problem/boj/java/step-by-step-one-dimensional-array/10871.jpg)

```java
import java.io.*;
import java.util.StringTokenizer;

public class Main {
    public static void main(String[] args) throws IOException {
	BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
	StringBuilder sb = new StringBuilder();
	StringTokenizer stkn = new StringTokenizer(br.readLine());
		
	int n = Integer.parseInt(stkn.nextToken());
	int[] nArray = new int[n];

	int x = Integer.parseInt(stkn.nextToken());
	stkn = new StringTokenizer(br.readLine());
		
	for (int i = 0; i < n; i++) {
	    nArray[i] = Integer.parseInt(stkn.nextToken());
		
	    if (nArray[i] < x) {
		sb.append(nArray[i] + " ");
	    }
	}
		
	System.out.println(sb.toString());
    }
}
```

## [10818번 - 최소, 최대](https://www.acmicpc.net/problem/10818){: target="_blank" }

![10818](/assets/img/posts/algorithm-problem/boj/java/step-by-step-one-dimensional-array/10818.jpg)

```java
import java.io.*;
import java.util.StringTokenizer;

public class Main {
    public static void main(String[] args) throws IOException {
	BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
	StringTokenizer stkn;
		
	int n = Integer.parseInt(br.readLine());
	stkn = new StringTokenizer(br.readLine());
		
	int max = Integer.parseInt(stkn.nextToken());
	int min = max;
		
	for (int i = 0; i < n - 1; i++) {
	    int num = Integer.parseInt(stkn.nextToken());
			
	    if (max < num) {
		max = num;
	    } else if (min > num) {
		min = num;
	    }
	}
		
	System.out.println(min + " " + max);
    }
}
```

## [2562번 - 최댓값](https://www.acmicpc.net/problem/2562){: target="_blank" }

![2562](/assets/img/posts/algorithm-problem/boj/java/step-by-step-one-dimensional-array/2562.jpg)

```java
import java.io.*;

public class Main {
    public static void main(String[] args) throws IOException {
	BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        int index = 1;
	int max = Integer.parseInt(br.readLine());
		
	for (int i = 2; i < 10; i++) {
	    int num = Integer.parseInt(br.readLine());
			
	    if (num > max) {
		max = num;
		index = i;
	    }
	}
		
	System.out.println(max);
	System.out.println(index);
    }
}
```

## [10810번 - 공 넣기](https://www.acmicpc.net/problem/10810){: target="_blank" }

![10810](/assets/img/posts/algorithm-problem/boj/java/step-by-step-one-dimensional-array/10810.jpg)

```java
import java.io.*;
import java.util.StringTokenizer;

public class Main {
    public static void main(String[] args) throws IOException {
	BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
	StringTokenizer stkn = new StringTokenizer(br.readLine());

	int n = Integer.parseInt(stkn.nextToken());
	int m = Integer.parseInt(stkn.nextToken());

	int[] basketArr = new int[n];

	for (int i = 0; i < m; i++) {
	    stkn = new StringTokenizer(br.readLine());

	    int start = Integer.parseInt(stkn.nextToken());
	    int end = Integer.parseInt(stkn.nextToken());
	    int number = Integer.parseInt(stkn.nextToken());

	    for (int j = start - 1; j < end; j++) {
	        basketArr[j] = number;
	}
	}

	for (int i = 0; i < basketArr.length; i++) {
		System.out.print(basketArr[i] + " ");
	}
    }
}
```

## [10813번 - 공 바꾸기](https://www.acmicpc.net/problem/10813){: target="_blank" }

![10813](/assets/img/posts/algorithm-problem/boj/java/step-by-step-one-dimensional-array/10813.jpg)

```java
import java.io.*;
import java.util.StringTokenizer;
import java.util.stream.IntStream;

public class Main {
    public static void main(String[] args) throws IOException {
	BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
	StringTokenizer stkn = new StringTokenizer(br.readLine());
		
	int n = Integer.parseInt(stkn.nextToken());
	int m = Integer.parseInt(stkn.nextToken());
		
	int[] basketArr = IntStream.rangeClosed(1, n).toArray();
		
	for (int k = 0; k < m; k++) {
	    stkn = new StringTokenizer(br.readLine());
			
	    int i = Integer.parseInt(stkn.nextToken());
	    int j = Integer.parseInt(stkn.nextToken());

	    int tmp = basketArr[i - 1];
	    basketArr[i - 1] = basketArr[j - 1];
	    basketArr[j - 1] = tmp;
	}
		
	for (int k = 0; k < basketArr.length; k++) {
	    System.out.print(basketArr[k] + " ");
	}
    }
}
```

## [5597번 - 과제 안 내신 분..?](https://www.acmicpc.net/problem/5597){: target="_blank" }

![5597(1)](/assets/img/posts/algorithm-problem/boj/java/step-by-step-one-dimensional-array/5597(1).jpg)
![5597(2)](/assets/img/posts/algorithm-problem/boj/java/step-by-step-one-dimensional-array/5597(2).jpg)
![5597(3)](/assets/img/posts/algorithm-problem/boj/java/step-by-step-one-dimensional-array/5597(3).jpg)

```java
import java.io.*;

public class Main {
    public static void main(String[] args) throws IOException {
	BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		
	boolean[] submissionStatus = new boolean[30];
		
	for (int i = 0; i < 28; i++) {
	    int num = Integer.parseInt(br.readLine());
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

![3052(1)](/assets/img/posts/algorithm-problem/boj/java/step-by-step-one-dimensional-array/3052(1).jpg)
![3052(2)](/assets/img/posts/algorithm-problem/boj/java/step-by-step-one-dimensional-array/3052(2).jpg)

```java
import java.io.*;
import java.util.HashSet;

public class Main {
    public static void main(String[] args) throws IOException {
	BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		
	HashSet<Integer> remainderSet = new HashSet<>();

	for (int i = 0; i < 10; i++) {
	    remainderSet.add(Integer.parseInt(br.readLine()) % 42);
	}
		
    System.out.println(remainderSet.size());
    }
}
```
```java
import java.io.*;

public class Main {
    public static void main(String[] args) throws IOException {
	BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

	boolean[] remainderCheck = new boolean[42];
	int cnt = 0;
		
	for (int i = 0; i < 10; i++) {
    	remainderCheck[Integer.parseInt(br.readLine()) % 42] = true;
	}
		
	for (int i = 0; i < remainderCheck.length; i++) {
	    if (remainderCheck[i]==true) {
                cnt++;
	    }
	}
		
	System.out.println(cnt);
    }
}
```

## [10811번 - 바구니 뒤집기](https://www.acmicpc.net/problem/10811){: target="_blank" }

![10811](/assets/img/posts/algorithm-problem/boj/java/step-by-step-one-dimensional-array/10811.jpg)

```java
import java.io.*;
import java.util.StringTokenizer;
import java.util.stream.IntStream;

public class Main {
    public static void main(String[] args) throws IOException {
	BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
	StringTokenizer stkn = new StringTokenizer(br.readLine());
		
	int n = Integer.parseInt(stkn.nextToken());
	int m = Integer.parseInt(stkn.nextToken());
		
	int[] basketArr = IntStream.rangeClosed(1, n).toArray();
		
	for (int k = 0; k < m; k++) {
	    stkn = new StringTokenizer(br.readLine());
			
	    int i = Integer.parseInt(stkn.nextToken()) - 1;
	    int j = Integer.parseInt(stkn.nextToken()) - 1;
	    int tmp = 0;
			
	    for (int l = 0; l < j - i + 1 / 2; i++, j--) {
	        tmp = basketArr[i];
	        basketArr[i] = basketArr[j];
	        basketArr[j] = tmp;
	    }
	}
		
        for (int k = 0; k < basketArr.length; k++) {
	    System.out.print(basketArr[k] + " ");
	}
    }
}
```

시작 값인 `i`에 해당하는 값과 끝 값인 `j`에 해당하는 값을 서로 교체한 후, `i`는 증가하고 `j`는 감소시키면서 다시 서로 교체하는 과정을 범위의 절반(`j - i + 1 / 2`) 번만 반복하면 역순으로 정렬이 된다.

예를 들어 1부터 4를 역순으로 정렬한다고 하면 2번(1과 4, 2와 3)만 교체해주면 역순이 완성되므로 4번(1과 4, 2와 3, 3과 2, 4와 1)을 교체할 필요가 없다.

## [1546번 - 평균](https://www.acmicpc.net/problem/1546){: target="_blank" }

![1546(1)](/assets/img/posts/algorithm-problem/boj/java/step-by-step-one-dimensional-array/1546(1).jpg)
![1546(2)](/assets/img/posts/algorithm-problem/boj/java/step-by-step-one-dimensional-array/1546(2).jpg)

```java
import java.io.*;
import java.util.StringTokenizer;

public class Main {
    public static void main(String[] args) throws IOException {
	BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

	int n = Integer.parseInt(br.readLine());
	StringTokenizer stkn = new StringTokenizer(br.readLine());

	int[] scoreArr = new int[n];
	int maxScore = 0;
	double sum = 0;

	for (int i = 0; i < scoreArr.length; i++) {
	    scoreArr[i] = Integer.parseInt(stkn.nextToken());

	    if (scoreArr[i] > maxScore) {
	        maxScore = scoreArr[i];
	    }
	}

	for (int i = 0; i < scoreArr.length; i++) {
	    sum += ((double) scoreArr[i] / maxScore) * 100;
        }

	double avg = sum / scoreArr.length;
	System.out.println(avg);
    }
}
```
