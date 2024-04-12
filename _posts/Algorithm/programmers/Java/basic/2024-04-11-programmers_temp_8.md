---
layout: single
title: "[알고리즘] 대충 만든 자판"
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
# 해시맵 문제

[문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/160586)

## 풀이 접근 

1. kepmap에 어떤 원소가 있던 알파벳중 가장 빠른 순번으로 조합하면된다.
2. 해당 자료를 해시맵으로 옮겨준다.
3. targets 에 있는 글씨들을 검색하다 완료할 수 없는 경우 초기화해주면서 반복문을 빠져나가준다.
4. N 값이 초기화 되어있지 않다면 N 값을 넣어주고 그렇지 않을 경우는 -1을 넣어준다.

## 구현 코드 

```java
import java.util.Arrays;
import java.util.HashMap;


class Solution {
    public int[] solution(String[] keymap, String[] targets) {
  int[] answer = new int[targets.length];
        HashMap<Character, Integer> map = new HashMap<>();

        for (int i = 0; i < keymap.length; i++) {
            for (int j = 0; j < keymap[i].length(); j++) {
                if (map.containsKey(keymap[i].charAt(j))) {
                    if (map.get(keymap[i].charAt(j)) > j + 1) {
                        map.put(keymap[i].charAt(j), j + 1);
                    }
                } else {
                    map.put(keymap[i].charAt(j), j + 1);
                }

            }
        }

        int N = 0;
        for (int i = 0; i < targets.length; i++) {
            for (int j = 0; j < targets[i].length(); j++) {
                if (map.containsKey(targets[i].charAt(j))) {
                    N += map.get(targets[i].charAt(j));
                }else{
                    N=0;
                    break;
                }
            }
                if (N != 0) {
                    answer[i] = N;
                } else {
                    answer[i] = -1;
                }
                N = 0;

        }
        return answer;
    }
}
```

![](https://i.imgur.com/fKHiZ9t.png)



### 문제점!

전체 문자를 검사하다가 실패했을 경우 초기화 하는 부분을 깜빡해서 실패했었다.  

```java
int N = 0;  
for (int i = 0; i < targets.length; i++) {  
    for (int j = 0; j < targets[i].length(); j++) {  
        if (map.containsKey(targets[i].charAt(j))) {  
            N += map.get(targets[i].charAt(j));  
        }else{  
            N=0;  <--- 이 부분
            break;  
        }  
    }  
        if (N != 0) {  
            answer[i] = N;  
        } else {  
            answer[i] = -1;  
        }  
        N = 0;  
  
}
```
## 정리

1. 귀찮아서 그냥 생각나는데로 코드를 짜다보니 이런 실수를 했다.
2. 노트에 적어도 비슷하지만 적어도 노트에 쓰면 코드를 짰을 때 수정하거나 다시 디버그를 하는 것 보다는 효율적이라고 생각했다.
3. 귀찮아도 노트에 꼭 로직을 정리하고 코드로 옮기자!

