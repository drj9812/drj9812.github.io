---
title: "[Java]자료 구조"
categories: [Study, Java]
tags: [Java, 자바, Data structures, 자료 구조, Iterable, Collection, List, ArrayList, LinkedList, Vetor, Stack, Queue,  Deque, Set, TreeSet, HashSet, Map, HashMa, LinkedHashMap, TreeMap]
---

# 자료 구조(Data Structures)

## 자료 구조(Data Structures)란?

- 효율적으로 접근하고 수정할 수 있도록 데이터를 구성하고 저장하는 법
- 각 원소들이 논리적으로 정의된 규칙에 의해 나열되며, 자료에 대한 처리를 효율적으로 수행할 수 있도록 자료를 구분하여 표현한 것

## Collection

### List

- 한 가지 타입의 데이터를 여러 개 저장할 수 있는 데이터 구조체
- 데이터를 저장하는 순서가 중요
	+ 인덱스를 가지고 있으며 인덱스 순서대로 저장
	+ 데이터를 저장할 때마다 인덱스가 자동으로 증가
		* 크기가 동적으로 조절될 수 있는 **가변 길이의 배열**
	+ 중간에 있는 데이터를 삭제하면 인덱스가 자동으로 재배열됨
- 중복 데이터 허용
- Java의 기본 타입을 원소 타입으로 사용할 수 없음
	+ Wrapper 클래스 사용
- 직접적으로 인스턴스화할 수 없으므로, **`List` 인터페이스를 구현한 구체적인 클래스를 사용**해야 함
	+ `ArrayList`
	+ `LinkedList`
	+ `Stack`
	+ `Vector`

#### List 주요 메서드

- `add(E e)`
	+ 리스트의 끝에 요소를 추가
- `add(int index, E element)`
	+ 지정된 위치에 요소를 삽입
- `get(int index)`
	+ 지정된 위치에 있는 요소를 반환
- `remove(int index)`
	+ 지정된 위치에 있는 요소를 제거하고 제거된 요소를 반환
	+ remove(Object o)
		* 리스트에서 처음 등장하는 원소를 찾아서 삭제하고 논리값 반환
- `size()`
	+ 리스트의 크기를 반환
- `isEmpty()`
	+ 리스트가 비어 있는지 확인
- `clear()`
	+ 리스트의 모든 요소를 제거

#### ArrayList

```java
import java.util.ArrayList;
import java.util.Iterator;

public class ListMain01 {
    public static void main(String[] args) {

	// 기본 타입 대신에 Wrapper 클래스를 사용해야 함
	ArrayList<String> languages = new ArrayList<>(); // ArrayList<String> languages = new ArrayList<String>();
		
	System.out.println("리스트 크기: " +languages.size()); // 리스트 크기: 0
	System.out.println(languages); // []
		
	//n리스트에 원소 추가
	languages.add("Java");
	System.out.println("리스트 크기: " + languages.size()); // 리스트 크기: 1
	System.out.println(languages); // [Java]
		
	languages.add("SQL");
	System.out.println("리스트 크기: " + languages.size()); // 리스트 크기: 2
	System.out.println(languages); // [Java, SQL]
		
	languages.add("HTML");
	System.out.println("리스트 크기: " + languages.size()); // 리스트 크기: 3
	System.out.println(languages); // [Java, SQL, HTML]
		
	languages.add("Java");
	System.out.println("리스트 크기: " + languages.size()); // 리스트 크기: 4
	System.out.println(languages); // [Java, SQL, HTML, Java]
		
	// 리스트에서 인덱스 위치의 원소를 리턴
	System.out.println(languages.get(0)); // Java
		
	for (int i = 0; i < languages.size(); i++) {
	    System.out.print(languages.get(i) + " "); // Java SQL HTML Java 
	}

	System.out.println();
		
	//리스트의 원소를 삭제하는 방법
	//remove(Object o) : 리스트에서 처음 등장하는 원소를 찾아서 삭제 -> return : boolean
	//remove(int index) : 리스트에서 인덱스 위치에 있는 원소를 삭제 -> return : String
	languages.remove("Java"); // 두 개의 "Java" 중 인덱스 번호가 빠른 "Java"가 삭제됨
	System.out.println(languages); // [SQL, HTML, Java]
		
	languages.remove(0); // 인덱스 0번째 값인 "SQL" 삭제
	System.out.println(languages); // [HTML, Java]
	System.out.println(languages.remove(0)); // HTML
		
	// 향상된 for 구문(for-each): for (변수 선언 : 리스트 이름) { // ... }
	for (String s : languages) {
	    System.out.print(s + " "); // Java
	}
		
	System.out.println();
		
	// Iterator<E> 객체를 사용한 리스트 원소들 반복(Iterator = interface)
	Iterator<String> itr = languages.iterator(); // Iterator 객체 생성
		
	//for-each 구문의 수동버전
	while (itr.hasNext()) {
	    System.out.println(itr.next()); // Java
	}
    }
}
```

