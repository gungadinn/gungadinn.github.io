---
layout: post
title: "Python (4) - 복사, 데코레이터"
date: 2019-07-04
tags: [python]
comments: true
---

---

## 빅데이터 청년인재 Day 4

---

### 목차

- 복사
- 데코레이터
- 클래스

------

<br>

## 복사

---

mutable일 경우 같이 바뀌기 때문에 주의해야한다.

```python
>>> a = b = [1, 2, 3]
>>> c=b.append(4)
>>> a
[1, 2, 3, 4]

>>> b
[1, 2, 3, 4]

>>> c
# append는 return None이니까 조심

>>> d = c[:]
TypeError
# c가 none이라 에러 발생
```

```python
>>> d = b[:]
>>> d
[1, 2, 3, 4]

>>> d.append(5)
>>> c
>>> b
[1, 2, 3, 4]
>>> a
[1, 2, 3, 4]
>>> d
[1, 2, 3, 4, 5]

>>> e=b
>>> e
[1, 2, 3, 4]
>>> e.append(5)
>>> e
[1, 2,3 , 4, 5]
>>> b
[1, 2, 3, 4, 5]
```

- `d = b[:]` 와 `e=b` 의 차이점

  -  `d=b[:]` 후 `d.append(5)` 를 하고 안 변하는 이유

    :는 얕은 복사 기법. 새로운 객체를 만들기 때문에 id가 다르다. 참조가 아님

  - `e=b` 는 id가 같다. 참조가 되어 바꿀 때 같이 바뀐다. 따라서 같이 바꾸고 싶으면 : 를 해야한다.

```python
>>> a = b = [1, 2, 3]
>>> c = b.append(4)
>>> d = b[::-1]
>>> d
[4, 3, 2, 1]
>>> d.append(5)
>>> d
[4, 3, 2, 1, 5]
>>> b
[1, 2, 3, 4]
```

<br>

### 중첩

```python
>>> a=[[1,2,3,],[4,5,6]]
>>> b = a

# b를 바꾸면 a까지 바꾼다. 카피가 아니라 참조이기 때문이다.
>>> b[0][0]='레골라스'
>>> a
[['문근영', 2, 3], [4, 5, 6]]
>>> b
[['문근영', 2, 3], [4, 5, 6]]

# [:]은 깊은 부분 (중첩된 부분)까지 카피하지 못한다.
>>> c = a[:]
>>> c
[['문근영', 2, 3], [4, 5, 6]]
>>> c[0]='1'
>>> c
['1', [4, 5, 6]]
>>> a
[['문근영', 2, 3], [4, 5, 6]]

>>> c[1][0]='레골라스'
>>> c
['1', ['레골라스', 5, 6]]
>>> a
[['문근영', 2, 3], ['레골라스', 5, 6]]
```

```python
>>> x = [1, 2, 3]
>>> y = x[:]
>>> x
[1, 2, 3]

>>> x[0]='레골라스'
>>> x
['레골라스', 2, 3]
>>> y
[1, 2, 3]
```

### import copy

```python
>>> import copy
# copy.deepcopy  # 깊은 복사
# copy.copy   # 얕은 복사

>>> z = copy.copy(y)
>>> x
['레골라스', 2, 3]
>>> z
[1, 2, 3]

>>> z = copy.deepcopy(y)
>>> x
['레골라스', 2, 3]
>>> z
[1, 2, 3]
```





<br>

## 데코레이터

---

다른 함수를 돌려주는 함수. 함수 또는 클래스의 기능을 바꿔주는 함수. 그렇지만 보통 기능을 바꿔줄 때 사용하기 보다는 기능을 추가할 때 사용한다.

ex) 프로그램을 작성하였는데 시간이 오래걸려 끝나는 시간을 측정하고 싶다. 그럴 경우 원래 함수는 그대로 두고 시간 측정하는 부분을 새로 만들면 된다.

<br>

##### 데코레이터 만드는 규칙

1. 겉에 있는 함수와 안에 있는 함수를 만든다

   ```python
   def hi(sa):
       print(sa)
       
   def wrapper(func):
       def inner():
   ```

2. 원래 있는 함수 만든다

   ```python
   def hi(sa):
       print(sa)
       
   def wrapper(func():
       def inner():
           func()
       return inner
   ```

3. 실행하기 위해 function을 만들어야함

   ```python
   def hi(sa):
       print(sa)
       
   def wrapper(func):
       def inner():
           func()
       return inner
       
   wrapper(hi)()
   # output : TypeError
   # hi는 인자를 받아야하므로 인자를 넣어준다
   ```

4. 사용할 함수의 인자를 맞춰준다

   ```python
   def hi(sa):
       print(sa)
       
   def wrapper(func):
       def inner(x):
           func(x+1)
           print('--------------')
       return inner
       
   wrapper(hi)(4)
   # output
# 5
   # -------------
   ```
   
   

함수를 첫번째 인자로 받고 안에 있는 것을 함수로 실행시키고 조작하는게 핵심이다.

예)

