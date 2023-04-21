---
layout: single
title: "오차역전파법!"
categories: ML_DL
tag: [Python,"오차역전파법!"]
toc: true
toc_sticky: true
author_profile: false

---
# 오차역전파법

- 앞에서는 신경망의 가중치 매개변수의 기울기는 수치 미분을 사용해 구했다.
	- 장점
		- 단순하고 구현하기 쉬움
	- 단점
		- 계산 시간이 오래걸림

>- 이러한 이유로 가중치 매개변수의 기울기를 효율적으로 계산하는 방법으로 ***오차역전파법*** 이 있다.
>- 오차역전파법은 단계별로 나눠 미분을 통해 기울기 값을 구한다고 이해하면 된다.

## 연쇄법칙

![](https://i.imgur.com/5Jk6m2y.png)
- 역전파는 순방향과는 반대 방향으로 국소적 미분을 곱한다.
- 연쇄법칙은 함성 함수의 미분에 대한 성질이며 함성 함수의 미분은 합성 함수를 구성하는 각 함수의 미분의 곱으로 나타낼 수 있다.
- ![](https://i.imgur.com/Kevglly.png)
- ![](https://i.imgur.com/SOeFXiu.png)
- ![](https://i.imgur.com/8CMJQUr.png)
- ![](https://i.imgur.com/DXS92uG.png)
- ![](https://i.imgur.com/Vn7L1xI.png)
- ![](https://i.imgur.com/8Hck1tI.png)
- ![](https://i.imgur.com/YFqaQtS.png)

## 그래프로 이해

![](https://i.imgur.com/I4Z2TV8.png)
- 사탕 가격의 미분은 5.5
- 사탕 개수의 미분은 220
- 소비세의 미분은 1000

>- 소비세와 사탕 가격이 같은 양만큼 오르면 최종 금액에는 소비세가 1000의 크기로 사탕 가격이 5.5 크기로 영향을 준다
>- 단 이 예에서 소비세와 사탕 가격은 단위가 다르니 주의 (소비세 1은 100%, 사과 가격 1은 1원)



## 단순한 계층 구현하기

- 위에 그림을 코드로 구현해보자

### 곱셉 계층
- forward() 는 순전파, backward() 역전파를 처리한다.

```python
class MulLayer:
    def __init__(self):
        self.x = None
        self.y = None

    def forward(self, x, y):
        self.x = x
        self.y = y                
        out = x * y

        return out
  
    def backward(self, dout):
        dx = dout * self.y  # x와 y를 바꾼다.
        dy = dout * self.x
  
        return dx, dy
```

#### 사탕그래프의 순전파 구현

```python
apple = 200
apple_num = 5
tax = 1.1

# 계층들
mul_apple_layer = MulLayer()
mul_tax_layer = MulLayer()

# 순전파
apple_price = mul_apple_layer.forward(apple, apple_num)
price = mul_tax_layer.forward(apple_price, tax)
print(price) # 1100
```

#### 사탕그래프의 역전파 구현

```python
dprice=1

dapple_price, dtax = mul_tax_layer.backward(dprice)
dapple, dapple_num = mul_apple_layer.backward(dapple_price)
print(dapple, dapple_num, dtax) # 5.5 220 1000
```

>- backward() 호출 순서는 forward() 때와는 반대로 하면 된다.

### 덧셈 계층

```python
class AddLayer:
    def __init__(self):
        pass

    def forward(self, x, y):
        out = x + y
        return out

    def backward(self, dout):
        dx = dout * 1
        dy = dout * 1
        return dx, dy

# dout 는 미분
```

## 활성화 함수 계층 구현하기

