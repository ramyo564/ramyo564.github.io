---
layout: single
title: "[알고리즘] 예상 대진표"
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

[문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/12985)

## 풀이 접근

1. 재귀로 풀어보기
	1. ArrayList에 `int[2]` 를 원소로 넣는다
	2. `[1,2] [3,4], [5,6], [7,8]`
	3. 반복문으로 돌리면서 index 0,1이 같으면 탈출
	4. 타겟번호가 없는 경우 항상 0번 인덱스의 숫자를 새로운 list에 1번과 2번을 넣음 타겟 번호가 있을 경우 타겟번호를 넣음
2. 근데 뭔가 정리해 놓고 구현하려고 보니 굳이 이렇게 할 필요가 없어보였다.
3. 타겟 넘버가 만나기 전까지는 무조건 이기는 상황이니 라운드가 끝날 때마다 거기에 맞게 숫자를 변경해주고 변한 숫자가 동일할 경우 반복문을 깨주면 된다.
	1. 라운드가 올라갈 때의 변화는 무조건 둘이 싸우니 /2 + 나머지를 해주면 끝
	2. 이렇게 해주면 라운드가 올라 갈 때 `[1,2]` 이렇게 있을 때 첫번째 순서에 있든 두 번째 순서에 있든 동일한 값으로 업데이트 해줄 수 있다
	3. 또한 타겟 넘버가 1 과 8이더라도 최종적으로는 1의 값에서 한쪽이 기다리니 반복문이 한 번 돌아갈 때 마다 라운드를 카운터해주면 된다.

## 구현 코드 

```java
class Solution
{
    public int solution(int n, int a, int b){
    int answer = 0;

    while (true) {
      a = a / 2 + a % 2;
      b = b / 2 + b % 2;
      answer++;
      if (a == b) {
        break;
      }

    }
    return answer;
  }

}

```

![](https://i.imgur.com/d23QmIj.png)


## 정리

1. 사실 재귀를 너무 못해서 문제를 보자마자 재귀적으로만 생각하다보니 문제를 푸는데 오래걸렸다.
2. 수학적 사고를 한다면 좀 더 쉽게 풀릴 수도 있는 부분을 명심하자