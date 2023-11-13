---
layout: single
title: "[20. Valid Parentheses] (알고리즘)"
categories: Algo
tags:
  - leetcode
  - Python
  - Basic_Algo
  - Stack
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
`Stack`
## 문제

[문제 링크](https://leetcode.com/problems/valid-parentheses/)

Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type.

**Example 1:**

**Input:** s = "()"
**Output:** true

**Example 2:**

**Input:** s = "()[]{}"
**Output:** true

**Example 3:**

**Input:** s = "(]"
**Output:** false

**Constraints:**

- `1 <= s.length <= 104`
- `s` consists of parentheses only `'()[]{}'`.


스텍 같은 경우 list로 문제를 푸는게 좋다.     
우선 list로 사용할 경우 pop, push 모두 시간 복잡도가 O(1)이다.

## 의사코드

우선 괄호가 시작하는 부분일 때 닫히는 부분으로 바꿔서 스택에 쌓고 닫히는 괄호가 나왔을 경우 pop을 통해 유효한지 검사한다.

## 구현 코드

```python
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []
        for x in s:
        
            if x == "(":
                stack.append(")")
            elif x == "{":
                stack.append("}")
            elif x == "[":
                stack.append("]")
            elif not stack or stack.pop() != x:
                return False
            
        return not stack

```

## 시간 복잡도와 공간 복잡도

- 반복문으로 최대 O(n) 나머지 연산들은 O(n) 이 넘을 경우는 없다.

## 회고 과정

- stack에 대한 개념이 없었을 때 뭔가 특별한 방법이라고 생각해서 deque로 문제를 풀려고 했다. 문제가 스텍일 경우 웬만하면 리스트로 풀면 된다.
- 문제를 주먹구구식으로 푸는 경우가 있다. 예전에는 상관없었는데 요즘에는 풀다가 꼬이면 처음부터 다시 시작하느라고 시간을 많이 잡아먹게 되는 경우가 많았다. 전체적인 구도를 잡을 때까지 키보드에 손 안대는 습관을 가져야겠다고 생각했다.      



