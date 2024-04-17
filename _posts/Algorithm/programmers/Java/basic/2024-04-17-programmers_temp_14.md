---
layout: single
title: "[알고리즘] 전력망을 둘로 나누기"
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
# 완전탐색 문제

[문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/86971)

## 풀이 접근 (실패)

1. 각 번호가 얼마나 많이 연결되어있는지 완전 탐색으로 푸는 문제다
2. 하 완전탐색... 부셔버리겠어
3. 우선 저번에 배웠덩 DFS 그리고 재귀로 풀 방법을 생각해봤다.
4. 솔직히 이해가 안가서 https://katastrophe.tistory.com/47 를 참고했다.
	1. 여러가지 정답을 찾아봤을 때 가장 쉽게 코드가 작성되어있다고 생각함

## 구현 코드 

```java
import java.util.*;
import java.io.*;
 
class Solution {
    static ArrayList<Integer>[] list;
    static boolean[] visit;
    static int[] sub;
    public int solution(int n, int[][] wires) {
        list = new ArrayList[n+1];
        visit = new boolean[n+1];
        sub = new int[n+1];
 
        for(int i=1;i<=n;i++){
            list[i] = new ArrayList<>();
        }
        for (int[] wire : wires) {
            list[wire[0]].add(wire[1]);
            list[wire[1]].add(wire[0]);
        }
        dfs(1,0);
 
        int min=Integer.MAX_VALUE;
        for(int i=1;i<=n;i++){
            sub[i]++;
            int diff = Math.abs(n-sub[i]-sub[i]);
            min = Math.min(min,diff);
        }

        return min;
    }
    public static void dfs(int x,int parent){
        visit[x] = true;
        for (Integer next : list[x]) {
            if(visit[next])
                continue;
            dfs(next,x);
            sub[x]++;
        }
        sub[parent]+=sub[x];
    }
}

```


## 정리

- 솔직히 아직도 dfs를 어떻게 푸는지 모르겠다.
- 그냥 무식하게 계속 이해하면서 노력 + 풀었던 문제 다시 풀기를 반복해야될 것 같다.
- 한 가지 알게 된 사실은 배열을 무지성으로 타입만 정해서 사용했는데 
	- 예를 들어 `int[] intArray`
	- 타입 자체를 ArrayList로도 가능 `ArrayList<Integer>`
	- 이 부분이 이해가 안갔었는데 하나 배웠다.
- 오늘 부터 하루에 한 번씩 풀어보면서 정말 이해가 제대로 되면 내 생각을 여기에 이어서 쓰면서 정리해야겠다.
- 아직은 코드 이해만 겨우 하는중이라서 자신있게 말을 못 하겠다ㅜㅜ