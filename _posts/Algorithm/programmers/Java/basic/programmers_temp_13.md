---
layout: single
title: "[알고리즘] 신고 결과 받기"
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
# 해시셋 문제

[문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/150370?language=java)

## 풀이 접근

1. 누가 신고 했는지는 중요하지 않다 하지만 중복 신고는 1회로 처리한다.
2. 해시set 과 해시맵으로 처리하면 간단하게 해결 가능하다.
	1. 중복을 따로 검사할 때 해시셋에서 자체적으로 검사할 때 중복을 무시하므로 이렇게 사용하는게 더 효율적이라고 생각했다.
3. 해시셋으로 중복 신고를 제거해 준 후 해당 자료구조로 각 사용자가 몇 번 신고 당했는지 카운팅 해준다.
4. 마지막으로 id_list 의 순서대로 키 값이 있다면 k 값과 같거나 큰 값은 해당 카운팅을 answer 배열에 넣어주고 키 값이 없다면 신고당한 적이 없는 것이므로 0으로 데이터를 넣어준다.
5. 성능면에서 이렇게 하는 게 가장 좋을 것 같아서 진행했다.

## 구현 코드 

```java
import java.util.Arrays;
import java.util.HashMap;
import java.util.HashSet;
class Solution {
    public int[] solution(String[] id_list, String[] report, int k) {
         int[] answer = new int[id_list.length];

         HashMap<String, HashSet<String>> map = new HashMap<>();

         for(String name : report){
             String[] array = name.split(" ");
             String user = array[0];
             String reported = array[1];

             if (map.containsKey(user)) {
                 HashSet<String> existingValues = map.get(user);
                 existingValues.add(reported);
                 map.put(user, existingValues);
             } else {
                 HashSet<String> newSet = new HashSet<>();
                 newSet.add(reported);
                 map.put(user, newSet);
             }
         }

         HashMap<String, Integer> countMap = new HashMap<>();
         for (HashSet<String> values : map.values()) {
             for (String value : values) {
                 countMap.put(value, countMap.getOrDefault(value, 0) + 1);
             }
         }
   
         for (int i = 0; i < answer.length; i++) {
             if (map.containsKey(id_list[i])) {
                 int cnt = 0;
                 for(String name : map.get(id_list[i])){
                     if(countMap.get(name) >= k){
                         cnt++;
                     }
                 }
                 answer[i] = cnt;
             }else {
                 answer[i] = 0;
             }
         }


         return answer;
     }

}

```

![](https://i.imgur.com/2pT3YSJ.png)

## Stream으로 풀어보기

```java
import java.util.Arrays;
import java.util.HashMap;
import java.util.List;
import java.util.stream.Collectors;

class Solution {
    public int[] solution(String[] id_list, String[] report, int k) {
        List<String> list = Arrays.stream(report).distinct().collect(Collectors.toList());
        HashMap<String, Integer> count = new HashMap<>();
        for (String s : list) {
            String target = s.split(" ")[1];
            count.put(target, count.getOrDefault(target, 0) + 1);
        }

        return Arrays.stream(id_list).map(_user -> {
            final String user = _user;
            List<String> reportList = list.stream().filter(s -> s.startsWith(user + " ")).collect(Collectors.toList());
            return reportList.stream().filter(s -> count.getOrDefault(s.split(" ")[1], 0) >= k).count();
        }).mapToInt(Long::intValue).toArray();
    }
}
```

![](https://i.imgur.com/0J3oxun.png)


## 정리

1. 일반적인 경우에서는 중복 제거에는 해시셋의 성능이 더 좋다는 걸 다시 한 번 알게 되었다.
2. 다른 정답들과 비교 했을 때 성능이 좋아서 뭔가 뿌듯했다.
3. 해시맵과 해시셋은 정말 자유롭게 사용할 수 있는데 완전탐색 부분이랑 재귀도 이렇게 자유롭게 쓸 수 있을 때까지 연습해야겠다.
