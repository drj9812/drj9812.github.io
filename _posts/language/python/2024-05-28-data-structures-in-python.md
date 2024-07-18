---
title: "[Python]자료구조"
categories: [Language, Python]
tags: [Python, 파이썬, Data Structures, 자료구조]
image:
  path: /assets/img/posts/language/python/01-python-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Python
---

# 자료구조

## 리스트(List)

- 여러 개의 값들을 하나의 변수에 저장하기 위한 데이터 타입
- elemnet(원소, 요소): 리스트에 저장되는 각각의 값
- index(인덱스): 리스트에서 원소가 저장된 위치

### 인덱싱(Indexing)

![01-indexing](/assets/img/posts/language/python/data-structures-in-python/01-indexing.jpg)

- 인덱스를 사용해서 리스트의 원소를 참조하는 방법

```python
alphabet = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I']

print(numbers[0]) # 'A'
print(numbers[8]) # 'I'
print(numbers[-1]) # 'I'(리스트에서 가장 마지막 원소)
print(numbers[-2]) # 'H'
```

### 슬라이싱(Slicing)

```python
list[start:end]
```

- 인덱스를 사용해서 리스트의 부분집합(리스트)를 잘라내는 방법
- start ≤ index < end 범위의 인덱스 위치의 원소들을 잘라냄
- start를 생략한 경우에는 첫 번째 원소부터 잘라냄
  + `list[0:9]` 와 `list[:9]`는 같은 결과
- end를 생략한 경우에는 마지막 원소까지 잘라냄
  + `list[0:9]` 와 `list[0:]`는 같은 결과
- **리스트를 슬라이싱한 결과는 또다른 리스트**
  + 인덱싱은 값을 반환

#### 예시

```python
numbers = [1, 2, 3, 10, 20, 30]

print(numbers[:3]) # [1, 2, 3]
print(numbers[3:]) # [10, 20, 30]
```  

### 리스트의 산술 연산

```python
print([1, 2, 3] + [4, 5, 6]) # [1, 2, 3, 4, 5, 6]
```

- 리스트 + 리스트

```python
print([1, 2, 3] * 3) # [1, 2, 3, 1, 2, 3, 1, 2, 3]
```

- 리스트 * 정수 또는 정수 * 리스트

### 리스트 내포(List comperhension)

```python
new_list = [expression for item in iterable if condition]

# filtering
filtering_list = [value for x in iterable if condition]

# mapping
mapping_list = [value if condition else value2 for x in iterable]
```

- 간결하게 리스트를 생성하고 변형하는 방법
- 기존의 리스트, 튜플, 셋 등의 반복 가능한(iterable) 객체를 이용하여 새로운 리스트를 생성하는 구문

#### 예시

```python
numbers = [random.randrange(10, 100) for _ in range(10)]
evens = [x for x in numbers if x % 2 == 0]
odds = [x for x in numbers if x % 2]

print(numbers) # [38, 55, 86, 61, 31, 12, 45, 93, 51, 95]
print(evens) # [38, 86, 12]
print(odds) # [55, 61, 31, 45, 93, 51, 95]
```

- filtering

```python
gender_codes = [random.randrange(2) for _ in range(10)]
genders = ['Male' if x == 0 else 'Female' for x in gender_codes]

print(gender_codes) # [1, 0, 0, 0, 1, 1, 1, 0, 0, 1]
print(genders) # # ['Female', 'Male', 'Male', 'Male', 'Female', 'Female', 'Female', 'Male', 'Male', 'Female']
```

- mapping

```sql
gender_codes = [1, 2, 1, 2, 3]

genders = ['M' if x  == 1 else ('F' if x == 2 else 'Unknown') for x in gender_codes]
print(genders) # ['M', 'F', 'M', 'F', 'Unknown']
```

- 삼항연산자를 사용한 mapping 

```python
numbers = [1, 2, 3, 4, 5]

even_squares = [x**2 for x in numbers if x % 2 == 0]
print(even_squares)  # [4, 16]
```

- filtering + mapping

## 문자열(str)

```python
message = '안녕하세요, 여러분!' # 11개의 문자들로 이루어진 리스트

# 문자열에서 첫 번째 문자
print(message[0]) # '안'

# 문자열에서 마지막 문자
print(message[-1]) # '!'

 # 문자열에서 앞에서 세 글자
print(message[:3]) # '안녕하'

# 문자열에서 뒤에서 세 글자
print(message[-3:]) # '!러분'
```

- 문자들의 리스트
- 인덱스 기반 자료 타입
    - 인덱싱, 슬라이싱 사용 가능
    - 음수 인덱스 사용 가능

---

## 튜플(Tuple)