```python
>>> @wrapper
>>> def hi(x):
        print(x)
    
>>> hi(3)
3
---------

>>> def hi(sa):
        print(sa)
    
>>> def wrapper(func):
        def inner(x):
            return func(x+1)
        return inner
  
>>> hi(3)
4
```

<br>

## 클래스

---

객체는 보통 크래스의 인스턴스들. 클래스 내부에서 정의해야 할 것은 크게 두가지인데 1) 어트리뷰트와 2) 메소드이다.

클래스 = 클래스 변수 + 클래스 메소드 + 인스턴스 변수 + 인스턴스 메소드

```python
>>> class Moon:
        a = 1
        def b(self):
            return 2
        
>>> a = Moon()
>>> a.b()
2
```



* 클래스 변수 : 클래스에서 접근 할 수 있는 변수

```python
>>> class Moon:
        a = 1    # 클래스 변수
>>> Moon.a
1

>>> callable(Moon)
True
```

* 인스턴스 변수 : 인스턴스가 가지는 변수

```python
>>> a = Moon()
>>> a.a
1
```

위의 코드에서는 인스턴스 변수가 없다. `인스턴스.변수` 를 접근했을 때 없다면  `클래스.변수` 를 찾는다.

```python
>>> class Moon:
        a = 1 #클래스 변수
        def x(self):
            self.a = 3 #인스턴스 변수
        
>>> a = Moon()
>>> a.a
1

>>> a.x()
>>> a.a
3
```

 첫 번째 `a.a` 의 결과가 1인 이유는 아직 `x()` 를 실행 안 했기 때문에 값이 없어서이다.

```python
>>> class A:
        a = 1
        def set_a(self, a):
            self.a = a
        
>>> a = A()   # 인스턴스화
>>> a.set_a(3)
>>> a.a
3

>>> b = A()    # 인스턴스화
>>> b.a        # 인스턴스 변수가 없어서 클래스 변수를 찾는다
1

>>> A.a = 9    # 파이썬에서는 클래스 변수를 클래스 밖에서 바꿀 수 있다
>>> b.a        # 인스턴스가 없으니 클래스 안에서 찾는다. 그래서 바뀜
9

# 아래와 같은 방법을 몽키패치
>>> A.xx = 2
>>> b.xx
2

>>> a.xx
2
```

* 몽키패치 : 몽키패치를 통해 쉽게 사용할 수 있지만 유지보수가 힘들다.
* 내부적으로 인스턴스 변수가 없으면 클래스 변수를 찾는다
* `.xx` 가 된다는 것은 __캡슐화__가 안 되었다는 것이다. 즉, 함수는 캡슐화가 되어있지만 클래스에서는 안 되어있다.

파이썬에는 private 개념이 없지만 갈래상 아래와 같이 쓸 수 있다.

```python
>>> class A:
        a = 1
        __b = 1
        
>>> A.__b
AttributeError
```

<center>
  <figure>
    <img src="/img/post/python/04-03.png"
  </figure>
</center>

갈래상 _변수면 private으로 쓰겠다.

```python
>>> class A:
        a = 1
        _b = 1

>>> A._b
1
```

<br>

```python
>>> class A:
        def x(self):
            self.a = 1
        
>>> a = A()
>>> dir(a)
[...
'x']

>>> a.x()
>>> dir(a)
[...
'a'
'x']
```

인스턴스 변수는 인스턴스에 따라서 생긴다. 클래스 변수는 클래스 하나에서 전체가 값을 공유하는데 인스턴스는 인스턴스마다 고유의 값을 가진다.

```python
>>> class A:
        def x(self, a):
            self.a = a
        
>>> a = A()
>>> b = A()

>>> vars(a)
{'a': 3}

>>> vars(b)
{'a': 4}

>>> b.__dict__
{'a': 4}

>>> A.x()
TypeError

# 클래스도 인스턴스 메소드 사용 할 수 있다
>>> A.x(a,6)
>>> vars(a)
{'a': 6}

>>> a.x
<bound method A.x of <__main__.A object at 0x1126120b8>>

>>> a.x(7)
>>> vars(a)
{'a': 7}
```

<center>
  <figure>
    <img src="/img/post/python/04-04.png">
  </figure>
</center>

a, b를 보다시피 인스턴스는 인스턴스마다 고유의 값을 가질 수 있다.

`vars()` 는 인스턴스 변수와 값에 대해서 딕셔너리 형태로 보여주는 명령어이다.

```python
>>> class A:
        @classmethod
        def x(cls,a):
            cls.a = a
        
>>> A.x(3)
>>> A.a
3
```



<br>

## 기타

------

### 1) 역순 출력 방법

#### (1)  reversed

```python
>>> a=reversed([1,2,3,4])
>>> list(a)
[4, 3, 2, 1]
```

#### (2) a[::-1]

```python
>>> a = [1, 2, 3, 4]
>>> a[::-1]
[4, 3, 2, 1]
```

<br>

### 2) 디버깅

1. `%pdb` : 파이썬에서 에러가 발생했을 때 디버깅 해주는 애