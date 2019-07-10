---
layout: post
title: "Python (3) - 함수형 기법"
category: python
tags: [python]
comments: true
---

## 빅데이터 청년인재 Day 3

### 목차



## 함수형 프로그래밍



함수형 기법 : 함수를 이용하여 프로그래밍을 하는 방식

파이썬은 멀티 패러다임 언어

객체지향 언어지만 함수형 언어를 지원한다

함수를 이용하게 되면 side effect를 없앨 수 있다

def 로 묶여 있다는 것을 모듈화





 에서 mutable 없음. 바뀌면 골치아프니까 함수형 기법에서는 immutable 타입만 가지고 있다.

<br>

```python
# 절차적 프로그래밍
>>> a = input()
>>> a = int(a)   
3
```

> 함수형 프로그래밍에서는 정의역을 한번에 처리하는 것에 더 효율적임
>
> 함수형 패러다임에서는 한꺼번에 처리하기 위해서는 한 개씩 뽑아내야 하기 때문에  처리한다.

<br>

```python
>>> import time
>>> def x(a=time.time):
>>>     return a

>>> time.time()
562419842.0954182
```

> output : 1971년 이후로 현재 시간까지 나노세컨드로 

멀티 패러다임 : 

객체지향언어를 기반으로 하지만 함수형 패러다임까지 지원

함수의 세트를 통해 흘러다니며. 함수의 입력으로부터 동작해서 출력을 만든다. 

<u>파이썬은 순수한 함수형 패러다임이 아니다. </u> 기본 객체지향의 함수형 패러다임을 지원을 한다. 때문에 함수형 패러다임에 집착할 필요가 없고 필요한 부분만 가져다 사용하면 된다.

 <br>

<br>

## 2.함수형 기법의 장점

---

### 1) 형식적 증명 가능성

중간에 값이 바뀌면 안되기 때문에 자료 형태가 mutable 형태이면 불가능하다. immutable만 가능.

<br>

### 2) 모듈성

프로그램을 작은 조각(함수)으로 분해할 수 있다. 작은 조각들을 잘 만들면 읽거나 오류 수정에 더 용이하고 다른 함수와 함성 시킬 수 있다.

<br>

### 3) 결합성

<br>

### 4) 디버깅과 테스트 용이성

<br>

<br>

## 3. 이터레이터 (Iterator)

---

이터레이터는 순환 가능한 객체로 전체 중에서 한 번에 한 개씩만 반환한다. 이터레이터는 next 함수 혹은 \__next__ 메소드를 통해 객체의 다음 요소를 반환한다. 

이터러블은 이터레이터를 얻을 수 있는 객체이다. `for X in Y` 에서 Y는 이터레이터여야 한다.

<br>

### list_iterator

```python
>>> a = [1, 2, 3]
>>> b = iter(a)
>>> type(b)   
list_iterator

>>> next(b)   
1
>>> next(b)  
2
>>> next(b)   
3
>>> next(b)    
StopIteration

>>> b    
<list_iterator at 0x10e41aba8>

>>> list(b)   
[]
```

> StopIteration : Iterator에 의해 생성된 항목이 더이상 없다는 것을 알려주기 위해, 내장함수 next() 나 이터레이터의 \__next__ 메소드가 일으킨다.

![image-20190703130313561](/Users/eunkyoung/Library/Application Support/typora-user-images/image-20190703130313561.png)

<br>

```python
>>> import dis

>>> def iterator_exam():
>>>     for i in [1, 2, 3]:
>>>         print(i)
   
>>> dis.dis(iterator_exam)
```

> dis : 어셈블리어로 바꿔준다.
>
> for문이 돌 때 이터레이터가 쓰인다.
>
> output
>
> ![image-20190703132140965](/Users/eunkyoung/Library/Application Support/typora-user-images/image-20190703132140965.png)

<br>

### set_iterator

```python
>>> a = {1, 2, 3}
>>> b = iter(a)
>>> type(b)   
set_iterator

>>> next(b)   
1
```

<br>

### str_iterator

```python
>>> a = 'legolas'
>>> b = iter(a)
>>> b   # output : 
<str_iterator at 0x10e582588>

>>> next(b)   
'l'
>>> list(b)  
['e', 'g', 'o', 'l', 'a', 's']
>>> type(b)   
str_iterator
```

<br>

