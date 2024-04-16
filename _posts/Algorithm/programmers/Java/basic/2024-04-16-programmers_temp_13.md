---
layout: single
title: "[알고리즘] 연속된 부분 수열의 합"
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
# 투포인터 문제

[문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/178870)

## 풀이 접근 (실패)

1. 계속 생각하다보니 투 포인터로 문제를 푸는거 까지는 접근했는데 최소사이즈 부분을 구현을 고민하다가 시간이 초과되었다. 

## 구현 코드 

```java
class Solution {
    public int[] solution(int[] sequence, int k) {
        
            int[] answer = new int[2];

        int size = Integer.MAX_VALUE;

        int right = 0;
        int left = 0;
        int current = 0;


        for (int i = 0; i < sequence.length; i++) {
            if(current == k){
                int sizeNow = right - left + 1;
                if(sizeNow < size){
                    size = sizeNow;
                }
            }else if(current == 0){
                current = sequence[i];

            }else{
                if(current < k){
                    current += sequence[i];
                    right ++;
                } else if (current > k) {

                    while (current > k){
                        current -= sequence[left];
                        left++;

                    }
                }

            }

        }

        answer[0] = left;
        answer[1] = right;
        
        return answer;
    }
}


```

## 정답에서 내가 생각하지 못 한 부분

```java
class Solution {
    public int[] solution(int[] sequence, int k) {
        
        int N = sequence.length;
        int left = 0, right = N;
        int sum = 0;
        for(int L = 0, R = 0; L < N; L++) {
            while(R < N && sum < k) {
                sum += sequence[R++];
            }
            
            if(sum == k) {
                int range = R - L - 1;
                if((right - left) > range) {
                    left = L;
                    right = R - 1;
                }
            }
            
            sum -= sequence[L];
        }
        
        int[] answer = {left, right};
        
        return answer;
    }
}
```

- 논리적으로 정리하고 했었어야 했는데 단순하게 쉬운문제라고 생각해서 막풀었던게 원인이다.
- 이전에 했던 실수를 반복한게 가장 바보짓이였다.
- 쉬워보이고 귀찮아도 반드시 노트에 먼저 정리하고 푸는 습관을 들이자...
	- 로직을 반드시 정리하고 코드로 작성하자! 막풀다가 나중에 수정하는 거 보다는 미리 정리하는 게 오히려 덜 귀찮고 시간 아끼는 길이다!
