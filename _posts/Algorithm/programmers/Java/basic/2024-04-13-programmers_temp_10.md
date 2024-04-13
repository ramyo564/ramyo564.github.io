---
layout: single
title: "[알고리즘] 모음사전"
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

[문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/84512)

## 풀이 접근

1. 문자열의 길이가 5일 경우 AAAAA 는 5번째부터 시작된다.
2. 문자열의 길이가 4일 경우 AAAA는 9번째부터 시작된다.
3. 완전 탐색 문제니 이 로직을 그냥 순수하게 코드로 작성해본다면 A부터시작해서 AA, AAA,AAAA,AAAAA, AAAAE 이렇게 돌아가는 로직을 만든 후 타켓 에 도달할 때까지 카운트를 해주면 된다.
4. 일단 지웠다 썼다 작업이 많으니 StringBuffer 를 사용하면 좋을 거 같다.
5. 라고 생각했는데 시간이 지나도록 로직을 제대로 만들지를 못 했다.... 왜냐하면 다시 지웠다 썼다 작업을 하면 성능면에서 너무 안 좋을 것 같았다.
6. 그냥 연산은 최대한 간단하고 로직도 간단한 방법을 생각해 봤는데 진짜 생각이 전혀 안났다
7. 1시간 동안 고민했는데 뭔가 이거다! 하고 생각나는게 없어서 결국 답을 봤는데 내가 제일 못 하는 dfs, bfs 완전탐색 문제였다.
8. 완전탐색이라고해서 부루트포스처럼 무식하게 생각했었다..

항상 완전탐색쪽에 가면 포기했었는데 이번 기회에 제대로 해보자

## 구현 코드 

```java
import java.util.ArrayList;
import java.util.List;
class Solution {
    static List<String> list = new ArrayList<>();;
    static String [] words = {"A", "E", "I", "O", "U"};
    public int solution(String word) {
        int answer = 0;
        dfs("", 0);
        int size = list.size();
        for (int i = 0; i < size; i++) {
            if (list.get(i).equals(word)) {
                answer = i;
                break;
            }
        }
        return answer;
    }

    static void dfs(String str, int len) {
        list.add(str);
        if (len == 5) return;
        for (int i = 0; i < 5; i++) {
            dfs(str + words[i], len + 1);
        }
    }
}
```

![](https://i.imgur.com/7fm1NPz.png)

## 정리

1. 완전 탐색에 대해서 여태까지 어려워서 포기했었는데 이제 받아드리고 그냥 공부 해야함 
2. static 으로 선언해서 사용하는걸 몰랐는데 알게 되었다. -> 간단하게 설명하면 전체적으로 공유된다.
3. 완전탐색으로 리스트를 만들어서 탈출한 후 다시 비교하는 방식이였는데 완전탐색에 대한 자세한 이해가 없으니 경우의 수 같이 이상한 접근을 했었다.
4. 어차피 코테를 공부하면 해당 부분은 피할 수 없으니 그냥 완전히 이해될 때까지 완전탐색으로 공부를 해야겠다고 생각했다 ㅜㅜ
5. https://school.programmers.co.kr/learn/courses/30/parts/12230 해당 문제를 풀어보고 백준까지 마스터하면 완전탐색으로 슬퍼할 일은 없겠지..?

