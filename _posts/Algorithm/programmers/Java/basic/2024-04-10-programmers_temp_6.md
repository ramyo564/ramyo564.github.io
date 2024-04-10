---
layout: single
title: "[알고리즘] 이진 변환 반복하기"
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
# 월간 코드 첼린지 시즌1


[문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/70129?language=java)

## 풀이 접근

- 0을 제거해주고 제거한 문자열의 길이를 측정한 뒤 그 값을 이진수로 바꿔서 1이 될 때까지 계속 반복한다

1. while 문으로 1이 아니거나 0이 포함되있을 경우 계속 값을 업데이트
2. 문자열에 0이 포함되어 있을 경우 0을 제거
3. 0을 제거한 후 이전 값과의 길이 변화를 측정해서 제거된 0을 카운팅
4. 이진수로 바꾼 숫자 값을 문자열로 변경해준뒤 횟수를 카운팅
5. 만약 이진수로 바꾼 값 혹은 초기 값에 0이 포함되어 있지 않을 경우 바로 문자열을 측정한뒤 이진수로 바꾸고 횟수를 업데이트해줌

## 구현 코드

```java
class Solution {
    public int[] solution(String s) {
        int cnt = 0;
        int removeZero = 0;

        while (Long.parseLong(s) != 1 || s.contains("0")){
            if(s.contains("0")){
                String temp = s.replaceAll("0","");
                int length = temp.length();
                int originLength = s.length();
                removeZero += originLength - length;

                s = String.valueOf(Integer.toBinaryString(length));
                cnt ++;
            } else {
                int length = s.length();

                s = String.valueOf(Integer.toBinaryString(length));
                cnt ++;
            }

        }

        return new int[]{cnt, removeZero};
    }
}

```

- 테스트 코드에서는 문제가 없었는데 제출하고 보니 통과를 하지 못 하는 케이스가 너무 많았다.
- 제한사항을 보니 s의 길이가 150,000 이하였고 문자열의 길이가 150,000 이라면 애초부터 위에 방식으로는 문제를 푸는게 불가능하다.
- long 타입이 -9,223,372,036,854,775,808부터 9,223,372,036,854,775,807 까지니까 길이가 19자리가 끝이다.
- 반복문의 조건에서 숫자가 문제니 탈출조건을 문자열 1과 같은지로 변경해준다.

```java
while (Long.parseLong(s) != 1 || s.contains("0"))
위에서 밑에껄로 조건 변경

while (!s.equals("1"))
```

![](https://i.imgur.com/LLwr1je.png)


## 정리

```java
class Solution {
    public int[] solution(String s) {
        int[] answer = new int[2];
        int temp;
        while( !s.equals("1") ) {
            answer[1] += s.length();
            s = s.replaceAll("0", "");
            temp = s.length();
            s = Integer.toBinaryString(temp);
            
            answer[0]++;
            answer[1] -= temp;
        }
        return answer;  
    }
}
```

비슷하지만 훨씬 더 가독성이 좋은 코드, 성능도 비슷비슷

![](https://i.imgur.com/aak2vMw.png)


```java
class Solution {
static int removeZeroCnt = 0;
    static int totalCnt = 0;

    static int[] solution(String s) {
        int[] answer = new int [2];
        delete(s);
        answer[0] = totalCnt;
        answer[1] = removeZeroCnt;
        return answer;
    }

    private static String delete(String binary) {

        //binary 값이 1이 되면 호출한 부분으로 돌아간다.
        if(binary.equals("1"))
            return "";

        int zerocount = 0;
        //0의 개수 출력
        for(int i = 0; i < binary.length(); i++) {
            if(binary.charAt(i) == '0')
                zerocount++;
        }
        //cnt는 총 0을 뺀 개수
        removeZeroCnt += zerocount;
        //length는 0제거 후의 길이
        int length = binary.length() - zerocount;
        //allcount는 몇 번 반복됬는지 확인하는 변수
        totalCnt++;

        return delete(Integer.toString(length,2));
    }
}

```

재귀로 풀은 케이스인데 성능은 3배에서 5배이상 차이가 난다 ㄷㄷ   
재귀로 풀어도 빠른 이유는 굳이 0을 바꿔주는 로직 없이 카운팅만 하고 오리지널 값과 비교하는 게 끝이기 때문이다.    


![](https://i.imgur.com/eXaUeel.png)

## 최종 

```java
class Solution {
    public int[] solution(String s) {
          int cnt = 0;
        int removeZero = 0;

        while (!s.equals("1")){

            int zerocount = 0;
            
            for(int i = 0; i < s.length(); i++) {
                if(s.charAt(i) == '0')
                    zerocount++;
            }
            removeZero += zerocount;

            s = String.valueOf(Integer.toBinaryString(
                    s.length() - zerocount));
            cnt ++;


        }

        return new int[]{cnt, removeZero};
    }
}
```

재귀를 사용하지 않고 While 문 + 0을 카운팅 하는 로직으로 다시 짜보았다.

![](https://i.imgur.com/NTSJRDl.png)

엄청난 성능향상은 아니였지만 확실히 재귀로 푸는 것보다 성능은 좀 더 좋았다.