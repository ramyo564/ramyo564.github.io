---
layout: single
title: "[알고리즘] 뒤에 있는 큰 수 찾기"
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
# 스텍 문제

[문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/154539)

## 풀이 접근 (실패)

1. 반복문을 돌려주면서 뒤에 숫자보다 같을 경우 continue
2. 뒤에 숫자가 현재 숫자보다 더 클 경우 answer에 넣기
3. 반복문의 마지막 인덱스가 끝이 나올 때까지 답이 안나올 경우 -1
4. 마지막은 무조건 -1 이니 해당 로직만 정리
## 구현 코드 (실패)

```java
class Solution {
    public int[] solution(int[] numbers) {
int[] answer = new int[numbers.length];

        for (int i = 0; i < numbers.length; i++) {
                for (int j = i+1; j < numbers.length; j++) {
                if(numbers[i] == numbers[j]){
                    continue;

                } else if (numbers[i] < numbers[j]) {
                    answer[i] = numbers[j];
                    break;
                } else if(j == numbers.length-1){
                    answer[i] = -1;
                    break;
                }
            }
        }
        answer[numbers.length-1] = -1;

        return answer;
    }

}
```

![](https://i.imgur.com/lUUKi0U.png)


### 문제점!

시간초과가 나왔다...  
제한사항에서 타입부분만 중요하게 봤지 O(n) 이런건 가볍게 흘려본 탓이였다.  
해당 문제의 제약 조건은 `4 ≤ `numbers`의 길이 ≤ 1,000,000`
내가 만든 코드는 O(n)^2 => 1,000,000 x 1,000,000 = 1,000,000,000,000     
최악의 조건은 1,000,000,000,000 -> 10^12  인데 보통 알고리즘은 10^8을 넘으면 안된다고 한다.   
결론적으로 애초에 아무리 못해도 nlog(n) 으로 풀어야 하는 문제..   

머릿속에 O(n) 으로 바꿀 수 있는 자료구조는 Queue 밖에 생각이 안나서 Queue로 바꿔서 도전

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.LinkedList;
import java.util.Queue;

class Solution {
    public ArrayList<Integer> solution(int[] numbers) {

        ArrayList<Integer> list = new ArrayList<>();
        Queue<Integer> q = new LinkedList<>();

        for(int i : numbers){
            q.add(i);
        }

        Arrays.sort(numbers);
        int arraySize = numbers.length-1;
        int Biggest = numbers[arraySize];

        int n = 0;
        int target = q.poll();
        while(!q.isEmpty()){

            if(target == Biggest){
                list.add(-1);
                target = q.poll();
                arraySize--;
                Biggest = numbers[arraySize];
            }else if(target >= q.peek()){
                n++;
                target = q.poll();
            } else if (target < q.peek()) {
                if(n > 0){
                    for (int i = 0; i <= n; i++) {
                        list.add(q.peek());
                    }
                    target = q.poll();
                }else {
                    list.add(q.peek());
                    target = q.poll();
                }

            }

        }
        list.add(-1);
        return list;
    }

}

```

![](https://i.imgur.com/p6WfXxj.png)

## 실패원인 분석

코드가 멸망수준으로 떨어졌다.   
반례를 보니 `{10, 1, 10, 2, 10, 3, 10, 10, 10, 11, 11, 11, 12};`
같은 경우 해당 로직에서 실패할 수 밖에 없었다.   

최종적을 한 시간 동안 풀지 못 해 답안을 찾아보니 보통 스택과 우선순위 큐를 사용해서 문제를 푸는 것 같았다.  


### 우선순위 큐

```java
import java.util.*;

class Solution {
    public int[] solution(int[] numbers) {
        int[] answer = new int[numbers.length];
        Arrays.fill(answer, -1);
        
        PriorityQueue<int[]> queue = new PriorityQueue<>(
            (o1, o2) -> o1[1] - o2[1]
        );
        
        for (int i = 0; i < numbers.length; i++) {
            int temp = numbers[i];
            
            while (!queue.isEmpty()) {
                if (queue.peek()[1] >= temp) break;
                answer[queue.poll()[0]] = temp;
            }
            queue.add(new int[] {i, temp});
        }
        
        return answer;
    }
}
```

![](https://i.imgur.com/CcQk2Q1.png)

### 스텍 

```java
import java.util.*;

class Solution {
    public int[] solution(int[] numbers) {
        int[] answer = new int[numbers.length];
		Arrays.fill(answer, -1);
		
		Stack<Integer> stack = new Stack<>();

		for (int i = 0; i < numbers.length; i++) {
			while (!stack.isEmpty() && numbers[i] > numbers[stack.peek()]) {
				answer[stack.pop()] = numbers[i];
			}
			
			stack.push(i);
		}

		return answer;
    }
}

```

![](https://i.imgur.com/rH2GqfA.png)



## 정리

1. 제한사항으로 인해 결과 값을 생각해내지 못 한게 가장 큰 원인이였다.
2. 자료구조를 효과적으로 사용하지 못 했다.
3. 그냥 막연하게 큐를 이용하면 검색 결과가 O(n) 이라서 해결을 할 수 있을 줄 알았는데 좀 더 깊게 생각해봐야겠다.  
4. 순차적인 로직이 중요할 경우 상황에 따라 큐 보다는 스택이 더 낫다

