---
layout: single
title: "[Basic Java] 생성자"
categories: Basic_Java
tags:
  - Java
toc: true
toc_sticky: true
author_profile: false
sidebar:
---

# 자바 - 생성자

롬복만 쓰다보니까 코테 풀때 정작 생성자를 어떻게 써야되는지 항상 헷갈려서 정리 ㄱㄱ

- 생성자의 이름은 **클래스 이름과 동일**해야 한다.
- 생성자는 다른 멤버함수(메소드)와는 다르게 **리턴 타입이 없다.** 
- 생성자는 **객체가 생성될때 자동으로 한번 호출** 된다. 
- 생성자는 **매개변수 조건에 따라 여러개를 작성할 수 있다**. **(오버로딩)**
- 생성자는 클래스에 최소 1개는 있어야 하며, **생성자 코드가 없을 경우 컴파일러가 기본생성자를 자동으로 생성**한다. 
- **(****주의할점****은, 생성자 코드가 1개라도 작성되어 있다면,** 컴파일러는 기본생성자가 없다고 하더라도 **기본생성자를 자동으로 생성하지 않는다.)**


### **클래스에 기본생성자와 매개변수를 가진 생성자를 정의했을 경우**

**1) Book 클래스 정의**       

```java
public class Book {
	String title;
	int price;
		
	public Book() {	}                   // 기본생성자
	
	public Book(String title, int price) {    // 매개변수를 가진 생성자
		this.title = title;
		this.price = price;
	}

	public void showPrice() {
		System.out.println(title + "의 가격은 " + price + "원 입니다");
	}
}
```

**2) Book 객체 생성 및 사용**      

```java
public class HelloWorld {
	public static void main(String[] args) {

		Book b1 = new Book();                 // 객체생성 - 기본생성자 호출됨
		Book b2 = new Book("국어책", 3000);   // 객체생성 - 매개변수를 가진 생성자 호출됨
		
		b1.showPrice();
		b2.showPrice();
	}
}
```

**3) 실행결과**      

![](https://blog.kakaocdn.net/dn/bJpCUu/btrFIdNveRs/KKZNEHtLqf32B5cHPSgEAK/img.png)

### **클래스에 기본생성자 없이 매개변수를 가진 생성자만 정의했을 경우**

**1) Book 클래스 정의**      

```java
public class Book {
	String title;
	int price;
	
	public Book(String title, int price) {       // 매개변수를 가진 생성자
		this.title = title;
		this.price = price;
	}

	public void showPrice() {
		System.out.println(title + "의 가격은 " + price + "원 입니다");
	}
}
```

**2) Book 객체 생성 및 사용**      

```java
public class HelloWorld {
	public static void main(String[] args) {

		Book b1 = new Book();                // Error발생 (기본생성자 자동생성 안됨)             
		Book b2 = new Book("국어책", 3000);
		
		b1.showPrice();
		b2.showPrice();
	}
}
```

**3) 실행결과**      

![](https://blog.kakaocdn.net/dn/cV2Als/btrFHEXHv8y/ahfeblAFiPZKrpCBkNDUS0/img.png)

기본생성자가 없기 때문에 아래와 같이 기본생성자가 호출되는 객체를 생성하려고 하면 에러가 발생한다.    

   **Book b1 = new Book( );**     

## **자바 - this와 this( )의 용도 및 사용예제**

자바 프로그램 작성시 생성자에서 많이 보게되는 this와 this( )에 대해서 알아보자    

### **1. this와 this( )의 용도**

**1) this**는 객체 자신을 가리키는 레퍼런스 변수로, **자신의 객체에 접근할 때 사용** 된다.    
    - 주로 멤버변수와 매개변수의 이름이 동일할 때, 이를 구분하기 위해 사용된다.    

**2) this( )**는 같은 클래스에서 **생성자가 다른 생성자를 호출할 때 사용** 된다.         
    - 주로 코드의 중복을 줄일 목적으로 사용된다.    
    - this( )는 **생성자 코드에서만 사용**할 수 있다.    
    - this( )는 생성자 코드안에서 사용될 때 **첫번째 문장으로 다른 코드보다 가장 윗줄에 위치**해야 한다.   

### **2. 사용예제**

