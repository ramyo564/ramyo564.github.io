---
layout: single
title: "[백준 2798번 블랙잭] (알고리즘)"
categories: Algo
tags:
  - Python
  - 백준
  - Brute_Force
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
`Brute_force`
## 문제

[문제링크](https://www.acmicpc.net/problem/2798)

카지노에서 제일 인기 있는 게임 블랙잭의 규칙은 상당히 쉽다. 카드의 합이 21을 넘지 않는 한도 내에서, 카드의 합을 최대한 크게 만드는 게임이다. 블랙잭은 카지노마다 다양한 규정이 있다.

한국 최고의 블랙잭 고수 김정인은 새로운 블랙잭 규칙을 만들어 상근, 창영이와 게임하려고 한다.

김정인 버전의 블랙잭에서 각 카드에는 양의 정수가 쓰여 있다. 그 다음, 딜러는 N장의 카드를 모두 숫자가 보이도록 바닥에 놓는다. 그런 후에 딜러는 숫자 M을 크게 외친다.

이제 플레이어는 제한된 시간 안에 N장의 카드 중에서 3장의 카드를 골라야 한다. 블랙잭 변형 게임이기 때문에, 플레이어가 고른 카드의 합은 M을 넘지 않으면서 M과 최대한 가깝게 만들어야 한다.

N장의 카드에 써져 있는 숫자가 주어졌을 때, M을 넘지 않으면서 M에 최대한 가까운 카드 3장의 합을 구해 출력하시오.

### 입력

첫째 줄에 카드의 개수 N(3 ≤ N ≤ 100)과 M(10 ≤ M ≤ 300,000)이 주어진다. 둘째 줄에는 카드에 쓰여 있는 수가 주어지며, 이 값은 100,000을 넘지 않는 양의 정수이다.

합이 M을 넘지 않는 카드 3장을 찾을 수 있는 경우만 입력으로 주어진다.

### 출력

첫째 줄에 M을 넘지 않으면서 M에 최대한 가까운 카드 3장의 합을 출력한다.

#### 예제 입력 1 복사

5 21
5 6 7 8 9

#### 예제 출력 1 복사

21

#### 예제 입력 2 복사

10 500
93 181 245 214 315 36 185 138 216 295

#### 예제 출력 2 복사

497


## 구현 코드

```python
import sys

n,m = map(int, sys.stdin.readline().split())

cards = list(map(int, sys.stdin.readline().split()))
cards.sort()

answer = 0
for i in range(len(cards)):
    for r in range(1, len(cards)):
        for x in range(2, len(cards)):
            if m >= cards[i] + cards[r] +cards[x]:
                answer = cards[i] + cards[r] +cards[x]
            
            
    
print(answer)

```

- 뭔가 맞게 만든거 같았는데 설계를 잘못했다 -_-; 


```python
import sys

n,m = map(int, sys.stdin.readline().split())
cards = list(map(int, sys.stdin.readline().split()))

answer = 0
for i in range(n):
    for r in range(i+1, n):
        for x in range(r+1, n):
            if cards[i] + cards[r] +cards[x] > m:
                continue
            else:
                answer = max(answer, cards[i] + cards[r] +cards[x])
            
            
    
print(answer)
```

## 시간 복잡도와 공간 복잡도

- O(n^3) 반복문이 3번 진행되는 완전탐색

## 회고 과정

백준에서 브루트 포스 단계를 풀 차례였다.     
처음 들어보는 알고리즘이라서 뭔가 했는데 그냥 초초완전탐색을 통해 문제를 푼다.   
생각없이 문제풀다가 도대체 이걸 어떻게 풀어야되나 했는데 싹다 탐색하면 된다.

입력 제한 부분에서 어떻게 풀어도 상관없겠구나 했는데 그게 아니라 완전탐색 브루투 포스로 풀어볼 생각을 먼저 했으면 더 좋았을 것 같다.     
