---
layout: single
title: "[알고리즘] 덧칠하기"
categories: Algo
tags:
  - Java
  - 프로그래머스
  - 연습문제
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
# 배열 문제

[문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/161989)

## 풀이 접근

1. 페인트 길이만큼 숫자 1로 채운 배열을 만들어준다.
2. 덧 칠해야되는 구간은 0으로 바꿔준다.
3. 반복문을 돌면서 0인 구간을 롤러 길이만큼 다시 덧칠해준다.
4. 마지막부분에 롤러길이보다 작은 구간에 0이 발견된다면 한 번만 칠할 수 있게 로직을 만들어준다.

## 구현 코드

```java
import java.util.Arrays;
class Solution {
    public int solution(int n, int m, int[] section) {
        int[] array = new int[n];
        // 1로 초기화
        Arrays.fill(array, 1);

        // 페인트 칠해야할 곳 0으로 만들어줌
        for(int i : section){
            array[i-1] = 0;
        }

        // 롤러 크기보다 작은 인덱스의 반복문에서 0을 검색
        int cnt = 0;
        for (int i = 0; i < array.length; i++) {
            if(array[i] == 0){
                if(array.length-m >= i){
                    for (int j = 0; j < m; j++) {
                        array[i+j] = 1;
                    }
                } else {
                // 마지막에 롤러 크기보다 작은 구간에 0이 있을 경우
                    for (int j = 0; j < array.length - i ; j++) {
                        array[i+j] = 1;
                    }
                }
                cnt ++;
            }
        }

        return cnt;
    }
}

```

## 정리

그냥 주어진 문제를 구현하는 문제였다. 다른 사람들은 이런 방식으로 풀었다.  

```java
class Solution {
    public int solution(int n, int m, int[] section) {
        int roller = section[0];
        int cnt = 1;
        for(int i = 1; i < section.length; i++) {
            if(roller + m - 1 < section[i]) {
                cnt++;
                roller = section[i];
            }
        }
        return cnt;
    }
}
```

롤러길이 자체로 페인트를 칠하고 만약 페인트가 칠해 졌다면 페인트가 롤러길이에 맞게 칠해졌다고 가정하고 롤러 인덱스를 업데이트한다.  
다만 무조건 칠해진다는 가정이 붙기 때문에 예외 케이스로 아무것도 안 칠하는 게 나오면 어떡하지 라고 생각했는데 제한 사항을 보니 무조건 한 개는 나온다.

```java
class Solution {
    public int solution(int n, int m, int[] section) {
        int maxPainted = 0, cntPaint = 0;
        for (int point : section) {
            if (maxPainted <= point) {
                maxPainted = point + m;
                cntPaint++;
            }
        }
        return cntPaint;
    }
}
```

같은 개념을 더 쉽게 만든 케이스