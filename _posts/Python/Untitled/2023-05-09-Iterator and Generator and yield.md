---

layout: single
title: "[Basic Python] Iterator and Generator and yield"
categories: Basic_Python
tag: [Python,"[Basic Python] Iterator and Generator and yield"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# 이터레이터와 제네레이너 이런건 도대체 뭘까?
/ 이터레이터 / 제네레이터 / yield /

## 이터레이터

>- 컬렉션 타입
>	-  list, tuple, set, dictionary와 같이 **여러개의 요소(객체)를 갖는 데이터 타입**
>- 시퀀스 타입
>	- - list, tuple, range, str등과 같이 **순서가 존재하는 데이터 타입**


### **Iterable VS Iterator**

**내부 요소(member)를 하나씩 리턴할 수 있는 객체를 보고 Iterable하다.** 쉽게 생각하면 우리가 평소에 많이 사용하는 **for문**을 떠올리면 된다.

```python
_list = [1,2,3,4,5]

for i in _list:
    print(i)
```

이렇게 **for문을 통해 순회할 수 있는 객체를 Iterable하다고 생각**하시면 된다. 대표적으로 위에서 잠깐 설명한 **시퀀스 타입**과 **컬렉션 타입**의 객체가 있다.

#### 그럼 Iterable한 것과 Iterator는 무슨 차이가 있는걸까?
- 쉽게 말하자면 **Iterable한 것****은 __next__ 메소드가** **존재하지 않고** **Iterator는 존재**한다고 생각하면 된다.
- 즉 **__next__ 메소드로 다음 값을 반환할 수 있으면 Iterator, 없으면 Iterabe한 객체**다.

더 자세한건 아래 링크
출처 : https://tibetsandfox.tistory.com/28


## 제네레이터

### **제너레터(Generator)란?**

- **제너레이터는 쉽게 설명해서** ***이터레이터(Iterator)****를 생성하는 객체**라고 할 수 있다. 즉 **모든 제너레이터는 이터레이터에 속한다.** 
- **컴프리헨션(Comprehension) 문법** 혹은 함수 내부에 ***yield*** 키워드를 사용함으로써 만들 수 있다.

#### **제너레이터 만들기**
우선 컴프리헨션 문법으로 만들어보자.

```python
foo = (i for i in range(1, 6))
print(type(foo)) # <class 'generator'>
```
- 이렇게 컴프리헨션 문법을 통해 생성된 제너레이터를 **제너레이터 표현식**이라고 칭한다.
	- []가 아니라 ()를 사용했음에 주의.

​ ***yield*** 키워드를 이용해 만들어보자.
```python
def func():
    for i in range(1, 6):
        yield i

foo2 = func()
print(type(foo2)) # <class 'generator'>
```
- 이렇게 생성된 foo와 foo2 모두 next 메소드를 통해 값에 접근할 수 있다.
- 또한 제너레이터는 이터레이터에 속하기 때문에 **불러올 값이 더 이상 없으면 StopIteration 예외를 발생**시킨다.

---

#### **yield 키워드**
- yield 키워드는 그럼 어떻게 동작하는걸까?
- 언뜻 보기엔 return과 비슷하게 동작하는 것 같지만 큰 차이가 있다.

``` python
def func():
    yield 1
    yield 2
    yield 3

foo = func()

print(next(foo)) # 1
print(next(foo)) # 2
print(next(foo)) # 3
```

- 제너레이터 함수는 yield를 통해 값을 리턴합니다.
- next메소드나 for문 순회 등을 통해 저 값들을 리턴받을 수 있는데 중요한 점은 **yield가 호출된다고 해서 함수가 종료되는 것이 아니라는 것**
- yield는 호출되면 **값을 반환하고 그 시점에서 함수를 잠시 정지**시킨다. 그리고 **다음 next가 호출되면 정지된 시점부터 다시 로직을 실행**한다.
- **return은 호출되면 함수를 종료**한다. yield와 return의 차이가 바로 이것

>- **제너레이터 내부에서 return을 사용 시 현재 값에 상관없이 무조건 StopIteration 예외가 발생**

```python
def func():
    print("1 리턴 전")
    yield 1
    print("1 리턴 후, 2 리턴 전")
    yield 2
    print("2 리턴 후, 3 리턴 전")
    yield 3
    print("3 리턴 후")


foo = func()

print(next(foo))
print(next(foo))
print(next(foo))
```

이해를 돕기 위해 각 yield 사이에 print문을 삽입

```python
1 리턴 전
1
1 리턴 후, 2 리턴 전
2
2 리턴 후, 3 리턴 전
3
```


이렇게 **제너레이터 내에 다양한 로직을 추가할 수 있다는 점**에서 yield 는 유용하다.


## 제너레이터의 체크포인트

---

### **yield from**
- yield로 **iterable한 객체의 요소를 하나씩 리턴**해 줄 수도 있다.
- 이 경우 yield에 더해 **from**이라는 키워드를 사용한다.
```python
def func():
    _list = [1, 2, 3, 4, 5]
    for i in _list:
        yield i
```

즉 위와 같이 리스트를 순회하며 각각 값을 yield로 반환하는 대신,

```python
def func():
    _list = [1, 2, 3, 4, 5]
    yield from _list
```
이렇게 **for문 없이 yield로 반환**하는 것도 가능

사용법은 위의 예제나 아래 예제나 같음

```python
def generator(stop_number):
    num = 0
    while True:
        if num >= stop_number:
            return
        num += 2
        yield num


def func(stop_number):
    gen = generator(stop_number)
    yield from gen


foo = func(10)
```

추가로 이렇게 **yield from 키워드로 제너레이터를 전달하는 것도 가능**

위 예제는 10 이하의 짝수를 foo 객체의 next로 하나씩 꺼내올 수 있다.

---

## **게으른 제너레이터**

제너레이터는 lazy하다. 이 특성때문에 제너레이터는 **지연 평가(Lazy** **Evaluation)** **구현체** 라고 부르기도 함

lazy 하지 않은 객체, 가장 대표적으로 list를 예로 든다면

```python
_list = [1,2,3,4,5]
```

- **list객체의 각 요소는 이미 값이 정해져 있다.**

0번 인덱스 : 1

1번 인덱스 : 2

2번 인덱스 : 3 ... 이런식

**이런 객체는 몇 번째 어떤 요소가 있는지 손쉽게 확인할 수 있다는 장점**이 있지만 그 **크기가 커질 수록 메모리의 낭비가 심해진다는 단점**이 있다.

만약 list의 길이가 100만이 넘는다면? 100만개가 넘는 값을 모두 기억해야 하니 메모리가 많이 필요하다.

>- 하지만 **lazy한 객체는 모든 요소를 저장하고 있지 않고**
>- **처음에 줘야 할 값, 그 다음부터 줘야 할 값, 값을 언제까지 줘야 하는지 에 대한 정보**만을 가지고 있다.
>	- 이 정보들을 바탕으로 각 호출마다 값을 반환하게 되는 것

정리하자면 모든 값을 메모리에 올려두지 않고 필요할 때(호출할 때) 값을 평가해서 반환하는 방식

이 특성을 이용한 재미있는 예시를 하나 보자

```python
def endless_numbers():
    num = 2
    while True:
        yield num
        num = num + 2


foo = endless_numbers()
```

처음 반환할 값 : 2 (num의 초기값)

그 다음부터 줄 값 : num에 2를 더한 값

언제까지? : 명시되지 않았으므로 호출하는 만큼 계속

​

이렇게 생성된 foo 객체는 **호출하는 만큼 짝수를 계속해서 반환**
그리고 이 횟수는 **이론상으로 100만, 1000만, 혹은 그 이상도 가능**

같은 로직을 list로 구현하는것은 당연히 불가능
이것이 **lazy한 객체의 특징이자 장점**

따라서 **불러올 다음 값에 대한 패턴이 존재한다면 lazy한 객체를 사용하는 것이 메모리 측면에서는 이득이라고 할 수 있다.**

더 자세한건 아래 링크
출처 : https://tibetsandfox.tistory.com/28