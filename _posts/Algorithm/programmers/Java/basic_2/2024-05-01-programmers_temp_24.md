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
2. 

## 구현 코드 

```java
import java.util.HashSet;
class Solution {
    public int solution(int[] elements) {
        int max_sum = 0;
        for (int i=0; i<elements.length; i++) {
            max_sum += elements[i];
        }
        HashSet<Integer> hashSet = new HashSet<>();
        int sum = 0;
        for (int j=0; j<elements.length; j++) {
            sum = elements[j];
            hashSet.add(sum);
            for (int i = j+1; i <= elements.length; i++) {
                if (i == elements.length) i = 0;
                sum += elements[i];
                if (sum > max_sum) break;
                else hashSet.add(sum);
            }
        }

        return hashSet.size();
    }
}

```

![](https://i.imgur.com/jekzPnp.png)


## 정리

1. 수학적인 접근이 아직 잘 안되는데 반복적으로 풀어서 해결을 해야될 것 같다 
2. 해당 접근을 무지성으로 풀기 보다는 로직을 나눠서 주석으로 적고 해당 주석대로 풀어야 오히려 시간이 덜 걸림