- 배열(Array)를 사용하는 `List`
- 데이터의 삽입/삭제(`add()`/`remove()`) 연산이 느림
	+ 데이터가 추가될 때마다 더 큰 배열을 생성한 뒤 생성된 배열에 값들이 추가되기 때문
	+ 데이터가 삭제될 때마다 삭제된 인덱스의 위치에 인덱스 + 1의 값들이 당겨져 오기 때문
- 데이터의 접근(`get()`)이 느림
	+ 검색이 빈번할 때 `ArrayList` 사용

> 생성자 호출에서는 리스트의 원소 타입을 생략할 수 있다.
{: .prompt-tip }

#### LinkedList

- `LinkedList` 알고리즘을 사용한 리스트
- 데이터의 삽입/삭제(`add()`/`remove()`) 연산이 용이
	+ 데이터가 추가되면 저장할 공간을 만든 뒤 생성된 공간의 주소가 추가되기 전의 데이터와 연길되기 때문
	+ 데이터가 삭제되면 값을 삭제하는 게 아니라 연결된 주소를 끊고 삭제된 값의 다음 값이 주소와 연결되기 때문
- 데이터의 접근(`get()`)이 느림
	+ 추가나 삭제가 빈번할 때 LinkedList 사용

#### Stack

#### Vector

### Queue

#### Deque

##### ArrayDeque

#### PrirorityQueue

### Set

- 중복된 데이터는 저장하지 않음
- 인덱스가 없음

#### TreeSet

- Tree 알고리즘을 사용한 `Set`
	+ 정렬이 빠름

```java
import java.util.Iterator;
import java.util.TreeSet;

public class SetMain01 {
    public static void main(String[] args) {
    // String을 원소로 갖는 TreeSet 변수 선언, 객체 생성.
    TreeSet<String> set = new TreeSet<>();
		
    System.out.println("set size: " + set.size()); // set size: 0
    System.out.println(set); // []
		
    set.add("hello");
    set.add("apple"); // TreeSet은 정렬된 형태.
    set.add("hello"); // 중복된 값은 저장되지 않음.
    set.add("zip");
    set.add("banana");
		
    System.out.println(set); // [apple, banana, hello, zip[

    //Set은 인덱스를 갖지 않기 때문에 get(int index) 메서드는 제공되지 않음
    //for 문장을 사용할 수 없음. for-each 문장(배열, list, set)과 Iterator는 사용 가능
		
    for (String x : set) {
        System.out.print(x + " "); // apple banana hello zip
    }

    System.out.println();

    // iterator() : 오름차순, descendingIterator() : 내림차순
    Iterator<String> itr = set.iterator(); 

    while (itr.hasNext()) {
        System.out.print(itr.next()+ " "); // apple banana hello zip
    }

    System.out.println();
		
    // TreeSet은 정렬 알고리즘이 적용된 객체이기 때문에 내림차순 반복 도구도 제공.
    // HashSet은 descendingIterator() 메서드를 제공하지 않음
    Iterator<String> itr2 = set.descendingIterator(); 
    while (itr2.hasNext()) {
        System.out.print(itr2.next() + " "); // zip hello banana apple
    }

    System.out.println();
    }
}
```

#### HashSet

- Hash 알고리즘을 사용한 `Set`
	+ 검색이 빠름

```java
import java.util.HashSet;
import java.util.Iterator;

public class SetMain02 {
    public static void main(String[] args) {

    // Integer를 원소로 갖는 HashSet 변수를 선언, 객체 생성.
    HashSet<Integer> set = new HashSet<>();
		
    set.add(1);
    set.add(100);
    set.add(52);
    set.add(123);
    set.add(456);
    set.add(134);
    set.add(3);
    set.add(15);
    set.add(16756);
    set.add(5);

    System.out.println(set); // 1, 3, 100 ... (무작위)
    // TreeSet을 사용했다면 정렬돼서 출력
		
    // for-each
    for (Integer x : set) {
        System.out.print(x + " ");
    }

    System.out.println(); // 1 3 100 52 16756 5 134 456 123 15 
		
    // Iterator
    Iterator<Integer> itr = set.iterator(); // TreeSet과 다르게 descendingIterator() 메서드가 없음
		
    while (itr.hasNext()) {
        System.out.print(itr.next() + " "); // 1 3 100 52 16756 5 134 456 123 15 
    }

    System.out.println();
    }
}
```

