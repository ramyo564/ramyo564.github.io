---
layout: single
title: "[알고리즘] 롤케이크 자르기"
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
# 부분합 문제

[문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/132265)
## 풀이 접근 (실패)

1. 그냥 해시셋 2개로 투포인트 개념으로 문제를 풀면 해결할 수 있을거라고 생각했다.
2. 근데 결과를 보니 최악의 케이스로 O(n^2) 까지 가는거 같아서 while문을 벗겨내서 문제를 푸는 방법을 생각해내야 했다.

## 구현 코드 (실패)

```java
import java.util.HashSet;
class Solution {
    public int solution(int[] topping) {
    int answer = 0;

    int len = topping.length;
    int mid = 1;

    while (len != mid) {
      HashSet<Integer> set_1 = new HashSet<>();
      HashSet<Integer> set_2 = new HashSet<>();

      for (int i = 0; i < mid; i++) {
        set_1.add(topping[i]);
      }
      for (int i = mid; i < len; i++) {
        set_2.add(topping[i]);
      }
      if (set_1.size() == set_2.size()) {
        answer++;
        mid++;
      } else if (set_1.size() > set_2.size()) {
        break;

      } else {
        mid++;
      }
    }
    return answer;
  }
}
```

![](https://i.imgur.com/qQO2OXZ.png)

## 풀이접근

1. 첫 시작을 set 자료구조에 넣어준다
2. 두 번째는 hashMap 으로 각 숫자는 그대로 유지하고 각 원소의 갯수가 몇 개인지 세준다 이렇게 하면 size는 set과 똑같이 유지할 수 있다.
3. 새로운 반복문을 통해 topping 에 하나를 추가할 때마다 어차피 set에서는 그대로 되지만 hashMap에서는 원소를 하나씩 줄여준다.
	1. 여기서 해당 원소에서 값이 0일 경우 키 값을 삭제해준다
4. 위에 로직이 돌아간후 해시맵과 해시셋의 사이즈가 같을 경우 answer의 값을 하나 올려준다.

## 구현코드

```java
import java.util.*;

class Solution {
    public int solution(int[] topping) {
        int answer = 0;
        int size = topping.length;
        
        HashSet<Integer> first = new HashSet<>();
        HashMap<Integer, Integer> second = new HashMap<>();
        
        first.add(topping[0]);
        for (int i = 1;i < size; i++) {
            second.put(topping[i], second.getOrDefault(topping[i], 0) + 1);
        }
        
        for (int i = 1;i < size; i++) {
            first.add(topping[i]);
            second.put(topping[i], second.get(topping[i]) - 1);
            if (second.get(topping[i]) == 0) {
                second.remove(topping[i]);
            }
            if (first.size() == second.size()) answer++;
        }
        
        
        return answer;
    }
}
```

![](https://i.imgur.com/YzhEhhd.png)

## 정리

- 제한 사항이 1,000,000 이였다면 O(n) 에서 끝내는 로직으로 갔어야 했는데 안일하게 생각해서 첫시도에서 성공하지 못 했다.