---
layout: single
title: "Java 자료구조 Sort(1) Binary Search 간략히 보기"
categories: Data_Structure
tag: ["Java","Binary Search"]
toc: true
toc_sticky: true
author_profile: false
sidebar:
  

---
# Binary Search 이란?

- 오름차순 정렬되어 있는 리스트 내에서 특정 값의 인덱스를 찾는 알고리즘
- ![](https://i.imgur.com/OVQU6Wr.png)
- 중간에 위치한 11을 기준으로 왼쪽부터 찾음
- ![](https://i.imgur.com/A3M2wXK.png)
- 5를 기준으로 오른쪽을 찾음
- 이런식으로 계속 둘로 나눠가면서 사이즈를 줄여가며 찾음
- 이러한 이유로 빠른 속도를 갖고 있음
	- 시간 복잡도 O(longN)
	- (N = 100) vs (logN = 6.64)
- 정렬된 리스트에서만 사용 가능

## 코드 구현

```java
package sort;  
  
public class BinarySearch {  
  
    public int search(int[] arr, int target) {  
        int l = 0;  // left
        int r = arr.length - 1;  // right
  
        int m;  // middle
        while (l <= r) {  
            m = l + ((r - l) / 2);  // overflow exception  
  
            if (arr[m] == target) {  
                return m;  
            }  
  
            if (arr[m] < target) {  // target 값이 더 큰 경우  
                l = m + 1;  
            } else {    // target 값이 더 작은 경우  
                r = m - 1;  
            }  
        }  
        return -1;  
    }  
}
```


