---
layout: single
title: "[프로그래머스 카카오 방금 그곡] (알고리즘)"
categories: Algo
tags:
  - Python
  - 프로그래머스
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
## 문제

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/17683)

### 방금그곡

라디오를 자주 듣는 네오는 라디오에서 방금 나왔던 음악이 무슨 음악인지 궁금해질 때가 많다. 그럴 때 네오는 다음 포털의 '방금그곡' 서비스를 이용하곤 한다. 방금그곡에서는 TV, 라디오 등에서 나온 음악에 관해 제목 등의 정보를 제공하는 서비스이다.

네오는 자신이 기억한 멜로디를 가지고 방금그곡을 이용해 음악을 찾는다. 그런데 라디오 방송에서는 한 음악을 반복해서 재생할 때도 있어서 네오가 기억하고 있는 멜로디는 음악 끝부분과 처음 부분이 이어서 재생된 멜로디일 수도 있다. 반대로, 한 음악을 중간에 끊을 경우 원본 음악에는 네오가 기억한 멜로디가 들어있다 해도 그 곡이 네오가 들은 곡이 아닐 수도 있다. 그렇기 때문에 네오는 기억한 멜로디를 재생 시간과 제공된 악보를 직접 보면서 비교하려고 한다. 다음과 같은 가정을 할 때 네오가 찾으려는 음악의 제목을 구하여라.

- 방금그곡 서비스에서는 음악 제목, 재생이 시작되고 끝난 시각, 악보를 제공한다.
- 네오가 기억한 멜로디와 악보에 사용되는 음은 C, C#, D, D#, E, F, F#, G, G#, A, A#, B 12개이다.
- 각 음은 1분에 1개씩 재생된다. 음악은 반드시 처음부터 재생되며 음악 길이보다 재생된 시간이 길 때는 음악이 끊김 없이 처음부터 반복해서 재생된다. 음악 길이보다 재생된 시간이 짧을 때는 처음부터 재생 시간만큼만 재생된다.
- 음악이 00:00를 넘겨서까지 재생되는 일은 없다.
- 조건이 일치하는 음악이 여러 개일 때에는 라디오에서 재생된 시간이 제일 긴 음악 제목을 반환한다. 재생된 시간도 같을 경우 먼저 입력된 음악 제목을 반환한다.
- 조건이 일치하는 음악이 없을 때에는 “`(None)`”을 반환한다.

### 입력 형식

입력으로 네오가 기억한 멜로디를 담은 문자열 `m`과 방송된 곡의 정보를 담고 있는 배열 `musicinfos`가 주어진다.

- `m`은 음 `1`개 이상 `1439`개 이하로 구성되어 있다.
- `musicinfos`는 `100`개 이하의 곡 정보를 담고 있는 배열로, 각각의 곡 정보는 음악이 시작한 시각, 끝난 시각, 음악 제목, 악보 정보가 '`,`'로 구분된 문자열이다.
    - 음악의 시작 시각과 끝난 시각은 24시간 `HH:MM` 형식이다.
    - 음악 제목은 '`,`' 이외의 출력 가능한 문자로 표현된 길이 `1` 이상 `64` 이하의 문자열이다.
    - 악보 정보는 음 `1`개 이상 `1439`개 이하로 구성되어 있다.

### 출력 형식

조건과 일치하는 음악 제목을 출력한다.

### 입출력 예시

|m|musicinfos|answer|
|---|---|---|
|"ABCDEFG"|["12:00,12:14,HELLO,CDEFGAB", "13:00,13:05,WORLD,ABCDEF"]|"HELLO"|
|"CC#BCC#BCC#BCC#B"|["03:00,03:30,FOO,CC#B", "04:00,04:08,BAR,CC#BCC#BCC#B"]|"FOO"|
|"ABC"|["12:00,12:14,HELLO,C#DEFGAB", "13:00,13:05,WORLD,ABCDEF"]|"WORLD"|

### 설명

첫 번째 예시에서 HELLO는 길이가 7분이지만 12:00부터 12:14까지 재생되었으므로 실제로 CDEFGABCDEFGAB로 재생되었고, 이 중에 기억한 멜로디인 ABCDEFG가 들어있다.  
세 번째 예시에서 HELLO는 C#DEFGABC#DEFGAB로, WORLD는 ABCDE로 재생되었다. HELLO 안에 있는 ABC#은 기억한 멜로디인 ABC와 일치하지 않고, WORLD 안에 있는 ABC가 기억한 멜로디와 일치한다

## 스스로 고민

프로그래머스 문제는 어렵기도 하지만 설명이 더 어렵다....     
한글인데도 도대체 무슨 말인지 이해가 안 갈 때가 많다. 😑     
그래도 뭐 어떡함 해야지.....      

