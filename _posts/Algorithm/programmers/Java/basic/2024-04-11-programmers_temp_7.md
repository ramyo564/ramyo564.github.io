---
layout: single
title: "[알고리즘] 기능개발"
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
# 배열 문제

[문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/42586)

## 풀이 접근 (실패)

1. progress와 speed 모두 arrayList에 담아준다.
2. 각 날짜마다의 업데이트를 진행해준다.
3. 반복문을 진행하면서 맨 처음 0번 인덱스가 100을 초과하는지 확인한다
4. 초과할 경우 그 다음에도 연속으로 100을 초과하는지 확인하고 실패할 경우 break;
5. 2번 부터 다시 반복

## 구현 코드 (실패코드)

```java
import java.util.*;
class Solution {
    public ArrayList<Integer> solution(int[] progresses, int[] speeds) {
        ArrayList<Integer> progList = new ArrayList<>();
        ArrayList<Integer> speedList = new ArrayList<>();


        for (int i : progresses) {
            progList.add(i);
        }
        for (int i : speeds) {
            speedList.add(i);
        }

        ArrayList<Integer> list = new ArrayList<>();
        int cnt = 0;

        while (!progList.isEmpty()) {
            for (int z = 0; z < progList.size(); z++) {
                int prog = progList.get(z);
                int speed = speedList.get(z);
                progList.set(z, prog + speed);
            }
            for (int z = 0; z < progList.size(); z++) {
                int prog = progList.get(z);

                if (prog >= 100 && z == 0) {
                    cnt++;
                    progList.remove(0);

                    for (int i = 0; i < progList.size(); i++) {
                        if (progList.get(i) >= 100) {
                            cnt++;
                            progList.remove(i);
                            i--;
                        } else {
                            break;
                        }
                    }
                    list.add(cnt);
                    cnt = 0;
                }
            }
        }
        return list;
    }
}
```

![](https://i.imgur.com/Zmk6qbJ.png)

반례에 나오는 테스트 케이스를 모두 실험했으나 정상적으로 작동했다.   
로직이 틀렸을 수도 있지만 정답 코드와 비교 했을 때 성능 자체가 일단 별로 좋지 않았다.   

한 시간동안 푼거라 결국에는 틀렸다.  
다시 복귀 해보니 자료구조상 삭제가 계속 이루어지는걸 감안한다면 ArrayList는 좋지 않은 선택이였다.  
해당 로직에서 자료구조만 Queue 로 바꾼다면 분명히 더 좋은 결과가 있을거라고 생각했다.
## 정리

