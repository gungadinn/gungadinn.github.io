---
layout: post
title: "Python - 데코레이터"
date: 2019-07-05
tags: [python]
comments: true
---

## 빅데이터 청년인재 Day 5

---

### 목차

* type()의 기능 3가지
  * type() == \__class__
  * 1) 클래스 이름을 알고 싶을 때
  * 2) metaclass 만들 때
  * 3) 메타클래스 자체로 사용할 때
* 상속
  * 단일 상속
    * 상속의 장점 3가지
  * 다중 상속
* composition(합성)
* 다형성
* 참고
  * 파이썬에서 클래스는 객체이다.
  * Singleton 패턴

---

<br>

## 1. Type()의 기능 3가지

---

`class type(object) ` : object의 형(type)을 돌려준다. 반환 값은 [형 객체](https://docs.python.org/ko/3/library/stdtypes.html#bltin-type-objects). 즉 객체면 클래스의 이름을 반환하고, 클래스면 type을 반환한다. 간단히 말해 객체가 어떤 타입인지 알 수 있게 해준다.

`class type(name, bases, dict)` : 새 형(type) 객체를 돌려준다. __name__ 문자열은 클래스 이름, __bases__ 튜플은 베이스 클래스들을 항목화하고, __dict__ 딕셔너리는 클래스 바디의 정의들이 들어있는 이름 공간이다.

<br>

`type` 은 기본적으로 파이썬에서 제공하는 행동이 아닌 클래스의 행동을 바꿀 수 있는 클래스이다. 예를 들어, 기본적으로 파이썬 클래스는 객체를 수없이 만들 수 있는데 메타클래스에서 이 행동을 제어할 수 있다. '이 클래스는 한개만 만들어라' '만들 때 특정 값만 가져라' 등

<br>

#### Type() == \__class__

`instance.__class` : 클래스 인스턴스가 속한 클래스

```python
>>> a.__class__   
<class '__main__.A'>    # 현재 정의된 파일의 경우 main 출력
```

<br>

```python
>>> b.__class__   
<class '__main__.A'>

>>> type(b)   
<class '__main__.A'>
```

즉 type() 대신에 \__class__를 쓸 수 있다.

<br>

### 1) 클래스 이름을 알고 싶을 때

```python
>>> class A:
        a = 1    # 클래스 변수
        def x(self):
            return a   # 인스턴스 변수?

>>> a = A()
>>> a.x()   
NameError
```

> NameError
>
> 그림 5-01

<br>

```python
>>> class A:
        a = 1    # 클래스 변수
        def x(self):
            return A.a   # 인스턴스 변수

>>> a = A()
>>> a.x()   
1
```

 인스턴스 변수가 클래스 변수에 접근하기 위해서는 클래스를 지정해야한다.

<br>

```python
>>> class A:
        a = 1
        def x(self):
            print self
            return self.a
            
>>> b = A()
>>> b.x()
1
```



```python
>>> class A:
        a = 1    # 클래스 변수
        def x(self):
            return type(self).a   # 인스턴스 변수

>>> b = A()
>>> b.x()
1
```

<br>

```python
>>> class A(object):
        a = 1    # 클래스 변수
        def x(self):
            return self.__class__.a   # 인스턴스 변수

>>> b = A()
>>> b.x()   
1
```

type()과 \__class__는 일반적으로 같기 때문에 위와 같이 쓸 수 있다.

<br>

*  `isinstance(object, classinfo)` : 서브 클래스를 고려하기 때문에 객체의 형을 검사하는데 권장되는 내장함수. __간단히 생각해서 인스턴스의 형을 검사할 때 사용한다.__ __object__ 인자가 __classinfo__ 인자 혹은 그것의 서브클래스의 인스턴스면 True 반환한다. object가 주어진 형의 객체가 아니면 함수는 항상 False를 반환한다. 

  __object__가 그 형 중 어느 하나의 인스턴스일 때 True를 반환한다. __classinfo__가 형이나, 형들의 튜플이나, 이런 튜플들의 튜플이 아니면 TypeError 예외를 일으킨다.

```python
>>> isinstance(1, int)
True

>>> isinstance(1, str)
False

# classinfo가 (int, str)인 튜플로 주어졌지만 1은 int의 인스턴스이므로 True. 반한
>>> isinstance(1, (int,str, float))
True

# classinfo의 인자 혹은 그것의 서브클래스의 인스턴스인 경우
>>> isinstance(True, bool)
True

>>> isinstance(True, int)   # bool은 int의 서브클래스
True 
```

* `issubclass (class, classinfo)` : class가 classinfo의 서브 클래스면 True를 반환한다. 클래스는 그 자체의 서브클래스로 간주하고 classinfo는 클래스 객체의 튜플일 수 있다. __간단히 생각해서 클래스 상속을 검사할 때 사용한다.__

```python
>>> issubclass(A, object)
True

>>> issubclass(bool, int)
True

>>> issubclass(float, int)
False
```

<br>

```python
# !주의! 아래에서 보는 것처럼 float, int, type은 클래스이다.
>>> type(float)
<class 'type'>

>>> type(int)
<class 'type'>

>>> type(type)
<class 'type'>

>>> a = 1   # int(클래스 이름)형의 객체를 만들었다
>>> type(a)   # type에 객체를 넣었으니 클래스 이름 반환
<class 'int'>
```

<br>

### 2) metaclass 만들 때

__metaclass__ : 클래스의 클래스. 클래스의 행동을 바꿀 수 있는 클래스. 객체를 만드는 '무언가'. 

파이썬에서 클래스를 만들 때 클래스를 선언하지 않아도 type()을 이용하여 클래스를 만들 수 있다. type()은 metaclass이기 때문이다.

<br>

`metaclass`를 만드는 과정은 클래스 정의 줄에 `metaclass` 키워드를 전달하거나, 그런 인자를 포함한 이미 존재하는 클래스를 상속함으로써 만들 수 있다.

```python
# 메타클래스를 만들려면 type을 상속받는다.
>>> class Meta(type):
        pass

# 클래스 정의줄에 metaclass 키워드 전달
>>> class MyClass(metaclass=Meta):
        pass
 
# 그런 인자를 포함한 이미 존재하는 클래스를 상속
>>> class MySubClass(MyClass):
        pass
```

<br>

새로운 메타클래스를 만드는 과정을 쉽게 쓰자면

1. 타입을 상속받는다.

   메타클래스를 상속하면 메타클래스는 기본적으로 타입을 상속한다

2. 그 메타클래스를 적용할 클래스에 (metaclass = 내가만든메타클래스)를 이용하여 새로운 클래스 정의한다.

   cf) 기본적으로 클래스를 상속할 때 object가 생략되면 object를 상속받는다. 이 때, 옵션으로 `,metaclass`가 있는데 `metaclass` 를 상속받으면 기본적으로 `metaclass`가 `type` 을 사용한다. 그래서 `metaclass=내가 만든 메타클래스`

