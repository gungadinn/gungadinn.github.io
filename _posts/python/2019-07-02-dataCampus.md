---
layout: post
title: "Python (2) - 할당, 조건문, 반복문, 매개변수"
category: python
tags: [python]
comments: true
---

## 빅데이터 청년인재 Day 2

---

### 목차

* 할당의 종류
  * 하나씩
  * 동시할당
  * \* 할당
  * 동시할당
  * 증감할당
  * 기타
* mutable 객체 사용 시 주의할 점
  * return 값 존재 X 
  * return 값 존재 O
  * return 값 존재 O && 내 자신도 변화
* 기타
  * 함수의 리턴값
  * 튜플 → 리스트
  * set
  * 연산자
  * 공통된 메소드를 찾을 때
  * Dictionary
  * in
* 조건문
  * if
  * and
  * or
* 반복문
* 3가지의 else
  * 반복문에서의 else
  * if문에서의 else
  * try-catch문에서의 else
* 매개변수
  * positional (포지셔널) 방식
  * keyword (키워드) 방식
  * positional-keyword (포지셔널-키워드) 방식
  * var-positional (가변-포지셔널) 방식
  * var-keyword (가변-키워드) 방식
* 정리
* 기타
* 에러 정리

---



## 1. 할당의 종류

### 1) 하나씩

```python
#숫자
>>> a = (1)   
>>> a
1

#튜플
>>> a = 1,   
>>> a
(1,)

#이것도 튜플. 한개로 묶어줘서 할당 
>>> a = 1,2,3,   
>>> a
(1,2,3)

#끝에 ,없어도 튜플 
>>> a = 1,2,3   
>>> a
(1,2,3)
```

<br>

```python
>>> a = 1
>>> b = 2

# 파이썬에서의 swap
>>> a,b = b,a   # a:2, b:1

# 아래와 같은 방법은 안된다.
>>> a = b
>>> b = a   # a:2, b:2

# 따로 쓰고 싶으면 C언어처럼 임시 객체 (ex : temp) 필요
>>> a = b
>>> temp = a
>>> b = temp
```

<br>

### 2) 동시할당 (다중대입)

```python
# 하나씩 할당
>>> a,b = 1,2
>>> a
1
>>> b
2

# 갯수가 다르면 에러 발생 
>>> a,b = 1,2,3
ValueError

>>> a, b, c = (1, 2, 3)   # a:1, b:2, c:3
```

> Value Error : too many values to unpack (expected 2)
>
> <figure>
>   <img src="https://i.imgur.com/dvUrtCB.png">
> </figure>

<br>

### 3) * 할당

```python
# 할당에서 *은 나머지를 받겠다는 의미
# 주의해야할 것은 b는 리스트
>>> a,*b = 1, 2, 3   # a: 1, b:[2,3]

>>> a,*b, c = 1, 2, 3, 4, 5, 6    # a:1, b:[2,3,4,5], c:6

# Systax Error
>>> *a = 1, 2, 3, 4, 5, 6

# *는 나머지를 받는데 그 나머지가 리스트 형태
# 튜플을 리스트로 바꿔주고 싶을 때 아래와 같이 하면 편하다
>>> *a, = 1, 2, 3, 4, 5, 6     #
>>> a
[1,2,3,4,5,6]
```

> SyntaxError : starred assignment target must be in a list or tuple
>
> <figure>
> 	<img src="https://i.imgur.com/7EScrna.png">  
> </figure>

<br>

### 4) 동시할당

```python
>>> a = b = [1, 2, 3]
>>> a   
[1, 2, 3]
>>> b   
[1, 2, 3]

>>> b.append(4)   # return None
>>> b    
[1, 2, 3, 4]
>>> a   
[1, 2, 3, 4]

>>> b = [1, 2, 3, 4, 5, 6]
>>> a = [1, 2, 3, 4]
```

> mutable 일 때는 같이 바뀔 수 있으므로 주의.
>
> `a = b= 1` 와 같이 초기화 할 때는 괜찮다.

