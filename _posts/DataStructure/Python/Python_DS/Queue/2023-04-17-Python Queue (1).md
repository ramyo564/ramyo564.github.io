---
layout: single
title: "Python 자료구조 Queue (1)"
categories: Data_Structure
tag: ["Queue","Python"]
toc: true
toc_sticky: true
author_profile: false
sidebar:
  

---

# Queue 뜯어보기!

- 언제나 기본기가 제일 중요함
- 그렇다면? 구조 뜯어 버리기!

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