예시는 아래와 같다.

```python
>>> type(int)
<class 'type'>

>>> a = type('Moon', (int,), {}) # int(type) 객체를 상속 받는다.
>>> type(a)
<class 'type'>

>>> a(3)   # int 형의 객체를 만들었다
3

>>> type(a(3))
<class '__main__.Moon'>

>>> x = a(3)
>>> type(x)   # class Moon을 만들어준다
<class '__main__.Moon'>
```

<br>

__참고__ : Singleton 패턴



### 3) 메타클래스 자체로 사용할 때

1. enum
2. 로깅
3. 인터페이스 검사
4. 자동화된 위임 (automatic delegation)
5. 자동화된 프로퍼티(property) 생성
6. 프락시(proxy)

 등

<br>

<br>

## 2. 상속

---

파이썬은 다중상속 언어이다. 동시에 여러 개를 상속 받을 수 있으니까 상속 순서가 중요하며 상속 받은 클래스들은 \__base__ 와 \__bases__ 를 통해 할 수 있다.

`class.__bases__` : 클래스 객체의 베이스 클래스들의 튜플

`class.__base__` : 클래스 객체의 베이스 클래스들 튜플 중 제일 처음

```python
>>> a = type(x, (int,), {})
>>> a.__bases__
(int,) 

>>> a.__base__
int
```

<br>

### 1) 단일상속

