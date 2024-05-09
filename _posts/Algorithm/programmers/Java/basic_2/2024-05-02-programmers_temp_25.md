---
layout: single
title: "[알고리즘] 완주하지 못한 선수"
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

# 해시 문제

[문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/42576)
## 풀이 접근

1. 해시 맵으로 동명이인이 있을 수 있으니 카운트를 해주면서 해시맵 데이터를 만들어준다.
2. 다시 배열을 돌며서 이름당 카운트 수를 하나씩 삭제해준다.
3. 마지막으로 남은 데이터를 반환

## 구현 코드 

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    public String solution(String[] participant, String[] completion) {
    String answer = "";

    HashMap <String, Integer> map = new HashMap<>();

    for (String s : participant){
      map.put(s, map.getOrDefault(s, 0)+1);
    }

    for (String s: completion){
      if((map.get(s)) != 0){
        map.put(s, map.get(s)-1);
      }
    }
    for (Map.Entry<String, Integer> entry : map.entrySet()) {
      if (entry.getValue() != 0) {
        answer = entry.getKey();
        break;
      }
    }
    return answer;
  }
}

```

![](https://i.imgur.com/enmfrq8.png)

## 정리