```java
import java.util.HashSet;

public class SetMain03 {
    public static void main(String[] args) {

    // set<E>은 중복된 값을 저장하지 않음
    HashSet<String> set = new HashSet<>();
    set.add("hello");
    set.add("olleh");
    set.add("hello");
		
    System.out.println(set); // [hello, olleh]
		
    / /Score 타입을 원소로 갖는 HashSet 변수 선언, 초기화
    HashSet<Score> scores = new HashSet<>();
		
    Score score1 = new Score(100, 100, 100);
    System.out.println(score1); // Scroe(java=100, sql=100, web=100)
    scores.add(score1); // Score 객체를 Set에 저장.
		
    Score score2 = new Score(100, 90, 80);
    System.out.println("score1 score2 eq? " + score1.equals(score2)); // score1 score2 eq? false
    scores.add(score2);
		
    Score score3 = new Score(100, 100, 100);
    System.out.println("score1 score3 eq? " + score1.equals(score3)); // 출력 : true
    scores.add(score3);
    // (1) equals만 override를 하고 hashCode를 override하지 않으면 score3이 Set에 추가가 됨
    // (2) euqlas와 hashCode를 모두 적절하게 override하면 score3은 Set에 추가되지 않음
		
    //equals()가 true이면 hashCode()의 리턴 값이 같다.
    //hashCode()의 리턴 값이 다르면 equals()가 false이다.
    //Set은 두 객체의 중복(같은지 다른지) 여부를 검사할 때 hashCode()를 먼저 비교하고,
    //hashCode()가 같으면 equals() 메서드를 호출해서 중복 여부를 검사함
    //object의 hash코드는 주소값을 참조하기 때문에 hash코드를 override 하지 않으면 실제 두 객체가 같을지라도 다르다고 판단해버리는 거임
		
    //equals가 같다 > hash코드가 간다
    //hash코드가 다르다 > equals가 다르다
    //(물어보기 ) hash코드가 같다 -> equals가 같다가 성립 x
    //j = 100, s = 100, w = 100 와 j = 300, s = 0, w = 0과 같은 사례 때문?
    System.out.println(scores); // [Scroe(java=100, sql=100, web=100), Scroe(java=100, sql=90, web=80)]
    }
}
```

##### LinkedHashSet

## Map

- 키(Key)와 값(Value)의 쌍으로 구성된 데이터를 저장하는 데이터 타입
- 키는 중복된 값을 가질 수 없고 연속된 값을 가질 필요가 없음
	+ 리스트에서의 인덱스 역할
- 값은 중복 데이터 허용

### HashMap

- 검색이 빠름

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.Set;


public class MapMain01 {
    public static void main(String[] args) {

	// 정수를 key로 하고, 문자열을 Value로 하는 HashpMap 변수 선언, 초기화(객체 생성)
	HashMap<Integer, String> students = new HashMap<>(); // HashMap<Integer, String> hash = new HashMap<Integer, String>();

	// put(key, value) : Map에 key-value 쌍의 데이터를 저장.
	students.put(1, "홍길동");
	students.put(2, "이순신");
	students.put(3, "강감찬");
	students.put(20, "장보고");
	students.put(10, "김유신");
	System.out.println("map size = " + students.size()); // 5
	System.out.println(students); // {1=홍길동, 2=이순신, 3=강감찬, 20=장보고, 10=김유신}
		
	// get(key): key에 해당하는 value를 리턴. key에 매핑된 value가 없으면 null을 리턴
	System.out.println(students.get(3)); // 강감찬
	System.out.println(students.get(5)); // null
		
	// getOrDefault(key, defaultValue) : 
	// key에 매핑된 value를 리턴. key에 매핑된 value가 없으면 defaultValue를 리턴
	// -> 절대로 null이 나올 수 없게끔

	System.out.println(students.getOrDefault(3, "무명씨")); // 강감찬
	System.out.println(students.getOrDefault(5, "무명씨")); // 무명씨
		
	// remove(key) : key에 매핑된 key-value 원소를 삭제.
	students.remove(20); //삭제한 key의 값을 리턴, key가 존재하지 않으면 null 리턴
	System.out.println("map size: " + students.size()); // map size: 4
	System.out.println(students); // {1=홍길동, 2=이순신, 3=강감찬, 10=김유신}
		
	// put(key, value) :
	// (1) Map에 key가 존재하지 않으면 key-value 데이터를 저장.
	// (2) Map에 key가 존재하면, key에 매핑된 값을 변경 (값만 !! key는 x)
	students.put(2, "Yi Sun-shin");
	System.out.println(students); // {1=홍길동, 2=Yi Sun-shin, 3=강감찬, 10=김유신}
		
	// Map에서 향상된 for 문장을 사용하는 방법 :
	Set<Integer> keySet = students.keySet(); // (1)Map의 key들로만 이루어진 Set을 만듦
	// 모든 맵이 공통적으로 가지고 있는 메서드(왜냐면 모든 맵들은 map을 상속받기 때문에)
	// ->Map에서 key만 빼면 set임 (중복값 존재 x) 
	// keySet()은 정렬되서 나옴
	System.out.println(keySet); // [1, 2, 3, 10]

	for (Integer k : keySet) { // (2)key들을 처음부터 끝까지 순회하면서
	    // (3)key에 매핑된 value를 찾음. 
	    System.out.print(k + ": "  + students.get(k) + " "); // 1: 홍길동 2: Yi Sun-shin 3: 강감찬 10: 김유신 
	}
    }
}
```

#### LinkedHashMap

### TreeMap

- 정렬이 빠름

```java
import java.util.Set;
import java.util.TreeMap;

