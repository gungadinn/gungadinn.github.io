---
layout: post
title: "python 중급 (1) - 할당"
date: 2019-07-01
excerpt: "빅데이터 청년인재 DAY 1"
tags: [python,빅데이터 청년인재,데이터청년캠퍼스,한국데이터진흥원]
comments: true

---

# python 중급 (1) - 할당

```python
import this
```

python 철학 출력

<br>

```python
import antigravity
```

[site](https://xkcd.com/353/)

<br>

```python
import keyword
dir(keyword)
```

> <u>output</u>
>
> ```
> ['__all__',
>  '__builtins__',
>  '__cached__',
>  '__doc__',
>  '__file__',
>  '__loader__',
>  '__name__',
>  '__package__',
>  '__spec__',
>  'iskeyword',
>  'kwlist',
>  'main']
> ```
>
> dir : [docs](https://docs.python.org/ko/3/library/functions.html#dir)
>
> 언더바 두개 (__) 붙은거  :  [magin method](https://docs.python.org/ko/3/glossary.html#term-magic-method), [special method](https://docs.python.org/ko/3/glossary.html#term-special-method)
>
> kw : keyword 줄임말
>
> kwlist : keyword 리스트 보여줌

<br>

```python
keyword.kwlist
```

> <u>output</u>
>
> ```
> ['False',
>  'None',
>  'True',
>  'and',
>  'as',
>  'assert',
>  'async',
>  'await',
>  'break',
>  'class',
>  'continue',
> ...]
> ```

<br>

```python
len(keyword.kwlist)
```

> <u>output</u> : 35

<br>

<br>

####1. 식별자와 표현식

a=1+2 (왼쪽 : 식별자 (identifier) / 오른쪽 : 표현식)

식 = 하나의 결과값으로 축약할 수 있는 것

ex) 1+2는 3으로 축약할 수 있음

<br>

```python
레골라스 = 1
print(레골라스)
```

output : 1

<br>

식별자 선언 시

1. 메모리에 공간 확보
2. 그 메모리를 가르키는 공간 확보 (binding)

<br>

<br>

#### 2. 네임스페이스

```python
a = 1
b = 3 if a>0 else 5
x
```

![image-20190702194155062](/Users/eunkyoung/Library/Application Support/typora-user-images/image-20190702194155062.png)

> NameError : 정의되지 않은 언어를 출력할 때 에러 발생

<br>

```python
%whos
```

어느 객체가 선언되었는지 네임스페이스를 보여준다.

%whos를 입력하였을 때 없는 객체를 입력하면 에러가 발생한다. 

즉 네임 스페이스에 없는 객체를 선언할 때 NameError 발생

![image-20190702194444445](/Users/eunkyoung/Library/Application Support/typora-user-images/image-20190702194444445.png)

<br>

```python
%who
```

![image-20190702194458822](/Users/eunkyoung/Library/Application Support/typora-user-images/image-20190702194458822.png)<br>

```python
%who_ls
```

![image-20190702194526560](/Users/eunkyoung/Library/Application Support/typora-user-images/image-20190702194526560.png)

<br>

<br>

#### 3. 메모리

```python
# 출력되는 output(주석)은 메모리 주소
id(a)   # output : 443780512
hex(id(a))    # output : '0x108deb5a0'

a=1000
hex(id(a))   # output : '0x10bd9c3f0'

b=1000
hex(id(b))   # output : '0x10bd9c490'

# -5~256 사이에 있는 정수이면 똑같은 메모리를 가르킨다. (interning)
a=10
id(a)   # output : 4443780800
b=10
id(b)   # output : 4443780800
```

<br>

##### 재할당 

기존 객체에 다른 값으로 바꾼다. 재할당하면 기본적으로 메모리 주소 또한 바뀐다.

```python
a=1000  
id(a)   # output : 4493790128

a=1000
id(a)   # output : 4493789552

```

<br>

아래의 예에서는 인터닝 때문에 메모리가 안 바뀐다. (그렇지만 파이썬 3.7부터는 인터닝 기법 바뀌었음)

```python
a=3
id(a)   # output : 4443780576

a=3
id(a)   # output : 4443780576 
```

<br>

##### mutable의 메모리 주소

```python
a=[2]
id(a)   # output : 4494359304

a.append(3)
id(a)   # output : 4494359304
```

id는 주소 값에서 첫번째 주소를 가리킨다. 때문에 변한 것이 없어보이지만 실제로 할당량은 바뀌었다. C언어의 포인터 개념으로 생각하면 된다.

<br>

<br>

#### 4. is와 ==

```python
a=10
b=10
a == b  # output : True
a is b  # output : True (is 연산자는 메모리까지 본다)

a=1000
b=1000
a == b   # output : True
a is b   # output : False (메모리 주소가 달라서)
```

<br>

<br>

#### 5. 타입

```python
a=1.
type(a)   # output : float

a=.1
type(a)   # output : float
```

<br>

<br>

#### 6. overflow

```python
import sys
dir(sys)
sys.maxsize   # output : 9223372036854775807

9223372036854775807+1   # output : 9223372036854775808
```

현대 프로그래밍 언어에서 overflow가 되는 언어는 적다. python 또한 overflow는 없지만 내부적으로 처리하는 속도는 급격히 떨어진다.

<br>

<br>

#### 7. 함수 사용 예시

```python
a=-1
dir(a)   # a에 사용할 수 있는 함수들 출력됨

a.__abs__   #<method-wrapper '__abs__' of int object at 0x108deb560>
a.__abs__()
```

output : 1

<br>

```python
-1.__abs__()
```

.을 float로 생각하여 에러가 출력된다.

![image-20190702195713768](/Users/eunkyoung/Library/Application Support/typora-user-images/image-20190702195713768.png)

> SyntaxError : 문법적으로 잘못 접근

<br>

```python
-1 .__abs__()    # output : -1
(-1).__abs__()   # output : 1 (연산자 우선순위)
```

<br>

<br>

#### 8. 부동소수

#####1) 부동소수점의 overflow

```python
sys.float_info   #부동소수점에 대한 정보
```

> output
>
> ```
> sys.float_info(max=1.7976931348623157e+308, max_exp=1024, max_10_exp=308, min=2.2250738585072014e-308, min_exp=-1021, min_10_exp=-307, dig=15, mant_dig=53, epsilon=2.220446049250313e-16, radix=2, rounds=1)
> ```

<br>

##### 2) 무한대

1. 이썬에는 무한대가 있고 무한대는 부동소수로 표현한다.

```python
x=float('inf')
x=float('infinity')

t=float('nan')   #  파이썬에는 숫자가 아닌 것도 지정할 수 있다.
t   # output : nan

a=float('inf')
a==a+1   # output : True (무한대+무한대는 무한대이므로 주의!)

a=1.7976931348623157e+308   # 부동소수점의 max값
a+1   # output : 1.7976931348623157e+308
```

<br>

2. 부동소수는 정확하게 구할 수 없다.

```python
0.1+0.1   # output : 0.2
0.1+0.1+0.1   # output : 0.300000000004 
```

<br>

3. 무한대 개념은 float 타입

4. 파이썬에서 복소수 표현은 j 사용

   ```python
   a=3+2j
   type(a)    # output : complex (complex는 복소수 타입)
   
   dir(a)   # a에 사용할 수 있는 함수들 출력됨
   a.conjugate()   # output : 3-2j (켤레복소수)
   ```

<br>

<br>

#### 9. bool

```python
type(True)   # output : bool (bool타입은 숫자)
issubclass(bool, int)   # output : True (bool 타입은 int형에서 상속되었느냐)
True+True   # output : 2
True is 1   # output : 1
```

<br>

<br>

#### 10. container

##### 1) 문자열

```python
a='abc'   # 한꺼번에 a,b,c를 남는다
a[0]   # output : 'a'
a[4]   # IndexError 발생
a[1:3]   # output : 'bc'  (slice)

a='abc'
a[1:5]   # output : 'bc'  (slice는 범위를 벗어나도 에러가 발생하지 않는다.)

a='반지의제왕'
a[slize(0,2)]  # output : '반지'
```

<br>

![image-20190702202847574](/Users/eunkyoung/Library/Application Support/typora-user-images/image-20190702202847574.png)

> IndexError : 인덱스의 범위를 벗어날 때 발생

<br>

##### 2) 리스트

안에 들어갈 수 있는 데이터 타입이 다양(헤테로) 하고 순서가 중요(시퀀스)하다.

```python
a=[1, 'a']
a   # output : [1, 'a']
range(100)[50]   # output : 50
```

<br>

##### 3) set

안에 들어갈 수 있는 데이터 타입이 다양(헤테로)하고 순서가 없다 (중복 X).

순서가 없으니 indexing, slicing 불가능하다.

```python
a={3,1,2}
a   # output : {1, 2, 3}

a={3,1,2,1,1}
a   # output : {1, 2, 3}

a=frozenset({3,1,2})
a   # output : frozenset({1, 2, 3})
```

<br>

#### 용어 정리

* mutable : 값을 변경할 수 있는 객체들

* immutable : 값을 변경할 수 없는 객체들

* 호모지니어스 (Homogenous) : 안에 들어갈 데이터 타입이 같은 것

* 헤테로지니어스 (Heterogenous) : 안에 들어갈 데이터 타입이 다른 것

  ex) set

* 시퀀스 타입 (sequence type) : 순서가 중요

ex) 문자열 : 안에 들어갈 데이터 타입이 중요하고 순서가 중요하다. (homogenous한 sequence type)

<br>

<br>

#### 에러 정리

* NameError : 정의되지 않은 언어를 출력할 때 에러 발생

<br>