---
layout: single
title: "Java 자료구조 Sort(2) Bubble Sort 간략히 보기"
categories: Data_Structure
tag: ["Java","Bubble Sort"]
toc: true
toc_sticky: true
author_profile: false
sidebar:
  

---
# Bubble Sort 이란?

## 정렬

- 안정(Stable) 정렬 vs 불안정 (unstable) 정렬
	- 중복된 값의 순서 보장 여부
- ![](https://i.imgur.com/agNWadt.png)
- 위의 데이터중 1이 중복 되어있는걸 정렬하면 위에 차트 아니면 밑에 1_b , 1_a 차트
- ![](https://i.imgur.com/5bn0ubA.png)
- ![](https://i.imgur.com/zHHO8lF.png)
- In-place 정렬 vs Out-of-place 정렬
	- 원본 데이터 내 정렬 여부

## Bubble Sort

- ![](https://i.imgur.com/cdzo1pI.png)
- 위의 데이터가 있다고 가정 할 시에 앞에 2개씩만 계속 비교해서 크면 뒤로 보내줌
- 5와 4를 비교 했을 때 5가 크므로 뒤로 보내줌
- 그래서 4,5,1,9,3,7,2
- 여기서 5를 다시 1과 비교
- 그럼 4,1,5,9,3,7,2
- 근데 여기서 5는 9보다 크므로 5에서 9로 넘어가고 9랑 3이랑 비교
- ![](https://i.imgur.com/zJU7HBk.png)
- 9가 맨 뒤로 가고 난 뒤에 다시 앞에서 부터 2개씩 비교
- ![](https://i.imgur.com/yJQltFI.png)
- 시간 복잡도 O(N^2)
- 직관적이고 단순한 알고리즘
- 하지만 느려서 잘 사용 안함

## 코드 구현

```java
package sort;  
  
public class BubbleSort implements ISort {  
    @Override  
    public void sort(int[] arr) {  
        // 안정 정렬  
        for (int i = 0; i < arr.length - 1; i++) { // 전체 리스트  
            for (int j = 0; j < arr.length - 1 - i; j++) {  // 정렬된 리스트 제외  
                if (arr[j] > arr[j + 1]) {  
                    int tmp = arr[j];  
                    arr[j] = arr[j + 1];  
                    arr[j + 1] = tmp;  
                }  
            }  
        }  
    }  
}
```