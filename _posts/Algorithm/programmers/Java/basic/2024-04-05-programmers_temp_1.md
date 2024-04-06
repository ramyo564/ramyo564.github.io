---
layout: single
title: "[알고리즘] 행렬의 곱셈"
categories: Algo
tags:
  - Java
  - 프로그래머스
  - 연습문제
  - 99클럽
  - 99일지
  - 코딩테스트
  - 개발스터디
  - 항해
  - TIL
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
# 수학 문제

[문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/12949)

## 풀이 접근

![](https://i.imgur.com/ctc1tCc.png)

수학문제 항상 까먹어서 다시 봤다.  
행렬의 곱은 위와 같이 2개의 행렬이 있을 때 `2 x 3 * 3 x 1` 이렇게 있을 때  
- 첫 번째 행렬의 열 길이와 두 번째 행렬 행 길이가 같아야 한다.
- 결과는 첫 번째 행렬의 행 (2) 두 번째 행렬의 열 (1) -> 2 x 1 이 된다.    

{% raw %}
```
int[][] arr1 = {{1, 4}, {3, 2}, {4, 1}};  
int[][] arr2 = {{3, 3}, {3, 3}};
```
{% endraw %}

주어진 제한 조건이 그렇게 빡빡하지 않아서 반복문으로 해결할 수 있을 것 같았다.   
우선 첫 번째 반복문은 arr1 의 행의 갯 수, 두 번째 반복문읜 두 번째 행렬의 열의 갯수만 나오게 만들고 마지막 반복문에서는 첫 번째 행렬의 열만 나오게 만들면 완성 할 수 있다.

## 구현 코드

```java
class Solution {
    public int[][] solution(int[][] arr1, int[][] arr2) {
int[][] result = new int[arr1.length][arr2[0].length];

        for (int i = 0; i < arr1.length; i++) {
            // 첫 번째 행렬에서 행 들만 나오게
            for (int j = 0; j < arr2[0].length; j++) {
                // 두 번째 행렬에서 열만 나오게
                for (int k = 0; k < arr1[0].length; k++) {
                    // 첫 번째 행렬에서 열만 나오게
                    result[i][j] += arr1[i][k] * arr2[k][j];
                }
            }
        }
        return result;
    }
}
```

## 정리
{% raw %}
```
int[][] arr1 = {{1, 4}, {3, 2}, {4, 1}};  
int[][] arr2 = {{3, 3}, {3, 3}};
```
{% endraw %}
- 첫 번째 행렬의 크기 3 x 2
- 두 번째 행렬의 크기 2 x 2
- 결과는 2 x 2
- 인덱스 i 는 첫 번째 행렬의 행의 길이 3개가 나온다 = 0,1,2
- 인덱스 j 는 두 번째 행렬의 열의 길이 2개가 나온다 = 0,1
- 마지막 k 는 첫 번째 행렬의 열의 길이 2개가 나온다 = 0,1

>- 조합해보면 arr1 과 arr2의 행과 열의 크기로 result 라는 배열을 만든다.
>- 예를 들어 `result[0][0]` 에는 `arr1[0][0]` x `arr2[0][0]` + `arr1[0][1]` x `arr2[1][0]` 이렇게 각 곱의 합이 다 들어가야 다음으로 넘어가진다.


