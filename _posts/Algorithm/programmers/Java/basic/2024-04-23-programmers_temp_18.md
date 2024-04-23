---
layout: single
title: "[알고리즘] 공원 산책"
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
# 구현 문제

[문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/172928)

## 풀이 접근

1. 나와있는 문제대로 그대로 구현만 하면 되는 문제
2. 단순 구현문제이지만 막상 안해보면 풀기 어려움

## 구현 코드 

```java
class Solution {
    public int[] solution(String[] park, String[] routes) {
        int sx = 0;
        int sy = 0;
        
        char[][] arr = new char[park.length][park[0].length()];
        
        for(int i = 0; i < park.length; i++){
            arr[i] = park[i].toCharArray();
            
            if(park[i].contains("S")){
                sy = i;
                sx = park[i].indexOf("S");
            }
        }
    
        for(String st : routes){
            String way = st.split(" ")[0];
            int len = Integer.parseInt(st.split(" ")[1]);
            
            int nx = sx;
            int ny = sy;
            
            for(int i = 0; i < len; i++){
                if(way.equals("E")){
                    nx++;
                }
                if(way.equals("W")){
                    nx--;
                }
                if(way.equals("S")){
                    ny++;
                }
                if(way.equals("N")){
                    ny--;
                }
                if(nx >=0 && ny >=0 && ny < arr.length && nx < arr[0].length){
                    if(arr[ny][nx] == 'X'){
                        break;
                    }
                    // 범위내 & 장애물 x
                    if(i == len-1){
                        sx = nx;
                        sy = ny;
                    }
                }
            }
        }       
        
        int[] answer = {sy, sx};
        return answer;
    }
}


```

![](https://i.imgur.com/jvIYXev.png)