- 하나의 변수에 여러 개의 값들을 저장하기 위한 자료 타입
- 저장된 원소(아이템)들을 **변경(추가/삭제)할 수 없는 리스트**
    - `append()`, `remove()`와 같은 원소를 변경하는 메서드는 제공하지 않음
    - 비어있는 배열을 생성한 뒤 값을 넣는 **comperhension를 할 수 없음**
        - 튜플은 변경할 수 없기에 comperhension할 수 없음
- 인덱스 기반 자료 타입
    - 인덱싱, 슬라이싱 사용 가능
    - 음수 인덱스 사용 가능

### 숫자들을 저장하는 튜플

```python
numbers = (1, 2, 10, 20, 100, 200)
print(numbers) # (1, 2, 10, 20, 100, 200)
print(type(numbers)) # <class 'tuple'>
```

### 튜플의 메서드

- `count()`
    - 튜플에서 특정 요소가 등장하는 횟수를 반환
- `index()`
    - 튜플에서 특정 요소의 첫 번째 인덱스를 반환

### 예시

```python
numbers = (1, 2, 10, 20, 100, 200)

# indexing
print(numbers[0]) # 1
print(numbers[-1]) # 200

# slicing
print(numbers[:3] # (1, 2, 10)
print(numbers[-3:] # (20, 100, 200)

```

### decomposition(분해)

```python
x, y, z = (1, 3, 5)
print(x, y, z) # 1 3 5
```

- 각 요소를 개별 변수에 할당하는 과정
- 반복 가능한 객체 내의 값들 편리하게 추출하고 활용할 수 있음
- **리스트에서도 사용 가능**

### 2차원 리스트/튜플

```python
numbers = [
    [1, 2, 3],
    [4, 5, 6]
]

print(numbers) # [[1, 2, 3], [4, 5, 6]]
print(numbers[0]) # [1, 2, 3]
print(numbers[0][0]) # 1
```

- 1차원 리스트/튜플을 원소로 갖는 리스트/튜플

```python
import random

array1 = []
for _ in range(2):
    row = []
    for _ in range(2):
        row.append(random.randrange(10))
    array1.append(row)

print(array1) # [[4, 4], [4, 9]]

array2 = []
for _ in range(2):
    row = []
    for _ in range(2):
        row.append(random.randrange(10))
    array2.append(row)

print(array2) # [[8, 6], [4, 8]]

add = []
for i in range(2):
    temp = []
    for j in range(2):
        temp.append(array1[i][j] + array2[i][j])
    add.append(temp)

print(add) # [[12, 10], [8, 17]]

add = []
for row1, row2 in zip(array1, array2):
    temp = []
    for x, y in zip(row1, row2):
        temp.append(x + y)
    add.append(temp)

print(add) # [[12, 10], [8, 17]]
```

- 2차원 리스트 생성

```python
import random

# 2 x 2 모양의 [0, 10) 범위의 정수 난수들을 저장하는 2차원 리스트
array1 = [[random.randrange(10) for _ in range(2)] for _ in range(2)]
print(array1) # [[7, 4], [9, 6]]

# 2 x 2 모양의 [0, 10) 범위의 정수 난수들을 저장하는 2차원 리스트
array2 = [[random.randrange(10) for _ in range(2)] for _ in range(2)]
print(array2) # [[9, 4], [8, 1]]

# array1과 array2의 같은 인덱스의 원소들끼리의 덧셈 결과를 저장하는 2차원 리스트
add = [[array1[i][j] + array2[i][j] for j in range(2)] for i in range(2)]
print(add) # [[16, 8], [17, 7]]

# array1과 array2의 같은 인덱스의 원소들끼리의 덧셈 결과를 저장하는 2차원 리스트
add = [[x + y for x, y in zip(row1, row2)] for row1, row2 in zip(array1, array2)]

print(add) # [[16, 8], [17, 7]]
```

- comperhension을 사용한 2차원 리스트 생성

## 셋(Set)

```python
s = { 1, 2, 2, 3, 3 }
print(s) # {1, 2, 3}
```

- 중복된 데이터를 허용하지 않음
- 인덱스 기반의 자료구조가 아님(인덱스가 없음)
    + 데이터의 저장 순서가 중요하지 않음
        * `{ 1, 2, 3 } = { 3, 1, 2 }`
        * **indexing, slicing 기능을 제공하지 않음**
        * **for-in 구문에서 사용할 수 있는 iterable 타입**

### 셋의 메서드

```python
s = { 1, 2, 2, 3, 3 }
s.remove(3)
print(s) # {1, 2}
```

- `remove()`
    + 삭제할 원소를 아규먼트로 전달하여 삭제

```python
s = { 1, 2, 2, 3, 3 }
element = s.pop()
print(element) # 1
print(s) # {2, 3}
```

- `pop()`
    + **set에서 임의의 요소를 제거하고 해당 요소를 반환하는 메서드**

