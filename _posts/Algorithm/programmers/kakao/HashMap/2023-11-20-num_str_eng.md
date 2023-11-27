---
layout: single
title: "[카카오 - 숫자 문자열과 영단어] (알고리즘)"
categories: Algo
tags:
  - leetcode
  - Python
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
`HashMap`
## 문제

[문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/81301)

###### 문제 설명

![img1.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/d31cb063-4025-4412-8cbc-6ac6909cf93e/img1.png)

네오와 프로도가 숫자놀이를 하고 있습니다. 네오가 프로도에게 숫자를 건넬 때 일부 자릿수를 영단어로 바꾼 카드를 건네주면 프로도는 원래 숫자를 찾는 게임입니다.  
  
다음은 숫자의 일부 자릿수를 영단어로 바꾸는 예시입니다.

- 1478 → "one4seveneight"
- 234567 → "23four5six7"
- 10203 → "1zerotwozero3"

이렇게 숫자의 일부 자릿수가 영단어로 바뀌어졌거나, 혹은 바뀌지 않고 그대로인 문자열 `s`가 매개변수로 주어집니다. `s`가 의미하는 원래 숫자를 return 하도록 solution 함수를 완성해주세요.

참고로 각 숫자에 대응되는 영단어는 다음 표와 같습니다.

|숫자|영단어|
|---|---|
|0|zero|
|1|one|
|2|two|
|3|three|
|4|four|
|5|five|
|6|six|
|7|seven|
|8|eight|
|9|nine|

---

##### 제한사항

- 1 ≤ `s`의 길이 ≤ 50
- `s`가 "zero" 또는 "0"으로 시작하는 경우는 주어지지 않습니다.
- return 값이 1 이상 2,000,000,000 이하의 정수가 되는 올바른 입력만 `s`로 주어집니다.

---

##### 입출력 예

|s|result|
|---|---|
|`"one4seveneight"`|1478|
|`"23four5six7"`|234567|
|`"2three45sixseven"`|234567|
|`"123"`|123|

---

##### 입출력 예 설명

**입출력 예 #1**

- 문제 예시와 같습니다.

**입출력 예 #2**

- 문제 예시와 같습니다.

**입출력 예 #3**

- "three"는 3, "six"는 6, "seven"은 7에 대응되기 때문에 정답은 입출력 예 #2와 같은 234567이 됩니다.
- 입출력 예 #2와 #3과 같이 같은 정답을 가리키는 문자열이 여러 가지가 나올 수 있습니다.

**입출력 예 #4**

- `s`에는 영단어로 바뀐 부분이 없습니다.

---

##### 제한시간 안내

- 정확성 테스트 : 10초

## 스스로 고민

해시맵으로 풀면 간단하게 풀 수 있을거라고 생각헀다.   
다만 아직까지 자바에서 구현하는 방법이 익숙하지 않은점이 문제 ㅜ

## 구현 코드

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    public int solution(String s) {
        
        int result = convertStringToNumber(s);
        return result;
    }
    private static int convertStringToNumber(String input) {
        // 숫자 문자열과 대응하는 정수 매핑
        Map<String, Integer> numberMap = new HashMap<>();
        numberMap.put("zero", 0);
        numberMap.put("one", 1);
        numberMap.put("two", 2);
        numberMap.put("three", 3);
        numberMap.put("four", 4);
        numberMap.put("five", 5);
        numberMap.put("six", 6);
        numberMap.put("seven", 7);
        numberMap.put("eight", 8);
        numberMap.put("nine", 9);

        // 문자열에서 숫자로 변환
        for (Map.Entry<String, Integer> entry : numberMap.entrySet()) {
            input = input.replace(entry.getKey(), String.valueOf(entry.getValue()));
        }

        // 변환된 문자열을 정수로 파싱
        return Integer.parseInt(input);
    }
}
```

```python
def solution(s):
    eng = ["zero", "one", "two", "three", "four", "five", "six", "seven", "eight", "nine"]
    answer = ""
    for idx,num in enumerate(eng):
        if num in s:
            s = s.replace(num, str(idx))
    answer = s
    return int(answer)
    
    
```

```python
num_dic = {"zero":"0", "one":"1", "two":"2", "three":"3", "four":"4", "five":"5", "six":"6", "seven":"7", "eight":"8", "nine":"9"}

def solution(s):
    answer = s
    for key, value in num_dic.items():
        answer = answer.replace(key, value)
    return int(answer)
```
## 시간 복잡도와 공간 복잡도

- 시간 복잡도는 루프를 통해 매핑된 문자를 찾아 교체하므로 O(n)
- 공간 복잡도는 해시맵을 사용하고 numberMap의 크기가 상수이므로 공간복잡도는 O(1)

## 회고 과정

자바를 능숙하게 사용하는게 어려웠다.   
```java
for (Map.Entry<String, Integer> entry : numberMap.entrySet()) {  
    input = input.replace(entry.getKey(), String.valueOf(entry.getValue()));  
}
```

여기서 Entry가 뭔가 했는데 Map 인터페이스 내부에서 선언된 중첩 인터페이스다.  
`numberMap.entrySet()`은 맵의 모든 키-값 쌍을 나타내는 `Set<Map.Entry<String, Integer>>`을 반환한다. 이 `Set`은 각각의 요소가 `Map.Entry<String, Integer>` 타입의 객체로 구성되어 있다.    

따라서 `for (Map.Entry<String, Integer> entry : numberMap.entrySet())`는 `numberMap`의 각 키-값 쌍에 대해 반복하며, 각 반복에서 현재 키-값 쌍을 `entry`에 할당하여 사용하게 된다.


### 다른 사람 코드를 보고 그 기준으로 어떤 부분을 개선 할 수 있는지 최종 회고

```java
class Solution {
    public int solution(String s) {
        String[] strArr = {"zero", "one", "two", "three", "four", "five", "six", "seven", "eight", "nine"};
        for(int i = 0; i < strArr.length; i++) {
            s = s.replaceAll(strArr[i], Integer.toString(i));
        }
        return Integer.parseInt(s);
    }
}
```

- 배열로 간단하게 처리되었다.
- replaceAll 같은 경우 replace에서 사용하는 특정 문자를 찾아 교체하는 것과는 다르게 정규표현식과 일치하는 패턴을 찾아 교체한다.   
- 반복문으로 숫자를 사용해 적절하게 잘 사용되었다.  