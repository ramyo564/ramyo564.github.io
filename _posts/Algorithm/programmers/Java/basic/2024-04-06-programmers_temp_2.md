---
layout: single
title: "[알고리즘] 문자열 내 마음대로 정렬하기"
categories: Algo
tags:
  - Java
  - 프로그래머스
  - 연습문제
  - 99일지
  - 99클럽
  - 코딩테스트
  - 개발스터디
  - 항해
  - TIL
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
# https://school.programmers.co.kr/learn/courses/30/lessons/12915
## 문제

[문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/12915)
## 풀이 접근

1. 주어진 배열에서 각 단어에서 인덱스 만큼 이동한 철자를 기준으로 정렬한다 -> sun, bed, car / 인덱스는 1 이라고 한다면 u,e,a 기준으로 단어를 정렬
2. 만약 같은 기준으로 정렬했는데 abce, abcd 이런식으로 되는 값이 있다면 사전순으로 abcd가 먼저오게됨
3. 인덱스 문자가 같은 문자열이 여러개일 경우 사전순으로 정렬되므로 하나의 키 값으로 여러 개의 값이 들어가는 해시맵이 딱 적당하다고 생각

반복문 한 번과 해시맵을 사용하면 O(n) 으로 끝낼 수 있을 것 같았다.   
- 파이썬 딕셔너리처럼 키 값에 딕셔너리(list)를 달아주는 자료구조를 만들어준다.
- 각 단어의 인덱스만큼 이동한 문자를 추출한 후 키 값을 설정하고 반복문을 돌면서 해당 키 값이 중복될 경우 그 뒤에 값을 이어줌
- 완성된 자료에서 키 값만 뽑아서 정렬을 해준다.
	- 정렬된 키 값은 순서가 보장되므로 해당 키 값으로 뽑아준 벨류 값으로 리스트를 만들어준다.
	- 현재 벨류 값을 다시 정렬해준 다음 ArrayList에 순서대로 넣어주면 주어진 조건에 맞게 순서가 정렬된다.
- 마지막에 배열 형태로 바꿔주면 끝

## 구현 코드

```java
import java.util.*;

class Solution {
    public String[] solution(String[] strings, int n) {
        Map<String, List<String>> resultMap = new HashMap<>();
        for (String item : strings){
            char[] charArray = item.toCharArray();
            char theChar = charArray[n];
            dict(resultMap,String.valueOf(theChar),item);
        }

        // 키 값 순서 정렬
        ArrayList<String> keyList = new ArrayList<>(resultMap.keySet());
        Collections.sort(keyList);

        // 정답 리스트
        ArrayList<String> answerList = new ArrayList<>();

        for (String item : keyList){
	        //키 값에 해당하는 벨류 값을 리스트에 넣어줌
            List<String> tempList = resultMap.get(item);
            // 해당 벨류 값들을 사전 순으로 정렬
            Collections.sort(tempList);
            // 고대로 정답 리스트에 넣어주면 순서 및 조건에 만족함
            answerList.addAll(tempList);
        }
		// 배열로 바꿔줌
        String[] answer = answerList.toArray(new String[0]);

        return answer;
    }
    static void dict(
            Map<String, List<String>> map, String key, String value){
        // 만약 키가 이미 존재한다면 기존 리스트에 값을 추가
        if (map.containsKey(key)) {
            List<String> list = map.get(key);
            list.add(value);
        } else { 
        // 키가 존재하지 않는다면 새로운 리스트를 만들어서 값 추가
            List<String> list = new ArrayList<>();
            list.add(value);
            map.put(key, list);
        }
    }
}
```

## 정리

![](https://i.imgur.com/0M7BpXl.png)

다른 코드들을 보니 확실히 자바스럽게 풀었다기 보다는 파이썬에서 풀던 방식으로 푼 느낌이다.  
아래는 다른 풀이다.

```java
import java.util.*;

class Solution {
  public String[] solution(String[] strings, int n) {
      Arrays.sort(strings, new Comparator<String>(){
          @Override
          public int compare(String s1, String s2){
              if(s1.charAt(n) > s2.charAt(n)) return 1;
              else if(s1.charAt(n) == s2.charAt(n)) return s1.compareTo(s2);
              else if(s1.charAt(n) < s2.charAt(n)) return -1;
              else return 0;
          }
      });
      return strings;
  }
}
```

![](https://i.imgur.com/22mQU3a.png)
위에는 뭔가 자바스럽게 풀은 느낌인데 성능은 비슷하다  