<br>

### 5) 증감할당

```python
>>> a += 1
```

<br>

### (+) 할당

<br>
<br>

## 2. mutable 객체 사용시 주의할 점

---

#### (1) return 값 존재 X 

눈에 보이는 결과는 없지만 행동은 한다.

```python
>>> a = [1]   
>>> a
[1]

>>> a = [1,]   
>>> a
[1]

# 똑같은 list 형태만. 리턴값 안 보임
>>> a.append(2)    
>>> a
[1,2]

# 형태까지. 리턴값 안 보임
>>> a.extend([3])    
>>> a
[1,2,3]

>>> a.index(1)
0
```

 append, extend는 리턴값이 none이기 때문에 함수를 실행해도 output 이 없다. 

<br>

함수의 형태가

```python
>>> def xx():
>>>     a = 1
```

과 같이 리턴값이 없으면 자동으로 return None을 붙여준다. 

행동을 하고 결과값을 반환하지 않는 void와 같은 형태인데 이들은 함수를 실행하더라도 리턴값이 안 나온다.

```python
>>> xx() is None
True
```

<br>

> immutable은 리턴값 있다. mutable은 리턴값이 있을수도 있고 없을수도 있다.

<br>

```python
>>> b=[1,2,3]
>>> def t(l):
>>>     return l.append(3)
>>> t(b)  
>>> b
[1, 2, 3, 3]
```

> 함수 t(b)에서 append가 return none이기 때문에 행동을 안 하는 것처럼 보이지만 실제로는 행동을 한다. 이러한 특징 때문에 빨리빨리 캐치가 안 되어 주의해야한다.
>
> 또한, 함수 t에서 return none이므로 굳이 return 을 쓸 필요가 없다.
>
> ```python
>>>> def t(l):
> >>>     return l.append(3)
>```

<br>

— 함수의 매개변수로 쓸 때  (default로 쓸 때)

```python
>>> b=[1,2,3]

>>> def s(t,L=[]):
>>> 	  L.append(t)

>>> s(3,b)
>>> b
[1, 2, 3, 3]
```

<br>
— 안 좋은 예

```python
>>> def s(t,L=[]):
>>>     L.append(t)
>>>     return L
  
>>> s(1)   
[1]
>>> s(1)   
[1, 1]
```

> default 값을 mutable로 하면 heap 영역에 올라가기 때문에 계속 유지된다.

<br>

#### (2) return 값 존재 O

```python

```

<br>

#### (3) return 값 존재 O && 내 자신도 변화

```python
>>> a = [1, 2, 3, 'legolas']
>>> a.pop()
'legolas'
```

> pop()은 맨 끝에 있는 것을 뽑으면서 출력시킨다. 즉 return 값이 있고 자기 자신도 변화시킨다. 따라서 mutable 일 때는 조심해야한다.

 <br>

참고 : mutable은 자기 자신이 변하면 첫 번째 주소는 안 변하지만 뒤에 추가가 되면 변한다. 일종의 pointer 개념으로 파이썬의 list는 linked list 개념이다.

<br>

## 3. 기타

---

### 1) 함수의 리턴값

```python
>>> def x():
>>> 	  return 1,2

>>> x()
(1, 2)
```

함수의 리턴 값은 튜플의 형태로 나오고 사실 상 하나이다.

<br>

### 2) 튜플 → 리스트

— 튜플에서 하나씩 뽑아서 리스트로 넣는 방법

```python
>>> list('레골라스')
['레','골','라','스']
```

<br>

### 3) set

```python
>>> a = {1, 2, 3, 2, 0}
>>> a   
{0, 1, 2, 3}
```

set은 내부적으로 순서를 관리하지만 사람은 관리할 수 없다.

<br>

```python
>>> for i in a:
>>>     print(i)

0
1
2
3
```

for문 안에 set 사용 가능하다. 내부적으로 순서를 관리하기 때문이다.

<br>

