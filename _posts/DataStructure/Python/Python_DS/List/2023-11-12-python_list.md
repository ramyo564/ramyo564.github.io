---
layout: single
title: "[Basic Python] 파이썬의 list 자료구조"
categories: Basic_Python
tags:
  - Python
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
# List

자료구조 list 는 Array list 와 Linked list 가 있다.     
Array list 는 Array와 Dynamic array 두 개가 있는데 Array 또한 Dynamic array로 구현된다. 파이썬의 list는 Dynamic array로 구현되어 있다.    

## 배열의 특성
1. 고정된 저장 공간
2. 순차적인 데이터 저장

- 배열은 선언시에 사이즈를 정해서 해당 사이즈 만큼 연속된 메모리를 할당 받아 데이터를 연속적/순차적으로 저장하는 자료구조다. -> 이러한 배열의 데이터는 연속,순차적으로 메모리에 저장된다.
	- 예를 들어 배열 사이즈가 5고 int 형 데이터만 받고 해당 데이터가 4 바이트일 경우 20바이트를 미리 할당 받고 **해당 메모리 주소**에 데이터를 저장한다.
	- 또한 이전에 선언한 배열의 인덱스마다 할당 받은 메모리 주소 또한 저장되어있다. 예를들어 0번 인덱스의 메모리주소가 xxxxxxx 라고 하고 1번 인덱스의 메모리 주소가 xxxx23라고 한다면 0번 인덱스의 주소 xxxxxxx를 찾고 거기에 저장 되어있는 값을 가져오는 식이다. 몇 번 떨어져있는지 확인만 하면 되므로 즉시 접근이 가능하다.
	- 이러한 부분을 렌덤 엑세스 혹은 다이렉트 엑세스라고 한다.


### 렌덤 엑세스 & 다이렉트 엑세스

메모리에 저장된 데이터에 접근하려면 주소값을 알아야한다. 배열변수는 ex `list[0]` `여기서 list가 배열 변수` 자신이 할당 받은 메모리의 첫 번째 주소를 가르키고 순차적으로 저장되기 때문에 첫 주소값을 토대로 인덱스에 바로 즉시 접근이 가능하다. 이렇게 때문에 **한 번의 연산으로 원하는 데이터에 바로 접근이 가능**하고 접근하는 **시간 복잡도는 O(1)** 이다.

### 한계

데이터 갯수가 정해져있다면 효율적이지만 그렇지 않을 경우 공간이 더 필요하거나 공간이 널럴할 경우 비효율적이다. 이러한 한계를 보안 한게 Dynamic array 다.


### Dynamic array 동적배열

기존의 어레이가 초과될 경우 기존의 어레이의 크기의 (보통은) 2배를 새로 선언하고 새로 선언한 배열에 이전 배열의 정보를 복사한다. **이후 이전 배열은 삭제하는데 옮기는 과정은 n번 수행 되므로 O(n)의 시간복잡도를 가진다**. 이를 resizing 혹은 더블링 이라고 한다.

