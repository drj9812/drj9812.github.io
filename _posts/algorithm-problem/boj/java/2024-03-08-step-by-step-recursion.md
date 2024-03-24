---
title: "[백준 | Java]단계별로 풀어보기 재귀 모든 문제"
categories: [Algorithm Problem, BOJ]
tags:  [Algorithm, 알고리즘, BOJ, 백준, Java, 자바, 단계별로 풀어보기, 재귀]
---

# 단계별로 풀어보기 재귀 모든 문제

## [27433번 - 팩토리얼 2](https://www.acmicpc.net/problem/27433){: target="_blank" }

![27433(1)](/assets/img/posts/algorithm-problem/boj/java/step-by-step-recursion/27433(1).jpg)

```java
import java.io.*;

public class Main {

    public static void main(String[] args) throws IOException {

        BufferedReader bfrReader = new BufferedReader(new InputStreamReader(System.in));
		
        long n = Integer.parseInt(bfrReader.readLine());
        System.out.println(factorial(n));
    }
	
    private static long factorial(long n) {
        if (n == 0) {
            return 1;
			
        } else {
            n *= factorial(n - 1);
			
            return n;
        }
    }
}
```

## [10870번 - 피보나치 수 5](https://www.acmicpc.net/problem/10870){: target="_blank" }

![10870(1)](/assets/img/posts/algorithm-problem/boj/java/step-by-step-recursion/10870(1).jpg)

```java
import java.io.*;

public class Main {

    public static void main(String[] args) throws IOException {

        BufferedReader bfrReader = new BufferedReader(new InputStreamReader(System.in));
		
        int n = Integer.parseInt(bfrReader.readLine());
        System.out.println(getFibonacciNumber(n));
    }
	
    private static int getFibonacciNumber (int n) {
        if (n <= 0) {
            return 0;
			
        } else if (n == 1) {
            return 1;
			
        } else {
            n = getFibonacciNumber(n - 1) + getFibonacciNumber(n - 2);
			
            return n;
        }
    }
}
```

## [25501번 - 재귀의 귀재](https://www.acmicpc.net/problem/25501){: target="_blank" }

![25501(1)](/assets/img/posts/algorithm-problem/boj/java/step-by-step-recursion/25501(1).jpg)
![25501(2)](/assets/img/posts/algorithm-problem/boj/java/step-by-step-recursion/25501(2).jpg)
![25501(3)](/assets/img/posts/algorithm-problem/boj/java/step-by-step-recursion/25501(3).jpg)

```java
import java.io.*;

public class Main {

    static int recursionCount = 0;
	
    public static void main(String[] args) throws IOException {

        BufferedReader bfrReader = new BufferedReader(new InputStreamReader(System.in));
    	
    	int t = Integer.parseInt(bfrReader.readLine());
    	
    	for (int i = 0; i < t; i++) {
    	    String s = bfrReader.readLine();
    	    int isPalindromReturnValue = isPalindrome(s);
    		
    	    System.out.println(isPalindromReturnValue + " " + recursionCount);
    	}
    }
    
    private static int isPalindrome(String s) {

    	recursionCount = 0;
    	
    	return recursion(s, 0, s.length() - 1);
    }
    
    private static int recursion(String s, int l, int r) {

    	recursionCount++;
    	
    	if (l >= r) {
    	    return 1;
    		
    	} else if (s.charAt(l) != s.charAt(r)) {
    	    return 0;
    		
    	} else {
    	    return recursion(s, l + 1, r - 1);
    	}
    }
}

```

## [24060번 - 알고리즘 수업 - 병합 정렬 1](https://www.acmicpc.net/problem/24060){: target="_blank" }

![24060(1)](/assets/img/posts/algorithm-problem/boj/java/step-by-step-recursion/24060(1).jpg)
![24060(2)](/assets/img/posts/algorithm-problem/boj/java/step-by-step-recursion/24060(2).jpg)

