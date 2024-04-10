---
layout: single
title: "[알고리즘] 괄호 회전하기"
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
# 월간 코드 챌린지 시즌 2

[문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/76502)

## 풀이 접근

1. 괄호하면 스택문제
2. 근데 뭔가 스택으로 사용하면 큐도 같이 사용해야될거 같고 여로모로 귀찮아서 데크로 자료구조 쓰면 걍 한 방에 다 해결 될 것 같았다.
3. 멍 같이 실패
4. 잠깐 꼬이니까 진짜 이상한 곳에서 해매다가 시간 초과되서 답을 봤다

## 정리

```java
import java.util.*;
class Solution {
    public int solution(String s) {
 int answer = 0;

        for (int i = 0; i < s.length(); i++) {
            Stack<Character> stack = new Stack<>();
            String str = s.substring(i, s.length()) + s.substring(0, i);
            for (int j = 0; j < str.length(); j++) {
                char c = str.charAt(j);
                if (stack.isEmpty()) {
                    stack.push(c);
                } else if (c == ')' && stack.peek() == '(') {
                    stack.pop();
                } else if (c == '}' && stack.peek() == '{') {
                    stack.pop();
                } else if (c == ']' && stack.peek() == '[') {
                    stack.pop();
                } else {
                    stack.push(c);
                }
            }
            if (stack.isEmpty()) {
                answer++;
            }
        }

        return answer;
    }
}
```

스텍으로 답을 푼 경우다. 애초부터 String 형태의 자료구조니 반복문을 돌면서 앞글자와 뒷 글자를 바꿔주고 char 자료형으로 스텍에 쌓았다 뺐다를 반복하면서 마지막에 남아있는지 확인하는 구조다.   

deque로 굳이 갈 필요가 없다고 생각했다.  
정말 다행인건 문제를 이해한거고 최악인건 문제를 정확하게 이해했는데도 엉뚱하게 문제를 풀지 못 했다.  

나와 비슷한 사례에서 극복한 사람이 있는지 찾아봤는데 메모장 같은 곳에 먼저 로직을 먼저 정리하고 코드를 짜는 사람들이 많았다.    

> - 메모장에 먼저 로직을 정리하고 코드작성하기
> - 그렇지 않을 경우 중간에 꼬이면 쓸데없이 시간낭비 많이 하게 됨...ㅜ

![](https://i.imgur.com/ZMiBwsA.png)
