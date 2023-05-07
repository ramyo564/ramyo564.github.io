---

layout: single
title: "[Spring] Exploring Lazy and Eager Initialization of Spring Framework Beans"
categories: Spring
tag: [Java,"[Spring] Exploring Lazy and Eager Initialization of Spring Framework Beans"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Exploring Lazy and Eager Initialization of Spring Framework Beans 
/ @Lazy /


- Default initialization for Spring Beans : ***Eager***
- Eager initialization is recommended:
	- Errors in the configuration are discovered immediately at application startup
- However, you can configure beans to be lazily initialized using ***Lazy*** annotation
	- NOT recommended (AND) Not frequently used
- ***Lazy*** annotation::
	- Can be used almost everywhere @Component and @Bean are used
	- Lazy-resolution proxy will be injected instead of actual dependency
	- Can be used on Configuration(@Configuration) class:
		- All @Bean methods within the @Configuration will be lazily initialized 

```java

import java.util.Arrays;  
  
@Component  
class ClassA {  
}  
@Component  
@Lazy  
class ClassB {  
    private ClassA classA;  
  
    public ClassB(ClassA classA) {  
        //Logic  
        System.out.println("Some Initialization logic");  
        this.classA = classA;  
    }  
}  
  
@Configuration  
@ComponentScan  
public class LazyInitializationLauncherApplication {  
  
    public static void main(String[] args) {  
  
        var context =  
                new AnnotationConfigApplicationContext(LazyInitializationLauncherApplication.class);  
//        Arrays.stream(context.getBeanDefinitionNames())  
//                .forEach(System.out::println);  
  
  
    }  
}
```


## Comparing Lazy Initialization vs Eager Initialiation

| Heading                                           | Lazy Initialization                                              | Eager Initialization                             |
| ------------------------------------------------- | ---------------------------------------------------------------- | ------------------------------------------------ |
| Initialization time                               | Bean initialized when it is first made use of in the application | Bean initializaed at startup of the application  |
| Default                                           | NOT Default                                                      | Default                                          |
| Code Snippet                                      | @Lazy OR @Lazy(value = ture)                                     | @Layz(value=false) OR (Absence of @Lazy)         |
| What happens if there are errors in initializing? | Errors will result in runtime exceptions                         | Errors will prevent application from starting up |
| Usage                                             | Rarely used                                                      | Very frequently used                             |
| Memory Consumption                                | Less (until bean is initialized)                                 | All beans are initialized at start up            |
| Recommended Scenario                              | Beans very rarely used in your app                               | Most of your beans                                                 |



## 정리

>- @Lazy 를 선언하면 Class B를 사용하기 전까지는 생성자 초기화 실행이 안됨
>- 일반적으로 추천안함 -> 오류나 버그 같은거 초기에 알 수 가 없음
>- 물론 더 이상 업데이트가 없고 오류나 버그에 100퍼센트 자신 있거나 한게 아니라면 비추천이라고 한다.
>- 심각한 성능저하에 대한 최적화 과정에는 필요할 수 있다.

`If you have a lot of complex initialization logic and if you don't want it to delay this startup.` <br> 
`In those kind of situations, you can think about lazy initialization of spring beans.`<br> 
`However, it is very, very important to remember that if you are making use of lazy initialization errors in spring configuration will not be discovered at application startup. ` <br>
`They will become runtime errors.`
