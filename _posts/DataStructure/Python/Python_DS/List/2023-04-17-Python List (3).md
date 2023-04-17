---
layout: single
title: "Python 자료구조 List (3)"
categories: Data_Structure
tag: ["List","DoubleLinkedList","Python"]
toc: true
toc_sticky: true
author_profile: false
sidebar:
  

---
# DoubleLinkedList 뜯어보기!

```python
class Node:  
    def __init__(self, data, prev_=None, next_=None):  
        self.data = data  
        self.prev = prev_  
        self.next = next_  
  
  
class DoubleLinkedList:  
    def __init__(self):  
        self.head = Node(None)  
        self.tail = Node(None)  
        self._size = 0  
  
        self.head.next = self.tail  
        self.tail.prev = self.head  
  
    def size(self):  
        return self._size  
  
    def add(self, data):  
        last = self.tail.prev  
        new_node = Node(data, last, self.tail)  
        last.next = new_node  
        self.tail.prev = new_node  
        self._size += 1  
  
    def insert(self, index, data):  
        if index > self._size or index < 0:  
            raise RuntimeError("Index out of error")  
  
        prev, current = None, None  
        i = 0  
        if index < self._size // 2:  
            prev = self.head  
            current = self.head.next  
            while i < index:  
                prev = prev.next  
                current = current.next  
                i += 1  
        else:  
            current = self.tail  
            prev = self.tail.prev  
            while i < (self._size - index):  
                current = current.prev  
                prev = prev.prev  
                i += 1  
  
        new_node = Node(data, prev, current)  
        current.prev = new_node  
        prev.next = new_node  
        self._size += 1  
  
    def clear(self):  
        self._size = 0  
        self.head.next = self.tail  
        self.head.prev = None  
        self.tail.next = None  
        self.tail.prev = self.head  
  
    def delete(self, data):  
        prev = self.head  
        current = prev.next  
        while current is not None:  
            if current.data == data:  
                prev.next = current.next  
                current.next.prev = prev  
                current.next = None  
                current.prev = None  
                self._size -= 1  
                return True  
  
            prev = prev.next  
            current = current.next  
  
        return False  
  
    def delete_by_index(self, index):  
        if index >= self._size or index < 0:  
            raise RuntimeError("Index out of error")  
  
        prev_, current_, next_ = None, None, None  
        i = 0  
        if index < self._size // 2:  
            prev_ = self.head  
            current_ = self.head.next  
            while i < index:  
                prev_ = prev_.next  
                current_ = current_.next  
                i += 1  
            prev_.next = current_.next  
            current_.next.prev = prev_  
            current_.next = None  
            current_.prev = None  
        else:  
            current_ = self.tail.prev  
            next_ = self.tail  
            while i < (self._size - index - 1):  
                next_ = next_.prev  
                current_ = current_.prev  
  
            next_.prev = current_.rpev  
            current_.prev.next = next_  
            current_.next = None  
            current_.prev = None  
  
        self._size -= 1  
        return True  
  
    def get(self, index):  
        if index >= self._size or index < 0:  
            raise RuntimeError("Index out of error")  
  
        i = 0  
        current = None  
        if index < self.size // 2:  
            current = self.head.next  
            while i < index:  
                current = current.next  
                i += 1  
        else:  
            current = self.head.prev  
            while i < (self._size - index - 1):  
                current = current.prev  
                i += 1  
  
        return current.data  
  
    def index_of(self, data):  
        current = self.head.next  
        index = 0  
        while current != None:  
            if current.data != None and current.data == data:  
                return index  
            current = current.next  
            index += 1  
        return -1  
  
    def is_empty(self):  
        return self.head.next == self.tail  
  
    def contains(self, data):  

        front = self.head.next  
        rear = self.tail.prev  
  
        while front != None and rear != None:  
            if front.data != None and front.data == data:  
                return True  
  
            if rear.data != None and rear.data == data:  
                return True  
  
            front = front.next  
            rear = rear.prev  
  
        return False  
  
  
if __name__ == "__main__":  
    l = DoubleLinkedList()  
    for elem in [3, 2, 5, 1, 4]:  
        print(f"l.add({elem})")  
        l.add(elem)  
  
    print(f"l.size(): {l.size()}")  
  
    print("=====================")  
    for elem in [3, 2, 5, 1, 4, 100]:  
        print(f"l.contains({elem}): {l.contains(elem)}")  
        print(f"l.index_of({elem}): {l.index_of(elem)}")  
  
    print("=====================")  
    for elem in [4, 5, 100]:  
        print(f"l.delete({elem}): {l.delete(elem)}")  
  
    print(f"l.size(): {l.size()}")  
  
    print(f"l.insert(0, 100)")  
    l.insert(0, 100)  
    print(f"l.insert(2, 101)")  
    l.insert(2, 101)  
  
    for elem in [3, 2, 5, 1, 4, 100, 101]:  
        print(f"l.contains({elem}): {l.contains(elem)}")  
        print(f"l.index_of({elem}): {l.index_of(elem)}")
```

>- 리스트의 가장 앞과 가장 뒤를 동시에 탐색  
>- 연산의 횟수는 데이터의 갯수가 N 일때 절반이 줄어든 N/2 만큼 loop 반복
>- 시간복잡도는 상수는 날린다는 특성에 따라 O(N/2) 가 아닌, O(N) 으로 표현
>- 두 개의 위치로 데이터에 접근하는 방식을 투포인터 알고리즘이라고 함
>- 실제로는 아래 코드 처럼 더블 링크드 리스트의 투포인터 노드 접근 보다 위치를 가르키는 인덱스를 활용할 때 투포인터로 인한 성능상 이득이 두드러지기 떄문에  인덱스를 활용한 투포인터 알고리즘 문제가 많음 
>- 투포인터 알고리즘을 응용한 문제는 아래 블로그 참고  
>- https://myeongmy.tistory.com/7  




참고 : https://pstudio411.tistory.com/entry/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-Linked-List-Hash-Table  , https://myeongmy.tistory.com/7  