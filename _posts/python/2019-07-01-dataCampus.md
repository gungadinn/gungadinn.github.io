---
layout: post
title: "python (1) - 할당"
category: python
tags: [python]
comments: true
---

---

## 빅데이터 청년인재 Day 1

---

### 목차

- 식별자와 표현식
- 네임스페이스
- 메모리
- is와 ==
- type
- overflow
- 함수 사용 예시
- 부동소수
- bool
- container
- 용어정리
- 에러 정리

------

<br>

```python
>>> import this
```

— python 철학 출력

<br>

```python
>>> import antigravity
```

— [site](https://xkcd.com/353/)로 연결됨

<br>

```python
>>> import keyword
>>> dir(keyword)

# 주피터 노트북에서는 아래처럼 나오고 인터프리터에서는 한 줄로 나옴.
['__all__',
'__builtins__',
'__cached__',
'__doc__',
'__file__',
'__loader__',
'__name__',
'__package__',
'__spec__',
'iskeyword',
'kwlist',
'main']
```

> * dir : 인자가 있으면, 현재 지역 스코프에 있는 이름들의 리스트를 돌려준다. 인자가 없으면, 현재 객체에 유효한 어트리뷰트의 리스트를 돌려주려고 시도한다. [docs](https://docs.python.org/ko/3/library/functions.html#dir)
>
> 언더바 두개 (__) 붙은거  :  [magin method](https://docs.python.org/ko/3/glossary.html#term-magic-method), [special method](https://docs.python.org/ko/3/glossary.html#term-special-method) (어떤 연산을 실행할 때 묵시적으로 호출되는 메소드)
> 
>  kw : keyword 줄임말
>  
>  kwlist : keyword 리스트 보여줌

<br>

```python
>>> keyword.kwlist

# 주피터 노트북에서는 아래처럼 나오고 인터프리터에서는 한 줄로 나옴.
['False',
'None',
'True',
'and',
'as',
'assert',
'async',
'await',
'break',
'class',
'continue',
...]
```

<br>

```python
>>> len(keyword.kwlist)
35
```

<br>

## 1) 식별자와 표현식

---

a=1+2 (왼쪽 : 식별자 (identifier) / 오른쪽 : 표현식)

식 = 하나의 결과값으로 축약할 수 있는 것

ex) 1+2는 3으로 축약할 수 있음

<br>

```python
>>> regolas = 1
>>> print(regolas)
1
```

<br>

식별자 선언 시

1. 메모리에 공간 확보
2. 그 메모리를 가르키는 공간 확보 (binding)

<br>

## 2. 네임스페이스

---

```python
>>> a = 1
>>> b = 3 if a>0 else 5
>>> x
NameError
```

> NameError : 정의되지 않은 언어를 출력할 때 에러 발생
>
> <figure>
>   <img src="https://i.imgur.com/UQghoBd.png">
> </figure>

<br>

```python
%whos
```

어느 객체가 선언되었는지 네임스페이스를 보여준다.

%whos를 입력하였을 때 없는 객체를 입력하면 에러가 발생한다. 

즉 네임 스페이스에 없는 객체를 선언할 때 NameError 발생

><figure>
>  <img src="https://i.imgur.com/yp7NPFN.png">
></figure>



<br>

```python
%who
```

><figure>
>  <img src="https://i.imgur.com/PkmrmK6.png">
></figure>

<br>

```python
%who_ls
```

><figure>
>  <img src="https://i.imgur.com/pJAEGMW.png">
></figure>

<br>

## 3. 메모리

---

```python
# 출력되는 output(주석)은 메모리 주소
>>> id(a)
443780512

>>> hex(id(a))    
'0x108deb5a0'

>>> a=1000
>>> hex(id(a))  
'0x10bd9c3f0'

>>> b=1000
>>> hex(id(b)) 
'0x10bd9c490'

# -5~256 사이에 있는 정수이면 똑같은 메모리를 가르킨다. (interning)
>>> a=10
>>> id(a)   
4443780800

>>> b=10
>>> id(b)   
4443780800
```

<br>

### 1) 재할당 

기존 객체에 다른 값으로 바꾼다. 재할당하면 기본적으로 메모리 주소 또한 바뀐다.

```python
>>> a=1000  
>>> id(a)   
4493790128

>>> a=1000
>>> id(a)   
4493789552
```

<br>

아래의 예에서는 인터닝 때문에 메모리가 안 바뀐다. (그렇지만 파이썬 3.7부터는 인터닝 기법 바뀌었음)

```python
>>> a=3
>>> id(a)   
4443780576

>>> a=3
>>> id(a)   
4443780576 
```

<br>

### 2) mutable의 메모리 주소

```python
>>> a=[2]
>>> id(a)   
4494359304

>>> a.append(3)
>>> id(a)   
4494359304
```

id는 주소 값에서 첫번째 주소를 가리킨다. 때문에 변한 것이 없어보이지만 실제로 할당량은 바뀌었다. C언어의 포인터 개념으로 생각하면 된다.

<br>

## 4. is와 ==

---

```python
>>> a=10
>>> b=10
>>> a == b  
True
>>> a is b  
True     #(is 연산자는 메모리까지 본다)

>>> a=1000
>>> b=1000
>>> a == b   
True
>>> a is b   
False    #(메모리 주소가 달라서)
```

<br>

## 5. 타입

---

```python
>>> a=1.
>>> type(a)    
float

>>> a=.1
>>> type(a)    
float
```

<br>

## 6. overflow

---

```python
>>> import sys
>>> dir(sys)
>>> sys.maxsize   
9223372036854775807

>>> 9223372036854775807+1  
9223372036854775808
```

현대 프로그래밍 언어에서 overflow가 되는 언어는 적다. python 또한 overflow는 없지만 내부적으로 처리하는 속도는 급격히 떨어진다.

<br>

## 7. 함수 사용 예시

---

```python
>>> a=-1
>>> dir(a)   # a에 사용할 수 있는 함수들 출력됨

>>> a.__abs__   
<method-wrapper '__abs__' of int object at 0x108deb560>

>>> a.__abs__()
1
```

<br>

```python
>>> -1.__abs__()
SyntaxError : invalid syntax
```

.을 float로 생각하여 에러가 출력된다.

> SyntaxError : 문법적으로 잘못 접근
>
> <figure>
>   <img src="https://i.imgur.com/yTeetnY.png">
> </figure>

<br>

```python
>>> -1 .__abs__() 
-1

>>> (-1).__abs__()    
1    #(연산자 우선순위)
```

<br>

## 8. 부동소수

---

### 1) 부동소수점의 overflow

```python
>>> sys.float_info   #부동소수점에 대한 정보

sys.float_info(max=1.7976931348623157e+308, max_exp=1024, max_10_exp=308, min=2.2250738585072014e-308, min_exp=-1021, min_10_exp=-307, dig=15, mant_dig=53, epsilon=2.220446049250313e-16, radix=2, rounds=1)
```

<br>

### 2) 무한대

1. 파이썬에는 무한대가 있고 무한대는 부동소수로 표현한다.

```python
>>> x = float('inf')
>>> x = float('infinity')

>>> t=float('nan')   #  파이썬에는 숫자가 아닌 것도 지정할 수 있다.
>>> t   
nan

>>> a = float('inf')
>>> a == a+1    
True   # (무한대+무한대는 무한대이므로 주의!)

>>> a=1.7976931348623157e+308   # 부동소수점의 max값
>>> a+1   
1.7976931348623157e+308
```

<br>

2. 부동소수는 정확하게 구할 수 없다.

```python
 # output : 
0.2

>>> 0.1+0.1+0.1   
0.300000000004 
```

<br>

3. 무한대 개념은 float 타입

4. 파이썬에서 복소수 표현은 j 사용

   ```python
   >>> a=3+2j
   >>> type(a)    
   complex    #(complex는 복소수 타입)
   
   >>> dir(a)   # a에 사용할 수 있는 함수들 출력됨
   >>> a.conjugate()   
   3-2j   #(켤레복소수)
   ```

<br>

## 9. bool

---

```python
>>> type(True)   # output : 
bool    # (bool타입은 숫자)

>>> issubclass(bool, int)   
True    #(bool 타입은 int형에서 상속되었느냐)

>>> True+True    
2

>>> True is 1   
1
```

<br>

## 10. container

---

### 1) 문자열

```python
>>> a='abc'   # 한꺼번에 a,b,c를 남는다
>>> a[0]  
'a'

>>> a[4]    
IndexError 

>>> a[1:3]   
'bc'  #(slice)

>>> a='abc'
>>> a[1:5]   
'bc'  #(slice는 범위를 벗어나도 에러가 발생하지 않는다.)

>>> a='반지의제왕'
>>> a[slice(0,2)]   
'반지'
```

> IndexError : 인덱스의 범위를 벗어날 때 발생
>
> <figure>
>   <img src="https://i.imgur.com/TW2TYbH.png">
> </figure>

<br>

### 2) 리스트

안에 들어갈 수 있는 데이터 타입이 다양(헤테로) 하고 순서가 중요(시퀀스)하다.

```python
>>> a = [1, 'a']
>>> a   
[1, 'a']

>>> range(100)[50]   
50
```

<br>

### 3) set

안에 들어갈 수 있는 데이터 타입이 다양(헤테로)하고 순서가 없다 (중복 X).

순서가 없으니 indexing, slicing 불가능하다.

```python
>>> a={3,1,2}
>>> a   
{1, 2, 3}

>>> a={3,1,2,1,1}
>>> a   
{1, 2, 3}

>>> a=frozenset({3,1,2})
>>> a   
frozenset({1, 2, 3})
```

<br>

## 용어 정리

---

* mutable(가변) : 값을 변경할 수 있는 객체들

  ex) list, set, dict 등

* immutable(불변) : 값을 변경할 수 없는 객체들

  ex) int, str, tuple, frozen set 등

* 호모지니어스 (Homogenous) : 안에 들어갈 데이터 타입이 같은 것

  ex) str 삼총사, range 등

* 헤테로지니어스 (Heterogenous) : 안에 들어갈 데이터 타입이 다른 것

  ex) set, list 등

* 시퀀스 타입 (sequence type) : 순서가 중요하다. indexing, slicing 가능

  ex) list, str, tuple, bytes, range 등

* 순서 중요 X

  ex) set, frozen set, dictionary 등

  <br>

ex) 문자열 : 안에 들어갈 데이터 타입이 같고 순서가 중요하다. (homogenous한 sequence type)

ex) 리스트 : 안에 들어갈 데이터 타입이 여러가지이고 순서가 중요하다. (heterogenous한 sequence type)

<br>

## 에러 정리

---

* NameError : 정의되지 않은 언어를 출력할 때 에러 발생

<br>