---

layout: single
title: " [Spring] Bean Scopes - Prototype and Singleton "
categories: Spring
tag: [Java,"[Spring] Bean Scopes - Prototype and Singleton","[Spring] Singleton vs Prototype","[Spring] Prototype Scope","[Spring] @Scope"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Bean Scopes - Prototype and Singleton 
/ Prototype / Singleton /

- Spring Beans are defined to be used in a specific scope
	- Singleton - One object instance per Spring IoC container
	- Prototype - Possibly many object instances per Spring IoC container
	- Scopes applicable ONLY for web-aware SpringApplicationContext
		- Request - One object instance per single HTTP request
		- Session - One object instance per user HTTP Session
		- Application - One object instance per web application runtime
		- Websocket - One object instance per WebSocket instance
- Java Singleton(GOF) vs Spring Singleton
	- Spring Singleton : One object instance per Spring IoC container
	- Java Singleton(GOF) : One Object instance per JVM

>- Usually people do not run multiple spring IoC containers per JVM
>- So in 99.99% of the scenarios spring singleton are the same as Java Singleton

```java

import org.springframework.beans.factory.config.ConfigurableBeanFactory;  
import org.springframework.context.annotation.AnnotationConfigApplicationContext;  
import org.springframework.context.annotation.ComponentScan;  
import org.springframework.context.annotation.Configuration;  
import org.springframework.context.annotation.Scope;  
import org.springframework.stereotype.Component;  
  
import java.util.Arrays;  
  
@Component  
class NormalClass {  
  
}  
@Scope(value= ConfigurableBeanFactory.SCOPE_PROTOTYPE)  
@Component  
class PrototypeClass {  
  
}  
  
@Configuration  
@ComponentScan  
public class SimpleSpringLauncherApplication {  
  
    public static void main(String[] args) {  
  
        var context =  
                new AnnotationConfigApplicationContext(SimpleSpringLauncherApplication.class);  
  
        System.out.println(context.getBean(NormalClass.class));  
        System.out.println(context.getBean(NormalClass.class));  
        System.out.println(context.getBean(PrototypeClass.class));  
	    System.out.println(context.getBean(PrototypeClass.class));  
  
  
    }  
}
```

## 출력

![](https://i.imgur.com/dDgdyMY.png)

- 해시코드를 보면 NormalClass 는 같은 instance 지만
- PrototypeClass 는 각각 다른 instance 다.

## Prototype vs Singleton Bean Scope

| Heading              | Prototype                                                    | Singleton                                                               |
| -------------------- | ------------------------------------------------------------ | ----------------------------------------------------------------------- |
| Instances            | Possibly Many per Spring IoC Container                       | One per Spring IoC Container                                            |
| Beans                | New bean instance created every time the bean is referred to | Same bean instance reused                                               |
| Default              | NOT Default                                                  | Default                                                                 |
| Code Snippet         | @Scope(Value = ConfigurableBeanFactory.SCOPE_PROTOTYPE)      | @Scope(value = ConfigurableBeanFactory.SCOPE_SINGLETON) <br> OR Default |
| Usage                | Rarely used                                                  | Very frequently used                                                    |
| Recommended Scenario | Stateful beans                                               | Stateless beans                                                                        |

## 정리
>- Prototype 쓰일 일이 아주 적다.
>- 대부분의 Bean 은 Singleton을 사용한다.
>- 유저 정보를 유지시키는 bean을 사용해야 된다면 혼용해서 사용하면 안된다.
>- 이럴 때 Prototype bean 을 쓴다.
>- lazy가 디폴트라 @Lazy 쓸 필요 없음
>- 싱글톤 : 기본 스코프, 스프링 컨테이너의 시작과 종료까지 유지되는 가장 넓은 범위의 스코프다
>- 프로토타입 : 스프링 컨테이너는 프로토 타입 빈의 생성과 의존관계 주입까지만 관여하고 더는 관리하지 않는 매우 짧은 범위의 스코프다.

@Scope 태그에 좀 더 정리해둔거 있음

참고 : https://9hyuk9.tistory.com/11