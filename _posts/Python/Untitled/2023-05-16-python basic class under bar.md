---

layout: single
title: "[Basic Python] 파이썬 클래스 & __ & Mangling 맹글링"
categories: Basic_Python
tag: [Python,"[Basic Python] 파이썬 클래스 & __ & Mangling 맹글링"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
## **파이썬의 클래스**

사실 C언어처럼 파이썬은 굳이 클래스가 없어도 프로그램을 충분히 만들 수 있다고 한다.
그리고 파이썬은 C로 만들어져 있어서 어차피 파이썬을 잘 쓰려면 결국 C를 배워야 한다.

-   아무튼 근데 왜 사용할까?
	- 재활용하기 편하려고!

---

```python
# order.py

customer1 = 0

def order(price):
  global customer1
  customer1 += price
  return customer1


print(order(3000))
print(order(5000))
```

```python
# 결과

3000
8000
```

고객의 누적 주문금액을 반환하는 order()라는 함수가 있다. 먼저 고객이 1명일때는 이렇게 함수를 1개만 구현하면 되겠죠. 근데 고객이 3명이라면?

```python
# order.py

customer1 = 0
customer2 = 0
customer3 = 0


def order1(price):
  global customer1
  customer1 += price
  return customer1

def order2(price):
  global customer2
  customer2 += price
  return customer2

def order3(price):
  global customer3
  customer3 += price
  return customer3


print('customer1의 총 주문금액 : ', order1(3000))
print('customer1의 총 주문금액 : ', order1(4000))
print()
print('customer2의 총 주문금액 : ', order2(1000))
print('customer2의 총 주문금액 : ', order2(2000))
print()
print('customer3의 총 주문금액 : ', order3(3000))
print('customer3의 총 주문금액 : ', order3(8000))
```

```python
# 결과
customer1의 총 주문금액 :  3000
customer1의 총 주문금액 :  7000

customer2의 총 주문금액 :  1000
customer2의 총 주문금액 :  3000

customer3의 총 주문금액 :  3000
customer3의 총 주문금액 :  11000
```

똑같은 일을 하는 함수를 3개 만들었다. 이를 클래스를 통해 1번만 만들어줄 수 있어요.

```python
# order.py

class Order:
    def __init__(self):
        self.customer = 0

    def order(self, price):
        self.customer += price
        return self.customer

customer1 = Order()
customer2 = Order()

print(customer1.order(14000))
print(customer1.order(5000))
print()
print(customer2.order(1000))
print(customer2.order(4000))
```

```python
# 결과

14000
19000

1000
5000
```

customer1, customer2 처럼 원하는 만큼 객체를 만들고 객체마다 총 합인 customer 변수 값이 독립적으로 유지 된다. 여기서 customer1, customer2는 클래스를 통해 생성한 객체이고 Oder()클래스의 인스턴스..! 라고 한다.

>-   self

클래스의 생성자나 클래스의 함수의 파라미터로 'self'라는 매개변수를 넣는데 self에는 객체가 생성자와 함수에 자동으로 전달되록 하는 역할을 한다. 
파이썬의 독특한 특징 중 하나. 
파이썬의 메서드의 첫 번재 매개변수 이름을 관례적으로 'self'로 사용하는 것이고 객체를 호출한 자신이 전달되기 때문에 'self'라는 이름을 붙였다고 한다. 
self말고 다른 이름을 사용해도 상관 없다.

>-   생성자

객체에 초기값을 설정해야 할 때 사용한다.

```python
# order.py

class Order:
    def setData(self, customer):
        self.customer = customer

    def order(self, price):
        self.customer += price
        return self.customer

customer1 = Order()

customer1.setData(0)
print(customer1.order(14000))
print(customer1.order(5000))
```

생성자를 사용하지 않고 setData()와 같이 초깃값을 정해주는 함수를 구현할 수도 있지만, 그렇게 되면 setData()를 호출한 후에만 order() 함수를 사용할 수 있게된다.

```python
class Order:
    def __init__(self):
        self.customer = 0

    def order(self, price):
        self.customer += price
        return self.customer

customer1 = Order()

print(customer1.order(14000))
print(customer1.order(5000))
```

생성자는 __init__메서드를 통해 만들 수 있고 위 예제는 생성자에 별다른 매개변수가 없지만,

```python
class Order:
    def __init__(self, name):
        self.customer = 0
        self.name = name

    def order(self, price):
        self.customer += price
        return self.customer

customer1 = Order('길동이')

print(customer1.order(14000))
print(customer1.order(5000))
```

이렇게 매개변수가 있는 경우에는 객체를 생성할 때 꼭 매개변수 값을 전달해주어야 한다.

customer1 = Order()처럼 매개변수를 입력하지 않으면 오류가 난다.

>-   클래스의 상속

상속은 말 그대로 어떤 클래스에 다른 클래스의 기능을 물려받을 수 있도록 할 수 있는 기능이다. 

class 클래스이름(상속할 클래스 이름)

```python
class Order:
    def __init__(self, name):
        self.customer = 0
        self.name = name

    def order(self, price):
        self.customer += price
        return self.customer

class extraOrder(Order):
    pass
```

이렇게 되면 Order()의 기능을 extraOrder()라는 클래스가 물려받게 된다. 그리고 Order의 기능을 모두 사용할 수 있게 된다.

```python
class Order:
    def __init__(self, name):
        self.customer = 0
        self.name = name

    def order(self, price):
        self.customer += price
        return self.customer

class extraOrder(Order):
    pass

extraCustomer = extraOrder('영희')

print(extraCustomer.order(1000))
```

extraOrder 생성자에 order라는 함수를 추가하지 않았지만 자동으로 사용할 수 있게 되었고 생성할 때도 마찬가지로 name이라는 매개변수를 넣어주지 않으면 에러가 난다.

상속은 기존 클래스를 변경하지 않고 기능을 추가하거나 기존 기능을 변경하려고 할 때 사용한다. 사실 기존 클래스를 수정하면 되지만 수정이 허용되지 않는 라이브러리 형태라든가 하는 상황이라면 상속을 이용해야 한다.

>-   메서드 오버라이딩

```python
class Order:
    def __init__(self, name):
        self.customer = 0
        self.name = name

    def order(self, price):
        self.customer += price
        return self.customer

class extraOrder(Order):
    def order(self, price):
        self.customer += price
        return str(self.customer) + '원'

extraCustomer = extraOrder('영희')

print(extraCustomer.order(1000))
```

상속한 클래스(부모 클래스)에 있는 메서드를 동일 이름으로 다시 만드는 것을 '메소드 오버라이딩(Method Overriding)이라고 한다. 

>-   클래스 변수

```python
class Family:
    lastname = '김'

print(Family.lastname)
print()

a = Family()
b = Family()

print(a.lastname)
print(b.lastname)
print()

Family.lastname = '박'

print(a.lastname)
print(b.lastname)
```

```python
# 결과

김

김
김

박
박
```

생성자(__init__)로 만든 변수는 '객체 변수' 이고 객체 변수는 다른 객체들에 영향을 받지 않고 독립적인 값을 유지한다.
'클래스 변수'는 클래스 내에 선언한 변수를 말하며 사용은 **클래스이름.클래스변수** 로 한다. 
클래스 변수는 객체 변수와 특징이 다른데 위 예시 처럼 객체가 여러개 있어도 클래스 변수의 값은 서로 공유한다. 
클래스 변수보다는 객체 변수를 훨씬 많이 사용하기 때문에 생성자와 객체 변수를 잘 익혀놓는게 편하다.

---

## **파이썬의 언더스코어 ( _ , __ )**

파이썬 유저라면 for _ in range()나 __init__ 문법을 자주 사용한다. 
여기서 언더스코어는 크게 다섯가지 경우가 있다고 한다.

#### **CASE 1 : 언더스코어( _ )가 독립적으로 사용되는 경우**

-> 인터프리터에서 마지막 실행결과 값을 가지는 변수로 사용된다.

![](https://blog.kakaocdn.net/dn/dxnljG/btqQJEE9fw7/CAgpUsw27vhB728tYHe3v1/img.png)

파이썬을 인터프리터(프롬프트)에서 사용할 경우 가장 마지막에 실행된 결과의 값을 가진 변수로 '_'가 사용된다.

---

#### **CASE 2: 변수로 사용될 경우** 

-> 변수 값을 굳이 사용할 필요가 없을때 사용한다.

```python
for _ in range(5): # 단순히 5번 반복할 목적으로
    print('hello world')

for _ in range(3): # _를 i로 바꿔서 보면 우리에게 익숙한 표현식일 것이다.
    print(_)

def values():
    return (1,2,3,4) # 튜플을 반환

a, b, _, _ = values() # 반환된 튜플 값중 2개만 필요할 경우
print(a, b)
```

```python
# 결과

hello world
hello world
hello world
hello world
hello world
0
1
2
1 2
```

변수를 문자로 할당해주어도 되지만 굳이 사용하지 않을 경우에 _로 치환해준다. 변수명이 낭비될 필요가 없기 때문이다.

---

#### **CASE 3 : __이름__**

-> 내장된 특수한 함수와 변수를 나타낸다.

```python
class vector:
  def __init__(self, x,y,z):
    self.x = x
    self.y = y
    self.z = z

  def __add__(self, other):
    x_ = self.x + other.x
    y_ = self.y + other.y
    z_ = self.z + other.z

    return vector(x_,y_,z_)

  def show(self):
    print(f"x:{self.x}, y:{self.y}, z:{self.z}")


v1 = vector(1,2,3)
v2 = vector(4,5,6)
v3 = v1 + v2
v3.show()
```

```python
# 결과

x:5, y:7, z:9
```

__init__은 클래스의 생성자 함수이며, __add__ 함수는 연산자 오버로드용으로 사용됐다.

---

#### **CASE 4 : 네이밍**

-> 가장 많이 사용하는 경우고 
1. 변수 이름 뒤에 _가 붙는 경우, 
2. 함수 이름 왼쪽 _를 붙이는 경우, 
3. 변수 왼쪽에 __를 붙이는 경우, 
4. 그리고 맹글링할 때 사용한다. 

>-   변수_

```python
class Order:
  def __int__(self):
    self.coffee = 'Americano'
    self.price = 3000

def printClassName(class_):
  print(class_)

order = Order()
printClassName(order.__class__)
```

```python
# 결과

<class '__main__.Order'>
```

예약어를 변수명으로 사용할 수 없을 때 사용한다. 위 예제도 className 이런식으로 지으면 되기때문에 굳이 사용하지는 않는 것 같다.
솔직히 뭔 소린지 모르겠다.
이걸 사용하는걸 발견하면 여기에 추가로 적을 예정

>-   _함수

```python
# func.py

def publicFunc():
  print('this is public function')

def _privateFunc():
  print('this is private function')
```

```python
# test.py

from func import *

publicFunc()
_privateFunc()
```

```python
# 결과

this is public function
Traceback (most recent call last):
  File "c:\Projects\test\test.py", line 4, in <module>
    _privateFunc()
NameError: name '_privateFunc' is not defined
```

함수 이름 왼쪽에 _를 붙이면 다른 파일을 임포트 할 때 _를 붙인 함수는 **가져오지 않는다..!**

보안성이 약한 private 의미로 사용하는 경우이다. 파이썬은 진정한 private를 지원하지 않기 때문에 임포트문에서는 누락이 가능하지만 직접 가져다쓰거나 호출을 할 때는 사용이 가능하다.

```python
# test.py

import func

func.publicFunc()
func._privateFunc()
```

```python
# 결과

this is public function
this is private function
```

모듈명은 가져올 수 있어버린다. 그래서 약한 private라고 하는 것이다.

>-   변수 왼쪽에 __(더블스코어)를 붙인경우 -> Mangling 맹글링

```python
# order.py

class Order:
  def __init__(self):
    self.coffee = 'Americano'
    self.__price = 3000

  def printInfo(self):
    print(f"coffee : {self.coffee}")
    print(f"price : {self.__price}")
```

```python
# test.py

import order

order1 = order.Order()
print(order1.coffee)
print(order1.__price)
```

```python
# test.py 결과

Americano
Traceback (most recent call last):
  File "c:\Projects\test\test.py", line 5, in <module>
    print(order1.__price)
AttributeError: 'Order' object has no attribute '__price'
```

언더스코어를 두개(__) 붙인 변수는 선언된 클래스 안에서만 해당 이름으로 사용 가능하다. 위 예제에서는 order.py안에서만 __price 변수가 __price이름으로 사용이 가능하고, 외부 모듈인 test.py 안에서는 __price이름으로 사용이 불가능하다.

```python
test.py

import order

order1 = order.Order()

print(order1._Order__price)
order1.printInfo()
```

```python
# 결과

3000
coffee : Americano
price : 3000
```

외부에서 order.py의 __price 변수에 접근하려면 **_클래스명__변수명** 형식으로 변경해서 사용할 수 있다. 이러한 작업을 '**맹글링**(Mangling)'이라고 한다.

[출처](https://ebbnflow.tistory.com/255)
