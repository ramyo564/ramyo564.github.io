---
layout: single
title: "[알고리즘] 선택의 기로"
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
# 정렬 문제

[문제링크](https://www.acmicpc.net/problem/30970)

## 풀이 접근

1. 입력 받은 값을 a,b 를 저장하는 Data 클래스를 만들어주고 출력할 때 편하게 하기 위해 toString 매소드를 다시 정의해준다.
2. ArrayList의 리턴 값을 Data로 해주고 반복문을 돌면서 모든 데이터를 Data에 넣어준다.
3. 완성된 ArrayList를 sort해주면 끝

## 구현 코드 

```java
import java.io.*;
import java.util.*;

public class Main {

  static class Data {
    int a, b;
    public Data(int a, int b) {
      this.a = a;
      this.b = b;
    }
    @Override
    public String toString() {
      return a +" "+ b;
    }
  }
  public static void main(String[] args) throws IOException {
    BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    int n = Integer.parseInt(br.readLine());
    StringTokenizer st;

    List<Data> list = new ArrayList<>();

    for(int i=0;i<n;i++) {
      st = new StringTokenizer(br.readLine());
      int a = Integer.parseInt(st.nextToken());
      int b = Integer.parseInt(st.nextToken());
      list.add(new Data(a, b));
    }
    //1) 고품질, 가격 낮음
    list.sort((o1, o2) -> o1.a == o2.a ? o1.b - o2.b : o2.a - o1.a);
    System.out.println(list.get(0)+" "+list.get(1));
    //2) 가격 낮음, 고품질
    list.sort((o1, o2) -> o1.b == o2.b ? o2.a - o1.a : o1.b - o2.b);
    System.out.println(list.get(0)+" "+list.get(1));
    br.close();
  }
}

```


![](https://i.imgur.com/vKjkxuG.png)

## 정리

- 람다식과 삼항연산자로 처리했다.
- 람다식 오름차순과 내림차순 정리!

```java
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        Integer[] numbers = {3, 2, 1, 5, 4};

		// 오름차순 람다식 정렬
        Arrays.sort(numbers, (a, b) -> a - b);
        
        System.out.println(Arrays.toString(numbers)); 
        // [1, 2, 3, 4, 5]
        
        // 내림차순 람다식 정렬
        Arrays.sort(numbers, (a, b) -> b - a);
        
        System.out.println(Arrays.toString(numbers)); 
        // [5, 4, 3, 2, 1]
    }
}
    }
}
```