한 개 상속 받는 것을 단일상속이라고 한다.

```python
>>> class A(object):
        pass
    
>>> type(object)   
type

>>> type(A)
type
```

<br>

```python
>>> class A:
        a = 1
        def x(self):
            return 1
    
>>> class B(A):
        a = 1
        def x(self):
            return 2
      
>>> b = B()
>>> b.x()    # 뒤에 인자를 안 봐서   
TypeError

>>> b.x(3)
3
```

> TypeError : 파이썬에서는 이름만 같으면 같은 메소드로 보기 때문에 안된다. (이름만 찾기 때문에 인자 조심할 것!)
>
>  그림 5-03

<br>

#### 상속의 장점 3가지

(1) 부모에 정의되어 있는 걸 그대로 가져다 사용

```python
>>> class A:
        a = 1
        def x(self):
            return 1
    
>>> class B(A):
        a = 2
        def x(self,a):
            return A.a   # class A의 a를 가져온다
        
>>> b = B()
>>> b.x(3)
1
```



```PYTHON
>>> class A:
        a = 1
        def x(self):
            return 1
    
>>> class B(A):
        a = 2
        def x(self,a):
            return B.a   # class B의 a를 가져온다
        
>>> b = B()
>>> b.x(3)
2
```



```python
>>> class A:
        a = 1
        def x(self):
              return 1
    
>>> class B(A):
        a = 1
        def x(self, a):
            return self.__class.__base__.a   # class B의 a를 가져온다
```



(2) 부모에 정의되어 있는 걸 완전히 바꾸기





(3) 부모의 일부만 가져오기 :  super()

```python
>>> class A:
        a = 1
        def x(self):
              return 1
    
>>> class B(A):
        a = 4
        def x(self,a):
            return super().x()   # super == 부모객체
      
>>> b = B()
>>> b.x(3)
1
```

<br>

그러면 부모객체이면 아래와 같이 쓰면 안될까?

```python
>>> class A:
        a = 1
        def x(self):
            return 1
    
>>> class B(A):
        a = 1
        def x(self,a):
            return A.x()   
```

정답은 안 되는게

```python
>>> b = B()
>>> b.x(3)   
TypeError
```

>안 되는 이유가 A는 클래스인데 A.x()는 인스턴스 메소드이므로 인스턴스화 시켜야한다. 그런데 매번 인스턴스화 할 수 없으므로 syntax를 추가하는데 이 때 사용되는 syntax가 super()이다. super()를 쓰면 인스턴스로 반환한다.
>
>TypeError : x() missing 1 required positional argument: 'self'

<br>

```python
# 아래처럼 응용도 가능하다.
>>> class A:
        a = 1
        def x(self, a):
            return a
    
>>> class B(A):
        a = 1
        def x(self):
            return super().x(A.a)

>>> b = B()
>>> b.x()
1
```

<br>



### 2) 다중상속

```python
# 다이아몬드 상속 문제
>>> class A:
        def __init__(self):
            print(A)
    
>>> class B(A):
        def __init__(self):
            print(B)
        
>>> class C(A):
        def __init__(self):
            print(C)
        
>>> class D(A,B):
        def __init__(self):
            print(D)
```

> __다이아몬드 상속문제__
>
> TypeError : 똑같은 메소드가 나왔을 때 (실행순서를 알 수 없을 때)
>

> MRO (Method Resolution Order) : (클래스 메소드) 파이썬에서 메소드 실행순서를 알 수 없을 때 `__mro__` 를 통해 알 수 있다.



```python
## 다중상속
>>> class A:
        def __init__(self):
            print(A)
    
>>> class B(A):
        def __init__(self):
            print(B)
        
>>> class C(A):
        def __init__(self):
            print(C)
        
>>> class D(B,C):
        def __init__(self):
            print(D)
        
>>> D.mro()
[__main__.D, __main__.C, __main__.B, __main__.A, object]

>>> D.__mro__
(__main__.D, __main__.C, __main__.B, __main__.A, object)
```

<br>

그렇다면 다중상속 순서에 따라서 어떻게 실행될까