```python
>>> a = {1, 2, 3, 2, 0}

>>> a - {2}   
{0, 1, 3}    #(차집합)

>>> a & {2}   
{2}    #(교집합)

>>> a | {4}   
{0, 1, 2, 3, 4}    # (합집합)

>>> a ^ {2}   
{0, 1, 3}   #(대칭 차집합)
```

set은 set만의 연산자 (고유의 연산자)를 지원한다. 단, 전체집합을 모르기 때문에 여집합은 지원하지 않는다.

<br>

### 4) 연산자

```python
>>> a = [1, 2, 3]
>>> b = [3, 4, 5]
>>> c = 3
```

컨테이너 등은 더하기 연산자와 곱하기 연산자를 지원하나 곱하기는 반복이다

<br>

```python
>>> a + b    
[1, 2, 3, 3, 4, 5]

>>> a * b   
TypeError   #(*연산자를 사용하려면 한 쪽이 숫자일때만 가능)

>>> a * c   
[1, 2, 3, 1, 2, 3, 1, 2, 3]   #(한 쪽이 숫자일때만 반복)

>>> 3 * b   
[3, 4, 5, 3, 4, 5, 3, 4, 5]
```

> TypeError : can't multiply sequence by non-init of type 'list'
>
> <figure>
>   <img src="https://i.imgur.com/RoYMQfH.png">
> </figure>

<br>

```python
>>> a = 'abc'
>>> b = '레골라스'

>>> a+b  
'abc레골라스'

>>> 'abc'+'레골라스'   
'abc레골라스'

>>> 'abc''레골라스'   
'abc레골라스'

>>> [1,2,][2,3,]   
TypeError
```

> TypeError : list indices must be integers or slices, not tuple (즉, 문자열은 더하기가 가능하지만 리스트는 불가능)
>
> <figure>
>   <img src="https://i.imgur.com/4upTLkO.png">
> </figure>

<br>

### 5) 공통된 메소드를 찾을 때

```python
>>> set(dir(list())) & set(dir(tuple()))
```

> 비어있는 리스트에서 쓸 수 있는 메소드 & 비어있는 튜플에 쓸 수 있는 메소드

<br>

```python
>>> t = set(dir(list())) & set(dir(tuple()))

# 튜플만의 고유 메소드
>>> set(dir(tuple())) - t
{'__getnewargs__'}

# 리스트만의 고유 메소드. 즉 자기자신을 변화시키는 mutable
>>> set(dir(list())) - t
{'reverse', 'insert', 'clear', 'sort', '__iadd__', '__reversed__', '__delitem__', '__setitem__', 'copy', 'append', 'pop', 'extend', 'remove', '__imul__'}
```

<br>

### 6) Dictionary

```python
# key만 보여줌
>>> {'c':2, 'a':1, 'b':2}.keys()   
dict_keys(['c', 'a', 'b'])

# value만 보여줌
>>> {'c':2, 'a':1, 'b':2}.values()   
dict+values([2, 1, 2])

# 전부 보여줌
>>> {'c':2, 'a':1, 'b':2}.items()   
dict_items(['c', 2), ('a', 1), ('b', 2)])
```

<br>

 ```python
# key만 보여줌
>>> for i in {'c':2, 'a':1, 'b':2}.items():
>>>     print(i)
    
# key만 보여줌
>>> for i in {'c':2, 'a':1, 'b':2}.keys():
>>>     print(i)
    
# Dictionary는 for문 돌리면 key만 나옴
>>> for i in {'c':2, 'a':1, 'b':2}:
>>>     print(i)
 ```

> output :
>
> c
>
> a
>
> b

<br>

```python
>>> x = {'a':1, 'b':2}

>>> x['a']    
1
>>> x['c']   
KeyError
```

> KeyError : key 값이 없을 때 발생하는 에러
>
> <figure>
>   <img src="https://i.imgur.com/KzISNgg.png">
> </figure>

<br>

