---
layout: single
title: "[Basic Java] 집합, set (기초수학)"
categories: Basic_Java
tags:
  - Java
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
# 자바로 집합을 구현해보자

- 기본적인 HashSet 사용법

```java
HashSet set1 = new HashSet();
set1.add(1)
set1.add(1)
set1.add(1)
set1.add(1) // 중복된 값이 들어갈 수 없음 -> set1 = [1] 만 있는 상태

set1.add(2)
set1.add(3)
set1.remove(2) // 값 자체를 삭제
set1.size()
set1.contains(2) // -> false

```

## 집합연산

### 교집합

```java
HashSet a = new HashSet(Arrays.asList(1,2,3,4,5));
HashSet b = new HashSet(Arrays.asList(2,4,6,8,13));
a.retainAll(b);
// a 값은 현재 [2,4]
```

### 합집함

```java
a.addAll(b);
// a 는 [1,2,3,4,5,6,8,10]
```

### 차집합

```java
a.removeAll(b);
// a [1,3,5]
```


## set 자료구조 도움 없이 직접 구현

```java
import java.util.ArrayList;  
  
class MySet {  
    ArrayList<Integer> list;  
  
    MySet() {  
        this.list = new ArrayList<Integer>();  
    }  
  
    MySet(int[] arr) {  
        this.list = new ArrayList<Integer>();  
  
        for(int item: arr) {  
            this.list.add(item);  
        }  
    }  
  
// 원소 추가 (중복 X)    public void add(int x) {  
        for (int item: this.list) {  
            if (item == x) {  
                return;  
            }  
        }  
        this.list.add(x);  
    }  
  
// 교집합  
    public MySet retainAll(MySet b) {  
        MySet result = new MySet();  
  
        for (int itemA: this.list) {  
            for (int itemB: b.list) {  
                if (itemA == itemB) {  
                    result.add(itemA);  
                }  
            }  
        }  
        return result;  
    }  
  
// 합집합  
    public MySet addAll(MySet b) {  
        MySet result = new MySet();  
  
        for (int itemA: this.list) {  
            result.add(itemA);  
        }  
  
        for (int itemB: b.list) {  
            result.add(itemB);  
        }  
        return result;  
    }  
  
// 차집합  
    public MySet removeAll(MySet b) {  
        MySet result = new MySet();  
  
        for (int itemA: this.list) {  
            boolean containFlag = false;  
  
            for (int itemB: b.list) {  
                if (itemA == itemB) {  
                    containFlag = true;  
                    break;  
                }  
            }  
  
            if (!containFlag) {  
                result.add(itemA);  
            }  
        }  
        return result;  
    }  
}  
  
public class Practice {  
    public static void main(String[] args) {  
  
//      Test code  
        MySet a = new MySet();  
  
        a.add(1);  
        a.add(1);  
        a.add(1);  
        System.out.println(a.list);  
        a.add(2);  
        a.add(3);  
        a.add(3);  
        a.add(3);  
        System.out.println(a.list);  
  
        a = new MySet(new int[]{1, 2, 3, 4, 5});  
        MySet b = new MySet(new int[]{2, 4, 6, 8, 10});  
        System.out.println("a: " + a.list);  
        System.out.println("b: " + b.list);  
  
        MySet result = a.retainAll(b);  
        System.out.println("교집합: " + result.list);  
  
        result = a.addAll(b);  
        System.out.println("합집합: " + result.list);  
  
        result = a.removeAll(b);  
        System.out.println("차집합: " + result.list);  
    }  
}
```