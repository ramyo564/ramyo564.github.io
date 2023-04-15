---
layout: single
title: "Python 자료구조 List (1)"
categories: Data_Structure
tag: ["List","ArrayList","Python"]
toc: true
toc_sticky: true
author_profile: false
sidebar:
  

---
# ArrayList 뜯어보기!

- 언제나 기본기가 제일 중요함
- 그렇다면? 구조 뜯어 버리기!


```python
class ArrayList:
    def __init__(self, size=10):
        self.max_size = size # maximum memory capacity
        self.list = [None] * self.max_size # allocate array
        self.size = 0 # current actual size (number of elements)

    def add(self, value):
        if self.size >= self.max_size: # check if enough memory capacity
            self._increase_size()
        self.list[self.size] = value # 기존에 인덱스 0에 값 넣기
        self.size += 1 # 인덱스 숫자 증가

    def _increase_size(self):
        new_max_size = self.max_size * 2 # double memory size
        new_list = [None] * new_max_size
        for i in range(0, self.max_size): # copy old list to new list
            new_list[i] = self.list[i] # 늘어난 list에 기존 list 복사
        self.max_size = new_max_size # 늘어난 사이즈로 갱신
        self.list = new_list # 업데이트된 데이터의 새로운 리스트를 다시 기존 이름으로 갱신

    def get(self, index):
        if index >= self.size or index < 0:
            raise Exception('Invalid index')

        return self.list[index]

    def delete(self, index):
        if index >= self.size or index < 0:
            raise Exception('Invalid index')

        # shift list from deleted index onwards
        # element before index are not affected by deletion
        for i in range(index, self.size): 
            self.list[index] = self.list[index+1]

        self.size -= 1

```

## 자바스크립트에서는 Array, 파이썬에서는 List

- 파이썬에서 Array를 사용하려면 내장 모듈을 사용해야하며 Array 와 List는 엄연히 다르다고 한다.
- 기능적으로는 거의 동일하지만 메모리 효율 면에서 Array가 더 유리하다고 한다.

## Array 특징
- 제일 큰 특징은 순차적으로 데이터를 저장한다는 점
- 데이터를 효율적인 고정 타입 데이터 버퍼에 저장
- numpy array는 다양한 연산도 가능
	- 동적으로 사용하기 편하게 쓰려면 기본 list
	- 저장만 효율적으로 하려면 array 모듈
	- 저장 이후 연산을 효율적으로 하려면 numpy array

## 그럼 Array 를 언제 사용하는게 좋을까?
-   순차열적인 데이터를 저장할 때  
    : ex) 주식 가격 등 값보다는 순서가 중요한 데이터
-   다차원 데이터를 다룰 때
-   특정한 요소를 빠르게 읽어야 할 때  
    : index 를 통해 곧바로 읽을 수 있으므로
-   데이터의 사이즈가 급변하지 않을 때
-   요소가 자주 삭제/추가되지 않을 때


참고 : https://wikidocs.net/189478 , 
https://velog.io/@lilyoh/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-1.-Array-List,
https://brownbears.tistory.com/636