```python
>>> class A:
        def __init__(self):
            print(A)
    
>>> class B(A):
        def __init__(self):
            print(B)
        
>>> class C(A):
        def __init__(self):
            print(C)
        
>>> class D(C,B):
        def __init__(self):
            B.__init__(self)
            C.__init__(self)
            print(D)
            
>>> d = D()
<class '__main__.B'>   # 클래스 D의 B.__init__(self)에 걸려서 print(B) 실행
<class '__main__.C'>   # 클래스 D의 C.__init__(self)에 걸려서 print(C) 실행
<class '__main__.D'>   # print(D) 실행
```





```python
>>> class A:
        def __init__(self):
            print(A)
    
>>> class B(A):
        def __init__(self):
            print(B)
            A.__init__(self)
        
        
>>> class C(A):
        def __init__(self):
            print(C)
            A.__init__(self)
        
        
>>> class D(C,B):
        def __init__(self):
            B.__init__(self)
            C.__init__(self)
            print(D)
            
>>> d = D()
<class '__main__.B'>   # 클래스 D의 B.__init__(self)에 걸려서 넘어간 후 print(B) 실행
<class '__main__.A'>   # 클래스 B의 A.__init__(self)에 걸려서 넘어간 후 print(A) 실행
<class '__main__.C'>   # 클래스 D의 C.__init__(self)에 걸려서 넘어간 후 print(C) 실행
<class '__main__.A'>   # 클래스 C의 A.__init__(self)에 걸려서 넘어간 후 print(A) 실행
<class '__main__.D'>   # 클래스 D의 print(B) 실행
```

위와 같은 코드처럼 class를 콕 찝어서 수행하는 것은 중복이 있을 수 있어 안 좋다. (ex. 초기화 할 때) 또한 여러번 실행되는 문제가 있는데 이는 super를 사용하면 파이썬이 내부적으로 두 번 실행하는걸 막아준다. 게다가 한번만 쓰면 되니까 코드의 양도 줄일 수 있다. 

```python
# super는 알아서 하는 거기 때문에
>>> class A:
        def __init__(self):
            print(A)
    
>>> class B(A):
        def __init__(self):
            super().__init__()
            print(B)
        
        
>>> class C(A):
        def __init__(self):
            super().__init__()
            print(C)
        
        
>>> class D(C,B):
        def __init__(self):
            super().__init__()  # 인스턴스!
            print(D)
        
>>> d = D()
<class '__main__.A'>
<class '__main__.B'>
<class '__main__.C'>
<class '__main__.D'>
```

<br>

## 3. Composition(합성)

---





## 4. 다형성

---

<br>

## 참고

---

### 1) 파이썬에서 클래스는 객체이다.

왜냐하면 클래스를 만들어주는 또 다른 클래스가 있기 때문이다.

```python
# object는 최상위 클래스에서 상속받는다.
>>> class MyClass(object):
        pass
```

위의 코드에서 `class` 키워드를 사용할 때 python은 실행하면서 객체를 만들어낸다. 

이 객체(클래스)는 그 자체로 새로운 객체(인스턴스)를 만들 수 있다. 이것이 파이썬에서 클래스가 객체인 이유이다. 위에서 말한 메타클래스로 이야기 했을 때 메타클래스는 클래스를 만드는 클래스이므로 클래스는 메타클래스 입장에서는 객체이다.

 <br>

`object` : 파이썬에서 기본적인 클래스

`type` : 파이썬에서 기본적인 메타클래스

`type(객체)` : 클래스 이름

`type(클래스 이름)` : type

타입은 상속의 형태로 내 마음대로 만들 수 있다. 그러니 타입은 클래스이면서 객체이고 이것이 파이썬의 모든 것이 객체인 이유다. 그래서 파이썬은 순수한 객체지향언어이다.

<br>

### 2) Singleton 패턴

```python
# type을 상속받는 새로운 메타클래스를 만든다.
>>> class Singleton(type):
        instance = None
        def __call__(cls, *args, **kw):
            if not cls.instance:
                cls.instance = super().__call__(*args, **kw)
            return cls.instance
      
# 내가 만든 메타클래스(Singleton)이라는 클래스를 통해 ASingleton이라는 메타클래스를 만들었다.
>>> class ASingleton(metaclass=Singleton):
        pass
```

