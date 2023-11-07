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
# 입출력이란?
2023-11-07-

## 콘솔 입력

- 입출력 방식 중 콘솔 입력 방법

```java
System.in.read()
InputStreamReader reader = ...
BufferedReader br = ...
Scanner ...
```


## 콘솔 출력

- 입출력 방식 중 콘솔 출력 방법

```java
System.out.println(...);
System.out.print(...);
System.out.printf(...);
```

```java
// Java 프로그래밍 - 입출력_1  
  
import java.io.BufferedReader;  
import java.io.IOException;  
import java.io.InputStreamReader;  
import java.util.Scanner;  
  
public class Main {  
  
    public static void referInputStream() throws IOException {  
//      System.in  
        System.out.println("== System.in ==");  
        System.out.print("입력: ");  
        int a = System.in.read() - '0';  
        System.out.println("a = " + a);  
        System.in.read(new byte[System.in.available()]);  
  
//      InputStreamReader  
        System.out.println("== InputStreamReader ==");  
        InputStreamReader reader = new InputStreamReader(System.in);  
        char[] c = new char[3];  
        System.out.print("입력: ");  
        reader.read(c);  
        System.out.println(c);  
  
//      BufferedReader  
        System.out.println("== BufferedReader ==");  
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));  
        System.out.print("입력: ");  
        String s1 = br.readLine();  
        System.out.println("s1 = " + s1);  
    }  
  
    public static void main(String[] args) throws IOException {  
  
//      1. 입력  
//      1-1. 다른 입력 방식 참고  
//        referInputStream();  
  
//      1-2. Scanner  
        System.out.println("== Scanner ==");  
        System.out.print("입력1: ");  
        Scanner sc = new Scanner(System.in);  
        System.out.println(sc.next());  
        sc.nextLine();  
  
        System.out.print("입력2: ");  
        System.out.println(sc.nextInt());  
        sc.nextLine();  
  
        System.out.print("입력3: ");  
        System.out.println(sc.nextLine());  
  
  
//      참고) 정수, 문자열 변환  
        int num = Integer.parseInt("12345");  
        String str = Integer.toString(12345);  
          
//      2. 출력  
        System.out.println("== 출력 ==");  
        System.out.println("Hello");  
        System.out.println("World!");  
  
        System.out.print("Hello ");  
        System.out.print("World!");  
  
        System.out.printf("Hello ");  
        System.out.printf("World!");  
        System.out.println();  
  
        String s = "자바";  
        int number = 3;  
  
        System.out.println(s + "는 언어 선호도 " + number + "위 입니다.");  
        System.out.printf("%s는 언어 선호도 %d위 입니다.\n", s, number);  
  
        System.out.printf("%d\n", 10);  
        System.out.printf("%o\n", 10);  
        System.out.printf("%x\n", 10);  
  
        System.out.printf("%f\n", 5.2f);  
          
        System.out.printf("%c\n", 'A');  
        System.out.printf("%s\n", "안녕하세요");  
  
        System.out.printf("%5d\n", 123);  
        System.out.printf("%5d\n", 1234);  
        System.out.printf("%5d\n", 12345);  
  
        System.out.printf("%.2f\n", 1.126123f);  
    }  
}
```

## 파일출력

- 입출력 방식 중 파일로 출력하는 방법

```java
FileOutputStream ...
FileWriter ...
PrintWriter ...
```


## 파일입력

- 입출력 방식 중 파일로부터 입력 받는 방법

```java
FileInputStream ..
BufferedReader ...
```