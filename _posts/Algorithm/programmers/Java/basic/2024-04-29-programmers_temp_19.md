---
layout: single
title: "[알고리즘] 1018 체스판 다시 칠하기"
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
# 문제

[문제링크](https://hyunipad.tistory.com/103)

## 풀이 접근

1. 단순한 구현문제인데 솔직히 풀지 못 했다.
2. 다른 사람들이 구현한 답도 보고 무료 강의도 찾아보면서 이해를 하려고 노력했다.
<iframe width="800" height="500" src="https://www.youtube.com/embed/QQUb4b6iWSw" title="체스판 다시 칠하기 백준 1018- 자바 java 문제 풀이" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## 구현 코드 

```java
```java
import java.io.*;
import java.util.StringTokenizer;

public class Main {
    public static int getMinCost(int startrow, int startcol, String[] chessboard) {
        String[] board = {"WBWBWBWB", "BWBWBWBW"}; 

        int whiteVerCount = 0; 

        for(int i = 0; i < 8; i++){ 
            int row = startrow + i; 
            for(int j = 0; j < 8; j++){ 
                int col = startcol + j;

                if(chessboard[row].charAt(col) != board[row%2].charAt(j)){
                    whiteVerCount++;
                }
            }
        }

        return Math.min(whiteVerCount, 64-whiteVerCount);

    }

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        StringTokenizer st = new StringTokenizer(br.readLine());

        int N = Integer.parseInt(st.nextToken()); 
        int M = Integer.parseInt(st.nextToken()); 

        String[] chessboard = new String[N];

        for(int i = 0; i < N; i++){
            chessboard[i] = br.readLine(); 
        }

        br.close();

        int count = Integer.MAX_VALUE;
        for(int i = 0; i <= N-8; i++){
            for(int j = 0; j <= M-8; j++){
                int resultCount = getMinCost(i, j, chessboard);

                if(count > resultCount){
                    count = resultCount;
                }
            }
        }

        bw.write(count + "\n");
        bw.flush();
        bw.close();

    }
}
```

## 정리

- 간혹 구현문제가 너무 어려울 때가 있다... 이럴 때 해결법!
- 우선 문제를 잘 이해하고 해당 문제를 자바로 어떻게 표현할 수 있는지 머릿 속에 넣어야 된다. 예를 들어서 다른 사람의 풀이를 보면서 내가 생각하지 못 했던 부분은 W B를 계속 번갈아가면서 바꿔줘야하는데 이 때 배열로 만들고 인덱스를 모듈러를 사용하면 줄이 바뀔 때 쉽게 전환 되는 점이였다.
- 또한 검은색과 흰색은 항상 반대되는 결과가 나온다는 발상의 접근도 있었는데 이런 부분은 미처 생각하지 못 했었다.
- 뭔가 구현문제에서 막막할 때 포기하지 말고 차근차근히 생각하는 방법을 기르는게 어떻게 보면 가장 좋은 해결방법인 것 같다 ㅜ