---
layout: single
title: "[알고리즘] 체육복"
categories: Algo
tags:
  - Java
  - 프로그래머스
  - 연습문제
  - 99일지
  - 99클럽
  - 항해
  - 코딩테스트
  - TIL
  - 개발스터디
toc: true
toc_sticky: true
author_profile: false
sidebar:
---

# 탐욕법 Greedy 문제

[문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/42862)
## 풀이 접근

1. 해시맵을 이용해준다.
2. n 만큼의 키 값에 기본으로 1을 넣어준다.
3. lost에 있는 키 값이 있으면 다시 값을 -1 해준다. 
	1. 이렇게 하는 이유는 여벌의 체육복을 가져왔는데 도난당했을 때의 로직을 단순화 해주기 위해서다.
4. reverse에 있는 키 값으로 다시 + 1을 해준다.
5. lost 반복문을 돌면서 벨류 값이 0인 키 값에 앞 뒤로 베류가 2이상일 경우 앞에 는 -1 본인은 +1 을 해준다.

## 구현 코드 

```java
import java.util.HashMap;
class Solution {
    public int solution(int n, int[] lost, int[] reserve) {
    HashMap<Integer, Integer> map = new HashMap<>();

    for (int i = 1; i <= n ; i++) {
      map.put(i,map.getOrDefault(i, 1));
    }

    for (int s : lost){
      if(map.containsKey(s)){
        map.put(s, 0);
      }
    }

    for (int s : reserve){
      if(map.containsKey(s)){
        map.put(s, map.get(s) + 1);
      }
    }

    for (int s : lost){
      if(map.get(s) == 0){
        if(map.get(s-1) >= 2){
          map.put(s-1, 1);
          map.put(s, 1);
        } else if (map.get(s+1) >=2) {
          map.put(s+1, 1);
          map.put(s, 0);
        }else {
          map.remove(s);
        }
      }
    }
    return map.size();
  }
}

```

