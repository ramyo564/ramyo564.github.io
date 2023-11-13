---
layout: single
title: "[Basic Java] 클래스와 객체"
categories: Basic_Java
tags:
  - Java
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
# 클래스와 객체
2023-11-05-

## 객체와 인스턴스

- 객체 (object)
	- 실체
- 인스턴스 (instance)
	- 클래스와 객체의 관계
	- 클래스로부터 객체를 선언 (인스턴스 화)
	- 어떤 객체는 어떤 클래스의 인스턴스

## 생성자

클래스명 객체명 = new 클래스명();
이렇게 만들어질 때 **자동으로 생성되는 게 생성자**

- 객체가 생성될 때 자동으로 호출됨
- 생성자 규칙
	- 클래스명과 이름 맞추기
	- 리턴 타입 없음
```java
public class 클래스명 {
	클래스명(){} // void 이런거 안 쓰고 그냥 비워둠
}
```
- 밑에가 생성자인데 이때 클래스명과 이름이 일치해야한다.


## this, this()

- this - 객체 자신을 의미
- this() - 생성자를 의미

```java
class Car {  
    String name;  
    String type;  
  
    public void printCarInfo() {  
        System.out.println("name: " + name);  
        System.out.println("type: " + type);  
    }  
  
}  
  
class Car2 {  
    String name;  
    String type;  
  
    Car2(String name, String type) {  
        this.name = name;  
        //this.name 은 바로 위에 String name이고 오른쪽 name은 아래에서 bdc, sedan2 의 데이터를 받아온거임
        this.type = type;  
    }  
  
    public void printCarInfo() {  
        System.out.println("name: " + name);  
        System.out.println("type: " + type);  
    }  
    
}  
  
  
public class Main {  
  
    public static void main(String[] args) {  
        Car myCar1 = new Car();  
        myCar1.name = "abc";  
        myCar1.type = "suv222";  
        myCar1.printCarInfo();  
  
        Car2 myCar2 = new Car2("bdc", "sedan2");  
        myCar2.printCarInfo();  
 
    }  
}
```

## 오버로딩

- 한 클래스 내에서 같은 이름의 메소드를 여러 개 정의
	- 오버라이딩이랑 다름
- 오버로딩 조건
	- 메소드의 이름이 같아야함
	- 매개변수의 개수 또는 타입이 달라야함
	- (리턴타입의 차이로는 오버로딩이 되지 않음)

```java
public class 클래스명 {
	클래스명(){}
	클래스명(int a, int b){
		구현 내용;
	}
	// 생성자 오버로딩
}
```

- 예를 들어 sum이란 메소드를 만들었는데 초기에 sum(int a, int b) 로 설정했을 경우 추가로 sum( Double a, Double b ) 이런식으로도 인스턴스를 받고 싶을 때 오버로딩을 이용할 수 있음
- 위와 같이 생성자를 오버로딩 할 수도 있음

## 접근제어자

- 클래스의 변수나 메소드의 접근에 제한을 두는 키워드
- 접근제어자 종류
	- private : 해당 클래스에서만 접근 가능
	- public : 어디서든 접근 가능
	- default : 해당 패키지 내에서만 접근 가능(외부 패키지 접근 불가)
	- protected : 해당 패키지 및 상속받은 클래스에서 접근 가능 (외부 패키지에서 일반 클래스는 불가능 하지만 자식 클래스는 가능)


## Static
- 변수나 메소드의 특성을 바꾸는 키워드
- Static 특징
	- 메모리에 한번만 할당됨 (객체가 새로 만들어지기 전부터 있다고 상상하면 편함)
	- 즉 static 변수나 메소드는 공유되는 특성을 가짐
- Static 클래스 변수
	- 해당 클래스의 각 객체들이 값을 공유
		- 메모리주소가 같다.
- Static 클래스 메소드
	- 객체를 생성하지 않아도 호출 가능
	- 이미 메모리에 올라가있는 상태라서 변수도 static이어야 함

```java
package bread;  
  
public class Bread2 {  
    public String name1;  
    public String name2;  
    public String name3;  
    public String name4;  
  
    public Bread2(String name1, String name2, String name3, String name4) {  
        this.name1 = name1;  
        this.name2 = name2;  
        this.name3 = name3;  
        this.name4 = name4;  
    }  
  
}
```


```java
// Java 프로그래밍 - 클래스와 객체_2  
  
import bread.Bread2;  
  
class Bread {  
    String name;  
    String type;  
  
    Bread(String name, String type) {  
        this.name = name;  
        this.type = type;  
    }  
  
    public void printBreadInfo() {   
        System.out.println("name: " + name);  
        System.out.println("type: " + type);  
    }  
  
    public void printBreadInfo(String date) {  
        printBreadInfo();  
        System.out.println("date: " + date);  
    }  
  
    public void printBreadInfo(int number) {  
        printBreadInfo();  
        System.out.println("number: " + number);  
    }  
  
    public void printBreadInfo(String date, int number) {  
        printBreadInfo();  
        System.out.println("date: " + date);  
        System.out.println("number: " + number);  
    }  
  
}  
  
  
class Bread3 {  
    static String name = "None";  
    String type;  
  
    Bread3(String name, String type) {  
        this.name = name;  
        this.type = type;  
    }  
  
    public void printBreadInfo() {  
        
        System.out.println("name: " + name);  
        System.out.println("type: " + type);  
    }  
  
    public static void getName() {  
        System.out.println("Bread name: " + name);  
//        System.out.println("Car type: " + type);  
    }  
}  
  
  
public class Main {  
  
    public static void main(String[] args) {  
        Bread myBread1 = new Bread("a", "Bread");  
        myBread1.printBreadInfo();  
  
//      1. 오버로딩  
        myBread1.printBreadInfo("2022");  
        myBread1.printBreadInfo(1);  
        myBread1.printBreadInfo("2022", 1);  
  
  
//      2. 접근 제어자  
        System.out.println("=== 접근 제어자 ===");  
        Bread2 myBread2 = new Bread2("a", "b", "c", "d");  
        System.out.println("name1: " + myBread2.name1);  
        System.out.println("name2: " + myBread2.name2);  
        System.out.println("name3: " + myBread2.name3);  
        System.out.println("name4: " + myBread2.name4);  
  
  
//      3. Static  
        System.out.println("=== Static ===");  
        Bread3.getName();  // <- 객체를 만들지 않아도 바로 사용 가능
        Bread3 Bread3_1 = new Bread3("a", "Bread");  
        Bread3 Bread3_2 = new Bread3("b", "Bread2");  
        Bread3 Bread3_3 = new Bread3("c", "Bread3");  
        myBread3_1.printBreadInfo();  
        myBread3_2.printBreadInfo();  
        myBread3_3.printBreadInfo();  
  
        Bread3.getName();  
          
    }  
  
}
```