---
layout: single
title: "[Basic Python] 파이썬의 재귀함수"
categories: Basic_Python
tags:
  - Python
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
# 재귀함수

- 자신을 정의할 때 자기 자신을 재참조하는 함수

```python

def factorial(n):
	if n == 1:
		return 1
	return n * factorial(n-1)
	
```

## 점화식 (recurrence relation)

![](https://i.imgur.com/q36YWcg.png)

- 나 자신을 계속 호출하는 방식

### base case

- 더 이상 재귀호출을 하지 않아도 계산 값을 반환할 수 있는 상황(조건)
- 모든 입력이 최종적으로 base case를 이용해서 문제를 해결할 수 있어야 함
- basecase가 무조건 있어야 무한루프 방지 가능