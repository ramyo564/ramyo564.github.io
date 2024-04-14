---
layout: single
title: "[알고리즘] JadenCase 문자열 만들기"
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
# 문자열 문제

[문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/12951?language=java)

## 풀이 접근

1. 문자열과 공백이 중요한데 자바에서는 String 과 char 구조에 따라 조금씩 다르다. String 데이터가 숫자인지 확인하려면 정규식을 사용하거나 char 데이터로 변환해야되는 번거로움이 있어 그냥 char 로 처음부터 바꿔서 진행
2. Boolean 타입을 사용해서 공백이 발생하면 true 에서 false로 바꿔서 소문자는 그대로 진행하고 공백이 나올경우 다시 true로 바꿔줘서 첫 글자가 대문자로 변환하도록 한다.
3. 주의할 사항은 첫 글자가 숫자거나 공백일 경우인데 아래와 같이 처리해서 문제를 풀었다.

## 구현 코드 

```java
class Solution {
    public String solution(String s) {
        StringBuilder z = new StringBuilder();

        char[] charArray = s.toCharArray();
        Boolean flag = true;

        for (char c : charArray) {
            if (flag && !Character.isDigit(c) && c != ' ') {
                z.append(Character.toUpperCase(c));
                flag = false;
            } else if (c == ' ') {
                flag = true;
                z.append(c);
            } else {
                z.append(Character.toLowerCase(c));
                flag = false;
            }
        }
        return z.toString();
    }
}
```

![](https://i.imgur.com/8jcIw3k.png)


## 삼항 연산자로 더 간단하게 풀기

```java
class Solution {
  public String solution(String s) {
        String answer = "";
        String[] sp = s.toLowerCase().split("");
        boolean flag = true;

        for(String ss : sp) {
            answer += flag ? ss.toUpperCase() : ss;
            flag = ss.equals(" ") ? true : false;
        }

        return answer;
  }
}
```

![](https://i.imgur.com/TuSMc2l.png)

## 정리

1. 단순히 알고리즘을 풀 때 반복문을 적게 쓰는 것만 사용했는데 반복문을 적게 사용하더라도 불필요한 연산을 줄일 수 있는 방법도 함께 생각하는데 성능을 높이는 주요한 열쇠라는걸 다시 한 번 알게 되었다.

```java
import java.util.regex.*;

class Solution {
    public String solution(String s) {
        Matcher m = Pattern.compile("\\b([\\w])([\\w]*)").matcher(s);
        while (m.find()) {
            s = s.replaceAll("\\b" + m.group(), m.group(1).toUpperCase() + m.group(2).toLowerCase());
        }
        return s;
    }
}
```

![](https://i.imgur.com/dp8w7AG.png)