```python
>>> a = {1, 2, 3}
>>> b = iter(a)
>>> type(b)   
str_iterator

>>> next(b)    
1
>>> next(b)  
2
>>> next(b)  
3
>>> next(b)  
StopIteration

>>> a = [1, 2, 3]
>>> b = iter(a)
>>> type(b)   # 
<class 'list_iterator'>
>>> next(b)
>>> list(b)  
[2, 3]

>>> a = [1, 2, 3]
>>> b = iter(a)
>>> next(b)   
[1]
>>> next(b)  
[2, 3]
>>> list(b)  
[3]
```

* 파이썬에서 이터레이터의 장점 : __Lazy__

  Lazy : 실행되기 전까지는 메모리 상에 안 올리고 실행되면 하나씩 메모리에 올려서 실행한다. 즉, 위의 예에서 next 할 때 하나씩 메모리에 올라간다. 이러한 방법은 속도가 느린데 파이썬에서는 Lazy의 속도를 올리기 위해 내부적으로 최적화가 되어있다. 따라서, python의 lazy 기법은 속도도 빠르다. 

* next는  pop하고 비슷하나 다르다. pop은 뒤에서부터 뽑는다면 next는 앞에서부터 뽑는다.

* 내부적으로 이터레이터로 바꾼다.
* iterator에는 리스트, 튜플, 집합, 딕셔너리 전부 가능하다.

<br>

객체지향을 사용하기 위해서는 클래스를 선언하고 인스턴스화 한다.

인스턴스화 하려면 클래스 이름을 알아야한다.

객체지향은 인스턴스 방식으로 값을 만든다. 리터럴 방식도 지원.#

이터러블(iterable)과 이터레이터(iterator) 차이점 : next의 유무

```python
>>> set(dir(Iterator)) - set(dir(Iterable))
{'\__next__'}
```

즉 이터레이터에는 next가 있고 이터러블에는 없다. 따라서 이터러블 쓸 곳에서 이터레이터를 써도 된다.

* 이터레이터 지원하는 데이터형 : 리스트, 튜플, 문자열, 딕셔너리, 집합

<br>

<br>

## 4. 제너레이터 (Generator)

---

제너레이터는 이터레이터를 작성하는 작업을 단순화 하는 특별한 클래스의 함수. 

```python
>>> def generator_exam():
>>>       yield 1
>>>       yield 2
>>>       yield 3
    
>>> x = generator_exam()
>>> type(x)   
generator

>>> next(x)   
1
>>> next(x)   
2
>>> next(x)   
3
>>> next(x)   
StopIteration
```

> StopIteration : try except와 같이 쓰면 좋을 듯

![image-20190703135234252](/Users/eunkyoung/Library/Application Support/typora-user-images/image-20190703135234252.png)



yield 키워드를 포함하는 함수는 제너레이터 함수이다. yield와 return의 차이점은 yield에 도달하면 제너레이터의 실행 상태가 일시중단되고 지역변수가 보존된다. 제너레이터 또한 next() 혹은 \__next__를 사용할 수 있으며 lazy한 장점이 있다.

<br>

```python
# 아래처럼도 사용 가능하다
>>> def generator_exam():
>>> 	  yield from [1, 2, 3, 4]
  
>>> x = generator_exam()

>>> next(x)   
1
>>> next(x)  
2
>>> next(x)  
3
>>> next(x)   
4
>>> next(x)   
StopIteration
```

> StopIteration![image-20190703140033729](/Users/eunkyoung/Library/Application Support/typora-user-images/image-20190703140033729.png)

<br>

 ```python
sum((x for x in range(1,11)))
sum(x for x in range(1,11))  # 위의 식을 현재 식처럼 축약 가능하다
 ```



<br>

## 5. 반복식 (Comprehension)

---

초기화하는데 유용하다. for문보다 속도가 더 빠르다.

### 1) 리스트 (List)

```python
>>> [x for x in range(10)] 
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

>>> [x+1 for x in range(10)]   
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

>>> [x/3 for x in range(10)]
[0.0, 0.3333333333333333, 0.6666666666666666, … , 3.0 ]
  
>>> [str(x) for x in range(10)]
['0', '1', '2', '3,' '4', '5', '6', '7', '8', '9']

>>> [0 for x in range(10)]   # 이처럼 초기화 하는데 유용하다
[0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
```

<br>

### 2) 집합 (Set)

```python
>>> {x for x in range(10)}
{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
```

<br>

### 3) 딕셔너리 (Dictonary)

```python
>>> {x:1 for x in range(10)}
{0:1, 1:1, 2:1, 3:1, 4:1, 5:1, 6;1, 7:1, 8:1, 9:1}
```

 <br>

### 4) 튜플 ? (tuple)

