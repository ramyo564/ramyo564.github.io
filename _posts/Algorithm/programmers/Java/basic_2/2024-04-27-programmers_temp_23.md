---
layout: single
title: "[알고리즘] 주차 요금 계산"
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

[문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/92341)

## 풀이 접근

1. 해시맵으로 구현을 하려고 했는데 키 해시맵의 경우 차량번호의 정렬 문제가 있었다. -> TreeMap이라는 구조를 사용하면 키 값이 정렬되어 쓸 수 있다!
	1. 해당 자료구조를 사용하면 정렬을 따로 구현할 필요가 없어진다
2. IN일 경우 키 값 + 시간을 저장해 뒀다가 OUT일 경우 분으로 계산한 후 두 번째 TreeMap 에 넣어준다.
3. 해당 데이터는 쓸 모가 없으니 삭제해준다 왜냐하면 또 주차하러 올 수 있기 때문이다.
4. OUT 데이터가 없이 IN 데이터만 있을 경우 23:59에 OUT으로 일괄처리해준다.
5. 트리맵의 경우 키값이 정렬되어 순서대로 나오는데 이 때 기본요금이 초과하는지 확인일 한 후 기본요금 시간이 초과되지 않는 경우는 기본요금으로 일괄 처리해서 ArrayList에 넣어준다.
6. 기본요금을 추가해줄 경우 fees에 있는 데이터데로 처리해준다.
7. 마지막으로 리턴 타입에 맞게 다시 만들어서 출력해주면 끝

## 구현 코드 

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Map;
import java.util.TreeMap;

class Solution {
    public int[] solution(int[] fees, String[] records) {
	    TreeMap<String, String> map = new TreeMap<>();
        TreeMap<String, Integer> result = new TreeMap<>();

        for (int i = 0; i < records.length; i++) {
            String[] list = records[i].split(" ");

            if (list[2].equals("IN")) {
                map.put(list[1], list[0]);
            } else {
                String[] time = map.get(list[1]).split(":");
                // IN
                int hour = Integer.parseInt(time[0]);
                int min = Integer.parseInt(time[1]);
                int total_1 = hour * 60 + min;

                // OUT
                String[] time2 = list[0].split(":");
                int hour2 = Integer.parseInt(time2[0]);
                int min2 = Integer.parseInt(time2[1]);
                int total_2 = hour2 * 60 + min2;

                int parkingTime = total_2 - total_1;
                result.put(list[1], result.getOrDefault(list[1], 0) + parkingTime); // 이 부분을 수정
                map.remove(list[1]);
            }
        }

        for (Map.Entry<String, String> entry : map.entrySet()) {
            String key = entry.getKey();
            String value = entry.getValue();
            String[] time = value.split(":");
            int hour = Integer.parseInt(time[0]);
            int min = Integer.parseInt(time[1]);
            int total_1 = hour * 60 + min;
            int parkingTime = 1439 - total_1;
            result.put(key, result.getOrDefault(key, 0) + parkingTime); // 이 부분을 수정
        }

        ArrayList<Integer> list = new ArrayList<>();

        for (Map.Entry<String, Integer> entry:result.entrySet()) {
            if (entry.getValue() <= fees[0]) {
                list.add(fees[1]);
            } else {
                int fee = fees[1] + (int) Math.ceil((entry.getValue() - fees[0]) / (double) fees[2]) * fees[3];
                list.add(fee);
            }
        }

        int[] intArray = new int[list.size()];
        for (int i = 0; i < list.size(); i++) {
            intArray[i] = list.get(i);
        }
        return intArray;
    }
}
```


![](https://i.imgur.com/PU0Q1YX.png)

## 정리

1. 맨 처음에 Null 포인트 처리를 잘못해서 로컬IDE에서는 통과했지만 제출했을 경우 에러가 났다.
2. 해시맵의 경우 Null 값의 처리를 주의하자!