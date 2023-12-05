---
layout: single
title: "[Basic Python] 파이썬의 Tree"
categories: Basic_Python
tags:
  - Python
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
# Tree

- 트리는 서로 연결된 Node의 계층형 자료구조로써 Root와 부모 자식 관계의 subtree로 구성되어 있다.

![](https://i.imgur.com/F3GhIGc.png)

![](https://i.imgur.com/V3vH3t9.png)

- 노드 (Node) : 트리는 보통 노드로 구현된다.
- 간선 (Edge) : 노드간에 연결된 선
- 루트 노드 (Root) : 트리는 항상 루트에서 시작
- 리프 노드 (Leef) : 더 이상 뻗어나갈 수 없는 마지막 노드
- 자식 노드 (Child), 부모 노드 (Parent), 형제 노드 (Sibling)
- 차수 (degredd) : 각 노드가 갖는 자식의 수, 모든 노드의 차수가 n 개 이하인 트리를 n진 트리라고 함
- 조상 (ancestor) : 위쪽으로 간선을 따라가면 만나는 모든 노드
- 자손 (descendant) : 아래쪽으로 간선을 따라가면 만나는 모든 노드
- 높이 (height) : 루트노드에서 가장 멀리 있는 리프 노드까지의 거리, 즉 리프 노드중에 최대 레벨 값
- 서브트리 (subtree) : 트리의 어떤 노드를 루트로 하고, 그 자손으로 구성된 트리를 subtree라고 함

## 이진트리

![](https://i.imgur.com/78nI7Ow.png)

- 모든 차수가 2 이하
- 마지막 레벨을 제외한 모든 노드들이 꽉 차있다 + 마지막 레벨은 맨 왼쪽부터 차있는 경우  -> 완전이진트리
- 각 노드들이 갖을 값은 벨류 + 왼쪽,오른쪽 자식 주소값

```python
class Node:
	def __init__(self):
		self.value = 0
		self.left_child = None
		self.right_child = None

class BinaryTree:
	def __init__(self):
		self.root = None

bt = BinaryTree()
bt.root = Node(value = 1)
bt.root.left = Node(value = 2)
bt.root.right = Node(value = 3)
bt.root.left.left = Node(value = 4)
```

## 트리순회 (Traversal)

- 트리 순회란 트리 탐색이라고도 불리우며 트리의 각 노드를 방문하는 과정을 말한다. 모든 노드를 한 번씩 방문 해야 하므로 완전 탐색이라고도 불린다. 순회 방법으로는 너비 우선 탐색의 BFS와 깊이 우선 탐색의 DFS가 있다.

###  BFS

- 각 동일 레벨에 있는 모든 노드들을 훓음

```python
def bfs(root):
	visited = []
	if root is None:
		return 00;
	q = deque()
	q.append(root)
	while q:
		cur_node = q.popleft()
		visited.append(cur_node.value)

		if cur_node.left:
			q.append(cur_node.left)
		if cur_node.right:
			q.append(cur_node.right)
	return visited

bfs(root)

```

### DFS (by recursion)

```python
def dfs(cur_node):
	if cur_node is None:
		return
	dfs(cur_node.left)
	dfs(cur_node.right)
dfs(root)
```

#### 전위순회(preorder)

노드가 아래와 같을 경우

```
        A
     B     C
   D  E  F
 G  H
```

```python
visited = []
def preorder(cur_node):
	if cur_node is None:
		return
	visited.append(cur_node.value)
	preorder(cur_node.left)
	preorder(cur_node.right)

preorder(root)
```

- 방문순서 : A B D G H E C F
#### 중위순회(inorder)

```python
visited = []
def preorder(cur_node):
	if cur_node is None:
		return
	
	preorder(cur_node.left)
	visited.append(cur_node.value)
	preorder(cur_node.right)

preorder(root)
```

- 방문순서 : G D H B E A C F

#### 후위순회(postorder)


```python
visited = []
def preorder(cur_node):
	if cur_node is None:
		return
	
	preorder(cur_node.left)
	preorder(cur_node.right)
	visited.append(cur_node.value)

preorder(root)
```

- 방문순서 : G H D E B F C A