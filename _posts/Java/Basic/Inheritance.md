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

```java
 
class Animal {  
    String desc;  
    Animal() {  
        this.desc = "동물 클래스.";  
    }  
  
    Animal(String desc) {  
        this.desc = desc;  
    }  
  
    public void printInfo() {  
        System.out.println(this.desc);  
    }  
}  
  
class Cat extends Animal {  
    String desc;  
    Cat() {  
        super.desc = "고양이.";  
    }  
  
}  
  
public class Practice1 {  
    public static void main(String[] args) {  
        // Test code  
        Cat cat = new Cat();  
        cat.printInfo();  
  
    }  
}
```

class Cat의 경우 만약 생성자 부분이 this.desc일 경우 동물클래스가 출력된다.    왜냐하면 printInfo() 부분은 부모 클래스의 동물클래스를 받아오고 있기 때문이다. 따라서 해당 부분은 super로 이식해주면 고양이가 출력된다.     


## 오버라이딩

오버로딩과는 다름 

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
//        a22 = 2;  private 라서 접근이 안됨
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

## 오버로딩 vs 오버라이딩

**오버로딩(Overloading):**

- **정의:** 같은 메서드 이름을 사용하지만 매개변수의 유형, 개수, 또는 순서가 다른 여러 메서드를 정의하는 것을 말한다.
- **특징:**
    - 메서드 이름이 동일하면서 매개변수가 달라야 한다.
    - 리턴 타입이나 접근 제어자 등은 오버로딩에 영향을 주지 않는다.
- **예시:**

```java
class Calculator {
    int add(int a, int b) {
        return a + b;
    }
    
    double add(double a, double b) {
        return a + b;
    }
}

```


- **오버라이딩(Overriding):**
    
    - **정의:** 상위 클래스에서 정의된 메서드를 하위 클래스에서 동일한 시그니처(메서드 이름, 매개변수 유형, 리턴 타입)로 다시 구현하는 것을 말한다.
        
    - **특징:**
        - 상속 관계에서 발생한다.
        - 메서드 시그니처가 동일해야 한다.
        - 상위 클래스의 메서드를 하위 클래스에서 재정의하여 다른 동작을 구현할 수 있다.
          
    - **예시:**

```java
class Animal {
    void makeSound() {
        System.out.println("Some generic sound");
    }
}

class Dog extends Animal {
    @Override
    void makeSound() {
        System.out.println("Bark");
    }
}

```
- 위의 예시에서 `Dog` 클래스는 `Animal` 클래스의 `makeSound` 메서드를 오버라이딩하여 자신만의 구현인 "Bark"로 바꿨다.


요약하면, 오버로딩은 같은 이름의 메서드를 매개변수의 차이로 여러 개 정의하는 것이고, 오버라이딩은 상위 클래스의 메서드를 하위 클래스에서 다시 구현하는 것이다.