---
layout: single
title: "[알고리즘] 두 개 뽑아서 더하기"
categories: Algo
tags:
  - Java
  - 프로그래머스
  - 연습문제
  - 99일지
  - 99클럽
  - 항해
  - 코딩테스트
  - TIL
  - 개발스터디
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
# 해시셋 문제 

[문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/68644)

## 풀이 접근

성능 생각안하고 굉장히 간단하게 풀어봤다.

- 해시셋을 만들어준다.
- 한 개의 배열로 2개의 반복문이 돌아가므로 인덱스가 같지 않을 경우에만 해시셋에 해당 데이터를 넣어준다.
- int 배열로 바꿔준 후 정렬해서 리턴
	- 코드 바꾸기는 귀찮은데 속도 더 빠르게 하는 방법 없나 찾아봤는데 굳이 int 배열로 리턴 안하고 Integer 배열로 바로 정렬해서 리턴해도 된다.


## 구현 코드

```java
import java.util.*;
class Solution {
    public int[] solution(int[] numbers) {

        HashSet<Integer> set = new HashSet<>();

        for (int i = 0; i < numbers.length; i++) {
            for (int j = 0; j < numbers.length; j++) {
                if(i != j){
                    set.add(numbers[i] + numbers[j]);
                }
            }
        }
        Integer[] intArray = set.toArray(new Integer[0]);
        int [] array = new int[set.size()];
        for (int i = 0; i < intArray.length; i++) {
            array[i] = intArray[i];
        }
	    Arrays.sort(array);

        return array;
    }
}
```

![](https://i.imgur.com/2VnFMJG.png)

밑에는 Integer 배열로 바로 반환시켰을 때의 차이다.

```java
import java.util.*;
class Solution {
    public Integer[] solution(int[] numbers) {

        HashSet<Integer> set = new HashSet<>();

        for (int i = 0; i < numbers.length; i++) {
            for (int j = 0; j < numbers.length; j++) {
                if(i != j){
                    set.add(numbers[i] + numbers[j]);
                }
            }
        }
        Integer[] intArray = set.toArray(new Integer[0]);
        Arrays.sort(intArray);
     
        return intArray;
    }
}
```

![](https://i.imgur.com/C5KSjqI.png)

엄청 유의미한 변화는 없지만 코드 덜 써도 되서 좀 좋음