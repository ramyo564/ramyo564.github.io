---
layout: single
title: "[1472. Design Browser History] (알고리즘)"
categories: Algo
tags:
  - leetcode
  - Python
  - DoubleLinkedList
  - LinkedList
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
`Double LinkedList`
## 문제

[문제 링크](https://leetcode.com/problems/design-browser-history/)

You have a **browser** of one tab where you start on the `homepage` and you can visit another `url`, get back in the history number of `steps` or move forward in the history number of `steps`.

Implement the `BrowserHistory` class:

- `BrowserHistory(string homepage)` Initializes the object with the `homepage` of the browser.
- `void visit(string url)` Visits `url` from the current page. It clears up all the forward history.
- `string back(int steps)` Move `steps` back in history. If you can only return `x` steps in the history and `steps > x`, you will return only `x` steps. Return the current `url` after moving back in history **at most** `steps`.
- `string forward(int steps)` Move `steps` forward in history. If you can only forward `x` steps in the history and `steps > x`, you will forward only `x` steps. Return the current `url` after forwarding in history **at most** `steps`.

**Example:**

**Input:**
["BrowserHistory","visit","visit","visit","back","back","forward","visit","forward","back","back"]
[["leetcode.com"],["google.com"],["facebook.com"],["youtube.com"],[1],[1],[1],["linkedin.com"],[2],[2],[7]]
**Output:**
[null,null,null,null,"facebook.com","google.com","facebook.com",null,"linkedin.com","google.com","leetcode.com"]

**Explanation:**
BrowserHistory browserHistory = new BrowserHistory("leetcode.com");
browserHistory.visit("google.com");       // You are in "leetcode.com". Visit "google.com"
browserHistory.visit("facebook.com");     // You are in "google.com". Visit "facebook.com"
browserHistory.visit("youtube.com");      // You are in "facebook.com". Visit "youtube.com"
browserHistory.back(1);                   // You are in "youtube.com", move back to "facebook.com" return "facebook.com"
browserHistory.back(1);                   // You are in "facebook.com", move back to "google.com" return "google.com"
browserHistory.forward(1);                // You are in "google.com", move forward to "facebook.com" return "facebook.com"
browserHistory.visit("linkedin.com");     // You are in "facebook.com". Visit "linkedin.com"
browserHistory.forward(2);                // You are in "linkedin.com", you cannot move forward any steps.
browserHistory.back(2);                   // You are in "linkedin.com", move back two steps to "facebook.com" then to "google.com". return "google.com"
browserHistory.back(7);                   // You are in "google.com", you can move back only one step to "leetcode.com". return "leetcode.com"

**Constraints:**

- `1 <= homepage.length <= 20`
- `1 <= url.length <= 20`
- `1 <= steps <= 100`
- `homepage` and `url` consist of  '.' or lower case English letters.
- At most `5000` calls will be made to `visit`, `back`, and `forward`.
## 스스로 고민

- 링크를 타고 들어가는 개념이다
- 리스트로도 문제를 풀 수 있지만 중간에 새로운 사이트를 들어갈 경우 뒤에 있는 데이터들을 삭제해줘야 하기 때문에 링크드 리스트가 적합하다고 생각했다.
- 링크드 리스트에서도 뒤로가기를 해야하는 경우가 있기 때문에 더블링크드 리스트가 적합

## 의사코드

- Node 를 만들어주고 value값은 0으로 값을 받고 나머지 next,prev는 None 으로 초기화 해준다
- BrowserHistory 에서 head 및 current를 홈페이지 주소로 설정해준다.
- visit을 사용하면 url을 value 값으로 넣어주고 노드를 이전과 현재를 연결 그리고 현재를 지금 받은 url로 연결해준다.
- step에 맞게 이전 혹은 앞으로 옮길 때 step만큼 옮겨주고 step이 현재 저장되어있는 노드를 초과할 경우 current값을 업데이트 한 후 while 문을 탈출해준다.


## 구현 코드

```python
class Node:
    def __init__(self, value=0, next=None, prev=None):
        self.value = value
        self.next = next
        self.prev = prev

class BrowserHistory:

    def __init__(self, homepage: str):
        self.head = self.current = Node(homepage)
        
        

    def visit(self, url: str) -> None:
        new_node = Node(url)
        new_node.prev =  self.current
        self.current.next = new_node
        self.current = new_node
        
        

    def back(self, steps: int) -> str:
        
        while(steps!=0):
            if self.current.prev:
                self.current = self.current.prev
                steps -=1
            else:
                break
        
        return self.current.value

    def forward(self, steps: int) -> str:
                
        while(steps!=0):
            if self.current.next:
                self.current = self.current.next
                steps -=1
            else:
                break
        
        return self.current.value

        


```


## 시간 복잡도와 공간 복잡도

### 시간 복잡도

1. **`__init__` 메서드:**
    - 시간 복잡도: O(1)
    - 노드 하나를 생성하여 head와 current를 초기화하는 작업이므로 상수 시간이 걸린다.
2. **`visit` 메서드:**
    - 시간 복잡도: O(1)
    - 방문한 URL에 대한 새로운 노드를 생성하고, 현재 노드의 `next`와 새로운 노드의 `prev`를 조정하는 과정이므로 상수 시간이 걸린다.
3. **`back` 메서드:**
    - 시간 복잡도: O(steps)
    - `steps`만큼 현재 노드의 이전 노드로 이동하는 작업을 수행한다. 이 때, `steps`가 현재 노드의 이전 노드의 개수에 비례하므로 O(steps)이다.
4. **`forward` 메서드:**
    - 시간 복잡도: O(steps)
    - `steps`만큼 현재 노드의 다음 노드로 이동하는 작업을 수행한다. 이 때, `steps`가 현재 노드의 다음 노드의 개수에 비례하므로 O(steps)이다.

### 공간 복잡도

1. **`__init__` 메서드:**    
    - 공간 복잡도: O(1)
    - head와 current 변수만을 사용하여 현재 페이지와 노드를 추적하므로, 상수만큼의 공간이 필요하다.
2. **`visit` 메서드:**
    - 공간 복잡도: O(1)
    - 새로운 노드를 생성하고, 현재 노드의 `next`와 새로운 노드의 `prev`를 조정하는데 상수만큼의 공간이 필요하다.
3. **`back` 메서드:**
    - 공간 복잡도: O(1)
    - 노드를 이동하는 과정에서 추가적인 공간이 필요하지 않다.
4. **`forward` 메서드:**
    - 공간 복잡도: O(1)
    - 노드를 이동하는 과정에서 추가적인 공간이 필요하지 않다.

전체적으로 보면, 메모리 사용이 한정적이며 노드의 이동이 주요 연산이므로 대부분의 연산이 상수 시간 또는 O(steps)의 시간이 소요된다.