![](https://i.imgur.com/cbGWZtC.png)

런타임 에러...

불필요한 부분을 지우고 다시 해봤다.   

```java
import java.util.HashMap;
class Solution {
    public int solution(int n, int[] lost, int[] reserve) {
    HashMap<Integer, Integer> map = new HashMap<>();

    for (int i = 1; i <= n ; i++) {
      map.put(i, 1);
    }

    for (int s : lost){
        map.put(s, 0);
    }

    for (int s : reserve){
	  if(map.get(s) == 1){  
	    map.put(s,2);  
	  }else {  
	    map.put(s,1);  
	  }  
	}

    for (int s : lost){
      if(map.get(s) == 0){
        if(map.get(s-1) >= 2){
          map.put(s-1, 1);
          map.put(s, 1);
        } else if (map.get(s+1) >=2) {
          map.put(s+1, 1);
          map.put(s, 0);
        }else {
          map.remove(s);
        }
      }
    }
    return map.size();
  }
}


```

![](https://i.imgur.com/a5Pb27Z.png)

- 추측으로는 시간복잡도 문제는 아닌거 같은데 아마 인덱스 부분에서 문제가 있는 것 같았다.   

```java
import java.util.Arrays;
import java.util.HashMap;
class Solution {
    public int solution(int n, int[] lost, int[] reserve) {
    HashMap<Integer, Integer> map = new HashMap<>();


    for (int i = 1; i <= n ; i++) {
      map.put(i, 1);
    }

    for (int s : lost){
      map.put(s, 0);
    }

    for (int s : reserve){
      if(map.get(s) == 1){
        map.put(s,2);
      }else {
        map.put(s,1);
      }
    }

    for (int s : lost){
      if(map.get(s) == 0){
        if(s-1 >= 1 && map.get(s-1) >= 2){
          map.put(s-1, 1);
          map.put(s, 1);
        } else if (s+1 <= n && map.get(s+1) >=2) {
          map.put(s+1, 1);
          map.put(s, 1);
        }else {
          map.remove(s);
        }
      }
    }
    return map.size();
  }

}
```

![](https://i.imgur.com/TbNQE7Z.png)

- 예외케이스를 살펴 봤을 때 해시맵에서 최소값이나 최대값의 키 값이 넘어가는 문제를 잡지 못해서 문제가 발생했었다. 하지만 여전히 오류는 잡지 못하고 있었는데 문제는 내가 해시맵을 지우는거에 있었다.
- 키 값을 삭제할 경우 에러가 나는거였다. 그래서 그냥 내비두고 벨류값이 0 이상일 때만 체크를 해줬다.

```java

import java.util.HashMap;
import java.util.Map;
class Solution {
    public int solution(int n, int[] lost, int[] reserve) {
    HashMap<Integer, Integer> map = new HashMap<>();

    for (int i = 1; i <= n; i++) {
      map.put(i, 1);
    }

    for (int s : lost) {
      map.put(s, 0);
    }

    for (int s : reserve) {
      if (map.get(s) == 1) {
        map.put(s, 2);
      } else {
        map.put(s, 1);
      }
    }

    for (int s : lost) {
      if (map.get(s) == 0) {
        if (s - 1 >= 1 && map.get(s - 1) >= 2) {
          map.put(s - 1, 1);
          map.put(s, 1);
        } else if (s + 1 <= n && map.get(s + 1) >= 2) {
          map.put(s + 1, 1);
          map.put(s, 1);
        }
      }
    }

    int count = 0;
    for(Map.Entry<Integer,Integer> entry : map.entrySet()){
      if (entry.getValue() > 0){
        count ++;
      }
    }
    return count;
  }
}
```

![](https://i.imgur.com/iaSw8hA.png)

다른 사람의 코드를 보고 내가 틀린점을 바로 잡아야겠다고 생각했다.

```java
import java.util.Arrays;

class Solution {
    public int solution(int n, int[] lost, int[] reserve) {
        int answer = 0;
        
        Arrays.sort(reserve);
        Arrays.sort(lost);
        
        // 도난 당하지 않은 학생 수
        answer = n - lost.length;
        
        // 여벌 체육복을 가져왔지만 도난당한 학생 수
        // 다른 학생에게 체육복을 빌려줄 수 없음
        for (int i = 0; i < lost.length; i++) {
			for (int j = 0; j < reserve.length; j++) {
				if (lost[i] == reserve[j]) {
					answer++;
					lost[i] = -1;
					reserve[j] = -1;
                    break;
				}
			}
		}
        
        // 도난당했지만 체육복을 빌릴 수 있는 학생 수
        for (int i = 0; i < lost.length; i++) {
			for (int j = 0; j < reserve.length; j++) {
				if (lost[i] - 1 == reserve[j] || lost[i] + 1 == reserve[j]) {
					answer++;
					reserve[j] = -1;
					break;
				}
			}
		}
        
        return answer;
    }
}
```

![](https://i.imgur.com/k2IYn9K.png)

정렬이 보장되어 있지 않아서가 틀린 이유였다..

```java

import java.util.Arrays;
import java.util.HashMap;
import java.util.Map;
class Solution {
    public int solution(int n, int[] lost, int[] reserve) {
    HashMap<Integer, Integer> map = new HashMap<>();
    Arrays.sort(lost);
    for (int i = 1; i <= n; i++) {
      map.put(i, 1);
    }

    for (int s : lost) {
      map.put(s, 0);
    }

    for (int s : reserve) {
      if (map.get(s) == 1) {
        map.put(s, 2);
      } else {
        map.put(s, 1);
      }
    }

    for (int s : lost) {
      if (map.get(s) == 0) {
        if (s - 1 >= 1 && map.get(s - 1) >= 2) {
          map.put(s - 1, 1);
          map.put(s, 1);
        } else if (s + 1 <= n && map.get(s + 1) >= 2) {
          map.put(s + 1, 1);
          map.put(s, 1);
        }
      }
    }

    int count = 0;
    for(Map.Entry<Integer,Integer> entry : map.entrySet()){
      if (entry.getValue() > 0){
        count ++;
      }
    }

    return count;
  }

}

```

![](https://i.imgur.com/tyxVkCF.png)

## 정리

- 정렬되어 있다고 따로 나오지 않았다면 정말 정렬이 안되어 있다고 생각해야한다.
- 해시맵으로 문제를 풀어서 정렬이 필요 없겠다고 생각했는데 정작 마지막 부분은 정렬이 보장되어있지 않았을 경우 당연히 틀린 답이 나올 수 밖에 없다.






