---
layout: single
title: "[Basic Java] 다형성"
categories: Basic_Java
tags:
  - Java
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
# 다형성이란?
2023-11-06-
- 한 객체가 여러 가지 타입을 가질 수 있는 것
- 부모클래스 타입의 참조 변수로 자식 클래스 인스턴스 참조
- 업캐스팅, 다운캐스팅이 있음 ->

```java
class Person{}
class Student extends Person{}

Person p1 = new Student();
// Student s1 = new Person();
```

## Instance of

- 실제 참조하고 있는 인스턴스의 타입 확인

```java
class Person{}
class Student extends Person {}

Person p1 = new Student();
// Student s1 = new Person();
System.out.println(p1 instanceof Person)
-> 값이 true 
```

## 활용

```java
  
class Car {  
    Car(){}  
    public void horn() {  
        System.out.println("빵빵빵!");  
    }  
}  
  
class FireTruck extends Car {  
    public void horn() {  
        System.out.println("불이야!");  
    }  
}  
  
class Ambulance extends Car {  
    public void horn() {  
        System.out.println("사람살려!");  
    }  
}  
  
public class Practice {  
    public static void main(String[] args) {  
        
        Car car = new Car();  
        car.horn();  
        car = new FireTruck();  
        car.horn();  
        car = new Ambulance();  
        car.horn();  

		// 이렇게 배열에 넣고 다형성을 응용해서 사용 가능
        Car car2[] = {new Car(), new FireTruck(), new Ambulance()};  
        for (Car item:car2){  
            item.horn();  
        }  
          
    }  
}
```