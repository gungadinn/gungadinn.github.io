---
layout: post
title: "python (7) - 자습서 돌리기"
category: python
tags: [python]
comments: true
---

------

[파이썬 공식 문서 자습서](https://docs.python.org/ko/3/tutorial/index.html) 한 바퀴 돌리면서 헷갈렸던 것들 + 몰랐던 것들을 정리했다. 

### 목차

* 숫자형
* 문자형
* 람다 표현식
* 리스트
* 튜플
* 표준 라이브러리



##  숫자형

---

* 나눗셈(`/`)은 항상 float를 돌려준다.
* 정수 결과를 얻으려면 `//` 을 사용하면 된다.
* 거듭제곱은  `**` 연산자 사용

<br>

##  문자형

---

* `\`이 특수 문자로 취급되게 하고 싶지 않으면, 첫 따음표 앞에  r을 붙인다.

  ex) `print(r'C:\some\name')`

* 문자열은 `+` 연산자로 이어붙이고,  `*` 연산자로 반복 가능하다.

<br>

## 람다 표현식 (람다 형식)

__lambda__ 이름 없는 함수를 만드는데 사용한다. 이 함수는 두 인자의 함을 돌려준다 : `lambda a,b: a+b` . 함수 객체가 있어야 하는 곳이면 어디든 람다 함수가 사용될 수 있고 문법적으로는 하나의 표현식으로 제한되지만, 의미적으로는 일반적인 함수 정의의 편의 문법이다. 표현식 `lambda parameters: expression` 은 함수 객체를 준다. 이 이름 없는 객체는 이렇게 정의된 함수 객체처럼 동작한다.

```python
def <lambda>(parameterers):
    return expression
```

<br>

## 리스트

---

* 슬라이스 연산(`:`)은 요청한 항목들을 포함하는 새 리스트를 돌려준다. 즉, 리스트의 새로운 (얕은) 복사본을 돌려준다.

  ex) 슬라이스 표기법의 장점

  ```python
  words = ['cat', 'window', 'defenestrate']
  
  for w in words[:]:
      if len(w)>6:
          words.insert(0, w)
          
  words # output : ['defenestrate', 'cat', 'window', 'defenestrate']
  ```

  `for w in words` 라고 쓰면,  `defenestrate` 를 반복해서 넣고 또 넣음으로써, 무한한 리스트를 만든다.

* range()와 len()의 결합

```python
>>> a = ['mary', 'had', 'a', 'little', 'lamb']
>>> for i in range(len(a)):
>>>     print(i, a[i])

0 mary
1 had
2 a
3 little
4 lamb
```

* enumerate()

```python
>>> a = ['mary', 'had', 'a', 'little', 'lamb']
>>> for i in enumerate(a):
>>>     print(i)

(0, 'mary')
(1, 'had')
(2, 'a')
(3, 'little')
(4, 'lamb')
```

* 리스트를 큐로 사용하기

 ```python
>>> from collections import deque
>>> queue = deque(["아라곤", "레골라스", "김리"])
>>> queue.append(["프로도"])
>>> queue.append(["샘"])

>>> queue.popleft()
'아라곤'

>>> queue.popleft()
'레골라스'

>>> queue.pop()
'샘'

>>> queue
deque(['김리', '샘'])
 ```

* del문

```python
>>> a = [-1, 1, 66.25, 333, 333, 1234.5]
>>> del a[0]
>>> a
[1, 66.25, 333, 333, 1234.5]
>>> del a[2:4]
>>> a
[1, 66.25, 1234.5]
>>> del a[:]   # 전체 리스트 비우기
>>> a
[]

>>> a = [-1, 1, 66.25, 333, 333, 1234.5]
>>> del a   # 객체 자체를 삭제
NameError : name is 'a' is not defined
```

<br>

## 튜플

---

```python
>>> t = 12345, 54321, 'hello!'
>>> t[0]
12345
>>> t
(12345, 54321, 'hello!')

>>> u = t, (1, 2, 3, 4, 5)    # 튜플은 중첩될 수 있다
>>> u
((12345, 54321, 'hello!'), (1, 2, 3, 4, 5))

>>> t[0] = 88888   # 튜플은 불변이다
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'tuple' object does not support item assignment
  
>>> # 그렇지만 가변 객체들을 포함할 수 있음
... v = ([1, 2, 3], [3, 2, 1])

>>> v
([1, 2, 3], [3, 2, 1])
```



```python
>>> empty = ()
>>> singleton = 'hello',    # <--  쉼표 찍기
>>> len(empty)
0
>>> len(singleton)
1
>>> singleton
('hello',)
```

빈 튜플은 빈 괄호쌍으로 만들고, 하나의 항목만 포함한 튜플은 값 뒤에 쉼표를 붙여서 만든다.

<br>

## 표준 라이브러리

1. 운영체제 인터페이스

 ` import os` : 운영 체제와 상호 작용하기 위한 수십가지 함수 제공

2. 파일 와일드카드

`import glob` : 디렉토리 와일드카드 검색으로 파일 목록을 만드는 함수 제공

3. 명령행 인자

`import sys` 

4. 에러 출력 리디렉션과 프로그램 종료

 ```python
>>> sys.stderr.write('Warning, log file not found starting a new one\n')
Warning, log file not found starting a new one
 ```

5. 정규식
6. 수학

`import math` : 부동 소수점 연산을 위한 C 라이브러리 함수들 제공

`import random` :  무작위 선택을 할 수 있는 도구 제공

`import statistics` : 수치 데이터의 기본적인 통계적 틍성 제공

7. 날짜와 시간

`from datetime import date` : 날짜와 시간을 조작하는 클래스 제공

8. 성능 측정

```python
>>> from timeit import Timer
>>> Timer('t=a; a=b; b=t', 'a=1; b=2').timeit()
0.57535828626024577
>>> Timer('a,b = b,a', 'a=1; b=2').timeit()
0.54962537085770791
```