```python
# get은 mutable (dir()을 통해 확인해보기)
>>> x.get('a')   #
1

# 아래와 같은 방법을 쓰면 key 에러 방지 가능
# x에 d가 없으면 3을 자동으로 넣어 key 에러를 방지한다
>>> x.get('d', 3)   
```

<br>

### 7) in

있는지 확인한다.

```python
>>> 3 in {1, 2, 3}   
True

>>> 3 not in {1, 2, 3}   
False
```

```python
# 내부 구현자의 우선순위는 key이므로 key만 체크한다.
>>> 2 in {'c':2, 'a':1, 'b':2}   
False

>>> 'a' in {'c':2, 'a':1, 'b':2}    
True
```

```python
>>> 'a' in [1, 2, 3]    
False

>>> '1' in [1, 2, 3]   
False
```

```python
>>> '1' in '112345'   
True

>>> 1 in '123455'    
TypeError
```

> TypeError : 오른쪽이 문자열이기 때문에 왼쪽에도 문자열이 와야하는데 숫자가 와서 에러 발생
>
> <figure>
>   <img src="https://i.imgur.com/OTLvpIX.png">
> </figure>

<br>

## 4. 조건문

---

조건문에서는 무엇을 넣든 있다, 없다, 즉 존재론적인 관점에서 체크한다. 조건은 모든 종류의 시퀀스가 될 수 있는데 길이가 0이 아닌 것은 모두 참이고, 빈 시퀀스는 거짓이다.

### 1) if

```python
>>> if 2 :
>>>     print('T')
>>> else :
>>>     print('F')
>>> 
T
```

```python
>>> if 0.0 :
>>>     print('T')
>>> else :
>>>     print('F')
>>> 
F
```

```python
>>> if [] :
>>>     print('T')
>>> else :
>>>     print('F')
>>> 
F
```

```python
>>> if None :
>>>     print('T')
>>> else :
>>>     print('F')
>>> 
F
```

```python
>>> None == False
False
```

(Fase=0 → 값 있다) == (None → 값 없다) → 때문에 false로 체크하면 존재론적인 관점에서 틀리다

<br>

```python
>>> False * 10293109238193
0
```

```python
>>> if '' :
>>>     print('T')
>>> else :
>>>     print('F')
>>> 
F
```

```python
>>> if '''''' :
>>>     print('T')
>>> else :
>>>     print('F')
>>> 
F
```

```python
>>> if a is not None:
>>>     print('T')
>>> else :
>>>     print('F')
>>> 
T
```

<br>

### 2) and

`True and True` 이면 후자 출력. `False and True` 이면 전자 출력

```python
>>> 1 and 3
3
```

```python
>>> 0 and 3
0
```

```python
>>> '''''' and 3
''
```

```python
>>> [1, 2, 3] and 3
3
```

<br>

### 3) or

`True or ???` 처럼 전자가  True면 후자 보지 않고 전자 출력.

전자가 True면 후자를 체크하지 않는 것 때문에 가끔씩 에러 발생.

```python
>>> 1 or 3
1
```

```python
>>> 1/0
ZeroDivisionError
```

> ZeroDivisionError : division by zero
>
> <figure>
>   <img src="https://i.imgur.com/CKRlxUk.png">
> </figure>

<br>

```python
>>> if True and False :
>>>     print(1)
>>> else :
>>>     print(2)
>>> 
2
```

<br>

```python
>>> 3 or 1/0
3
```

<br>

```python
>>> if True or False :
>>>     print(1)
>>> else :
>>>     print(2)
>>> 
2
```



<br>

## 5. 반복문

---

in 다음에 들어갈 수 있는 것은 이터러블이다. 이터러블은 member를 하나씩 반환 가능한 객체를 말하는데 기본적으로 순서가 있는 시퀀스 타입(list, tuple, range 등)은 이터러블이다. 시퀀스 타입이 아닌 set이나 dictonary도 이터러블이다. (컨테이너는 기본적으로 이터러블이다)

```python
>>> for i in '레골라스':
>>>     print(i)
>>> 
레
골
라
스
```