```java
import java.io.*;
import java.util.StringTokenizer;

public class Main {

    private static int[] tmp; // 병합할 때 사용할 임시 배열
    private static int k;
    private static int target = -1;
    private static int count = 0;

    public static void main(String[] args) throws IOException {

        BufferedReader bfrReader = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer strTokenizer = new StringTokenizer(bfrReader.readLine());
		
        int n = Integer.parseInt(strTokenizer.nextToken());
        k = Integer.parseInt(strTokenizer.nextToken());
		
        int[] arr = new int[n];
        tmp = new int[arr.length]; // 병합할 때 사용할 임시 배열 생성
		
        strTokenizer = new StringTokenizer(bfrReader.readLine());
		
        for(int i = 0; i < n; i++){
            arr[i] = Integer.parseInt(strTokenizer.nextToken());
        }

        mergeSort(arr, 0, arr.length - 1); // 병합 정렬 수행

        System.out.println(target);
	}

    private static void mergeSort(int[] arr, int start, int end) {

        if (count > k) return;
		
        if (start < end) { // 배열의 길이가 2 이상일 경우에만 분할
            int mid = (start + end) / 2; // 배열을 반으로 나누는 중간 인덱스

            mergeSort(arr, start, mid); // 왼쪽 부분 배열을 재귀적으로 정렬
            mergeSort(arr, mid + 1, end); // 오른쪽 부분 배열을 재귀적으로 정렬

            merge(arr, start, mid, end); // 정렬된 두 배열을 병합
        }
    }

    private static void merge(int[] arr, int start, int mid, int end) {

        int l = start; // 왼쪽 부분 배열의 시작 인덱스
        int r = mid + 1; // 오른쪽 부분 배열의 시작 인덱스
        int index = start; // 임시 배열에 값을 저장할 인덱스

        while (l <= mid && r <= end) { // 분할한 배열 중 어느 배열이라도 배열의 끝을 넘어가면 실행 X
            if (arr[l] <= arr[r]) {
                tmp[index++] = arr[l++]; // 왼쪽 부분 배열의 값이 작으면 tmp 배열에 저장하고 l, idx 증가

            } else {
                tmp[index++] = arr[r++]; // 오른쪽 부분 배열의 값이 작으면 tmp 배열에 저장하고 r, idx 증가
            }
        }

        while (l <= mid) {
            tmp[index++] = arr[l++]; // 왼쪽 부분 배열이 남은 경우 tmp 배열에 복사
        }

        while (r <= end) {
            tmp[index++] = arr[r++]; // 오른쪽 부분 배열이 남은 경우 tmp 배열에 복사
        }

        for (int i = start; i <= end; i++) {
            arr[i] = tmp[i]; // 임시 배열의 값을 원래 배열에 복사
            count++;
			
            if (count == k) {
                target = arr[i];

                break;
            }
        }
    }
}
```

병합 정렬에서 정렬할 때 사용되는 임시 배열(`tmp`)의 내용을 비우겠다고 `merge()` 메서드 내에서 계속 초기화를 시켜줬더니 시간초과되서 오답처리가 되었다. 임시 배열은 어차피 분할 정복 과정에서 기존의 데이터들을 덮어씌우기 때문에 굳이 초기화를 안 해도 상관없다.

저장 횟수가 K보다 작을 때 -1을 출력하라는 문제의 조건을 구현하는 `if(cnt > k) { return; }` 코드가 어디에 위치하는 것이 적절할지 고민을 해봤는데, 속도 측면에서는 `merge()` 메서드 내에 위치하더라도 유의미한 차이는 없는 것 같다.

