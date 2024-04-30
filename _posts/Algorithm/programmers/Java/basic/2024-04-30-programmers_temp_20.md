---
layout: single
title: "[알고리즘] N개의 최소 공배수"
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
# 수학 문제

[문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/12953)

## 풀이 접근

1. 최대공약수를 구한다.
2. 구해놓은 최대공약수로 최소공배수를 다시 구하면 끝

## 구현 코드 

```java
class Solution {
    public int solution(int[] arr) {
int answer = 0;
    int lcmResult = calculateLCM(arr);
    return lcmResult;
  }

  public static int gcd(int a, int b) {
    while (b != 0) {
      int temp = b;
      b = a % b;
      a = temp;
    }
    return a;
  }

  // 최소공배수 계산하는 메서드
  public static int lcm(int a, int b) {
    return (a * b) / gcd(a, b);
  }

  // N개의 숫자들의 최소공배수 계산하는 메서드
  public static int calculateLCM(int[] numbers) {
    int result = numbers[0];
    for (int i = 1; i < numbers.length; i++) {
      result = lcm(result, numbers[i]);
    }
    return result;
  }
}


```

![](https://i.imgur.com/voOQSJN.png)


## 정리

1. 맨날까먹는 수학문제인데 그냥 외워야 할 거 같다 ㅜ