[파이썬 리사이징 코드](https://github.com/python/cpython/blob/main/Objects/listobject.c)
(파이썬은 처음에 2배씩 커지다가 좀 더 작은 배율로 커짐)


## Linked list

- Node로 직접 구현해야함 -> 트리로도 만들 수 있음
	- 노드라는 구조체가 연결되는 형식으로 데이터를 저장하는 자료구조 -> next node의 주소값을 저장 -> 메모리상 비연속적으로 저장이 되지만 각 노드들은 다음 노드의 메모리 주소값을 가리킴으로서 논리적 연속성을 갖게됨
- 물리적인 메모리 주소는 연속적인게 아님 -> 무조건 노드를 타고 들어가야함 -> 시간복잡도는 **O(n)** 

### 물리적 비연속적, 논리적 연속적

- 각 노드들은 데이터 자장 및 다음 노드의 메모리 주소정보도 갖고 있기 때문에 논리적으로 연속성을 유지하면서 연결된다.
- Array 같은 경우 연속성을 유지하기 위해 메모리 상에서 순차적으로 데이터를 저장하는 방식을 사용했지만 Linked list 같은 경우 메모리상 연속성을 유지하지 않기 때문에 메모리 사용이 좀 더 자유롭다. 다만 다음 메모리 주소를 저장해야되기 때문에 데이터 하나당 차지하는 메모리가 더 커진다.

### Node

```python
class Node:
	def __init__(self, value = 0, next = None):
		self.value = value
		self.next = next

first = Node(1)
second = Node(2)
third = Node(3)
first.next = second
second.next = third
first.value = 6
```

- 링크드 리스트는 아래와 같이 헤드 값이 있어야함
	- 헤드는 첫 번째 노드를 가리켜야함
	- 그래야지 헤드를 통해서 위에 만들어서 연결한 노드들에 접근이 가능함

```python
class LinkedList(obj):
	def __init__(self):
		self.head = None
	def append(self, value):
		pass
	def get(self, idx):
		pass
	def insert(self, idx, value):
		pass
	def delete(delf, idx):
		pass
```

### append 구현

```python
class LinkedList(obj):
	def __init__(self):
		self.head = None
	def append(self, value):
		new_node = Node(value)
		if self.head in None:
			self.head = new_node
		else:
			current = self.head
			while(current.next):
				current = current.next
			current.next = new_node

li = LinkedList()
li.append(1)
li.append(2)
li.append(3)
```
- 시간 복잡도는 O(n) 근데 만들기 나름이라 앞 단에 붙이거나 tail을 만들어서 가장 뒤에 넣을 경우 O(1)
### get 구현

```python
class LinkedList(obj):
	def __init__(self):
		self.head = None
	def append(self, value):
		pass
	def get(self, idx):
		current = self.head
		for _ in range (idx):
			current = current.next
		return current.value

li = LinkedList()
li.append(1)
li.append(2)
li.append(3)
li.get(0)
li.get(1)
li.get(2)
```


### insert_at(idx, value) 구현

```python
class LinkedList(obj):
	def __init__(self):
		self.size = 0 # node의 개수
		self.head = None
	def append(self, value):
		pass
	def insert_at(self, idx, value):
		new_node = Node(value)
		if idx == 0:
			new_node.next = self.head
			self.head = new_node
		else:
			current = self.head
			for _ in range (idx - 1):
				current = current.next
			new_node.next = current.next
			current.next = new_node
		self.size +=1
		
li = LinkedList()
li.append(1)
li.append(2)
li.append(3)
li.insert(idx=2, value=9)

```

![](https://i.imgur.com/cPEt9jR.png)

- 1 번부터 연결하면 2번 메모리 주소가 사라지니깐 1번의 next가 새로 삽입하는 new의 next로 오게 하는 걸 첫 번째로 하고 1번의 next 메모리 주소를 new로 바꿔주면 된다.

### remove_at(idx) 구현

```python
class LinkedList(obj):
	def __init__(self):
		self.size = 0 # node의 개수
		self.head = None
	def append(self, value):
		pass
	def insert_at(self, idx, value):
		pass
	def remove_at(self, idx):
		if idx == 0:
			self.head = self.head.next
		else:
			current = self.head
			for _ in range(idx-1):
				current = current.next
			current.next = current.next.next
		self.size -= 1
		
li = LinkedList()
li.append(1)
li.append(2)
li.append(3)
li.remove_at(idx=2)

```

![](https://i.imgur.com/XT4b6ck.png)

1번의 next를 3번으로 옮긴다 이렇게 될 경우 아무도 2번을 참조하지 않으므로 가비지 컬렉터가 알아서 수거해간다.     

## 정리

- 링크드 리스트는 상황에 따라 tail을 만들 수도 있고 다음 노드 뿐만 아니라 이전 노드도 바라보게 만들어줄 수도 있다.
- 문제상황에서 적절하다고 생각하는 방법으로 구현해주면 된다.

## 코테 적용 방법

- 링크드 리스트의 자유자재 수현 (선형 자료구조 + 중간에 데이터 추가/삭제 용이)
- Tree or Graph 에 활용

- input, output 확인
	- input 값의 특징 (정수인가? 값 크기의 범위는? 마이너스도 되는건가? 소수인가? 자료형은 문자열인가? 등등)
	- output 값의 특징 (내가 어떤 값을 반환해줘야 하는지, 정해진 형식대로 반환하려면 어떻게 구현할지)
- input size N 확인
	- 시간복잡도를 계산하기 위한 input size N 또는 M이 무엇인지 확인
- 제약조건 확인
	- 시간복잡도가 제한이 있는지 확인
	- 내가 선택할 수 있는 알고리즘이 무엇이 있는지
- 예상할 수 있는 오류 파악하기
	- 상황을 가정하면서 예상할 수 있는 오류를 파악한다.
	- 입력 값의 범위, stack overflow 등등