```python
>>> for i in 1, 2, 3, '레골라스', 4:
>>>     print(i)
>>> 
1
2
3
레골라스
4
```



```python
# set은 내부적으로 순서를 정하니까 사용 가능하다
>>> for i in {4, 3, 2, 5, 1}:
>>>     print(i)
>>> 
1
2
3
4
5
```



```python
# 딕셔너리도 가능
>>> for i in {'a':1, 'b':2}:
>>>     print(i)
>>> 
a
b
```



```python
>>> for i in {'a':1, 'b':2}.items():
>>>     print(i)
>>> 
('a', 1)
('b', 2)
```



```python
>>> for i in {'a':1, 'b':2}.values():
>>>     print(i)
>>> 
1
2
```

<br>

```python
# 괄호를 씌우든 안 씌우든 튜플로 인식한다
>>> for i,j in {'a':1, 'b':2}.items():
>>>     print(i)
a 1
b 2
```

<br>

## 6. 3가지의 else

---

### 1) 반복문에서의 else

반복문이 다 올바르게 실행될 때만 else를 실행한다

```python
>>> for i,j in {'a':1, 'b':2}.items():
>>>     print(i,j)
>>> else :
>>>     print('레골라스')
>>> 
a 1
b 2
레골라스
```



```python
>>> for i in range(10000):
>>>     if i==10:
>>>         break
>>>     else:
>>>         print(i)
>>> else:
>>>     print('레골라스')
>>> 
0
1
2
3
4
5
6
7
8
9
```

i==10에서 break가 걸렸으므로 레골라스를 출력하지 않는다. (전부 올바르게 실행되지 않아서)



```python
>>> a = 10
>>> while a>0:
>>>     a -= 1
>>>     print(a)
>>> else :
>>>     print('레골라스')
9
8
7
6
5
4
4
3
2
1
0
레골라스
```

<br>

```python
>>> a = 10
>>> while a>10 :
>>>     a -= 1
>>>     print(a)
>>> else :
>>>     print('레골라스')
>>> 
레골라스
```

while문이 조건에 안 맞아 한번도 안 돌아도 0번 돈 것이기 때문에 else에 해당하는 조건문이 실행된다.

```python
>>> a = 10
>>> while a>10 :
>>>     a -= 1
>>>     if a==5 :
>>>         break
>>>     print(a)
>>> else :
>>>     print('레골라스')
>>> 
9
8
7
6
```

중단되서 레골라스가 찍히지 않는다. 

<br>

#### cf) 중단되는 경우

1. 내가 손으로 직접 중단하는 경우 (break, raise)

   ```python
   # raise : 에러를 발생시킨다
   >>> a = 10
   >>> while a>10 :
   >>>     a -= 1
   >>>     if a==5 :
   >>>         raise
   >>>     print(a)
   >>> else :
   >>>     print('레골라스')
   >>> 
   9
   8
   7
   6
   RuntimeError
   ```

   >RuntimeError : No active exception to reraise
   >
   ><figure>
   ><img src="https://i.imgur.com/XMIknx3.png">
   ></figure>

   중단되서 안 찍힌다. (완벽하게 돌지 않았기 때문에)

   <br>

2. 외부 상황으로 중단되는 경우

    ctrl+c 같은 인터럽트가 들어올 때

<br>

### 2) if문에서의 else

if의 else는 if문이 틀렸을 때 실행한다.

i==10에서 break가 걸렸으므로 레골라스를 출력하지 않는다. (for문이 올바르게 실행되지 않아서)

```python
>>> for n in range(2, 10):
>>>     for x in range(2, n):
>>>         if n % x == 0:
>>>             print(n, 'equals', x, '*', n//x)
>>>             break
>>>     else:
>>>         # loop fell through without finding a factor
>>>         print(n, 'is a prime number')
...
2 is a prime number
3 is a prime number
4 equals 2 * 2
5 is a prime number
6 equals 2 * 3
7 is a prime number
8 equals 2 * 4
9 equals 3 * 3
```

