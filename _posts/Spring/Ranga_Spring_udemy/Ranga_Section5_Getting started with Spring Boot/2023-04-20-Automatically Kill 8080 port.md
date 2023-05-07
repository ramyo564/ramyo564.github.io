---

layout: single
title: " [Spring] Automatically Kill 8080 port "
categories: Spring
tag: [Java,"[Spring] Automatically Kill 8080 port"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Automatically Kill 8080 port
/ Automatically Kill 8080 port /

Spring 을 연습할 때 매번 8080 포트 죽이는거 인터넷에 찾아서 하는거 너무 지겹다 <br> 
그러던 중 batch 파일로 작성해서 그냥 클릭 몇 번으로 해결하는 방법을 찾았다.

```
@ECHO OFF ECHO --------------------------------------------------------- ECHO ------[8080 포트를 사용하는 프로세스를 종료합니다]------- ECHO --------------------------------------------------------- SET killport=8080 for /f "tokens=5" %%p in ('netstat -aon ^| find /i "listening" ^| find "%killport%"') do taskkill /F /PID %%p pause

```


>- 메모장을 열고 위 명령어를 입력한뒤 확장자를 .bat 파일로 저장
>- 그리고 실행하면 알아서  8080 포트를 종료시킨다.

![](https://i.imgur.com/rgLPw0K.png)

![](https://i.imgur.com/AjwV8Hw.png)




참고 : https://immose93.tistory.com/11