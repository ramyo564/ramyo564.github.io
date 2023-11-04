---
layout: single
title: "[Basic Python] 내장함수 __str__, __repr__"
categories: Basic_Python
tags:
  - Python
  - __repr__
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
# 파이썬 내장함수 `__str__`,  `__repr__`?

/ `__str__` / `__repr__` / 

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def __str__(self):
        return '{0}: {1}'.format(self.name, self.age)

def main():
    p = Person('Jack',23)
    print(p)

main()
```
- ![](https://i.imgur.com/JWlBCoC.png)

- 위와같이 `__str__` 이 print로 출력하는 일반 문자열을 표기하는 방식이며 오버라이딩을해서 쓸 수도 있다.
- 반면 `__repr__`은 객체 생성방법을 알 수 있도록 하는 표준 문자열 표시방식이다.

## `__str__` 과 `__repr__` 의 차이점

str의 경우 흔히 아래와 같이 사용을 한다.

```python
a=3
print (type(a))
# <class 'int'>

str_a = str(a)
print (type(str_a))
# <class 'str'>
```

내장함수 `__str__` 은 아래와 같다.

```python
class Color():
    def __init__(self,name):
        self.name = name

red = Color('red')
yellow = Color('yellow')
black = Color('black')

arr = [red, yellow, black]

for ar in arr:
    print (ar)
    
<__main__.Color object at 0x00D663E8>
<__main__.Color object at 0x00D66448>
<__main__.Color object at 0x00D66478>
```

Color 함수를 정의하고 객체를 3개 만든후
이후 출력을 하였을 때 Color함수의 메모리 주소가 나온다.
함수에 __str__, __repr__을 정의하지 않았기 때문이다.

```python
class Color():
    def __init__(self,name):
        self.name = name
    
    def __str__(self):
        return f'__str__: {self.name=}'
    
    def __repr__(self):
        return f'__repr__: {self.name=}'

red = Color('red')
yellow = Color('yellow')
black = Color('black')

arr = [red, yellow, black]

for ar in arr:
    print (ar)
    
__str__: self.name='red'
__str__: self.name='yellow'
__str__: self.name='black
```

__str__ 함수와 __repr__함수를 추가한 뒤, 출력을 하였을 때 __str__의 함수가 호출되는 것을 확인할 수 있다.
만약 __repr__함수만 정의하였을 때는 아래와 같이 출력된다.

```python
__repr__: self.name='red'
__repr__: self.name='yellow'
__repr__: self.name='black'
```

위를 통해 출력을 할 때 __str__이 __repr__보다 우선순위가 높다는 것을 알 수 있다.
그리고 위에서 arr이라는 리스트에 각 객체들을 담았는데 아래와 같이 호출을 하면 __repr__의 함수가 호출된다.

```python
print(arr)

[__repr__: self.name='red', __repr__: self.name='yellow', __repr__: self.name='black']
```

만약 __repr__이 정의되지 않았다면 각 객체의 메모리주소가 출력된다.
위의 내용을 정리하면 클래스를 생성할 때 해당 클래스의 정보를 출력하기 위해 또는 표현하고자 하는 내용은
__str__과 __repr__을 통해 정의한다.

__str__의 목적은 **문자열화를 하여 서로 다른 객체 간의 정보를 전달하는 데** 사용하는 것이고
__repr__은 **인간이 이해할 수 있는 표현**으로 나타내기 위한 것이다.

즉 해당 클래스에 대해 사용자에게 정보를 전달하고 싶은 경우 __repr__을 사용하고
해당 클래스의 특정 데이터를 다른 객체들 간에 전달하고 싶은 경우 __str__을 사용하면 된다.


출처 : 
https://recordnb.tistory.com/47
[https://docs.python.org/ko/3.9/library/functions.html](https://docs.python.org/ko/3.9/library/functions.html#repr)
](https://docs.python.org/ko/3.9/library/functions.html#repr)