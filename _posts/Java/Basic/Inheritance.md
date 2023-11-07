---
layout: single
title: "[Basic Java] 상속"
categories: Basic_Java
tags:
  - Java
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
# 상속이란?
2023-11-06-
## 상속
- 기존 클래스에 기능 추가 및 재정의하여 새로운 클래스를 정의
	- 부모 클래스 : 상속 대상이 되는 기존 클래스 = 상위 클래스, 기초 클래스
	- 자식 클래스 : 기존 클래스를 상속하는 클래스 = 하위 클래스, 파생 클래스
- 부모 클래스의 필드와 메소드가 상속됨 ->생성자, 초기화 블록은 상속되지 않음
- 다중 상속은 불가능
- private, default 맴버는 자식 클래스에서 접근 불가
	- default의 경우, 내부 패키지의 자식 클래스는 가능


## super, super()
- super
	- 부모 클래스와 자식 클래스의 맴버 이름이 같을 때 구분하는 키워드
- super()
	- 부모 클래스의 생성자 호출

## 오버라이딩
- 부모 클래스의 메소드를 자식 클래스에서 재정의
	- 오버로딩이랑 다름
- 오버라이딩 조건
	- 메소드의 선언부는 부모 클래스의 메소드와 동일해야함
	- 반환 타입에 한해, 부모 클래스의 반환 타입으로 변환할 수 있는 타입으로 변경 가능
	- 부모 클래스의 메소드보다 접근 제어자를 더 좁은 범위로 변경 불가
	- 부모 클래스의 메소드보다 더 큰 범위의 예외 선언 불가

```java
// Java 프로그래밍 - 상속  
  
class Person {  
    String name;  
    int age;  
    public int a11;  
    private int a22;  
  
    Person() {}  
    Person(String name, int age) {  
        this.name = name;  
        this.age = age;  
    }  
  
    public void printInfo() {  
        System.out.println("--Person.printInfo--");  
        System.out.println("name: " + name);  
        System.out.println("age: " + age);  
    }  
}  
  
// Student 클래스 - Person 상속, 접근제어자 확인  
class Student extends Person {  
    Student() {  
        a11 = 1;  
//        a22 = 2;  
    }  
}  
  
  
// Student 클래스 - Person 상속, super 사용, 오버라이딩  
class Student2 extends Person {  
    String name;  
    int stdId;  
  
    //  super, super()  
    Student2(String name, int age, int stdId) {  
        this.name = name;  // Student2 
//        super.name = name;  // 부모클래스
        this.age = age;  // 부모클래스
//        super(name, age);  // 부모클래스
        this.stdId = stdId;  // Student2 
    }  
  
    //  오버라이딩  
    public void printInfo() {  
        System.out.println("Student2.printInfo");  
        System.out.println("name: " + name);  
        System.out.println("age: " + age);  
        System.out.println("stdId: " + stdId);  
    }  
}  
  
  
public class Main {  
  
    public static void main(String[] args) {  
  
//      Test code  
//      1. 상속  
        System.out.println("=============");  
        Student s1 = new Student();  
        s1.name = "a";  
        s1.age = 25;  
        s1.printInfo();  
  
          
//      2. super, super(), 오버라이딩  
        System.out.println("=============");  
        Student2 s2 = new Student2("b",32, 1);  
        s2.printInfo();  
  
    }  
}
```