튜플(immutable)은 제너레이터이기 때문에 next를 사용하면 된다.

<br>

### 5) 참고

```python
>>> a = [x for x in range(10)]

# 위아래 코드는 같은 코드이다

>>> temp=[]
>>> for i in range(10)
>>>     temp.append(i)
```



<br>

## 6. First Class Funtion

---

파이썬에서 함수는 값이기 때문에 할당문에서 오른쪽에 쓸 수 있다.

> example 1

```python
>>> a = print
>>> a('legolas')  
legolas

>>> a.__name__   
'print'    #(실제로는 print지만 a로도 쓸 수 있다)
```

```python
>>> printf = print
>>> printf("hello world")  : 
hello world
```

<br>

> example 2

```python
>>> sum = 0
>>> sum + 1  : 
1
>>> del sum   
>>> del sum   
NameError   #(함수 sum만 남았기 때문에 name 'sum'은 정의되지 않음)
```

<br>

> example 3 : 아래와 같은 예시에서는 더 이상 sum 함수를 사용하지 못한다.

```python
>>> sum = 0
>>> for i in range(1,101):
>>>     sum+=i
    
>>> sum    
5050
```

<br>

> example 4 

```python
>>> a = [str, int, float]
>>> a[0]   
str
>>> a[0](2)  
'2'
```

<br>

```python
>>> def a(x):
>>>     print(x())
  
>>> a(print)   
None
```

<br>

<br>

## 7. 파이썬의 LEGB rule

---

변수 찾는 순서도 L → E → G → B이다.

<br>

### 1) L (LOCAL)

함수 내의 범위

```python
>>> def y():
>>>     x=1
>>>     print(x)
  
>>> y()   
1
```

<br>

```python
>>> def y():
>>>     z=1
>>>     print(z)

>>> z    
NameError   # 캡슐화
>>> y.z = 3
>>> y()   
1
>>> y.z   
3
```

즉 z와 y.z는 다르다.

<br>

### 2) E (Enclosing function rule)

중첩 함수(nested function / 함수 안에 함수) 라면 그 밖에 있는 함수 범위

```python
>>> def y():
>>>     def z():
>>>         return 1
>>>     return z()

>>> y()()  
1
```

<br>

```python
>>> def y():
>>>     print('legolas')
>>>         return y
  
  
>>> y()   
legolas 
>>> y()()   
legolas
legolas
```

<br>

#### closure

```python
>>> def y(x):
>>>     def z():
>>>         return 1
>>>     return z

>>> y(3)()   
1
```

<br>

```python
def y(x):
>>>     def z():
>>>         return x+1
>>>     return z

>>> y(3)()    
4
```

<br>

```python
>>> def y(x):
>>>     def z(n):
>>>         return x+n
>>>     return z


>>> two_add = y(2)   
>>> two_add(3)   
3

>>> two_add = y(4)
>>> two_add(3)   
7

#lambda 사용
>>> def y(x):
>>>     return lambda n:n+x

>>> two_add = y(2)
>>> two_add(4)
```

> 첫 번째 인자를 무엇으로 받느냐에 따라 두번째 인자를 전체적으로 바꿀 수 있다.

<br>

### 3) G (Global)

함수 밖을 일컫는 범위

```python
>>> x = 1
>>> def y():
>>>   print(x)

>>> y()   
1
```

<br>

```python
>>> x = 1   # 접근 할 수 있지만 변경은 불가
>>> def y():
>>>   x=x+1
>>>   print(x)
  
>>> y()    
UnBoundLocalError   #할당 전에 참조되기 때문에 에러
```

<br>

```python
>>> x = 1
>>> def y():
>>>   global x   
>>>   x=x+1
>>>   print(x)
  
>>> y()   
2
>>> y()   
3
```

> global 선언 시 함수 안밖의 객체가 싱크된다.
>
> 즉 함수 내부에서 함수 외부의 것을 바꿀 수 없지만 global을 통해 싱크가 되어 바꿀 수 있다. 하지만 global을 사용하는 것은 안 좋다. 내가 생각하는 값이 있는데 global 객체가 있는 함수를 실행함으로써 값이 바뀔 수 있기 때문이다. 일종의 C언어의 goto 같은 존재이다. (python consenting adults)

<br>

### 4) B (Built-in)

내장 영역

<br>

### 5) nonlocal

