---
layout: single
title: "[Basic Python] Decorator"
categories: Basic_Python
tags:
  - Python
  - Decorator
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
# Decorator

데코레이터(Decorator)는 파이썬에서 함수나 메서드의 동작을 확장하거나 수정할 수 있는 강력한 기능 중 하나다. 데코레이터는 함수나 메서드를 다른 함수로 감싸서 그 함수의 동작을 변경하거나 보완하는 역할을 한다. 이를 통해 코드 재사용성과 가독성을 향상시킬 수 있다.

데코레이터를 정의하려면 `@` 기호를 사용하며, 함수의 정의 바로 위에 데코레이터를 적용한다. 다음은 데코레이터의 기본 구조와 사용법에 대한 예제다:

```python
def my_decorator(func):
    def wrapper():
        print("Something is happening before the function is called.")
        func()
        print("Something is happening after the function is called.")
    return wrapper

@my_decorator
def say_hello():
    print("Hello!")

say_hello()

```

위의 예제에서 `my_decorator`라는 데코레이터 함수를 정의하고, `wrapper` 함수를 내부에서 정의한다. `wrapper` 함수는 원래 함수인 `say_hello`를 호출하기 전과 후에 어떤 동작을 추가로 수행한다.     

`@my_decorator`라고 표시된 부분은 `say_hello` 함수에 `my_decorator` 데코레이터를 적용하는 것을 나타낸다. 따라서 `say_hello` 함수를 호출하면 `my_decorator` 내의 `wrapper` 함수가 실행되고, 이로 인해 

```
Something is happening before the function is called."
"Hello!"
"Something is happening after the function is called."
```

와 같은 추가 메시지가 출력된다.      

데코레이터를 사용하면 코드를 재사용하고 함수나 메서드의 동작을 확장할 수 있으므로, 예를 들어 로깅, 인증, 캐싱, 예외 처리 등과 같은 다양한 작업을 쉽게 추가할 수 있다. 파이썬에서는 내장된 데코레이터뿐만 아니라 사용자 지정 데코레이터를 정의하여 원하는 동작을 추가할 수 있다.

## 궁금해지는 부분 -> 클래스랑 무슨 큰 차이가 있는 걸까?

  
데코레이터와 클래스는 둘 다 파이썬에서 코드를 구성하고 동작을 변경하거나 보완하는 데 사용되는 다른 개념이며, 각각의 사용 사례와 특징이 있다.

1. **데코레이터 (Decorator):**
    - 데코레이터는 함수나 메서드에 적용되며, 함수를 감싸서 그 동작을 변경하거나 확장한다.
    - 주로 함수 레벨에서 사용되며, `@` 기호를 통해 함수 정의 위에 데코레이터를 적용한다.
    - 예를 들어, 로깅, 인증, 캐싱과 같은 함수에 적용되는 부가적인 기능을 제공하는 데 사용된다.
    - 함수나 메서드의 실행 전후에 추가 동작을 수행할 수 있다.

2. **클래스 (Class):**  
    - 클래스는 객체 지향 프로그래밍(OOP)의 주요 개념으로, 데이터와 메서드를 하나의 단위로 묶어서 캡슐화하고 객체를 생성할 때 사용된다.
    - 주로 데이터와 데이터를 처리하는 메서드를 묶어서 관리하며, 객체의 상태를 나타내고 조작하는 데 사용된다.
    - 클래스를 사용하여 복잡한 데이터 구조와 동작을 정의하고 추상화 할 수 있다.
    - 클래스는 인스턴스화하여 여러 객체를 생성하고 각각의 객체는 고유한 상태를 가질 수 있다.

간단한 예제로 설명하면, 데코레이터는 함수의 동작을 확장하거나 수정하는 데 사용되고, 클래스는 데이터와 해당 데이터를 조작하는 메서드를 함께 묶어 객체를 정의하는 데 사용된다. 이 둘은 다른 목적과 사용 사례를 가지고 있으며, 프로그램의 구조와 설계에 따라 어떤 것을 사용할지 결정된다.      
일반적으로 OOP 프로젝트에서는 클래스를 사용하고, 함수나 메서드의 부가 기능을 추가하려면 데코레이터를 사용하는 것이 일반적이다.      


### 데코레이터

```python
def log_function_call(func):
    def wrapper(*args, **kwargs):
        print(f"Calling {func.__name__} with arguments {args} and keyword arguments {kwargs}")
        result = func(*args, **kwargs)
        print(f"{func.__name__} returned {result}")
        return result
    return wrapper

@log_function_call
def add(a, b):
    return a + b

result = add(3, 5)

```

이 예제에서 `log_function_call` 데코레이터는 `add` 함수를 감싸고 있으며 함수가 호출될 때 로깅 메시지를 출력한다.       
`@log_function_call`을 사용하여 `add` 함수에 로깅 동작을 추가한다.

```
Calling add with arguments (3, 5) and keyword arguments {}
add returned 8
```
### 클래스

```python
class Calculator:
    def __init__(self):
        self.result = 0

    def add(self, a, b):
        self.result = a + b

    def subtract(self, a, b):
        self.result = a - b

calculator = Calculator()
calculator.add(3, 5)

```

이 예제에서 `Calculator` 클래스는 계산을 수행하는 객체를 정의한다.     
객체를 생성하고 `add` 메서드를 호출하여 결과를 계산한다.

**차이점:**
1. **데코레이터**는 함수나 메서드에 부가 기능을 추가하는 데 주로 사용된다. 위의 예제에서 `add` 함수에 로깅 기능을 추가한다.
2. **클래스**는 데이터와 해당 데이터를 처리하는 메서드를 묶어 객체를 정의하고, 객체의 상태를 나타내는 데 사용된다. 위의 예제에서 `Calculator` 클래스는 계산을 수행하는 객체를 생성하고 관리한다.
3. **데코레이터**는 함수 레벨에서 사용되고, 함수나 메서드의 동작을 수정하거나 확장한다.
4. **클래스**는 데이터와 메서드를 묶어 하나의 단위로 정의하며, 객체를 생성하고 해당 객체의 메서드를 호출하여 작업을 수행한다.

이 두 가지 개념은 다른 사용 사례와 목적을 가지고 있으며, 필요에 따라 각각을 적절하게 선택하여 사용해야 합니다.