메타클래스로 싱글톤을 만든다.

```python
>>> a = ASingleton()   # 인스턴스화
>>> b = ASingleton()   # 인스턴스화

>> a is b
True

>>> id(a)
4609520920
>>> id(b)
4609520920
```

싱글톤은 값을 만들면 공유하고 그렇기 때문에 메모리 주소가 같다.

이처럼 클래스 자체의 기능을 바꿔버리는 것이 `metaclass` 이다.

<br>

## 기타

---

### 1) \__dir__과 dir()의 차이점

```python
>>> class A(object):
        pass
>>> a = A()
>>> a.__dir__()
['__module__', '__dict__', '__weakref__', '__doc__', '__repr__', '__hash__', '__str__', '__getattribute__', '__setattr__', '__delattr__', '__lt__', '__le__', '__eq__', '__ne__', '__gt__', '__ge__', '__init__', '__new__', '__reduce_ex__', '__reduce__', '__subclasshook__', '__init_subclass__', '__format__', '__sizeof__', '__dir__', '__class__']

>>> dir(a)
['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__']
```

하는 행동은 같으나 보여주는게 다르다. 함수로 사용하면 정렬이 되고 메소드로 사용하면 정렬이 안 되어있다.

```python
>>> class A(object):
        pass
>>> a = A()

>>> dir(a)
>>> dir(A)
# 출력 같게 나옴

>>> a.__dir__()
>>> A.__dir__()
TypeError
```

> 함수 dir()은 클래스와 인스턴스에 전부 사용 가능하나 \__dir__은 인스턴스에만 사용할 수 있다.

>TypeError
>
>그림 5-02

<br>

### 2) print 할 때와 안 할 때

```python
class B:
    # 갈래적으로 low 포맷을 사용 (\n 안된다)
    def __repr__(self):
        return '레골라스'
      
    # 
    def __str__(self):
        return '레골라스2'
        
b = B()
b   # output : 레골라스
print(b)    # 레골라스2
```







```python
class A:
    def __init__(self):
        print('1')
        print(A)
    
class B(A):
    def __init__(self):
        print('2')
        super().__init__()
        print(B)
        
class C(A):
    def __init__(self):
        print('3')
#        super().__init__()
        print(C)
        
class D(C, B):
    def __init__(self):
        print('4')
        super().__init__()  # 인스턴스!
        print(D)
        

# 셀프 질문
```



<br>

##  objects 와 types

---

```python
>>> data = (13, 63, 5, 378, 58, 40)

>>> def avg(d):
        return sum(d)/len(d)
    
>>> avg(data)
92.833333333333333
```

 이 함수는 나열된 데이터에서 작동하여 시퀀스 항목의 평균을 반환한다. 매우 좋지만, 우리는 데이터 및 절차를 별도로 유지하면서 관리 할 필요가 있다. 

```python
>>> door1 = [1, 'closed']
>>> door2 = [2, 'closed']

>>> def open_door(door):
        door[1]='open'
    
>>> open_door(door1)
door1
```

위와 같이 두 개의 문을 만들고 문을 여는 함수를 정의하였다. 그렇다면 잠겨있지 않은 상태에서만 열 수 있는 "잠글 수 있는 문"은 어떻게 만들까?

```python
>>> door1 = [1, 'closed']
>>> door2 = [1, 'closed']

>>> ldoor1 = [1, 'closed', 'unlocked']

>>> def open_door(door):
         door[1] = 'open'
    
>>> def open_ldoor(door):
        if door[2] == 'unlocked':
            door[1] = 'open'
        
>>> open_door(door1)
>>> door1
[1, 'open']

>>> open_ldoor(ldoor1)
>>> print(ldoor1)
[1, 'open', 'unlocked']
```

문을 여는 행위 자체는 동일한데 왜 open_door() 대신에 open_ldoor()를 사용해서 잠긴 문을 열어야할까? 데이터와 절차 간의 이러한 분리가 일부 상황에 완벽하게 맞지 않을 가능성이 있다. 중요한 것은 "open" 동작이 실제로 문을 사용하지 않고 그것의 상태를 변화시키는 것이다.  "open" 프로시저의 "door" 데이터 개념에 충실해야한다.

