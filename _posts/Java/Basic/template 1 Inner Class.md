---
layout: single
title: "[Basic Java] Generic"
categories: Basic_Java
tags:
  - Java
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
# 내부클라스란?
2023-11-07-

- 클래스 in 클래스 (클래스 안에 선언한 클래스)

```java
class Outer {
	...
	class Inner {
		...
	}
}
```


## 내부 클래스 특징

- 내부 클래스에서 외부 클래스 맴버에 접근가능
- 외부에서는 내부 클래스에 접근 불가

## 내부 클래스 종류

- 인스턴스 클래스 (instance class) 
	- 밖에 쪽에 클래스가 만들어져야서 안에서 클래스를 사용함
- 정적 클래스 (static class)
	- 밖에 클래스가 만들어지지 않아도 사용 가능  -> 메모리에 상주
- 지역 클래스 (local class)
	- 메소드 안에 클래스가 있음
- 익명 클래스 (anonymous class)
	- 이름을 가지지 않는 클래스
	- 선언과 동시에 객체 생성
	- 일회용 클래스

```java
클래스이름 참조변수 이름 = new 클래스 이름(){
	...
}
```

```java
// Java 프로그래밍 - 내부 클래스  
  
class Outer {  
  
    public void print() {  
        System.out.println("Outer.print");  
    }  
  
    class Inner {  
        void innerPrint() {  
            Outer.this.print();  
        }  
    }  
  
    static class InnerStaticClass {  
        void innerPrint() {  
//            Outer.this.print();  
        }  
    }  
  
    public void outerMethod() {  
        class InnerLocal {  
  
        }  
  
        InnerLocal il1 = new InnerLocal();  
    }  
}  
  
abstract class Person {  
    public abstract void printInfo();  
}  
  
class Student extends Person {  
    public void printInfo() {  
        System.out.println("Student.printInfo");  
    }  
}  
  
public class Main {  
  
    public static void main(String[] args) {  
  
//      외부 클래스  
        Outer o1 = new Outer();  
  
//      내부 클래스 - 인스턴스  
        Outer.Inner i1 = new Outer().new Inner();  
  
//      내부 클래스 - 정적  
        Outer.InnerStaticClass is1 = new Outer.InnerStaticClass();  
          
//      익명 클래스  
        Person p1 = new Person() {  
            @Override  
            public void printInfo() {  
                System.out.println("Main.printInfo");  
            }  
        };  
  
    }  
  
}
```