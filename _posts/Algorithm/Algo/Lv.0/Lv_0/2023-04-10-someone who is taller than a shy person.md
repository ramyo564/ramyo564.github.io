---
layout: single
title: "머쓱이보다 키 큰 사람 (알고리즘)"
categories: Algo
tag: [Java,Python,NodeJs,Lv_0,"나이 출력","자스함수filter"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Java vs Node.Js vs Python
## 문제 푸는건 5초 정리는 10분...

- 하루 시작은 알고리즘 한 문제 풀기로 시작
- 자바가 특히 헷갈려서 언어에 익숙해질겸 lv_0부터 다시 하는게 나을 듯
- 어차피 기본기가 안되면 뭘 배워도 응용이 안됨

- 문제 -> https://school.programmers.co.kr/learn/courses/30/lessons/120585

## Java

```java
// 내가 푼 답

// 다른사람풀이 

```
### 정리
- pass



## Python
```python
# 내가 푼 답

# 다른 사람 풀이 1


```
### 정리
- pass



## Node.js

```javascript
// 내가 푼 답
function solution(array, height) {
    array.sort();
    for(i=0; i < array.length; i++){
        if(array[i] > height){
            return i
        }
        return i
    }
}
// 다시 수정
function solution(array, height) {
    array.sort(); 
    for(let i=0; i <array.length; i++){
        if(array[i] > height){
            return array.length - i;
        }
    }
    return 0;
}


// 다른 사람 풀이

function solution(array, height) {
    var answer = array.filter(item => item > height);
    return answer.length;
}
```
### 정리
#### 문제점
1. 처음 푼 문제에서 i가 선언이 안되서 그런줄 알았는데 상관 없다.
2. 반복문이 끝까지 실행되지 않고 바로 return 으로 빠져 나왔다.
	- return 위치가 잘못되어 있었다. 저 위치에서 return 해버리면 반복문이 빠져나가는데 { }를 잘못봤다.
3. 문제 자체를 잘못 읽었다
	- 몇 번 째로 반에서 키가 큰건지인줄 알았는데 계속 틀려서 도저히 이해가 안갔다.
	- 문제를 다시보니 반에서 자기보다 키큰 애들이 몇 명인가를 세는 거였다 하하 
4. 자바스크립트가 자바보다 더 어려운거 같다
5. filter 쓰면 간편하게 해결가능하다.

https://www.opentutorials.org/module/570/4962

출처 : 프로그래머스,