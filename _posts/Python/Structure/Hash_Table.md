---
layout: single
title: "[Basic Python] 파이썬의 Hash Table 자료구조"
categories: Basic_Python
tags:
  - Python
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
# Hash Table

- Array list based (Open addressing)
- Array list + Linked list based(Seperate chaining)
- Find key O(1)

파이썬의 딕셔너리는 Array list로 구현되어 있다.     
해시테이블은 충돌이 일어나는데 Array list based 는 Open addressing으로 해당 부분을 해결하고 Array list + Linked list based 는 Seperate chaining 으로 충돌을 해결한다.     

해시테이블은 효율적인 탐색(빠른 탄샘)을 위한 잘구조로써 key-value 쌍의 데이터를 입력 받는다. hash function h에 key 값을 입력으로 넣어 얻은 해시값 h(k)를 위치로 지정하여 key-value 데이터 쌍을 저장한다.      
저장,삭제,검색의 시간복잡도는 모두 O(1) 이다.     

```python
def hashFunction(key):
	return key % 9
```

위와 같이 간단한게 해시함수가 있을 경우 어떠한 데이터를 키값과 벨류를 리스트에 담아 둘 때 인덱스 값을 해시함수에 키값을 넣어 리턴된 값으로 갖는다.     

### Direct-address Table

일반적인 리스트를 생각하면 되는데 여기에는 두 가지 문제점이 있다.     
인덱스 값을 문자열로 받을 수 없다는점 나머지는 키 값의 인덱스가 101일경우 앞에 쓰이지 않은 나머지 100개의 리스트가 낭비되는 단점이 있다.     

## Collision

하지만 위와 같이 해시함수를 쓰더라도 충돌이 발생할 수 있다.    
예를들어서 0번 인덱스에 이미 데이터가 있고 새로운 데이터를 넣으려고 해시함수를 돌렸는데도 똑같이 0번 인덱스가 나오는 경우다.      

이럴 때 여러가지 방법이 있는데 가장 간단한 방법은 그냥 다음 인덱스에 데이터를 넣는 것
이 때 시간 복잡도는 O(1)이다.     
파이썬은 딕셔너리라는 자료구조로 구현되어 있는데 해시테이블의 충돌 과정은 자바에서 좀 더 자세하게 볼 예정!      





