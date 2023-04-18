---
layout: single
title: "Python 자료구조 Stack (1)"
categories: Data_Structure
tag: ["Stack","Python"]
toc: true
toc_sticky: true
author_profile: false
sidebar:
  

---

# Stack 뜯어보기!

- 언제나 기본기가 제일 중요함
- 그렇다면? 구조 뜯어 버리기!

## Stack 구현
- 파이썬에서는 Stack을 따로 제공하지 않는다.
- 자세한 설명은 `Java 자료구조 Stack` 포스팅에 해 놓았다.

### 코드 구현

```python
class Stack:  
    def __init__(self):  
        self.head = Node()  
        self._size = 0  
  
    def push(self, data):  
        new_node = Node()  
        new_node.data = data  
        new_node.next = self.head.next  
        self.head.next = new_node  
        self._size += 1  
  
    def pop(self):  
        if self.size == 0:  
            return None  
  
        ret = self.head.next  
        self.head.next = ret.next  
        self._size -= 1  
        return ret.data  
  
    def peek(self):  
        if self._size == 0:  
            return None  
        return self.head.next.data  
  
    def size(self):  
        return self._size  
  
  
class Node:  
    def __init__(self):  
        self.next = None  
        self.data = None  
  
  
if __name__ == "__main__":  
    s = Stack()  
    print("s.push(3)")  
    s.push(3)  
    print("s.push(5)")  
    s.push(5)  
    print("s.push(1)")  
    s.push(1)  
    print("s.push(9)")  
    s.push(9)  
    print(f"s.size(): {s.size()}")  
    print(f"s.peek(): {s.peek()}")  
    print(f"s.pop(): {s.pop()}")  
    print(f"s.pop(): {s.pop()}")  
    print(f"s.pop(): {s.pop()}")  
    print(f"s.pop(): {s.pop()}")
```


## 참고
https://wikidocs.net/192069