<br>

### 3) try-catch문에서의 else

try에서 에러가 발생하지 않으면 else 실행, 에러 발생하면 except 실행한다.

```python
# cf) while문은 무한루프 만들기 좋다

>>> while True:
>>>     try :
>>>         a=int(input())
>>>     except :
>>>         continue
>>>     else : # 숫자면 break
>>>         break
>>> 
아라곤
레골라스
3
```

<br>

```python
# while 뒤에 조건이 True라서 실행된다
>>> while [123, 1, 2, 13, 'as'] :
>>>     try :
>>>         a=int(input())
>>>     except :
>>>         continue
>>>     else :
>>>         break
>>> 
해리포터
헤르미온느
론 위즐리
2
```

<br>

## 7. 매개변수

---

### 1) positional (포지셔널) 방식

C언어랑 똑같다고 생각하면 된다. 즉, 파라미터와 인자 갯수를 맞춰줘야 한다.

```python
>>> def moon_beauty(a,b):
>>>     ''' 함수 설명 '''
>>>     a = 1
    
>>> moon_beauty()    
TypeError
```

> TypeError
>
> <figure>
>   <img src="https://i.imgur.com/7TeWnV3.png">
> </figure>

```python
>>> def moon_beauty(a, b):
>>>     print(a, b)
    
>>> moon_beauty(1,2)    
1 2
```

<br>

### 2) keyword (키워드) 방식

키워드로만 제공될 수 있는 인자를 지정한다. 따라서 순서를 바꿔써도 된다.

```python
>>> def moon_beauty(a, b):
>>>     print(a, b)
    
>>> moon_beauty(b=3,a=4)    
4 3
```

<br>

### 3) positional-keyword (포지셔널-키워드) 방식

두 방법을 섞어 사용하는 것도 가능하다. 단, 한 번 키워드를 사용하면 그 뒤로도 계속 키워드를 사용해야한다.

```python
>>> def moon_beauty(a, b, c):
>>>     print(a, b, c)
    
>>> moon_beauty(3, c=4, 2)   
SyntaxError

>>> moon_beauty(3, 2, b=4)   
TypeError
```

> SyntaxError : 키워드 사용 이후 사용안해줘서 에러 발생
>
> <figure>
>   <img src="https://i.imgur.com/rjo3onn.png">
> </figure>
>
> 
>
> TypeError : 3은 a, 2는 b로 들어가는데 b=4를 통해 한 번 더 값을 전달해주므로 에러 발생
>
> <figure>
><img src="https://i.imgur.com/IX00Nvv.png">
> </figure>

<br>

```python
# 무조건 키워드. 포지셔널 방식 절대 불가
>>> def moon_beauty(*, a=0, b=0, c=0) :
>>>     print(a, b, c)
    
>>> moon_beauty()    
0 0 0

>>> moon_beauty(1)   
TypeError

>>> moon_beauty(1,2)   
TypeError

>>> moon_beauty(1,2,3)   
TypeError

>>> moon_beauty(b=4)   
0 4 0
```

> TypeError : 키워드 방식을 사용해야 하는데 포지셔널 방식을 사용해서 에러 발생
>
> <figure>
>   <img src="https://i.imgur.com/StO3umm.png">
> </figure>
>
> <figure>
>   <img src="https://i.imgur.com/AQ3mzIk.png">
> </figure>

<br>

```python
>>> def moon_beauty(x=0, *, a=0, b=0, c=0) :
>>>     print(x, a, b, c)
    
>>> moon_beauty(3)    
3 0 0 0
```

<br>

####   cf) default 값 지정

default는 뒤부터 채워줘야 에러가 발생하지 않는다.

```python
>>> def moon_beauty(a, b=0, c):
>>>     print(a, b, c)
```

> SyntaxError
>
> <figure>
>   <img src="https://i.imgur.com/m195sBo.png">
> </figure>
>
> 

