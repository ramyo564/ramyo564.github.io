---
layout: single
title: "[알고리즘] 연속 부분 수열 합의 개수"
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
# 문제

[문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/131701)

## 풀이 접근

1. 부분 수열의 합이 무슨 소린지 잘 몰라서 오래걸렸는데 결론은 처음 1개 일 때 2 개를 합 쳤을 때의 합 3 개를 합쳤을 때의 합의 갯수를 세어주면 된다.
2. 처음 시작 부분을 set에 넣어주고 그 다음부터 한칸씩 밀면 원형수열을 만족하게 만들 수 있다.

## 구현 코드 

```java
import java.util.*;
class Solution {
    public int solution(int[] elements) {
        int answer = 0;
        
        // 부분 수열 크기
        int size = 1;
        Set<Integer> set = new HashSet<>();
        
        // 부분 수열 크기가 elements 길이가 될때까지 반복
        while (size <= elements.length) {
            int sum = 0;
            // 부분 수열의 시작을 set에 추가
            for (int i = 0; i < size; i++) {
                sum += elements[i % elements.length];
                set.add(sum);
            }
            // 시작 인덱스부터 elements 끝까지 한칸씩 밀며 진행 
            for (int i = 0; i < elements.length; i++) {
                sum -= elements[i % elements.length];
                sum += elements[(i + size) % elements.length];
                set.add(sum);
            }
            size++;
        }
        answer = set.size();
        return answer;
    }
}

```

![](https://i.imgur.com/jekzPnp.png)


## 정리

1. 수학적인 접근이 아직 잘 안되는데 반복적으로 풀어서 해결을 해야될 것 같다 
2. 해당 접근을 무지성으로 풀기 보다는 로직을 나눠서 주석으로 적고 해당 주석대로 풀어야 
