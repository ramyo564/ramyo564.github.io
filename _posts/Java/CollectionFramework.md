---
layout: single
title: "[Basic Java] Generic"
categories: Basic_Java
tags:
  - Java
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
# 컬렉션 프레임워크란?
2023-11-07-

- 여러 데이터를 편리하게 관리할 수 있게 만들어 놓은 것
	- 자료 구조 및 알고리즘을 구조화
- 대표 인터페이스
	- List 인터페이스, Set 인터페이스, Map 인터페이스

## List 인터페이스

- 순서가 있는 데이터의 집합
- 데이터 중복 허용
- 대표 구현 클래스
	- ArrayList
	- LinkedList
	- Vector

```java
ArrayList list1 = new ArrayList();
LinkedList list2 = new LinkedList();
Vector v = new Vector();
```

## Set 인터페이스

- 순서가 없는 데이터의 집합
- 데이터의 중복을 허용하지 않음
- 대표 구현 클래스
	- HashSet
	- TreeSet
		-TreeSet은 순서가 없는데 first 와 last를 뽑을 수 있음 ;; (아마 링크드 리스트 같은거 아닐까) 그리고 기준 값을 정해 위 아래로 서치가 가능함 -> 바이너리 서치 이런거 가능할 듯

```java
HashSet set1 = new HashSet();
TreeSet set2 = new TreeSet();
```

## Map 인터페이스

- 키와 값의 쌍으로 이루어진 데이터 집합
- 순서를 유지 하지 않음
- 대표 구현 클래스
	- HashMap
	- TreeMap
		- 마찬가지로 첫 번째와 마지막 값을 뽑을 수 있으며 기준 값을 기준으로 (키값) 데이터를 뽑을 수 있음

```java
HashMap map1 = new HashMap();
TreeMap map2 = new TreeMap();
```