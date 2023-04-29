---
layout: single
title: "Managing Objects and performing auto-wiring with Spring"
categories: Spring
tag: [Java,"[BIG] Starting Spring Framework","@Component","@ComponentScan"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Starting Spring Framework (5)
/ @ComponentScan / @Component / @Configuration /

## Spring is managing objects and performing auto-wiring

	1. But aren't we writing the code to create objects?
	2. How do we get Spring to create objects for us?

![](https://i.imgur.com/4X6BDRZ.png)


## Is Spring really making things easy?

### 매직 스프링 과정 (1)

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

### 매직 스프링 과정 (2)

>- 단순히 한 파일에 우선 합쳐준다.

```java
  
@Configuration  
class GamingConfiguration {  
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
public class App03GamingSpringBeans {  
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
    public static void main(String[] args) {  
        var context =  
                new AnnotationConfigApplicationContext(GamingConfiguration.class);  
        context.getBean(GamingConsole.class).up();  
        context.getBean(GameRunner.class).run();  
  
  
    }  
}
```


### 매직 스프링 과정 (3)
- 이제 런처클래스 자체에 이식
- 그리고 이전 GamingConfiguration 없앴으니 App03GamingSpringBeans 로 대체


```java
  
@Configuration  
public class App03GamingSpringBeans {  
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
    public static void main(String[] args) {  
  
        var context =  
                new AnnotationConfigApplicationContext(App03GamingSpringBeans.class);  
        context.getBean(GamingConsole.class).up();  
        context.getBean(GameRunner.class).run();  
  
  
    }  
}
```


### 매직 스프링 과정 (4)
- 스프링의 마법을 구현하기 위해 Pacman을 주석처리
- 기존의 Pacman 클래스에 @Component 추가
- @ComponentScan 에 경로 추가

```java
@Configuration  
@ComponentScan("클래스가 있는 경로")
public class App03GamingSpringBeans { 

   /* @Bean  
    public GamingConsole game() {  
        var game = new PacmanGame();  
        return game;  
    }  
  */
    @Bean  
    public GameRunner gameRunner(GamingConsole game) {  
        var gameRunner = new GameRunner(game);  
        return gameRunner;  
    }  
    public static void main(String[] args) {  
  
        var context =  
                new AnnotationConfigApplicationContext(App03GamingSpringBeans.class);  
        context.getBean(GamingConsole.class).up();  
        context.getBean(GameRunner.class).run();  
  
  
    }  
}
```

```java
@Component   <- 추가
public class PacmanGame implements GamingConsole {  
    public void up(){  
        System.out.println("UPUPUP");  
    }  
    public void down(){  
        System.out.println("DOWNDOWNDOWN");  
    }  
    public void left(){  
        System.out.println("LEFTLEFT");  
    }  
    public void right(){  
        System.out.println("RIGHTRIGHT");  
    }  
}
```

### 매직 스프링 과정 (5)
- 게임러너에도 @Component 추가
- @ComponentScan 경로에 해당 클래스가 존재하는한 
  스프링이 알아서 찾는다.

```java
@Configuration  
@ComponentScan("클래스가 있는 경로")
public class App03GamingSpringBeans { 
    public static void main(String[] args) {  
        var context =  
                new AnnotationConfigApplicationContext(App03GamingSpringBeans.class);  
        context.getBean(GamingConsole.class).up();  
        context.getBean(GameRunner.class).run();  
    }  
}
```

>- @Component, @ComponentScan, @Configuration 3개만으로 많은 부분이 간편해졌다
>- 스프링 매애직


## 정리
- Spring이 Objects 를 알아서 관리한다.
- Spring이 알아서 자동으로 Objects 생성해주고 북치고 장구치고 다 해준다.
- @Bean 도 알아서 생성해주고 다 해준다.
- @Component 는 스프링이 관리하는 요소중 하나다.
	- 우리가 어디서 Pacman Game 을 찾아야하는지만 스프링한테 알려주면 된다.
	- @ComponentScan 에서 설정한 경로에서 @Component가 달린 클래스를 자동으로 감지하고 찾는다.
- 위와 같은 특징들로 수동으로 Game Runner, Pac-Man game 을 작성하지 않아도 된다.
- Spring is able to scan the right packages, find the components, create instances, auto, add them for us and get the entire application up and runniung

그럼 Component랑 Bean 이랑 뭐가 다르지?
@Component vs @ Bean 에 정리해두었다
블로그 내에서 검색하면 된다.