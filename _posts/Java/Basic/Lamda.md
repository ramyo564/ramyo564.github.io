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

# 람다표현식이란?

메소드 대신 하나의 식으로 표현하는 것

익명함수 (Anonymous function)
```java
변환타입 메소드 이름(매개변수,...){
 실행문
}

public int sum(int x, int y){

}

-> (int x, int y) -> {return x + y};
```

## 람다식 장점

- 일반적으로 코드가 간결해짐
- 코드 가독성이 높아짐
- 생산성이 높아짐

## 람다식 단점

- 재사용이 불가능(익명)
- 디버깅 어려움
- 재귀함수로는 맞지 않음