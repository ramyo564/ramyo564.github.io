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
	- 수치 미분에 대한 장점
		- 단순하고 구현하기 쉬움
	- 수치 미분에 대한 단점
		- 계산 시간이 오래걸림

>- 이러한 이유로 가중치 매개변수의 기울기를 효율적으로 계산하는 방법으로 ***오차역전파법*** 이 있다.
>- 오차역전파법은 단계별로 나눠 미분을 통해 기울기 값을 구한다고 이해하면 된다.

## Why backpropagation is faster than Numerical differentiation?

- Backpropagation is faster than numerical differentiation for computing gradients in neural networks because it utilizes the ***chain rule*** of differentiation to propagate errors backward from the output layer to the input layer, instead of estimating the gradients numerically using finite differences.

- ***Computing gradients numerically involves calculating the function at least two times for each input dimension***, one for a small positive perturbation and one for a small negative perturbation, and then computing the difference between them divided by the perturbation size. ***This approach is computationally expensive because it requires a large number of function evaluations, especially when dealing with high-dimensional inputs, such as images or text.***

- In contrast, backpropagation calculates gradients in a much more efficient way ***by reusing the computations done during the forward pass of the neural network to calculate the gradients of the loss function with respect to each parameter in the network.*** By exploiting the structure of the network, backpropagation can efficiently compute gradients for all the parameters in a single pass, which makes it much faster than numerical differentiation, especially for deep neural networks with many layers and parameters.

>In summary, backpropagation is faster than numerical differentiation because **it leverages the structure of the neural network and the chain rule of differentiation to efficiently compute gradients with respect to all the parameters in the network in a single pass.**


## 연쇄법칙 Chain rule

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
>- 함성 함수 처럼 여러 변수가 주어졌을 때 구하고자 하는 변수를 제외한 나머지 변수를 고정
>	- 사탕 개수와 소비세의 값이 고정 되었을 때
>	- 사탕 가격의 변화량은 5.5
>		- 사탕가격인 500원으로 변화했다고 해본다면
>		- 500 x 5 x 1.1 = 2750
>		- 2750 * 5.5 미분값을 곱하면 원래 값 500원이 나온다.



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

# 사탕그래프의 역전파 구현
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

### ReLU 계층

- 수식
- ![](https://i.imgur.com/jVpxzcQ.png)
- X 에 대한 미분
- ![](https://i.imgur.com/ErJcumP.png)
- ![](https://i.imgur.com/OsDJBz3.png)

#### 코드 구현

```python
class Relu:
    def __init__(self):
        self.mask = None

    def forward(self, x):
        self.mask = (x <= 0) # mask에 true false로 저장됨
        out = x.copy() # out은 원래 값
        out[self.mask] = 0
        return out

    def backward(self, dout):
        dout[self.mask] = 0
        dx = dout
        return dx
```

```python
x = np.array( [[1.0, -0.5], [-2.0, 3.0]] )
print(x)
# [[ 1. -0.5]
# [-2. 3. ]]

# relu = Relu()
# print(relu.forward(x))

mask = (x <= 0)
print(mask)
# [[False True]
# [ True False]]
```

>- mask 는 True / False로 구성된 넘파이 배열
>- 순전파의 입력인 x의 원소 값이 0 이하인 인덱스는 True
>- 그 외 (0보다 큰 원소)는 False로 유지
>	- mask 변수는 -> True/False로 구성된 넘파이 배열을 유지한다.



### Sigmoid 계층

참고 : http://taewan.kim/post/sigmoid_diff/

- ![](https://i.imgur.com/9rKoigP.png)
- ![](https://i.imgur.com/cqiS1Op.png)
- Sigmoid 계층의 계산 그래프 (순전파)
	- ![](https://i.imgur.com/STr2ehB.png)
	- 위 식을 분석해보자

#### 1 단계
'/'노드 -> y= 1/x 를 미분하면 다음과 같은 식이 된다.
- x = 1+exp(-1) 를 치환했다고 보면 된다.
- ![](https://i.imgur.com/JOO3MIf.png)
- ![](https://i.imgur.com/U9frr9G.png)
- -y^2 가 되는 이유
- <iframe width="1040" height="596" src="https://www.youtube.com/embed/bluxeoECN7U" title="73. 몫의 미분법 - 개념정리" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
- 참고 : https://ok-lab.tistory.com/151


#### 2 단계
'+' 노드는 상류의 값을 여과 없이 하류로 보낸다.
- ![](https://i.imgur.com/jkHaKUy.png)

#### 3 단계
'exp' 노드는 y = exp(x) 연산을 수행한다.
- ![](https://i.imgur.com/Ygeinrh.png)
- ![](https://i.imgur.com/iJUXatr.png)

#### 4 단계
'x' 노드는 순전파 때의 값을 서로 바꿔 곱한다. 여기서는 -1을 곱한다.
- ![](https://i.imgur.com/xqxv4sy.png)

#### 간소화 버전
- ![](https://i.imgur.com/LO9ejwv.png)
- ![](https://i.imgur.com/eiLG2Ir.png)

#### 코드 구현
```python
class Sigmoid:
    def __init__(self):
        self.out = None

    def forward(self, x):
        out = 1 / (1 + np.exp(-x))
        self.out = out
        return out

    def backward(self, dout):
        dx = dout * (1.0 - self.out) * self.out
        return dx
```

>- 순전파의 출력을 인스턴스 변수 out에 보관했다가 역전파 계산 때 그 값을 사용한다.


출처 : 밑바닥부터 시작하는 딥러닝