> [[Algorithm]병합 정렬(Merge Sort)](https://drj9812.github.io/posts/merge-sort/){: target="_blank" } 참조
{: .prompt-tip }

## [4779번 - 칸토어 집합](https://www.acmicpc.net/problem/4779){: target="_blank" }

![4779(1)](/assets/img/posts/algorithm-problem/boj/java/step-by-step-recursion/4779(1).jpg)
![4779(2)](/assets/img/posts/algorithm-problem/boj/java/step-by-step-recursion/4779(2).jpg)

```java
import java.io.*;

public class Main {

    static StringBuilder strBuilder;

    public static void main(String[] args) throws IOException {

        BufferedReader bfrReader = new BufferedReader(new InputStreamReader(System.in));
        String input;

        while ((input = bfrReader.readLine()) != null) {
            int n = Integer.parseInt(input);
            strBuilder = new StringBuilder();
            int length = (int) Math.pow(3, n);

            for (int i = 0; i < length; i++) {
                strBuilder.append("-");
            }

            getCantorSet(0, length);
            System.out.println(strBuilder.toString());
        }
    }

    public static void getCantorSet(int startIdx, int length) {
        if (length == 1) {
            return;
        }

        int newLength = length / 3;

        for (int i = startIdx + newLength; i < startIdx + 2 * newLength; i++) {
            sb.setCharAt(i, ' ');
        }

        getCantorSet(startIdx, newLength);
        getCantorSet(startIdx + 2 * newLength, newLength);
    }
}
```

## [2447번 - 별 찍기 - 10](https://www.acmicpc.net/problem/2447){: target="_blank" }

![2447(1)](/assets/img/posts/algorithm-problem/boj/java/step-by-step-recursion/2447(1).jpg)
![2447(2)](/assets/img/posts/algorithm-problem/boj/java/step-by-step-recursion/2447(2).jpg)

```java
import java.io.*;
import java.util.Arrays;

public class Main {

    private static StringBuilder strBuilder;
    private static String[][] stars;

    public static void main(String[] args) throws IOException {

        BufferedReader bfrReader = new BufferedReader(new InputStreamReader(System.in));
        strBuilder = new StringBuilder();

        int n = Integer.parseInt(bfrReader.readLine());

        stars = new String[n][n];
        Arrays.stream(stars).forEach(row -> Arrays.fill(row, " "));

        printStars(0, 0, n);

        for (int i = 0; i < stars.length; i++) {
            for (int j = 0; j < stars[i].length; j++) {
                strBuilder.append(stars[i][j]);
            }
            strBuilder.append("\n");
        }

        System.out.println(strBuilder.toString());
    }

    private static void printStars(int x, int y, int size) {
        if (size == 1) {
            stars[x][y] = "*";
			
            return;
        }
		
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                if (!(i == 1 && j == 1)) {
                    printStars(x + i * (size / 3), y + j * (size / 3), size / 3);
                }
            }
        }
    }
}
```

## [11729번 - 하노이의 탑 이동 순서](https://www.acmicpc.net/problem/11729){: target="_blank" }

![11729(1)](/assets/img/posts/algorithm-problem/boj/java/step-by-step-recursion/11729(1).jpg)
![11729(2)](/assets/img/posts/algorithm-problem/boj/java/step-by-step-recursion/11729(2).jpg)

```java
import java.io.*;
 
public class Main {
 
    private static StringBuilder strBuilder = new StringBuilder();
 
    public static void main(String[] args) throws IOException {
		
    BufferedReader bfrReader = new BufferedReader(new InputStreamReader(System.in));
 
    int n = Integer.parseInt(bfrReader.readLine());
 
    strBuilder.append((int) (Math.pow(2, n) - 1)).append('\n');
 
    solveTowerOfHanoi(n, 1, 2, 3);
    System.out.println(strBuilder);
}
 
    private static void solveTowerOfHanoi(int n, int start, int mid, int end) {
        if (n == 1) {
	    strBuilder.append(start + " " + end + "\n");
	    return;
	}
 
	solveTowerOfHanoi(n - 1, start, end, mid);
	strBuilder.append(start + " " + end + "\n");
	solveTowerOfHanoi(n - 1, mid, start, end);
    }
}
```

## 참고자료
- [ST_, "[백준] 2447번 : 별 찍기 - 10 - JAVA [자바]", Stranger's LAB, 2020-05-16](https://st-lab.tistory.com/95){: target="_blank" }
- [ST_, "[백준] 11729번 : 하노이 탑 이동 순서 - JAVA [자바]", Stranger's LAB, 2020-05-16](https://st-lab.tistory.com/96){: target="_blank" }