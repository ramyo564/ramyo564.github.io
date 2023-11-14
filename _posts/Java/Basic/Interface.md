---
layout: single
title: "[Basic Java] 인터페이스"
categories: Basic_Java
tags:
  - Java
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
# 인터페이스란?
2023-11-07-

- 다중 상속처럼 사용할 수 있는 기능
- 추상 메소드와 상수만으로 이루어짐

```java
접근제어자 interface 인터페이스이름 {
	public static final 타입 상수이름 = 값;
	public abstract 반환타입 메소드이름(매개변수);
	..
}
class 클래스이름 implements 인터페이스이름 {

}

```

- 동시 사용으로 다중 상속과 같은 효과
```java
접근제어자 interface 인터페이스이름 {
...
}
접근제어자 class 클래스이름 {
...
}
class 클래스이름 extends 클래스이름 implements 인터페이스이름 {

}
```

## 그럼 추상 클레스와 인터페이스의 차이가 뭐지?

- 추상 클래스는 그 추상 클래스를 상속 받아서 기능을 이용하고 확장시킨다.
- 반면 인터페이스는 함수의 껍데기만 있는데, 그 이유는 해당 함수의 구현을 강제하기 위해서다. 구현을 강제함으로서 구현 객체의 같은 동작을 보장할 수 있다.

이런 특징이 있는 이유는 자바가 다중 상속을 지원하지 않기 때문이다.     

```java
class MyCar extends car, plane{
	@Overide
	public void goTo(){
		super.drive();
	}
}
```

위와 같은 코드에서 car, plane 클래스 모드 drive() 라는 메소드를 가지고 있다면, 어떤 메소드가 실행될지 애매하다. 이런 특징으로 자바는 다중 상속을 못하게 되어 있다. 하지만 인터페이스는 여러개의 인터페이스를 구현할 수 있다.     

```java
class car implements vehicle, engine
	@Override
	public void drive(){
		@doSomething
	}
```

여러개를 상속받는 것처럼 보이지만 목적성에 대해 생각해봐야 한다.    

상속은 슈퍼클래스의 기능을 이용하거나 확장하기 위해서고, 다중 상속의 모호성 때문에 하나만 상속 받을 수 있다.     

반면 인터페이스는 해당 인터페이스를 구현한 객체들에 대해서 동일한 동작을 약속하기 위해 존다한다.    
