---
layout: single
title: "[알고리즘] 숫자 짝꿍"
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

[문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/131128)
## 풀이 접근

1. 같은 숫자가 몇 개 있는지 센 후 그중에서 가장 숫자를 크게 만들어주면 끝이다.
2. 최적화를 위해서는 숫자를 해시맵을 사용하지 않고 반복문과 min 으로도 만들 수 있지만 뭔가 너무 귀찮았고 무엇보다 제한사항이 3,000,000 이라 해시맵을 사용하면 O(n^2) 을 넘지 않게 설계할 수 있을 것 같았다.
3. 우선 X, Y의 값을 해시맵으로 각각의 숫자가 몇 개있는지 카운팅하고 해시맵에서 둘 다 키 값이 있을 경우에 둘중 가장 작은 값으로 반복문을 통해 StringBuilder에 넣어서 리턴해주면 끝
4. 예외 케이스로 숫자가 000 이렇게 나올 경우 "000" 이렇게 나오는데 이럴 때만 "0" 으로 나올 수 있게 처리

## 구현 코드 

```java
import java.util.HashMap;

class Solution {
    public String solution(String X, String Y) {
    String answer = "-1";
    HashMap<Character, Long> mapX = new HashMap<>();
    HashMap<Character, Long> mapY = new HashMap<>();


    for (int i = 0; i < X.length(); i++) {
      mapX.put(X.charAt(i), mapX.getOrDefault(X.charAt(i),0L)+1);
    }
    for (int i = 0; i < Y.length(); i++) {
      mapY.put(Y.charAt(i), mapY.getOrDefault(Y.charAt(i),0L)+1);
    }

    boolean flag = false;
    StringBuilder sb = new StringBuilder();
    for (int i = 9; i >= 0; i--) {
      char charValue = (char) (i + '0');
      if(mapX.containsKey(charValue) && mapY.containsKey(charValue)){
        if(!flag && charValue == '0'){
          return "0";
        }
        long minValue = Math.min(mapX.get(charValue), mapY.get(charValue));
        for (int j = 0; j < minValue; j++) {
          sb.append(i);
        }
        flag = true;
      }
    }

    if(flag){
      return sb.toString();
    }else {
      return answer;
    }
  }
}
```

![](https://i.imgur.com/iAg9G8B.png)

## 성능 좋은 풀이

```java

class Solution {
    public String solution(String X, String Y) {
        StringBuilder answer = new StringBuilder();
        int[] x = {0,0,0,0,0,0,0,0,0,0};
        int[] y = {0,0,0,0,0,0,0,0,0,0};
        for(int i=0; i<X.length();i++){
           x[X.charAt(i)-48] += 1;
        }
        for(int i=0; i<Y.length();i++){
           y[Y.charAt(i)-48] += 1;
        }

        for(int i=9; i >= 0; i--){
            for(int j=0; j<Math.min(x[i],y[i]); j++){
                answer.append(i);
            }
        }
        if("".equals(answer.toString())){
           return "-1";
        }else if(answer.toString().charAt(0)==48){
           return "0";
        }else {
            return answer.toString();
        }
    }
}
```

![](https://i.imgur.com/8EmJb2I.png)

## 정리

- 제한 사항을 꼼꼼히 보고 문제를 잘 이해하는 게 중요하다.
- 테스트 케이스에서 "00" 을 리턴하는걸 확인하지 못 했으면 해시맵을 사용했을 때 이에 대한 케이스 예외를 생각하지 못하고 해결하지 못 했을 수도 있다.
- 배열로 문제를 푸는게 생각 나면 귀찮더라도 배열부터 접근하는 게 좋은 방법 일 수도 있다(너무 복잡해지지만 않는다면!!)

