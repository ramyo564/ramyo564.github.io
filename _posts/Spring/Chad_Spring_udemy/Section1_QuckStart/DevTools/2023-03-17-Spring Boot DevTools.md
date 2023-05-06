---
layout: single
title: "[Spring] Spring Boot DevTools? 인텔리제이 커뮤니티버전 DevTools 설정방법"
categories: Spring
tag: [Java,"[Spring] Devtools"]
toc: true
toc_sticky: true
author_profile: false
sidebar:
     

---

## The problem

- When running Spring Boot applications
  - If you make changes to your source code
  - Then you have to manually restart your application

## Solution: Spring Boot Dev Tools

- Spring-boot-devtools to the rescue!
- Automatically restart your application when code is updated
- Simply add the dependency to your POM file
- No need to write additional code
- For IntelliJ, need to set additional configurations

## Spring Boot DevTools

- Adding the dependency to your POM file

```java
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtoos</artifactId>
        </dependency>
```

- 근데 IntelliJ 무료 버전은 DevTools 기본으로 제공 안해줌..
- Mac 은 Preferences -> Build, Execution, Deployment > Compiler
- Window 는 File -> Settings -> Build, Execution, Deployment > Compiler
  - Check box: Build project automatically


![](https://i.imgur.com/XnbBQNm.png)

- Additional setting
- Select: Preferences -> Advanced Settings
  - Check box : Allow auto-make-to ..

  ![](https://i.imgur.com/LYrporD.png)

  

이렇게 2개 체크하고 밑에 코드 pom.xml에 추가해 주면 끝 (새로고침 잊지말기)

```java
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
        </dependency>
```

출처 유데미
