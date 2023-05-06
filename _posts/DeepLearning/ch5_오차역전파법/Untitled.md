---
layout: single
title: "오차역전파법 구현하기!"
categories: ML_DL
tag: [Python,"[BIG][밑딥] 오차역전파법 구현하기"]
toc: true
toc_sticky: true
author_profile: false

---

# 오차역전파법 구현하기!

## 신경망 학습의 전체 그림
- 전제 
	- 신경망에는 적응 가능한 가중치와 편향이 있고, 이 가중치와 편향을 훈련 데이터에 적응하도록 조정하는 과정 을 ‘학습’이라 한다. 
	- 신경망 학습은 다음과 같이 4단계로 수행
- 1 단계- 미니배치 
	- 훈련 데이터 중 일부를 무작위로 가져온다. 
	- 이렇게 선별한 데이터를 미니배치라 하며, 그 미니배치의 손실 함수 값을 줄이는 것이 목표
- 2단계-기울기 산출 
	- 미니배치의 손실 함수 값을 줄이기 위해 각 가중치 매개변수의 기울기를 구한다. 
	- 기울기는 손실 함수의 값을 가장 작게 하는 방향을 제시한다. 
- 3단계-매개변수 갱신 
	- 가중치 매개변수를 기울기 방향으로 아주 조금 갱신 
- 4단계 - 반복 
	- 1 ~3단계를 반복

>- 지금까지는 오차역전파법에서 두 번째인 `기울기 산출` 이다.
>- 앞에서는 이 기울기를 구하기 위해 수치 미분을 사용했다.
>- 그런데 수치 미분은 구현하기는 쉽지만 계산이 오래 걸렸다.
>- 오차 역전파법을 이용하면 느릴 수치 미분과 달리 기울기를 효율적이고 빠르게 구현 가능하다.


## 오차역전파법을 적용한 신경망 구현하기
- 앞에서 한 것과 크게 다른 부분은 계층을 사용한다는 점이다.
- 계층을 사용함으로써 인식 결과를 얻는 처리 (predict()) 와 기울기를 구하는 처리(gradient()) 계층의 전파만으로 동작이 이루어진다는 점이다.

```python
import sys, os
sys.path.append(os.pardir)
import numpy as np
from common.layers import *
from common.gradient import numerical_gradient
from collections import OrderedDict

class TwoLayerNet:
    def __init__(self, input_size, hidden_size, output_size, weight_init_std = 0.01):
        # 가중치 초기화
        self.params = {}
        self.params['W1'] = weight_init_std * np.random.randn(input_size, hidden_size)
        self.params['b1'] = np.zeros(hidden_size)
        self.params['W2'] = weight_init_std * np.random.randn(hidden_size, output_size)
        self.params['b2'] = np.zeros(output_size)

        # 계층 생성
        self.layers = OrderedDict()
        self.layers['Affine1'] = Affine(self.params['W1'], self.params['b1'])
        self.layers['Relu1'] = Relu()
        self.layers['Affine2'] = Affine(self.params['W2'], self.params['b2'])
        self.lastLayer = SoftmaxWithLoss()

    def predict(self, x):
        for layer in self.layers.values():
            x = layer.forward(x)
        return x

    # x : 입력 데이터, t : 정답 레이블
    def loss(self, x, t):
        y = self.predict(x)
        return self.lastLayer.forward(y, t)

    def accuracy(self, x, t):
        y = self.predict(x)
        y = np.argmax(y, axis=1)
        if t.ndim != 1 : t = np.argmax(t, axis=1)
        
        accuracy = np.sum(y == t) / float(x.shape[0])
        return accuracy

    # x : 입력 데이터, t : 정답 레이블
    def numerical_gradient(self, x, t):
        loss_W = lambda W: self.loss(x, t)
        grads = {}
        grads['W1'] = numerical_gradient(loss_W, self.params['W1'])
        grads['b1'] = numerical_gradient(loss_W, self.params['b1'])
        grads['W2'] = numerical_gradient(loss_W, self.params['W2'])
        grads['b2'] = numerical_gradient(loss_W, self.params['b2'])
        return grads

    def gradient(self, x, t):
        # forward
        self.loss(x, t)
  
        # backward
        dout = 1
        dout = self.lastLayer.backward(dout)
        layers = list(self.layers.values())
        layers.reverse()

        for layer in layers:
            dout = layer.backward(dout)
            
        # 결과 저장
        grads = {}
        grads['W1'], grads['b1'] = self.layers['Affine1'].dW, self.layers['Affine1'].db
        grads['W2'], grads['b2'] = self.layers['Affine2'].dW, self.layers['Affine2'].db

        return grads
```

>- 정리해야함
>- 코드 안맞음







출처 : 밑바닥부터 시작하는 딥러닝