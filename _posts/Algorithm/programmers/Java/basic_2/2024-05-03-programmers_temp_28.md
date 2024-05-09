---
layout: single
title: "[알고리즘] 둘만의 암호"
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

# 둘만의 암호 문제

[문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/155652)

## 풀이 접근

1. skip에 나와있을 경우에는 반복문 횟수를 추가해준다.
2. z가 넘었을 떄는 다시 앞으로 돌아갈 수 있게 -26을 해준다.

## 구현 코드 

```java
class Solution {

    public String solution(String s, String skip, int index) {
        String answer = "";

        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            for (int j = 0; j < index; j++) {
                c += 1;
                if (c > 'z') {
                    c -= 26;
                }
                if (skip.contains(String.valueOf(c))) {
                    j--;
                }
            }
            answer += c;
        }

        return answer;
    }
}

```

![](https://i.imgur.com/c4BtQrG.png)







