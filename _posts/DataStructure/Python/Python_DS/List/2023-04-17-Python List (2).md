---
layout: single
title: "Python 자료구조 List (2)"
categories: Data_Structure
tag: ["List","LinkedList","Python"]
toc: true
toc_sticky: true
author_profile: false
sidebar:
  

---
# LinkedList 뜯어보기! 

- 언제나 기본기가 제일 중요함
- 그렇다면? 구조 뜯어 버리기!

```python
class Node(object):  
  
    def __init__(self, data=None, next_node=None):  
        self.data = data  
        self.next_node = next_node  
  
    def get_data(self):  
        return self.data  
  
    def get_next(self):  
        return self.next_node  
  
    def set_next(self, new_next):  
        self.next_node = new_next  
  
  
class LinkedList(object):  
    def __init__(self, head=None):  
        self.head = head  
  
    def insert(self, data):  
        new_node = Node(data)  
        new_node.set_next(self.head)  
        self.head = new_node  
  
    def size(self):  
        current = self.head  
        count = 0  
        while current:  
            count += 1  
            current = current.get_next()  
        return count  
  
    def search(self, data):  
        current = self.head  
        found = False  
        while current and found is False:  
            if current.get_data() == data:  
                found = True  
            else:  
                current = current.get_next()  
        if current is None:  
            raise ValueError("Data not in list")  
        return current  
  
    def delete(self, data):  
        current = self.head  
        previous = None  
        found = False  
        while current and found is False:  
            if current.get_data() == data:  
                found = True  
            else:  
                previous = current  
                current = current.get_next()  
        if current is None:  
            raise ValueError("Data not in list")  
        if previous is None:  
            self.head = current.get_next()  
        else:  
            previous.set_next(current.get_next())


```


<iframe width="1188" height="578" src="https://www.youtube.com/embed/dA8F4SPMnb0" title="Python으로 구현한 간단한 linked list 1편 (append 구현)" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
<iframe width="1188" height="578" src="https://www.youtube.com/embed/wd3utYBTdMk" title="Python으로 구현한 간단한 linked list 2편 (pop, find 구현)" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
<iframe width="1183" height="578" src="https://www.youtube.com/embed/1S_iqIonYzs" title="Python으로 구현한 간단한 linked list 3편 (iter, insert구현)" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

## 파이썬의 Linked List?
- 기본적으로 파이썬에서는 list 자료형이 링크드 리스트의 모든기능을 지원한다.
- 하지만 기본 list보다는 훨씬 가볍게 사용할 수 있다..
- 중간에 delete로 끊어진 데이터는 가비지 컬렉터가 알아서 수거해감


참고 : https://wikidocs.net/191324