---
layout: single
title: "[알고리즘] 큰 수 만들기"
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
# 스텍 문제 (그리디)

[문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/42883)

## 풀이 접근

1. 제한사항이 1,000,000 이하인 수라 O(n^2) 이 넘으면 안됨
2. 가장 큰 수를 뽑고 최적화를 위해 9일 경우  break; 
	1. 9가 없을 경우 어쩔 수 없이 다 돌아야함
3. 반복문을 돌면서 가장 큰수로 시작할 경우 새로운 반복문으로 k번 째까지의 수를 캡쳐해서 큰 수를 업데이트
	1. 만약 999911999191991 이런식으로 들어올 경우 아마 실패할 듯..
--------
1. 문제를 잘못 봤다...
2. 그냥 갖고 있는 문자열중에 제일 큰수를 만드는거다
3. 이전에 만든 반복문을 그냥 해시맵에 각 숫자를 카운팅해서 숫자를 만들면 간단하게 만들 수 있는 문제...
--------
1. 문제를 또 잘못 봤다... 하
2. 문자를 그대로 뽑는게 아니라 문자열의 순서는 유지하면서 그 안에서 문자 2개만 삭제하는 거였다.
3. ㅜㅜ
---- 
1. 와우 어떻게 낑낑 대면서 풀었는데 10번에서 계속 시간 초과가 났다
2. 통곡의 벽을 넘지 못해 답안을 봤다.

## 구현 코드 (실패코드) 

```java
class Solution {
    public String solution(String number, int k) {
        StringBuilder answer = new StringBuilder("");
        int len = number.length() - k;
        int start = 0;
        
        while(start < number.length() && answer.length() != len) {
            int leftNum = k + answer.length() + 1;
            int max = 0;
            for (int j = start; j < leftNum; j++) {
                if (max < number.charAt(j) - '0') {
                    max = number.charAt(j) - '0';
                    start = j + 1;
                }
            }
            answer.append(Integer.toString(max));
        }
        return answer.toString();
    }
}

```

![](https://i.imgur.com/Ipj8uhi.png)


## Stack 으로 풀기

```java
import java.util.Stack;

class Solution {
    public String solution(String number, int k) {
        Stack<Character> stack = new Stack<>();
        int keep = number.length() - k;
        
        for (int i = 0; i < number.length(); i++) {
            char digit = number.charAt(i);
            while (!stack.isEmpty() && k > 0 && stack.peek() < digit) {
                stack.pop();
                k--;
            }
            stack.push(digit);
        }
        
        StringBuilder sb = new StringBuilder();
        while (!stack.isEmpty()) {
            sb.append(stack.pop());
        }
        return sb.reverse().substring(0, keep);
    }
}
```

![](https://i.imgur.com/HGR1bPS.png)


[출처](https://programmer-may.tistory.com/201)

## 정리

- 제한 사항처럼 최악의 상황으로 가면 시간 초과로 문제를 풀 수 없다.
- 따라서 더 효율적인 자료구조를 사용해야되는데 Stack을 사용하면 숫자를 차례대로 쌓아가면서 현재 숫자가 스택의 맨 위 숫자보다 클 경우 맨 위 숫자를 제거하고 현재 숫자를 스택에 넣는다.
- 이렇게 하면 시간 복잡도는 O(n)

1. 뭔가 문제를 풀수록 더 못해지는거 같다 
	1. 그만큼 몰랐던 부분을 더 알게 되는 과정이라고 합리화..
2. 이번에 그리디 알고리즘을 알게 되었는데 좀 더 공부해야겠다...
