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

### 문제점!

여기서 문제가 있는데 실제로 큐를 돌려버리면 언제가 처음인지 알 수 없으므로 처음부터 값을 정리한 값을 큐에 넣어야한다.  
예를들어 93,30,55 - 1,30,5 일경우 93은 적어도 7번은 돌아야한다.  
그렇다면 돌아야 하는 값으로 정리를 다시 한다면
7,3,9 의 값이 나오고 이 떄 큐를 돌리면서 앞에 숫자보다 작을 때맨 빼주고 크면 다시 큐에 넣어준다. 이렇게 하면 순서대로 작업을 마치는게 가능하다.

```java
import java.util.*;
class Solution {
    public ArrayList<Integer> solution(int[] progresses, int[] speeds) {  
  
        ArrayList<Integer> list = new ArrayList<>();  
        Queue<Integer> progQ = new LinkedList<>();  
  
        for (int i = 0; i < progresses.length; i++) {  
            if ((100 - progresses[i]) % speeds[i] == 0) {  
                progQ.add((100 - progresses[i]) / speeds[i]);  
            } else {  
                progQ.add((100 - progresses[i]) / speeds[i] + 1);  
            }  
        }  
        System.out.println("q = " + progQ);  
        int x = progQ.poll();  
        int count = 1;  
        while (!progQ.isEmpty()) {  
            if (x >= progQ.peek()) {  
                count++;  
                progQ.poll();  
            } else {  
                list.add(count);  
                count = 1;  
                x = progQ.poll();  
            }  
        }  
        list.add(count);  
        return list;  
    }  
}
```
## 정리

1. 단순하게 반복문만 적게 쓰면 된다고 생각했던게 가장 큰 실패 요인이였다.
2. 해당 문제 같은 경우 읽기 및 쓰기가 반복되는데 이럴 경우 ArrayList 같은 경우 맨 앞에 인덱스를 삭제할 경우 전부 다 다시 앞으로 땡겨와야되기 때문에 굉장히 비효율 적이다.
3. 읽고 쓰기가 많은 경우에는 배열보다는 LinkedList나 해시맵이 더 적당한 걸 다시 한 번 느끼게 되었다.
4. 마지막으로 자료구조를 이용할 때 미리 계산해서 복잡한 로직을 줄일 수 있는지 생각을 다시 해보는 습관을 가져야 겠다고 생각했다.

