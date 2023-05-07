---

layout: single
title: "[Spring] PostConstruct and PreDestroy"
categories: Spring
tag: [Java,"[Spring] PostConstruct and PreDestroy","[Spring] @PreDestroy","[Spring] @PostConstruct","[Spring] Bean Lifecycle"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# PostConstruct and PreDestroy
/ PreDestroy / PostConstruct /


```java

import jakarta.annotation.PostConstruct;  
import jakarta.annotation.PreDestroy;  
import org.springframework.context.annotation.AnnotationConfigApplicationContext;  
import org.springframework.context.annotation.ComponentScan;  
import org.springframework.context.annotation.Configuration;  
import org.springframework.stereotype.Component;  
  
import java.util.Arrays;  
  
@Component  
class SomeClass {  
    private SomeDependency someDependency;  
  
    public SomeClass(SomeDependency someDependency){  
        super();  
        this.someDependency = someDependency;  
        System.out.println("All dependencies are ready");  
    }  
  
    @PostConstruct  
    public void initialize(){  
        someDependency.getReady();  
    }  
    @PreDestroy  
    public void cleanup(){  
        System.out.println("Cleanup");  
    }  
}  
@Component  
class SomeDependency {  
    public void getReady(){  
        System.out.println("Some logic using SomeDependency");  
    }  
}  
  
@Configuration  
@ComponentScan  
public class PrePostAnnotationsContextLauncherApplication {  
  
    public static void main(String[] args) {  
  
        try(  
        var context =  
                new AnnotationConfigApplicationContext(PrePostAnnotationsContextLauncherApplication.class)) {  
            Arrays.stream(context.getBeanDefinitionNames())  
                    .forEach(System.out::println);  
  
        }  
    }  
}
```

## 출력

![](https://i.imgur.com/RpkUT63.png)


## 정리

>- @PostConstruct
>	- 객체의 초기화 부분
>	- Bean이 여러번 초기화 되는 것을 방지
>	- 객체가 생성된 후 별도의 초기화 작업을 위해 실행하는 메소드를 선언
>	- WAS가 뜰 깨 bean이 생성된 다음 딱 한 번만 실행됨
>	- Identifies the method that will be executed after dependency injection is done to perform any initialization
>- @PreDestroy 
>	- 마지막 소멸 단계
>	- 스프링 컨테이너에서 객체(빈)을 제거하기 전에 해야할 작업이 있다면 메소드 위에 사용하는 어노테이션
>	- bean 제거하기 직전 한 번만 실행됨
>	- Identifies the method that will receive the callback notification to signal that the instance is in the process of being removed by the container. 
>	- Typically used to release resources that it has been holding.

- 개념만 이해했을 뿐 실제로 써보기 전 까지는 정확히 어떤 느낌인지 아직은 모르겠다.
태그 Bean Lifecycle 참고

참고 https://wonsjung.tistory.com/m/392, https://wecandev.tistory.com/105 , https://wildeveloperetrain.tistory.com/221