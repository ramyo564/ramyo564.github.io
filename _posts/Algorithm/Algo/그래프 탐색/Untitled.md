---
layout: single
title: "DFS와 BFS 그래프 탐색(알고리즘)"
categories: Algo
tag: ["백준","Python","1260문제"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
[문제 링크](https://www.acmicpc.net/problem/1260)

우선 정점과 간선이 뭔 소리인가 했다.
```python
     A
    / \
   /   \
  B-----C
   \
    \
     D

```
정점이 A,B,C,D 간선이 연결된 선의 갯수다.
여기서 정점은 4개 간선은 4개다.

그래도 이해가 안가서 유튜브로 찾아봤다.
[유튭 영상 강의](https://www.youtube.com/watch?v=kkZFEwoZ3fA)

### 입력변수

```python
N, M, V = map(int, input().split())
```

```python
DFS
# 스택

BFS
# 큐
```