**1) Book 클래스 정의**     
**①** this는 객체 자신에 대한 레퍼런스 변수로, **this.price 는 멤버변수 price를 나타낸다.**     
**②** this( )는 생성자안에서 다른 생성자를 호출하므로, **this(title, 0); 는 매개변수 2개를 가진 생성자를 호출하게 된다.**      

![](https://blog.kakaocdn.net/dn/ceF57q/btrFNF21Fvy/f2TlcJXQ3KGchp6sefUSN1/img.png)

**2) Book 객체 생성 및 실행**     

![](https://blog.kakaocdn.net/dn/GIaq1/btrFKHmYTXD/4zugfhKB2XXcBSkkUyvmj0/img.png)

**3) 실행결과**      

![](https://blog.kakaocdn.net/dn/bC7MYP/btrFMCMgFgW/4DbnAMANk26JwRqqBOTNv0/img.png)

## **자바 - super 및 super( )의 용도와 사용방법**

자바에서 상속하여 클래스를 사용할 경우 부모 클래스에 접근하기 위해서 사용되는 super와 super( )에 대해서 알아보도록 하겠다.      

### **1.  super 및 super( )의 용도**

**1) super**는 자신이 상속받은 부모 클래스에 대한 레퍼런스 변수로, **부모 클래스의 멤버에 접근할 때 사용** 된다.     
- 주로 객체안에 있는 부모의 멤버변수와 자신의 멤버변수를 구별하기 위해 사용된다.     
**2) super( )**는 자식 클래스의 생성자에서 **부모 클래스의 생성자를 호출하기 위해서 사용** 된다.     
- super( )는 생성자 코드안에서 사용 될 때, 다른 코드에 앞서 **첫줄에 사용**되어야 된다.   
- **자식 클래스의 모든 생성자는 부모 클래스의 생성자를 포함하고 있어야 한다.** 그런데 만약 **자식 클래스의 생성자에 부모 클래스의 생성자가 지정되어 있지 않다면, 컴파일러가 자동으로 부모 클래스의 기본생성자를 호출** 한다. (이러한 경우, 부모클래스에 매개변수가 있는 생성자만 있고, 기본생성자가 없어 기본생성자를 호출할수 없다면 에러가 발생한다.)    

![](https://blog.kakaocdn.net/dn/OHm1a/btrFOwzdRy6/nZgl2beTKwdSkjCIgXvnk1/img.png)

### **2. 사용예제**

**1) Book 클래스 정의 (부모클래스)**       

```java
public class Book {

	String title ="미입력";
	int price = -1;
	int code = 100;
		
	public Book() {  	}                     // 기본생성자
		
	public Book(String title, int price) {      // 매개변수 2개인 생성자
		this.title = title;
		this.price = price;
	}

	
	public void showPrice() {
		System.out.println(title + "의 가격은 " + price + "원 입니다");
	}
}
```

**2) EnglishBook 클래스 정의 (자식클래스)**      
- **super(title, price);** 구문은 부모클래스에 있는 매개변수 2개를 가진 생성자를 호출한다.    

```java
public class EnglishBook extends Book {

	int code = 200;
	
	public EnglishBook() {    }                       // 기본생성자

	public EnglishBook(String title, int price) {    // 매개변수 2개인 생성자
		super(title, price);     
	}

	
	public void showPrice() {

		super.showPrice();      // 부모클래스의 메소드 호출
		
		System.out.println("");
		System.out.println("code       : " + code);
		System.out.println("this.code  : " + this.code);
		System.out.println("super.code : " + super.code);
		
		System.out.println("");
		System.out.println("price       : " + price);
		System.out.println("this.price  : " + this.price);
		System.out.println("super.price : " + super.price);
	
	}
}
```

**3) 객체 생성 및 실행**      

```java
public class HelloWorld {
	public static void main(String[] args) {

		EnglishBook b1 = new EnglishBook("영어책", 1000);
		b1.showPrice();
	}
}
```

**4) 실행결과**     

![](https://blog.kakaocdn.net/dn/AthME/btrFInhZOcI/cpOKLOsN3o6Ix1K4Ft9Sik/img.png)

출처: [https://kadosholy.tistory.com/91](https://kadosholy.tistory.com/91) 
