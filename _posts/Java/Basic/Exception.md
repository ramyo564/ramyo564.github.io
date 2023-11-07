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
# 예외처리란?
2023-11-07-

## 예외 (Exception)

- 정상적이지 않은 Case
	- 0으로 나누기
	- 배열의 인덱스 초과
	- 없는 파일 열기
	- ...
- finally 
	- 예외 발생 여부와 관계없이 항상 실행되는 부분

```java

try {
	예외가 발생할 수도 있는 부분;
}catch (예외케이스1) {
	예외 case 1이 발생해야 실행되는 부분;
}catch (예외케이스2){
	예외 case 2이 발생해야 실행되는 부분;
}finally {
	항상 실행되는 부분;
}
```

## throw, throws

- throw : 예외를 발생 시킴
- throws : 예외를 전가 시킴

```java
함수이름 () {
	throw new Exception();
}

함수이름 () throws Exception {
 ...
}
```