---
layout: single
title: "Java 시간복잡도"
categories: Data_Structure
tag: ["Java","시간복잡도"]
toc: true
toc_sticky: true
author_profile: false
sidebar:
  

---
# 시간복잡도
- 시간복잡도에 대해서 간단히 알아보기

## O(1)
- 입력 데이터의 크기와 상관없이 항상 일정한 시간이 걸리는 알고리즘
	- 배열의 Random Access
	- Hash

## O(N)
- 입력 데이터의 크기에 비례해서 시간이 소요되는 알고리즘
```java
for (int i = 0; i < N; i++){
	//~~~
}
```

## O(N^2)
- 입력 데이터의 크기에 비례해서 시간이 소요되는 알고리즘
```java
for (int i = 0; i < N; i++){
	for (int i = 0; j < N; j++){
		//~~~
	}
}
```

## O(logN)
- 이진탐색(Binary search)
	- 숫자를 절반씩 잘라서 탐색하는 식으로 이해하면 됨
	- 한 번의 연산을 할 때마다 반 씩 줄어들 때의 시간 복잡도


![](https://i.imgur.com/jrPc4c1.png)

![](https://i.imgur.com/8etVtJq.png)

## O(NlogN)
- Merge sort
	- 절반씩 데이터를 나눠서 데이터를 분활 -> logN
	- 절반씩 나눈 데이터를 하나씩 값을 비교하면서 다시 정렬
	   -> 데이터 갯수만큼 N번 만큼 값을 비교 -> NlogN

![](https://i.imgur.com/5Hy9a9Z.png)


## 요구사항에 따라 적절한 알고리즘 설계하기
- 시간제한이 1초인 문제를 만났을 때, 일반적인 기준은 다음과 같다.
	- N의 범위가 500인 경우 : 시간 복잡도 O(N^3)
	- N의 범위가 2,000인 경우 : 시간 복잡도 O(N^2)
	- N의 범위가 100,000인 경우 : 시간 복잡도 O(NlogN)
	- N의 범위가 10,000,000인 경우 : 시간 복잡도 O(N)

### 알고리즘 문제 해결 과정
1. 지문 읽기 및 컴퓨터적 사고
2. 요구사항(복잡도)분석
3. 문제 해결을 위한 아이디어 찾기
4. 소스코드 설계 및 코딩

출처 : https://www.youtube.com/watch?v=m-9pAwq1o3w&list=PLRx0vPvlEmdAghTr5mXQxGpHjWqSz0dgC