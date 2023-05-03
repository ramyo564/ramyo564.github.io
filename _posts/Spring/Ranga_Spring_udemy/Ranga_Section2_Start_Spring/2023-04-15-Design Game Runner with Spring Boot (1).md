---
layout: single
title: "[Spring] Design Game Runner with Spring Boot"
categories: Spring
tag: [Java,"record클래스",getBean,"@Bean","[BIG][Spring] Starting Spring Framework"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Starting Spring Framework(1)
/Interface / @Bean / getBean / 클래스 record

- Design Game Runner to run games(Mario, SuperContra, Pacman etc) in an iterative approach:
	- Iteration 1: Tightly Copuled Java Code
		- GameRunner class
		- Game classes: Mario, SuperContra, Pacman etc
		- ![](https://i.imgur.com/CEduo2V.png)


	- Iteration 2: Loose Coupling - interfaces
		- GameRunner class
		- GamingConsole interface
		- Game classes: Mario, SuperContra, Pacman etc
		- ![](https://i.imgur.com/YDGjzze.png)


## Why is Coupling Important?
- Coupling: How much work is involved in changing something?
	- Coupling is important everywhere:
		- An engine is tightly coupled to a Car
		- A Wheel is loosely coupled to a Car
		- You can take a laptop anywhere you go
		- A computer, on the other hand, is a little bit more difficult to move
	- Coupling is even more important in building great software
		- only thing constant in technology is change
			- Business requirements change
			- Frameworks change
			- Code changes
		- We want Loose Coupling as much as possible
		- We want to make functional changes with as less code changes code as possible
	

## Tightly Copuled Java Code

- ![](https://i.imgur.com/WEBG93a.png)

- ![](https://i.imgur.com/nEQbvtH.png)
- ![](https://i.imgur.com/JtVmzdU.png)
- ![](https://i.imgur.com/PKh7EUB.png)
> - 의존성이 매우 높음 
> 	- 마리오 게임에서 슈퍼콘트라로 바꾸려면 관련된 것들을 다 바꿔줘야함

## Loose Coupling - interfaces
- ![](https://i.imgur.com/hsfxtbU.png)
- ![](https://i.imgur.com/YkSFxTZ.png)
- ![](https://i.imgur.com/nfadWX2.png)
>-  이렇게 인터페이스를 만들면 동일 메소드에서 갈아끼워서 사용가능하다.
>- 막상 답답함을 느껴보니 왜 인터페이스를 사용하는지 알거 같다
>- 하지만 여기서 Spring을 추가한다면?!

## Spring! Bean으로 관리하기

![](https://i.imgur.com/auHMrII.png)

### Practice!

```java
public class App02HelloWorldSpring {  
    public static void main(String[] args) {  
  
        //1: Launch a Spring Context  
  
        var context =  
                new AnnotationConfigApplicationContext(HelloWorldConfiguration.class);  
  
        //2: Configure the things that we want Spring to manage - @Configuration  
        //HelloWorldConfiguration - @Configuration        //name -@Bean  
        //3: Retrieving Beans managed by Spring        System.out.println(context.getBean("name"));  
        System.out.println(context.getBean("age"));  
        System.out.println(context.getBean("person"));  
        System.out.println(context.getBean("address"));  
    }  
}
```

```java
package com.in28minutes.learinspringframework;  
  
import org.springframework.context.annotation.Bean;  
import org.springframework.context.annotation.Configuration;  


//Eliminate verbosity in creating Java Beans
//Public accessor methods, constructor
//equals, hashcode and toString are automatically created 
//Released in JDK 16 (record)  
record Person (String name, int age){};  
  
//Address  
record Address(String firstLine, String city){};  
@Configuration  
public class HelloWorldConfiguration {  
  
    @Bean  
    public String name() {  
        return "Yoyo";  
    }  
    @Bean  
    public int age() {  
        return 18;  
    }  
  
    @Bean  
    public Person person(){  
        return new Person("J", 20);  
    }  
    @Bean  
    public Address address(){  
        return new Address("Bupyeong", "Incheon");  
    }  
}
```
>- Java 16 이후 `record` 덕분에 getter setter 등 이전보다 훨씬 편하게 작성 가능
>- 생성자 작성 필요 X
>- toString, equals, hashCode 등 자동으로  생성됨

참고 : https://scshim.tistory.com/372

### @Bean -> name 재정의



```java
@Bean(name = "address2")  
public Address address(){  
    return new Address("Bupyeong", "Incheon");  
}
```

```java

public class App02HelloWorldSpring {  
    public static void main(String[] args) {  

  ...
        var context =  
                new AnnotationConfigApplicationContext(HelloWorldConfiguration.class);  
  
...
System.out.println(context.getBean("address2"));  
 
    }  
}
```

### How you retrieve the beans back from spring context
```java
System.out.println(context.getBean(Address.class)); 
```
- Once spring Starts managing a bean, you can either use the name of the bean or you can use the type of the bean to be able to get it back and play with it

#### 이미 존재하는 Bean 으로 새로운 Bean 만들기
```java
@Bean  
public String name() {  
    return "Yoyo";  
}  
@Bean  
public int age() {  
    return 18;  
}

@Bean  
public Person person2MethodCall(){  
    return new Person(name(), age());  
}
```

#### 새로운 Bean 추가
```java

//Released in JDK 16 (record)  
record Person (String name, int age, Address address){};  
  
//Address  
record Address(String firstLine, String city){};  
@Configuration  
public class HelloWorldConfiguration {  
  
    @Bean  
    public String name() {  
        return "Yoyo";  
    }  
    @Bean  
    public int age() {  
        return 18;  
    }  
  
    @Bean  
    public Person person(){  
        return new Person("J", 20, new Address("Marine","Port"));  
    }  
    @Bean  
    public Person person2MethodCall(){  
        return new Person(name(), age(), address());  
    }  
    @Bean(name = "address2")  
    public Address address(){  
        return new Address("Bupyeong", "Incheon");  
    }  
}
```

#### 출력

![](https://i.imgur.com/7ULd7Jr.png)

#### Alternative approach

```java

//Released in JDK 16 (record)  
record Person (String name, int age, Address address){};  
  
//Address  
record Address(String firstLine, String city){};  
@Configuration  
public class HelloWorldConfiguration {  

.....

	@Bean  
    public Person person3Parameters(String name, int age, Address address2){  
        return new Person(name, age, address2);  
    } 

}
```

>- 메소드를 직접 부르는거 대신에 파라미터로 부를 수 있다.\
>	- (name, age, address2);


## 정리

1. The first thing we learnt is the fact that you can configure your own custom names for your beans.
	- So instead of just using the default method Dimm, you can configure your own custom name.

2. The second thing we learned was the fact that you can retrieve the beans from a spring context in multiple ways.
	- One of the ways is by using a bean name.
	- The other one is to use the type of the class name as well.

3. The third thing that we learned is that you can use existing beans, which are managed by spring to create new beans.
	- So we are now creating a person three in here using existing beans, which are managed by spring framework.

### Spring 으로 관리

#### Basic (Before)
```java
public class App01GamingBasicJava {  
    public static void main(String[] args) {  
  
        //var marioGame = new MarioGame();  
        var superContraGame = new SuperContraGame(); // 1: Object creation  
        var gameRunner = new GameRunner(superContraGame);  
        // 2: Object Creation + Wiring of Dependencies  
        // Game is a Dependency of GameRunner        
        gameRunner.run();  
    }  
}
```

>- 인터페이스를 만들어서 갈아 끼우는 방식으로 관리

#### Spring (New)
```java
public class App03GamingSpringBeans {  
    public static void main(String[] args) {  
  
        var context =  
                new AnnotationConfigApplicationContext(GamingConfiguration.class);  
        context.getBean(GamingConsole.class).up();  
        context.getBean(GameRunner.class).run();  
  
    }  
}
```

```java
@Configuration  
public class GamingConfiguration {  
  
    @Bean  
    public GamingConsole game() {  
        var game = new PacmanGame();  
        return game;  
    }  
  
    @Bean  
    public GameRunner gameRunner(GamingConsole game) {  
        var gameRunner = new GameRunner(game);  
        return gameRunner;  
    }  
  
  
}
```

>- Spring 으로 관리하게 만드는게 더 적은 코드를 쓰는 것도 아니고 interface 등 그대로 유지된 상태에서 더 얹는 느낌이다.
>- 그럼 Spring이 어느 부분에서 도대체 왜!! 좋은걸까?
>- Starting Spring Framework(5) 에 정리

