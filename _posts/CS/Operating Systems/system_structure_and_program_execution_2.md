---
layout: single
title: " [운영체제] 시스템 구조(2)"
categories: CS
tags:
  - CS
  - OperatingSystems
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
# 동기식 입출력과 비동기식 입출력

- 동기식 입출력 (synchronous I/O)
	- I/O 요청 후 입출력 작업이 완료된 후에야 제어가 사용자 프로그램에 넘어감
	- 구현 방법 1
		- I/O가 끝날 때까지 CPU를 낭비시킴
		- 매시점 하나의 I/O만 일어날 수 있음
	- 구현 방법 2
		- I/O가 완료될 때까지 해당 프로그램에게서 CPU를 빼앗음
		- I/O 처리를 기다리는 줄에 그 프로그램을 줄 세움
		- 다른 프로그램에게 CPU를 줌
- 비동기식 입출력 (asynchronous I/O)
	- I/O가 시작된 후 입출력 작업이 끝나기를 기다리지 않고 제어가 사용자 프로그램에 즉시 넘어감

- 두 경우 모두 I/O의 완료는 인터럽트로 알려줌

![](https://i.imgur.com/qMTqjuC.png)

## DMA(Direct Memory Access)

![](https://i.imgur.com/w714XxJ.png)


원래는 메모리에 접근할 수 있는 장치가 CPU밖에 없음 근데 예를 들어서 키보드 타자 하나의 하나의 요청을 보낼 때마다 인터럽트 요청을해서 CPU가 방해를 너무 많이 당함 -> 그래서 DMA도 메모리에 직접 접근이 가능함 (DMA가 대신 메모리에 카피하고 어느정도가 넘어가면 그 때 CPU에게 일 끝났다고 인터룹트를 걸음)
-> CPU를 좀 더 효율적으로 사용 가능해짐    

- DMA (Direct Memory Access)
	- 빠른 입출력 장치를 메모리에 가까운 속도로 처리하기 위해 사용
	- CPU의 중재 없이 device controller가 device 의 buffer storage의 내용을 메모리에 block단위로 직접 전송
	- 바이트 단위가 아니라 block 단위로 인터럽트를 발생시킴

![](https://i.imgur.com/eCz67bS.png)

## 서로 다른 입출력 명령어

- I/O 를 수행하는 special instruction 에 의해 (좌) ->일반적인 I/O
- Memory Mapped I/O에 의해 (우)

![](https://i.imgur.com/dc0Hf8r.png)

## 저장장치 계층 구조 

![](https://i.imgur.com/826azxH.png)

## 프로그램의 실행 (메모리 load)

![](https://i.imgur.com/GRYLYS4.png)

## 커널 주소 공간의 내용

![](https://i.imgur.com/NF0LjMI.png)


## 사용자 프로그램이 사용하는 함수

- 함수 
	- 사용자 정의 함수
		- 자신의 프로그램에서 정의한 함수
	- 라이브러리 함수
		- 자신의 프로그램에서 정의하지 않고 갖다 쓴 함수
		- 자신의 프로그램의 실행 파일에 포함되어 있다.
	- 커널 함수
		- 운영체제 프로그램의 함수
		- 커널 함수의 호출 = 시스템 콜

![](https://i.imgur.com/hA4m4uq.png)

## 프로그램의 실행

![](https://i.imgur.com/RkpPbL9.png)


[출처 링크](https://core.ewha.ac.kr/publicview/C0101020140314151238067290?vmode=f)