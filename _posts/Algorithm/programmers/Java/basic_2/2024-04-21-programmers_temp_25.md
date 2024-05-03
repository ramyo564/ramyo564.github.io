---
layout: single
title: "[알고리즘] 달리기 경주"
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

[문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/178871?language=java)

## 풀이 접근

1. 그냥 딱 봤을 때 링크드 리스트 문제라고 생각했고 시간복잡도도 `O(n*m)` 이라서 충분히 통과할 줄 알았다.
	1. n = players , m = callings
2. 근데 아래와 같이 통과를 하지 못 했다.

## 구현 코드 (실패)

```java
import java.util.Arrays;
import java.util.LinkedList;

class Solution {
    public String[] solution(String[] players, String[] callings) {

    LinkedList<String> linkedList = new LinkedList<>(Arrays.asList(players));
    for (String s : callings){
      if(linkedList.contains(s)){
        // 해당 요소의 인덱스를 찾음
        int index = linkedList.indexOf(s);
        // 해당 요소를 제거하고, 다시 그 앞에 삽입하여 위치 변경
        linkedList.add(index - 1, linkedList.remove(index));
      }
    }
    return linkedList.toArray(new String[0]);
  }

}

```

![](https://i.imgur.com/fRiaEob.png)

## 원인

LinkedList 에서 contains() 및 indexof() 는 각각 O(n) 시간 복잡도를 갖는다. 여기서 HashSet을 사용한다면 이 부분을 O(1) 로 줄일 수 있다.

```java
import java.util.*;

class Solution {
    public String[] solution(String[] players, String[] callings) {

        LinkedList<String> linkedList = new LinkedList<>(Arrays.asList(players));
        Set<String> playerSet = new HashSet<>(Arrays.asList(players)); 
        // HashSet으로 players 배열의 요소를 저장

        for (String s : callings) {
            if (playerSet.contains(s)) { 
            // O(1) 시간 복잡도로 요소의 존재 여부 확인
                // 해당 요소의 인덱스를 찾음
                int index = linkedList.indexOf(s);
                // 해당 요소를 제거하고, 다시 그 앞에 삽입하여 위치 변경
                linkedList.add(index - 1, linkedList.remove(index));
            }
        }
        return linkedList.toArray(new String[0]);
    }
}
```

![](https://i.imgur.com/wArCS2q.png)

예상대로 줄긴 했지만 링크드 리스트보다 효율적인 자료구조를 사용해서 풀어야하는데 해시맵 밖에 생각이 안났다.   
처음에 해시맵을 생각하기는 했었지만 복잡해질 것 같아서 선택을 안했는데 시간도 다 써서 다른 사람은 어떻게 해결했는지 확인해 봤다.   

```java
import java.util.*;

class Solution {
    public String[] solution(String[] players, String[] callings) {

        int numOfPlayers = players.length;
        Map<String, Integer> ranking = new HashMap<>();
        
        for (int i=0; i<numOfPlayers ; i++) {
            ranking.put(players[i], i);
        }
        

        
        //경주 진행
        for (String player : callings) {
            //1) player의 이름에 해당하는 value를 저장한다. 
            int playerRanking = ranking.get(player);
            
            //2) player보다 앞에 있는 사람을 발견하고, value를 변경함
            String frontPlayer = players[playerRanking-1];
            
            ranking.replace(frontPlayer, playerRanking);
            players[playerRanking] = frontPlayer;
            
            //3) player의 랭킹을 앞으로 변경함.
            ranking.replace(player, playerRanking-1);
            players[playerRanking-1] = player; 
        }
        
        return players;
    }
}
```

- hashMap에 replace 메소드를 사용해서 해결
## 정리

- hashMap에 대해서 잘 안다고 생각했는데 replace를 오늘 처음 알게 되었다. 
- 다른 사람이 풀어놓은 답도 대충 보고 안다고 생각하지 말고 자세히 봐야겠다.





