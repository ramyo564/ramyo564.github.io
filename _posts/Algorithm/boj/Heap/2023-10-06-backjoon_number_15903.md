---
layout: single
title: "[백준 15903번 카드 합체 놀이] (알고리즘)"
categories: Algo
tags:
  - Python
  - 백준
  - Heap
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
`Heap`
## 문제

[문제 링크](https://www.acmicpc.net/problem/15903)

석환이는 아기다. 아기 석환이는 자연수가 쓰여져있는 카드를 갖고 다양한 놀이를 하며 노는 것을 좋아한다. 오늘 아기 석환이는 무슨 놀이를 하고 있을까? 바로 카드 합체 놀이이다!

아기 석환이는 자연수가 쓰여진 카드를 n장 갖고 있다. 처음에 i번 카드엔 ai가 쓰여있다. 카드 합체 놀이는 이 카드들을 합체하며 노는 놀이이다. 카드 합체는 다음과 같은 과정으로 이루어진다.

1. x번 카드와 y번 카드를 골라 그 두 장에 쓰여진 수를 더한 값을 계산한다. (x ≠ y)
2. 계산한 값을 x번 카드와 y번 카드 두 장 모두에 덮어 쓴다.

이 카드 합체를 총 m번 하면 놀이가 끝난다. m번의 합체를 모두 끝낸 뒤, n장의 카드에 쓰여있는 수를 모두 더한 값이 이 놀이의 점수가 된다. 이 점수를 가장 작게 만드는 것이 놀이의 목표이다.

아기 석환이는 수학을 좋아하긴 하지만, 아직 아기이기 때문에 점수를 얼마나 작게 만들 수 있는지를 알 수는 없었다(어른 석환이는 당연히 쉽게 알 수 있다). 그래서 문제 해결 능력이 뛰어난 여러분에게 도움을 요청했다. 만들 수 있는 가장 작은 점수를 계산하는 프로그램을 만들어보자.

### 입력

첫 번째 줄에 카드의 개수를 나타내는 수 n(2 ≤ n ≤ 1,000)과 카드 합체를 몇 번 하는지를 나타내는 수 m(0 ≤ m ≤ 15×n)이 주어진다.

두 번째 줄에 맨 처음 카드의 상태를 나타내는 n개의 자연수 a1, a2, …, an이 공백으로 구분되어 주어진다. (1 ≤ ai ≤ 1,000,000)

### 출력

첫 번째 줄에 만들 수 있는 가장 작은 점수를 출력한다.

#### 예제 입력 1 복사

3 1
3 2 6

#### 예제 출력 1 복사

16

#### 예제 입력 2 복사

4 2
4 2 3 1

#### 예제 출력 2 복사

19

## 스스로 고민

- 값을 계속 업데이트해야한다.
- 최소힙으로 구현한 후 카드를 2개씩 뽑아 쓰고 값을 새롭게 업데이트한 후 다시 힙에 넣는 식으로 만들면 되지 않을까 생각했다.

## 구현 코드

```python
import heapq
import sys

n, m = map(int, sys.stdin.readline().split())
a= list(map(int, sys.stdin.readline().split()))

heap=[]
heapq.heapify(heap)

for i in a:
    heapq.heappush(heap, i)
    
for _ in range(m):
    num1 = heapq.heappop(heap)
    num2 = heapq.heappop(heap)
    new_num = num1 + num2
    
    heapq.heappush(heap, new_num)
    heapq.heappush(heap, new_num)
    

print(sum(heap))
    
```

## 시간 복잡도와 공간 복잡도

**시간 복잡도**:
1. 입력 읽기: 코드의 처음 두 줄은 `n`과 `m`의 값을 읽는다. 이 작업은 상수 시간인 O(1)이 걸린다.
2. 힙 구축: 다음 루프에서는 리스트 `a`의 `n`개 요소를 힙에 `heappush`를 사용하여 삽입한다. `heappush` 작업은 최악의 경우에 O(log n) 시간이 걸리며, 여기서 `n`은 힙의 요소 수다. 따라서 이 루프의 시간 복잡도는 O(n * log(n))다.
3. M개의 연산 수행: 다음 루프는 `m`번의 연산을 수행한다. 각 연산에서 `heappop`을 사용하여 힙에서 요소를 추출하고, 새로운 숫자를 생성하고 다시 힙에 추가한다. 이러한 연산은 힙의 크기에 따라 O(log n) 시간이 걸린다. 따라서 이 루프의 시간 복잡도는 O(m * log(n))다.
4. 마지막으로 힙의 모든 요소를 합산하는 작업은 O(n) 시간이 걸린다.

따라서 전체 코드의 시간 복잡도는 O(n * log(n)) + O(m * log(n)) + O(n) = O(n * (log(n) + m)) 다.

**공간 복잡도**:       
공간 복잡도는 사용된 추가 공간을 나타낸다.       
코드에서는 입력 값을 저장하는 변수와 힙을 저장하는 변수 `heap`이 사용된다.       
입력 변수는 공간 복잡도에 큰 영향을 미치지 않으므로 주요 공간 복잡도는 힙에 의해 결정된다. 힙은 `n`개의 원소를 저장하므로 공간 복잡도는 O(n)다.

요약하면, 이 코드의 시간 복잡도는 O(n * (log(n) + m))이며, 공간 복잡도는 O(n)입니다.

## 회고 과정

- 실버 1단계인데 혼자 스스로 풀었다!

![](https://media2.giphy.com/media/t3sZxY5zS5B0z5zMIz/giphy.gif?cid=ecf05e4796qpjmy2zh9ozpfyfl9u72r13etvyfvf4tzsd25i&ep=v1_gifs_search&rid=giphy.gif&ct=g)

- 몇 문제만 더 풀어보고 골드 힙 문제 도전!!

### 다른 사람 코드를 보고 그 기준으로 어떤 부분을 개선 할 수 있는지 최종 회고

참 신기하게도 다른 사람들 코드도 나랑 비슷해서 진짜 이번에 잘 풀었구나 생각했다.    
한편으로는 이 방법밖에 없나도 생각이 들었는데 좀 더 실력이 늘어나면 아마 새로운 방법도 생각해낼 수 있겠지? 🥳🥳🥳