나는 대부분 문제에 대해서 이해가안간다.. 그래서 스스로 정리를 해봐야 하는데 10분안에 정리하는 습관을 가져보려고 한다.

시작!

![](https://media0.giphy.com/media/2qfIXM9jZHPAje5tRe/giphy.gif?cid=ecf05e47bqkyq5b31z60e2g21a69fk760gip449fis5lhtuh&ep=v1_gifs_gifId&rid=giphy.gif&ct=g)

### 접근법

- m 은 악보에 사용되는 음이고 12개가 나온다.
	- 알파벳 , 알파벳 + #
- musicinfos 은 곡 정보를 담고 있는 배열
	- 음악시작시간 : 음악 끝난시간 , 곡 이름, 악보

- 듣는이가 어떤 음절? 을 들었다면 그건 m 으로 입력을 받음
- 그 음절이 musicinfos 에 포함되어 있는지 확인

- 각 음은 1분에 1개씩만 재생된다.
- 반드시 처음부터 재생된다 (첫 음 검색은 무조건 포함)
- 만약에 음악이 1분짜리인데 3분이 재생 되었다면 끊김 없이 다시 처음부터 재생 된다.
- 조건이 일치하는 음악이 여러개 일 때는 재생된 시간이 제일 긴 음악 제목을 반환한다.
- 재생된 시간이 같을 경우 먼저 입력된 음악 제목을 반환한다.
- 없을 때는 None

### 아는 패턴

어제 배운 DP 개념으로 가야 좋을까?    
여러개가 걸리는지 확인하는 거니 트라이를 사용해야할까?
근데 사용자가 음을 중간부터 기억하기 때문에 트라이는 좀 사용이 어려울거 같다.
일단 우선 해보자!

![](https://media1.giphy.com/media/JyWM5hdkKQ2DjYoCW9/giphy.webp)

## 의사코드

- 문제 이해했지만 어떤 방식으로 풀어야될지 감이 안잡히므로 내가 쓰기 편하게 데이터를 다시 가공하자
- `[12:00,12:14,HELLO,CDEFGAB"]` 여기서 CDEFGAB 의 요소개 7개이므로 Hello 라는 곡은 7분 짜리고 2번 재생된 경우다.
	- 또한 조건이 일치하는 음악이 여러 개일 경우 라디오에서 재생된 시간이 제일 긴 음악 제목을 반환한다고 한다.
	- 재생된 시간이 같을 경우는 먼저 입력된 음악 제목을 반환한다.

> 해시맵을 사용하보기로 했다.
> 	해시맵에는 키 값은 
> 		- 재생된 음악이 제일 긴 순으로 정렬
> 		- 시간순 정렬
> 			- 위의 값으로 정렬후 순서대로 인덱스를 설정하여 키 값으로 배정

- 해당 곡이 두 번 반복되었을 경우 키 벨류 값을 복사해서 넣는다.
	- "CDEFGAB" 라면 "CDEFGABCDEFGAB" 이런 식으로

## 구현 코드

```python
def solution(m, musicinfos):

    dic={}
    index = 0
    for music_info in musicinfos:
        start_time_str, end_time_str, title, song = music_info.split(',')

        # 시간 계산 
        start_hour, start_minute = map(int, start_time_str.split(':'))
        end_hour, end_minute = map(int, end_time_str.split(':'))

        # 몇분동안 재생 되었는지
        minutes_difference = (end_hour * 60 + end_minute) - (start_hour * 60 + start_minute)

        # 노래의 길이가 얼마나 되는지
        song_length = len(song)

        # 두 번 반복 되었다면 song 부분 변경
        if minutes_difference // song_length >= 2:
            song = song+song

        # 찾는 음이 있는지 확인
        if m in song:
            dic[index] = start_hour, minutes_difference, title, song
            index +=1
        
        # 라디오가 중간에 끊겼을 경우
        elif song in m:
            dic[index] = start_hour, minutes_difference, title, song
            index +=1

    # 우선순위 선별
    order = 0
    length = 0
    answer = None
    for i in range(len(dic)):
        if dic[i][1] > length:
            length = dic[i][1]
            order = dic[i][0]
            answer = dic[i][2]
        if dic[i][1] == length and dic[i][0] < order:
            answer = dic[i][2]

    return answer
  
```

- 채점 결과 정확성이 73.3 이 나왔다.
- 어딘가에서 놓치는 부분이 있다는건데 

![](https://media3.giphy.com/media/3ohs7KViF6rA4aan5u/giphy.gif?cid=ecf05e47ja5phe0pkwng06c1i440lk8c4jwz5dp7am3zkiy5&ep=v1_gifs_search&rid=giphy.gif&ct=g)


- 우선 답안지를 안보고 테스트 케이스를 살펴본 결과 `None` 이 아니라 `"(None)"` 을 반환했어야 헀다.
-  "(None)" 으로 바꾸니 정확성 83.3 프로...
- 또 무엇을 놓쳤을까?

![](https://media0.giphy.com/media/l0MYHq0IFikDrVQOc/giphy.gif?cid=ecf05e47pdo5gjokhifywvpvvdd0eibp10nula41mv5idk9c&ep=v1_gifs_search&rid=giphy.gif&ct=g)

- 사람들 질문을 보니까 마지막 00:00 분 처리에 대해서도 이야기를 하는 것을 봤다.
- `TC: "CDEFGAC", ["12:00,12:06,HELLO,CDEFGA"] 일 때, 정답은 none 이지만 hello를 출력합니다` 라는 글을 봤다.

``` python
def solution(m, musicinfos):

    dic={}
    index = 0
    for music_info in musicinfos:
        start_time_str, end_time_str, title, song = music_info.split(',')

        # 시간 계산 
        start_hour, start_minute = map(int, start_time_str.split(':'))
        end_hour, end_minute = map(int, end_time_str.split(':'))

        # 몇분동안 재생 되었는지
        minutes_difference = (end_hour * 60 + end_minute) - (start_hour * 60 + start_minute)

        # 노래의 길이가 얼마나 되는지
        song_length = len(song)

        song = song * 10

        # 찾는 음이 있는지 확인
        if m in song:
            dic[index] = start_hour, minutes_difference, title, song, start_minute
            index +=1
    

    # 우선순위 선별
    hour = 0
    minute = 0
    length = 0
    answer = "(None)"
    for i in range(len(dic)):
        if dic[i][1] > length:
            length = dic[i][1]
            hour = dic[i][0]
            answer = dic[i][2]
            minute = dic[i][4]
        if dic[i][1] == length and dic[i][0] < hour:
            length = dic[i][1]
            hour = dic[i][0]
            answer = dic[i][2]
            minute = dic[i][4]
        
        if dic[i][0] == hour and dic[i][0] == hour and dic[i][4] < minute:
            length = dic[i][1]
            hour = dic[i][0]
            answer = dic[i][2]
            minute = dic[i][4]

    return answer
```

- 한 세 시간 고민하다가 뭐가 잘못되었는지 몰라서 다른사람의 코드를 확인했다.

```python
def change(music):
    if 'A#' in music:
        music = music.replace('A#', 'a')
    if 'F#' in music:
        music = music.replace('F#', 'f')
    if 'C#' in music:
        music = music.replace('C#', 'c')
    if 'G#' in music:
        music = music.replace('G#', 'g')
    if 'D#' in music:
        music = music.replace('D#', 'd')
    return music

def solution(m, musicinfos):
    answer = []
    index = 0  # 먼저 입력된 음악을 판단하기 위해 index 추가
    for info in musicinfos:
        index += 1
        music = info.split(',')
        start = music[0].split(':') # 시작 시간
        end = music[1].split(':')  # 종료 시간
        # 재생시간 계산
        time = (int(end[0])*60 + int(end[1])) - (int(start[0])*60 + int(start[1]))
        
        # 악보에 #이 붙은 음을 소문자로 변환
        changed = change(music[3])
        
        # 음악 길이
        a = len(changed)
        
        # 재생시간에 재생된 음
        b = changed * (time // a) + changed[:time%a]
        
        # 기억한 멜로디도 #을 제거
        m = change(m)
        
        # 기억한 멜로디가 재생된 음에 있다면 결과배열에 [시간, index, 제목]을 추가
        if m in b:
            answer.append([time, index, music[2]])
    
    # 결과배열이 비어있다면 "None" 리턴
    if not answer:
        return "(None)"
    # 결과배열의 크기가 1이라면 제목 리턴
    elif len(answer) == 1:
        return answer[0][2]
    # 결과 배열의 크기가 2보다 크다면 재생된 시간이 긴 음악, 먼저 입력된 음악순으로 정렬
    else:
        answer = sorted(answer, key=lambda x: (-x[0], x[1]))
        return answer[0][2] # 첫번째 제목을 리턴
```
## 시간 복잡도와 공간 복잡도

**시간 복잡도**:

1. `change` 함수는 주어진 문자열 `music`에서 특정 음악 기호('#')를 찾아 소문자로 바꾸는 작업을 수행한다. 이 작업은 입력 문자열의 길이에 비례하므로 O(N) 시간이 소요된다. 여기서 N은 입력 문자열 `music`의 길이다.
2. `solution` 함수는 `musicinfos` 목록을 반복한다. 이 목록에는 여러 곡에 대한 정보가 포함되어 있으며, 목록의 길이를 N이라고 한다.
3. 각 `musicinfos` 항목을 처리할 때, 문자열 분할 및 다양한 문자열 작업을 수행하고, 멜로디 'm'이 재생된 멜로디 'b'에 있는지 확인한다. 이러한 작업은 입력 데이터의 크기에 비례하지 않으며, 상수 시간 또는 선형 시간 복잡도를 가진다.
4. `answer` 리스트에 결과를 추가하는 데 O(1) 시간이 소요된다.
5. `answer` 리스트를 재생 시간과 인덱스에 따라 정렬하는 부분은 `sorted` 함수를 사용하며, 이 함수의 시간 복잡도는 O(N log N)다.

따라서 전체 시간 복잡도는 O(N log N)다.

**공간 복잡도**:

1. `change` 함수에서는 입력 문자열 `music`의 복사본을 생성하지 않고 직접 수정하므로 추가 공간이 필요하지 않는다.
2. `solution` 함수에서는 몇 가지 변수를 사용하며, 입력 데이터의 크기에 영향을 받지 않는 상수 공간을 사용한다.
3. `answer` 리스트는 결과를 저장하는 데 사용되며, 이 리스트의 크기는 일반적으로 입력 데이터의 크기에 비례하지 않는다. 최대 N개의 항목을 포함할 수 있다.

따라서 전체 공간 복잡도는 O(N)다.

## 회고 과정

내가 사용한 코드는 왜 통과하지 못 했을까를 생각해봤다.  논리적으로는 그렇게 다르지 않았고 코드에도 문제가 있어보이지는 않았다.     
다만 눈에 띄는게 왜 통과가 된 코드는`#` 부분에 대한 치환을 했을까를 곰곰이 생각했다.

![](https://media3.giphy.com/media/NRXleEopnqL3a/giphy.gif?cid=ecf05e47t5fohim3ofx47qdfmu1yr8ouqqus4g0xmlihnvms&ep=v1_gifs_search&rid=giphy.gif&ct=g)

> C 와 C#은 다른 음이다.

![](https://media1.giphy.com/media/PmRgaD2xj0KH2pPrVF/giphy.gif?cid=ecf05e47t5fohim3ofx47qdfmu1yr8ouqqus4g0xmlihnvms&ep=v1_gifs_search&rid=giphy.gif&ct=g)

두 개는 다른 음이라서 통과가 되면 안되는데 내가 만든 코드로 진행하면 통과가 되기 때문이였다.

### 지금 코드에서 뭘 개선할 수 있지?

```python
def solution(m, musicinfos):

    def change(music):
        if 'A#' in music:
            music = music.replace('A#', 'a')
        if 'F#' in music:
            music = music.replace('F#', 'f')
        if 'C#' in music:
            music = music.replace('C#', 'c')
        if 'G#' in music:
            music = music.replace('G#', 'g')
        if 'D#' in music:
            music = music.replace('D#', 'd')
        return music

    m = change(m)

    dic={}
    index = 0
    
    for music_info in musicinfos:
        start_time_str, end_time_str, title, song = music_info.split(',')

        # 시간 계산 
        start_hour, start_minute = map(int, start_time_str.split(':'))
        end_hour, end_minute = map(int, end_time_str.split(':'))

        # 몇분동안 재생 되었는지
        minutes_difference = (end_hour * 60 + end_minute) - (start_hour * 60 + start_minute)

        # 플랫음 정리
        song = change(song)

        # 노래의 길이
        song = song * 2

        # 찾는 음이 있는지 확인 혹은 라디오가 중간에 끊겼을 경우
        if m in song or song in m:
            dic[index] = start_hour, minutes_difference, title, song, start_minute
            index +=1
  

    hour = 0
    minute = 0
    length = 0
    answer = "(None)"
    for i in range(len(dic)):
        if dic[i][1] > length:
            length = dic[i][1]
            order = dic[i][0]
            answer = dic[i][2]
            minute = dic[i][4]

        if dic[i][1] == length and dic[i][0] < hour:
            length = dic[i][1]
            hour = dic[i][0]
            answer = dic[i][2]
            minute = dic[i][4]

        if dic[i][0] == hour and dic[i][4] < minute:
            length = dic[i][1]
            hour = dic[i][0]
            answer = dic[i][2]
            minute = dic[i][4]

    return answer
```

- `#` 부분만 치환해서 실행해봤는데 정확성은 90.0 으로 올라갔지만 여전히 통과는 하지 못 했다.
- 내가 어떤 걸 또 놓쳤는지 생각해봤다.
![](https://media4.giphy.com/media/z8rEcJ6I0hiUM/giphy.gif?cid=ecf05e47k3ouzrmw5yuhgmdv8s80cjyovd9a2wbrwo0e1yyv&ep=v1_gifs_search&rid=giphy.gif&ct=g)

