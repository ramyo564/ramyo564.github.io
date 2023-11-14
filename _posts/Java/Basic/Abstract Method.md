---
layout: single
title: "[Basic Java] 추상메소드"
categories: Basic_Java
tags:
  - Java
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
# 추상 메소드란?

- 자식클래스에서 반드시 오버라이딩 해야하는 메소드
- 선언만하고 구현 내용 없음

```java
abstract void print();
```

## 추상 클래스 

- 하나 이상의 추상 메소드를 포함하는 클래스
- 반드시 구현해야 하는 부분에 대해 명시적으로 표현
- 추상 클래스 자체는 객체 생성 불가

```java
abstract class 클래스명 {
	abstract void print();
}
```


```java
// 추상 클래스 Personabstract class Person {  
  
    abstract void printInfo();  
}  
  
// 추상 클래스 상속  
class Student extends Person {  
  
    public void printInfo() {  
        System.out.println("Student.printInfo");  
    }  
}  
  
  
public class Main {  
  
    public static void main(String[] args) {  
  
//        추상 클래스의 사용  
//        Person p1 = new Person();  
        Student s1 = new Student();  
        s1.printInfo();  
  
        Person p2 = new Person() {  
            @Override  
            void printInfo() {  
                System.out.println("Main.printInfo");  
            }  
        };  
        p2.printInfo();  
  
    }  
  
}
```

- @Override -> 익명 클래스로도 사용 가능