public class MapMain02 {
    public static void main(String[] args) {
	//문자열을 key로 하고 정수를 value로 하는 TreeMap을 선언, 초기화(객체 생성).
	TreeMap<String, Integer> menu = new TreeMap<>();
		
	menu.put("짜장면", 8000);
	System.out.println(menu); // {짜장면=8000}
		
	menu.put("볶음밥", 9000);
		
	menu.put("짬뽕", 9000);
	System.out.println(menu); // {볶음밥=9000, 짜장면=8000, 짬뽕=9000}
		
	System.out.println(menu.get("짜장면")); // 8000
	System.out.println(menu.get("냉면")); // null
	System.out.println(menu.getOrDefault("냉면", 0)); // 0
		
	// TreeMap은 정렬 알고리즘이 적용된 Map이기 때문에, 오름차순/내림차순 키 집합을 제공.
	Set<String> keySet = menu.keySet();
		
	for (String k : keySet) {
	    System.out.print(k + ": " + menu.get(k) + " "); // 볶음밥 : 9000 짜장면 : 8000 짬뽕 : 9000
	}
		
	Set<String> descKeySet = menu.descendingKeySet();
		
	for (String k : descKeySet) {
	    System.out.print(k + " - " + menu.get(k) + " "); // 짬뽕 - 9000 짜장면 - 8000 볶음밥 - 9000
	}
    }
}
```

```java
import java.util.HashMap;
import java.util.TreeMap;
import java.util.PrimitiveIterator.OfDouble;

public class MapMain03 {
    public static void main(String[] args) {
    // 단어 개수 세기(word counting):
    // 문자열에 등장하는 단어를 key로 하고, 그 단어의 등장 횟수를 value로 하는 Map을 만들고 출력
    String sentence = "하늘 땅 하늘 땅 하늘 sky sky";
    String[] sArr = sentence.split(" ");

    int haneul = 0;
    int ddang = 0;
    int sky = 0;

    TreeMap<String, Integer> wordcloud = new TreeMap<>();

    for (int i = 0; i < sArr.length; i++) {
        switch (sArr[i]) {
	    case "하늘":
                haneul++;
                wordcloud.put("하늘", haneul);
                break;
            case "땅":
                ddang++;
                wordcloud.put("땅", ddang);
                break;
            case "sky":
                sky++;
                wordcloud.put("sky", sky);
                break;
        }
    }

    System.out.println(wordcloud); // {sky=2, 땅=2, 하늘=3}

    String sentence1 = "하늘 땅 하늘 땅 하늘 sky sky";
    String[] sArr1 = sentence1.split(" ");
		
    HashMap<String, Integer> wordcloud1 = new HashMap<>();

    for (String x : sArr1) {
        Integer count = wordcloud1.get(x);

	if (count == null) {
	    wordcloud1.put(x , 1);
	} else {
	    wordcloud1.put(x, count + 1);
	}
    }
    System.out.println(wordcloud1); // {sky=2, 땅=2, 하늘=3}
		
    String sentence2 = "하늘 땅 하늘 땅 하늘 sky sky";
    String[] sArr2 = sentence1.split(" ");
		
    HashMap<String, Integer> wordcloud2 = new HashMap<>();
		
    for (String x : sArr2) {
        Integer count = wordcloud2.getOrDefault(x , 0);
	wordcloud2.put(x, count + 1);
    }

    System.out.println(wordcloud); // {sky=2, 땅=2, 하늘=3}

    }
}
```
## 참고 자료

- [어라운드 허브 스튜디오 - Around Hub Studio, "Java로 배우는 자료구조", 2021-12-24](https://www.youtube.com/playlist?list=PLlTylS8uB2fCoXCVtfJ0wzotOU8a3ti6M){: target="_blank" }