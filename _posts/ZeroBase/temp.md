---
layout: single
title: "[개발일기] 백엔드 신입 개발자가 쌓아야 하는 역량은?"
categories: 개발일기
tags:
  - Java
  - 제로베이스
  - 백엔드
  - Spring
  - 개발자
  - 백엔드공부
  - 백엔드스쿨
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
# 
# 핀테크 
1. 계좌 검색 기능
2. 계좌 관리 (생성 / 삭제 / 금액 인출 / 금액 입금)
3. 송금 기능
4. 로그인 / 로그아웃에 따른 계좌 접근 허가 기능 구현

## 1. 계좌 검색 기능
1. 거래 내역 검색 
   - 기본 -> 최근날짜순 정렬
   - 입금, 출금 순으로 정렬 기능
   - 날짜 지정 기간 거래내역 정렬
   - 보내는 사람 이나 받는 사람 조건으로 검색
  
## 2. 계좌 관리
1. 계좌 생성
2. 계좌 삭제(해지)
    - 잔액이 없을 경우에만 삭제
3. 금액 인출
4. 금액 입금

## 3. 송금 기능
1. 부분 취소

## 4. 로그인 / 로그아웃
1. 계좌 접근 허가 기능 구현

## Build
- Java 17
- Gradle
- H2 -> 추후 mysql로 db 변경

## Trouble Shooting

## 첫번째 코드 할인행사

```java
HashMap<String,Integer> map = new HashMap<>();  
        HashMap<String,Integer> map_check = new HashMap<>();  
        for (int i = 0; i < want.length; i++) {  
            map.put(want[i], number[i]);  
        }  
  
        int day = 1;  
  
        for (int i = 0; i < discount.length; i++) {  
            if(map.containsKey(discount[i])){  
  
                for (int j = 0; j < 10; j++) {  
                    map_check.put(  
                            discount[i+j],  
                            map_check.getOrDefault(  
                                    discount[i+j],0)+1);  
                }  
                boolean isEqual = map.size() ==  
                        map_check.size() && map.entrySet().stream()  
                        .allMatch(e ->  
                                e.getValue().equals(  
                                        map_check.get(e.getKey())));  
  
                if (isEqual) {  
                    return day;  
                } else {  
                    map_check.clear();  
                    day++;  
                }  
  
            }else {  
                day++;  
            }  
        }  
  
        return 0;  
    }  
}
```


```java
package programmers.middle;  
  
import java.util.HashMap;  
  
public class prog_5 {  
  
    /*  
    https://school.programmers.co.kr/learn/courses/30/lessons/131127     */    public static void main(String[] args) {  
  
        String[] want = {"banana", "apple", "rice", "pork", "pot"};  
  
        int[] number = {3, 2, 2, 2, 1};  
  
        String[] discount = {  
                "chicken", "apple", "apple", "banana", "rice", "apple", "pork", "banana", "pork", "rice", "pot", "banana", "apple", "banana"  
        };  
  
        System.out.println(solution(want, number, discount));  
  
    }  
    static int solution(String[] want, int[] number, String[] discount){  
  
        HashMap<String,Integer> map = new HashMap<>();  
        HashMap<String,Integer> map_check = new HashMap<>();  
        for (int i = 0; i < want.length; i++) {  
            map.put(want[i], number[i]);  
        }  
  
        int answer = 0;  
  
        for (int i = 0; i < discount.length-9; i++) {  
            if (map.containsKey(discount[i])) {  
                for (int j = 0; j < 10; j++) {  
                    map_check.put(  
                            discount[i + j],  
                            map_check.getOrDefault(  
                                    discount[i + j], 0) + 1);  
                }  
                boolean isEqual = map.size() ==  
                        map_check.size() && map.entrySet().stream()  
                        .allMatch(e ->  
                                e.getValue().equals(  
                                        map_check.get(e.getKey())));  
  
                if (isEqual) {  
                    answer ++;  
                    map_check.clear();  
                } else {  
                    map_check.clear();  
                }  
  
            }  
        }  
  
        return answer;  
    }  
}
```


```java
package programmers.middle;  
  
import java.util.Arrays;  
  
public class prog_6 {  
  
    //https://school.programmers.co.kr/learn/courses/30/lessons/42842  
  
    public static void main(String[] args) {  
        int brown = 24;  
        int yellow = 24;  
  
  
        System.out.println(Arrays.toString(solution(brown, yellow)));  
    }  
  
    static int[] solution(int brown, int yellow){  
  
        int result = brown + yellow;  
  
        double squareRoot = Math.sqrt(result);  
        if (squareRoot == Math.floor(squareRoot)) {  
            int num = (int) squareRoot;  
  
            return new int[]{num, num};  
  
        } else {  
            int num_1 = (int) squareRoot;  
            int num_2 = (int) squareRoot;  
            int cnt = 0;  
            for (int i = 0; i < num_1; i++) {  
                if((num_2+cnt)*(num_1) == result){  
                    return new int[]{num_2+cnt, num_1};  
                } else {  
                    cnt++;  
                }  
            }  
        }  
        return null;  
    }  
}
```