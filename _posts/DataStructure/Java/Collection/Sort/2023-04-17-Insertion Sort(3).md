---
layout: single
title: "Java 자료구조 Sort(3) Insertion Sort  간략히 보기"
categories: Data_Structure
tag: ["Java","Insertion Sort"]
toc: true
toc_sticky: true
author_profile: false
sidebar:
  

---
# Insertion Sort 이란?

- 리스트의 앞에서부터
- 이미 정렬된 서브 리스트의 값들과 비교
- 자신의 위치에 삽입
- ![](https://i.imgur.com/fnelBXS.png)
- 이미 정렬된 리스트란 앞에 초기 값을 비교로 시작한다고 이해하면 됨
- 5와 4를 비교한 후 4가 5보다 작으니 4,5,1,9,3,7,2
- 앞에 인덱스 0 과 1은 정렬이 되어으니 인덱스 2부터 비교 시작
- 1은 앞과 비교 해보면 가장 앞에 삽입됨
- ![](https://i.imgur.com/5x1IARw.png)
- 그 후 9를 앞에 정렬된 리스트와 비교
- 계속 반복

>- 안정정렬
>- 단순한 알고리즘
>- 데이터의 이동이 많음
>	- 리스트 내의 데이터가 어느 정도 정렬이 되어있는 상태일 경우 데이터의 이동이 적어짐
>- 시간 복잡도
>	- 평균 -> O(n^2)
>	- 최선의 경우
>		- 모두 정렬이 되어있는 경우 -> O(n)
>	- 최악의 경우
>		- 역으로 정렬되어 있는 경우 -> O(n^2)

## 코드 구현

```java
package sort;  
  
public class InsertionSort implements ISort {  
    @Override  
    public void sort(int[] arr) {  
        int n = arr.length;  
        for (int i = 1; i < n; ++i) {  
            int key = arr[i];   // 삽입 위치를 찾아줄 데이터  
            int j = i - 1;  // 0-j 정렬된 서브 리스트  
            while (j >= 0 && arr[j] > key) {  
                // key 값보다 정렬된 배열에 있는 값이 크면  
                arr[j + 1] = arr[j];    // 데이터를 한칸씩 오른 쪽으로 이동  
                j = j - 1; // 역순으로 값 비교  
            }  
            arr[j + 1] = key;  
        }  
    }  
}
```
