---
layout: single
title: "[알고리즘] 올바른 괄호"
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

# 스텍 문제

[문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/12909)

## 풀이 접근

1. 전형적인 스택문제 -> `(` 가 들어올 경우 `)` 를 넣어주고 다음 문자열이 스택에 있을 경우 뽑고 아닐 경우 false로 반환해주면 된다.

## 구현 코드 

```java
import java.util.Stack;
class Solution {
    boolean solution(String s) {
    boolean answer = true;

    Stack<String> stack = new Stack<>();

    for (String z : s.split("")){
      if(z.equals("(")){
        stack.add(")");
      } else if (z.equals(")") && !stack.isEmpty()) {
        stack.pop();
      }else {
        return false;
      }
    }
    if(!stack.isEmpty()){
      return false;
    }
    return answer;
  }
}

```

![](https://i.imgur.com/x8Sfk3J.png)

뭐지....

```java
import java.util.Stack;
class Solution {
    boolean solution(String s) {

    Stack<String> stack = new Stack<>();

    for (String z : s.split("")){
      if(z.equals("(")){
        stack.add(")");
      } else {
        if(stack.isEmpty()){
          return false;
        }else{
          stack.pop();
        }
      }
    }
    if(!stack.isEmpty()){
      return false;
    }
    return true;
  }
}


```

별 차이가 없다.  
마지막으로 String 변환하는 부분을 char로 바꿔 리스트로 만드는 과정을 줄여봤다. 

```java
import java.util.Stack;
class Solution {
    boolean solution(String s) {
    Stack<Character> stack = new Stack<>();

    for (int i = 0; i < s.length(); i++) {
      if (s.charAt(i) == '(') {
        stack.add(')');
      } else {
        if (stack.isEmpty()) {
          return false;
        } else {
          stack.pop();
        }
      }
    }
    
    if (!stack.isEmpty()) {
      return false;
    }
    return true;
  }

}
```

![](https://i.imgur.com/WjByehH.png)

## 정리

- 문자열을 split 하는 것 보다는 charAt을 사용하는 게 더 빠르다!