```python
# OK
>>> def moon_beauty(a, b, c=0):
>>>     print(a, b, c)
    
# OK
>>> def moon_beauty(a=0, b=0, c=0):
>>>     print(a, b, c)
    
>>> moon_beauty()   
0 0 0

>>> moon_beauty(c=1)   
0 0 1
```

<br>

### 4) var-positional (가변-포지셔널) 방식

갯수가 정해지지 않았을 때. 인자 갯수 상관없이 대입 가능하다

```python
>>> def moon_beauty(*a):
>>>     print(a)
    
>>> moon_beauty()   
()

>>> moon_beauty(1)   
(1,)

>>> moon_beauty(1, 2)   
(1, 2)

>>> moon_beauty(1, 2, 4)   
(1, 2, 4)
```

<br>

### 5) var-keyword (가변-키워드) 방식 

**부터는 return 값이 딕셔너리 형태이다

```python
>>> def moon_beauty(**a):
>>>     print(a)
    
>>> moon_beauty(a=3, b=2, c=4)   
{'a': 3, 'b': 2, 'c':4}
```

<br>

#### example)

```python
>>> def moon_beauty(a, b, c):
>>>     print(a, b, c)
    
>>> a = [1, 2, 3]
>>> moon_beauty(a[0], a[1], a[2])   
1 2 3

# 인자에서 * 쓸 때는 쪼개준다.
>>> moon_beauty(*a)   
1 2 3

# 갯수가 안 맞으면 에러가 발생한다.
>>> a = [1, 2]
>>> moon_beauty(*a)   
TypeError
```

> TypeError
>
> <figure>
>   <img src="https://i.imgur.com/kgrWR75.png">
> </figure>

<br>

```python
>>> def moon_beauty(a, b, c):
>>>     print(a, b, c)
    
# 별표 한개이므로 key가 포지셔널 방식으로 들어감
>>> a = {'a': 1, 'b': 2, 'c': 3}
>>> moon_beauty(*a)   
a b c


# 별표 두개이므로 key가 키워드 방식으로 들어감
>>> a = {'a': 4, 'b': 2, 'c': 3}
>>> moon_beauty(*a)   
a b c
>>> moon_beauty(**a)   
4 2 3

>>> a = {'d': 1, 'b': 2, 'c': 3}
>>> moon_beauty(**a)   
TypeError

>>> a = {'a': 4, 'c': 3, 'b': 2}
>>> moon_beauty(*a)   
a c b
>>> moon_beauty(**a)   
4 2 3
```

> TypeError
>
> <figure>
>   <img src="https://i.imgur.com/c841G1t.png">
> </figure>

<br>

## 정리

---

### 1) 파이썬에서 * 이 가지는 5가지

(1) 할당 : 나머지를 받겠다.   `a, b = 1, 2, 3, 4, 5`

(2) unzip

3) ** 딕셔너리 형식

4) 라이브러리 불러올 때

5) 함수 선언시, 파라미터 형식 ()

6) 

7) 

<br>

## 기타

1. `%%timeit` : 파이썬 상태나 표현의 시간 측정

 ```python
%%timeit
temp=[]
for i in range(100):
    temp.append(i)
 ```

2. `%%time` : 

```python
%%time
temp=[]
for i in range(100):
    temp.append(i)
```



3. `import builtins` :  를 통해 기본적으로 탑재된 함수들을 import하고 `dir(builtins)` 을 통해을 볼 수 있다.

 ```python
>>> import builtins
>>> dir(builtins)
 ```

4.  `eval` : 반환 값은 계산된 표현식의 결과이다.

```python
>>> type(a)
str

>>> 'eval' in dir(builtins)
True
```



<br>

## 에러 정리

---

1. NameError : %whos 입력했을 때 출력되는 객체들 말고 없는 거 부를 때
2. IndexError : 범위 벗어날 때
3. TypeError : 같은 타입이 아닌데 메소드 사용할 시
4. KeyError : key가 없을 때
5. Syntax : 문법에 맞지 않는다