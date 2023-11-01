---
layout: single
title: "[Basic Java] Generic"
categories: Basic_Java
tags:
  - Java
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
# 자바로 구구단?

```java
    
public class Main {  
    public static void main(String[] args) {  
  
        System.out.println("[구구단 출력]");  
        int start = 1;  
  
        while (start < 10) {  
            for (int i = 1; i < 10; i++) {  
                int result = i*start;  
                if (result < 10){  
                    String multiplicationTable = String.format("0%s x 0%s = 0%s", i, start, result);  
                    System.out.print(multiplicationTable + "    ");  
                }else {  
                    String multiplicationTable = String.format("0%s x 0%s = %s", i, start, result);  
                    System.out.print(multiplicationTable + "    ");  
                }  
            }  
            System.out.println();  
            start += 1;  
        }  
    
    }  
}
```

더 간단하게 만들 수 있지만 과제 목적 자체가 자바랑 친해지기 + format 함수 사용하기 였다.
예전에 자바 좀 깔짝거려서 다행히 전혀 어렵지는 않았는데 오랜만에 자바로 코드 만들면서 느낀건 간단한 구구단 출력도 자바는 너무 귀찮다.      

근데 또 성능 집착시작하면 C 언어도 결국 배울거 같긴한데 일단 지금 주어진거나 잘하자!