```python
s1 = { 1, 2, 3, 4 }
s2 = { 2, 3, 5, 6 }

print(s1.union(s2)) # {1, 2, 3, 4, 5, 6}
```

- `union()`
    + 합집합

```python
s1 = { 1, 2, 3, 4 }
s2 = { 2, 3, 5, 6 }

print(s1.intersection(s2)) # {2, 3}
```

- `intersection()`
    + 교집합

```python
s1 = { 1, 2, 3, 4 }
s2 = { 2, 3, 5, 6 }

# s1 - s2
print(s1.difference(s2)) # {1, 4}

# s2 - s1
print(s1.difference(s2)) # {5, 6}
```

- `difference()`
    + 차집합

## dict

```python
students = { 1: '홍길동', 2: '허균', 3: '이순신' }
print(type(students)) # <class 'dict'>
print(students) # {1: '홍길동', 2: '허균', 3: '이순신'}
```

- dictionary(사전) 형식의 데이터 타입
- 키(key)를 기반으로 값(value)를 저장하는 데이터 타입
    + `list`, `tuple`: 인덱스 기반의 데이터 타입
    + `dict`: key-value 기반의 타입
    + dict에서 key의 역할은 1개 아이템의 값(value)를 찾기 위한 용도
    + `key`
        * 중복 X
    + `value`
        * 중복 O

```python
students = { 1: '홍길동', 2: '허균', 3: '이순신' }
print(students[1]) # 홍길동

students[10] = '홍길동'
print(students) # {1: '홍길동', 2: '허균', 3: '이순신', 10: '홍길동'}

students[3] = 'scott'
print(students) # {1: '홍길동', 2: '허균', 3: 'scott', 10: '홍길동'}
```

- **dict에서 키를 사용하는 방법은 list 또는 tuple과 마찬가지로 인덱스 연산자(`[]`)를 사용함**

### dict의 메서드

```python
students = {1: '홍길동', 2: '허균', 3: 'scott', 10: '홍길동'}
students.pop(3) # dict에서 해당하는 key의 아이템(key: value)을 삭제
print(students) # {1: '홍길동', 2: '허균', 10: '홍길동'}
```

- `pop()`
    + dict에서 해당하는 `key`의 아이템(key: value)을 삭제

```python
book = {
    'title': '점프 투 파이썬',
    'authors': ['박응용', '웨스 매키니'],
    'isbn': 123456789
}
print(book) # {'title': '점프 투 파이썬', 'authors': ['박응용', '웨스 매키니'], 'isbn': 123456789}
print(book['title']) # 점프 투 파이썬
print(book['authors']) # ['박응용', '웨스 매키니']
print(book['authors'][0]) # 박응용
```

- `keys()`
    + dict의 key들의 리스트를 리턴
- `values()`
    + dict의 value들의 리스트를 리턴
- `items()`
    + dict에서 (key, value) 튜플들의 리스트를 리턴
- value는 다양한 타입으로 저장 가능

### for-in

```python
contact = {
    'no': 1,
    'name': '오쌤',
    'phones': ['010-0000-0000', '02-0000-0000'],
    'emails': {
        'company': 'jake@itwill.co.kr',
        'personal': 'jake@naver.com'
    }
}

for k in contact:
		print(k)

for k in contact:
		print(k, ':', contact[k])

print(contact.keys())
print()

print(contact.values())
print()

print(contact.items())
print()

for k, v in contact.items():
    print(k, v)
```

![02-result-dict-for-in](/assets/img/posts/language/python/data-structures-in-python/02-result-dict-for-in.jpg)
*출력 결과*

- for-in 구문에서 dict를 사용하면 key를 iteration함

### Dictonary Comprehension

```python
emp_no = [1001, 1002, 2001, 2002]
emp_name = ['King', 'Scott', 'Allen', 'Tiger']

emp = {}

for no, name in zip(emp_no, emp_name):
		emp[no] = name

print(emp) # {1001: 'King', 1002: 'Scott', 2001: 'Allen', 2002: 'Tiger'}

# Dictonary Comprehension
emp = {no: name for no, name in zip(emp_no, emp_name)}
print(emp) # {1001: 'King', 1002: 'Scott', 2001: 'Allen', 2002: 'Tiger'}
```

```python
strings = {'hello', 'java', 'sql', 'python', 'javascript'}

# 리스트 strings의 문자열을 key로 하고, 그 문자열의 길이를 값으로 하는 dict
string_lengths = dict()
for s in strings:
    string_lengths[s] = len(s)

print(string_lengths) # {'hello': 5, 'java': 4, 'javascript': 10, 'python': 6, 'sql': 3}

# 리스트 strings의 문자열을 key로 하고, 그 문자열의 길이를 값으로 하는 dict
dict = {
    str: len(str) for str in strings
}
print(dict) # {'hello': 5, 'java': 4, 'javascript': 10, 'python': 6, 'sql': 3}
```