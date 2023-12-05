---
layout: single
title: "[Basic Python] 파이썬의 Queue(Deque), Stack 자료구조"
categories: Basic_Python
tags:
  - Python
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
# Queue(Deque) , Stack

## Queue(Deque)

- Array list based -> Circular queue, Dynamic array (큐로 사용할 때 별 장점이 없음) -> 앞 쪽에서 데이터를 뽑으면 앞으로 다시 옮겨주는 작업이 O(n) ->별 장점이 없음
- Linked list based
- FIFO

## 정의

Queue는 시간 순서상 먼저 저장한 데이터가 먼저 출력되는 선입선출 FIFO(First in First Out) 형식으로 데이터를 저장하는 자료구조다.      
큐의 뒷 쪽에 데이터를 추가하는 것을 enqueue 앞쪽의 데이터를 꺼내는 것을 dequeue 라고 한다.     

파이썬에서 Deque 자료구조는 더블리 링크드 리스트로 구현되어 있다.     
앞으로 빼나 뒤로 빼나 O(1)의 시간복잡도를 갖는다.


## Stack

- Array list based
	- 큐에서는 디큐를 할 때 O(n)이 걸리지만 스텍에서 리스트를 사용할 경우 push 와 pop의 연산이 O(1) 로 가능함
- Linked list based -> 위와 같은 이유로 스택에서는 링크드 리스트를 사용안함 (deque는 더블리 링크드 리스트니깐 사용해도 상관은 없지만 리스트 사용이 좀 더 간편하고 짧아서 리스트가 좀 더 낫다.)
- LIFO

## 정리

스택은 시간 순서상 가장 최근에 추가한 데이터가 가장 먼저 나오는 후입선출 FIFO (Last In First Out) 프링글스 형식으로 데이터를 저장하는 구조다.     
스텍의 top에 데이터를 추가하는 것을 push , stack의 top에서 데이터를 추출하는 것을 pop이라고 한다.      

## 코테 적용법

- `(),{},[]` 를 포함하고 있는 문자열 s가 주어졌을 때 괄호가 유효한지 판별하는 문제
- 직관적으로 생각하기
	- 보통 완전탐색으로 시작
	- 문제 상황을 단순화 하여 생각하기
	- 문제 상황을 근한화 하여 생각하기
- 자료구조와 알고리즘 활용
	- 문제이해 에서 파악한 내용을 토대로 어떤 자료구조를 사용하는게 가장 적합한지 결정
	- 대놓고 특정 자료구조와 알고리즘을 묻는 문제도 많음
	- 자료구조에 따라 선택할 수 있는 알고리즘을 문제에 적용
- 메모리 사용
	- 시간복잡도를 줄이기 위해 메모리를 사용하는 방법
	- 대표적으로 해시테이블



## Queue 구현
- 파이썬에서는 Queue를 list 혹은 queue 라이브러리를 사용해서 구현할 수 있다.
- 자세한 설명은 `Java 자료구조 Queue` 포스팅에 해 놓았다.
- 라이브러리나 리스트 없이 생으로 구현해보자

## Linked Queue 구현


```python
  
class Node:  
    def __init__(self, data, next_=None):  
        self.data = data  
        self.next = next_  
  
class LinkedQueue:  
  
    def __init__(self):  
        self._size = 0  
        self.head = Node(None)  
        self.tail = self.head  
  
    def offer(self, data):  
        node = Node(data)  
        self.tail.next = node  
        self.tail = self.tail.next  
        self._size += 1  
  
    def poll(self):  
        if self._size == 0:  
            raise RuntimeError("Empty")  
  
        ret = self.head.next  
        self.head.next = ret.next  
        ret.next = None  
        self._size -= 1  
        if self.head.next is None:  
            self.tail = self.head  
        return ret.data  
  
    def peek(self):  
        if self._size == 0:  
            raise RuntimeError("Empty")  
        return self.head.next.data  
  
    def size(self):  
        return self._size  
  
    def clear(self):  
        self.head.next = None  
        self.tail = self.head  
        self._size = 0  
  
  
if __name__ == '__main__':  
    q = LinkedQueue()  
    for elem in [5, 3, 6, 8, 9, 10]:  
        print(f"q.offer({elem})")  
        q.offer(elem)  
  
    print(f"q.size(): {q.size()}")  
    while q.size() > 0:  
        print(f"q.peek(): {q.peek()}")  
        print(f"q.pop(): {q.poll()}")  
  
    print(f"q.size(): {q.size()}")
```


## Circular Queue 구현

```python
class CircularQueue:  
    def __init__(self, max_size):  
        self.elements = [None] * (max_size + 1)  
        self.front = 0  
        self.rear = 0  
        self.max_size = max_size + 1  
  
    def is_full(self):  
        return (self.rear + 1) % self.max_size == self.front  
  
    def is_empty(self):  
        return self.front == self.rear  
  
    def clear(self):  
        self.front = 0  
        self.rear = 0  
  
    def offer(self, data):  
        if self.is_full():  
            raise RuntimeError("Queue is full")  
  
        self.rear = (self.rear + 1) % self.max_size  
        self.elements[self.rear] = data  
  
    def poll(self):  
        if self.is_empty():  
            raise RuntimeError("Queue is empty")  
        self.front = (self.front + 1) % self.max_size  
        return self.elements[self.front]  
  
    def size(self):  
        if self.front <= self.rear:  
            return self.rear - self.front  
        return self.max_size - self.front + self.rear  
  
  
if __name__ == "__main__":  
    q = CircularQueue(5)  
    for e in range(5):  
        print(f"is_full: {q.is_full()}, is_empty: {q.is_empty()}, size: {q.size()}")  
        q.offer(e)  
        print(f"q.offer({e})")  
        print(f"is_full: {q.is_full()}, is_empty: {q.is_empty()}, size: {q.size()}")  
  
    print("--------------------------------------------")  
    while q.size() > 0:  
        print(f"is_full: {q.is_full()}, is_empty: {q.is_empty()}, size: {q.size()}")  
        print(f"q.poll(): {q.poll()}")
```

## 참고 

https://wikidocs.net/192523
https://velog.io/@gnwjd309/python-queue