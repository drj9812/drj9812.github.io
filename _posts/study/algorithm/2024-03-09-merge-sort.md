---
title: "[Algorithm]병합 정렬(Merge Sort)"
categories: [Study, Algorithm]
tags: [Algorithm, 알고리즘, Mergo Sort, 병합 정렬]
---

# 병합 정렬(Merge Sort)

## 병합 정렬이란?

- **배열을 반으로 나눈 후 각 부분을 정렬**하고, 이를 **병합**하여 전체 배열을 정렬하는 알고리즘
	+ 분할 정복(Divide and Conquer) 알고리즘 사용
- 일반적으로 재귀적으로 구현함
- 합병 정렬이라고도 불림

## 장점

- 중복 계산을 줄일 수 있기 때문에 일반적으로 **시간 복잡도가 낮음**
- 문제를 독립적인 부분으로 나누어 각각의 부분을 병렬로 처리할 수 있다는 특성으로 인해 병렬 컴퓨팅에 매우 유용
- 복잡한 문제를 간단한 부분 문제로 분해하여 해결하는 방식이므로 문제 해결이 보다 용이

## 단점

- 부분 문제를 해결하기 위해 재귀적으로 호출되는데, 이때 매 호출마다 추가적인 메모리 공간이 필요하기 때문에 **공간 복잡도가 높음**
- 문제를 여러 부분으로 나누어 해결하기 때문에 부분 문제의 크기가 작을 때에도 계속해서 함수 호출이 이루어지며, 이에 따라 함수 호출에 따른 **오버헤드**가 발생할 수 있음
- 재귀적인 호출이나 부분 문제의 분할 및 병합 과정의 구현이 복잡할 수 있음
- 임시 배열을 생성하고 생성된 임시 배열을 정렬시킨 후 원본 배열에 복사하는 방식이기 때문에 **배열의 길이가 길어질 수록 시간 복잡도가 높음** 

## 동작 과정

![01-merge-sort-process(1)](/assets/img/posts/study/algorithm/merge-sort/01-merge-sort-process(1).jpg){: target="_blank" }
*위 그림은 병합 정렬의 동작 과정을 간단하게 도식화한 것이다.*

### 분할(Divide)

- 주어진 배열의 중간 지점을 찾고, 중간을 기준으로 두 개의 배열로 분할
- 배열의 길이가 0개 또는 1개일 때는 이미 정렬된 상태이므로 더 이상 분할할 필요가 없음

### 정복(Conquer)

![02-merge-sort-process(2)](/assets/img/posts/study/algorithm/merge-sort/02-merge-sort-process(2).jpg){: target="_blank" }
*위 그림은 병합 정렬의 동작 과정을 간단하게 도식화한 것이다.*

- 각각의 분할된 배열을 재귀적으로 정렬
	1. 분할된 배열의 첫 번째 요소끼리 값을 비교해서 작은 값을 선택하고 임시 배열에 저장
	2. 선택된 요소의 배열의 인덱스를 증가시키면서 1번 단계 반복
	3. 배열의 끝에 다다라 더 이상 증가시킬 인덱스가 없다면, 남은 배열의 요소들을 전부 임시 배열에 저장
	4. 정렬이된 임시 배열의 값을 원본 배열에 복사
- 정렬을 하기 위해 임시 배열이 필요 

### 결합(Combine)

- 정렬된 부분 배열들을 하나의 배열에 병합
- 병합 정렬에서 실제로 정렬이 이루어지는 시점

## 구현 예시

```java
import java.io.*;
import java.util.Arrays;

public class mergeSortTest {
    private static int[] tmp; // 병합할 때 사용할 임시 배열

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		
        int[] arr = Arrays.stream(br.readLine().split(" ")).mapToInt(Integer::parseInt).toArray(); // 입력을 통해 배열 생성
        tmp = new int[arr.length]; // 병합할 때 사용할 임시 배열 생성
		
        mergeSort(arr, 0, arr.length - 1); // 병합 정렬 수행
        
        System.out.println(Arrays.toString(arr));
	}

    private static void mergeSort(int[] arr, int start, int end) { // 분할한 배열 중 어느 배열이라도 배열의 끝을 넘어가면 실행 X
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
        int idx = start; // 임시 배열에 값을 저장할 인덱스

        while (l <= mid && r <= end) {
            if (arr[l] <= arr[r]) {
                tmp[idx++] = arr[l++]; // 왼쪽 부분 배열의 값이 작으면 tmp 배열에 저장하고 l, idx 증가

            } else {
                tmp[idx++] = arr[r++]; // 오른쪽 부분 배열의 값이 작으면 tmp 배열에 저장하고 r, idx 증가
            }
        }

        while (l <= mid) {
            tmp[idx++] = arr[l++]; // 왼쪽 부분 배열이 남은 경우 tmp 배열에 복사
        }

        while (r <= end) {
            tmp[idx++] = arr[r++]; // 오른쪽 부분 배열이 남은 경우 tmp 배열에 복사
        }

        for (int i = start; i <= end; i++) {
            arr[i] = tmp[i]; // 임시 배열의 값을 원래 배열에 복사
        }
    }
}
```

![03-merge-sort-flow-chart](/assets/img/posts/study/algorithm/merge-sort/03-merge-sort-flow-chart.jpg){: target="_blank" }
			
## 참고 자료

- [혀니C코딩, "[📶sort 5-1]병합정렬(merge sort)이 뭐예요??? \| 분할 정복 알고리즘 \| O(NlogN)", 2022-08-27](https://www.youtube.com/watch?v=y0ToATXjYHY){: target="_blank" }