nonlocal 문은 나열된 식별자들이 전역을 제외하고 가장 가까이서 둘러싸는 스코프에서 이미 연결된 변수를 가르키도록 만든다. nonlocal 문에 나열된 이름들은, global 문에 나열된 것들과는 달리, 둘러싼 스코프에서 이미 존재하는 연결들을 가르켜야한다. 또한, nonlocal 문에 나열되는 이름들은 지역 스코프에 이미 존재하는 연결들과 겹치지 않아야한다.

```python
>>> a = 1
>>> def x():
>>>     return a
    
>>> a
1
```



```python
>>> a = 1
>>> def x():
>>>     global a
>>>     a = 2
>>>     return a
    
>>> x()
2
```



```python
>>> a=1
>>> def x():
>>>     def y():
>>>         return a
>>>     return y()

>>> x()
1
```



```python
>>> a=1
>>> def x():
>>>     a=3
>>>     def y():
>>>         return a
>>>     return y()
  
>>> x()
3
```



```python
>>> a=1
>>> def x():
>>>     a=3
>>>     def y():
>>>         a=a+1 #
>>>         return a
>>>     return y()
    
>>> x()
UnboundLocalError
```



```python
>>> a=1
>>> def x():
>>>     a=3
>>>     def y():
>>>         global a
>>>         a=a+1 #
>>>         return a
>>>     return y()
  
>>> x()
2
```



```python
>>> a=1
>>> def x():
>>>     a=3
>>>     def y():
>>>         nonlocal a
>>>         a=a+1 #
>>>         return a
>>>     return y()
  
>>> x()
4
```



```python
>>> a=1
>>> def x():
>>>     b=3
>>>     def y():
>>>         return b
>>>     return y()
  
>>> x()
3
```



```python
>>> a=1
>>> def x():
>>>     b=3
>>>     def y():
>>>         return b
>>>     print(b+1)
>>>     return y()
  
>>> x()
4    # 출력값
3    # 리턴값
```



```python
>>> a=1
>>> def x():
>>>     def y():
>>>         print(c)
>>>         c=3
>>>         return b
>>>     print(b+1)
>>>     return y()
  
>>> x()
NameError
```



```python
>>> a=1
>>> def x():
>>>     def y():
>>>         print(c)
>>>         c=3
>>>         return 1
>>>     return y()
  
>>> x()
UnbounLocalError
```



```python
>>> print(yy)
>>> yy=3  
NameError
```



```python
>>> a=1
>>> def x():
        def y():
            return 1
        print(b)
>>>     return y()
  
>>> x()
NameError
```



```python
>>> a=1
>>> def x():
        def y():
            return 1
        print(b)
        b=1
        return y()
  
>>> x()
UnboundLocalError
```



<br>

## 기타

---

### 1) for 안에 if를 쓰는 두 가지 방식

1. if를 뒤에 쓴다.

   ```python
   >>> [x for x in range(10) if x%2 == 0]
   [0, 2, 4, 6, 8]
   ```
```
   
   for 뒤에 있는 x를 만나면 if문에 간 후 참이면 x를 실행한다. 
   

   
2. if를 앞에 쓴다

   ```python
   >>> [x if x%2 == 0 else 3 for x in range(10)]
   [0, 3, 2, 3, 4, 3, 6, 3, 8, 3]
```

   if가 참이면 앞의 문장(if문)을 실행하고 거짓이면 뒤의 문장(else)을 실행한다.




3. 재귀

   ```python
   >>> def fibo(n):
           if n<2:
   >>>         return 1
   >>>     return fibo(n-1)+fibo(n-2)
   ```

4. 내장함수

   1) map

<br>

### 2) 식은 식끼리 붙일 수 있다.

```python
>>> [(x,y) for x in range(1,11) for y in range(10)]
[(1, 0),
(1, 1),
(1, 2),
(1, 3),
(1, 4),
(1, 5),
(1, 6),
(1, 7),
(1, 8),
(1, 9),
(2, 0),
(2, 1),
(2, 2),
(2, 3),
(2, 4),
(2, 5),
...
(10, 9)]
```

<br>

```python
>>> ((x,y) for x in range(1,11) for y in range(10))
<generator object <genexpr> at 0x10e568ed0>
```

<br>

### 3) for를 쓰지 않는 기법

1.  이터레이터 제너레이터
2. 실제 for는 있지만 for문을 쓰지 않는다. (for문을 쓰지 않고 동시에 여러개를 처리하는 기법)

> 

## 에러정리

---

* StopIteration : Iterator에 의해 생성된 항목이 더이상 없다는 것을 알려주기 위해, 내장함수 next() 나 이터레이터의 _ _next__ 메소드가 일으킨다.

b : builtin