---
layout: single
title: "Exploring Dependency Injection Types"
categories: Spring
tag: [Java,"[BIG] Starting Spring Framework"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Starting Spring Framework (6)
/ Constructor-based / Setter-based / Field /

- Constructor-based : Dependencies are set by creating the Bean using its Constructor
- Setter-based : Dependencies are set by calling setter methods on your beans
- Field : No setter or constructor.
	  Dependencies is injected using reflection

## Field Injection

```java

import org.springframework.beans.factory.annotation.Autowired;  
import org.springframework.context.annotation.AnnotationConfigApplicationContext;  
import org.springframework.context.annotation.ComponentScan;  
import org.springframework.context.annotation.Configuration;  
import org.springframework.stereotype.Component;  
  
import java.util.Arrays;  
  
@Component  
class YourBusinessClass{  
    @Autowired  
    Dependency1 dependency1;  
    @Autowired  
    Dependency2 dependency2;  
  
    public String toString() {  
        return "Using " + dependency1 + " and " + dependency2;  
    }  
  
}  
@Component  
class Dependency1{  
  
}  
@Component  
class Dependency2{  
  
}  
  
@Configuration  
@ComponentScan//("com.in28minutes.learinspringframework.examples.a1") <= 현재패키지에서 알아서 찾음  
public class DependencyInjectionLauncherApplication {  
  
    public static void main(String[] args) {  
  
        var context =  
                new AnnotationConfigApplicationContext(DependencyInjectionLauncherApplication.class);  
        Arrays.stream(context.getBeanDefinitionNames())  
                .forEach(System.out::println);  
  
        System.out.println(context.getBean(YourBusinessClass.class));  
  
    }  
}
```

>- @ComponentScan의 경로를 따로 지정해주지 않는다면 현재 패키지를 기준으로 스캔해줌


## Setter-based

```java

import org.springframework.beans.factory.annotation.Autowired;  
import org.springframework.context.annotation.AnnotationConfigApplicationContext;  
import org.springframework.context.annotation.ComponentScan;  
import org.springframework.context.annotation.Configuration;  
import org.springframework.stereotype.Component;  
  
import java.util.Arrays;  
  
@Component  
class YourBusinessClass{  
  
    Dependency1 dependency1;  
    Dependency2 dependency2;  
  
    @Autowired  
    public void setDependency1(Dependency1 dependency1) {  
        this.dependency1 = dependency1;  
    }  
    @Autowired  
    public void setDependency2(Dependency2 dependency2) {  
        this.dependency2 = dependency2;  
    }  
  
    public String toString() {  
        return "Using " + dependency1 + " and " + dependency2;  
    }  
  
}  
@Component  
class Dependency1{  
  
}  
@Component  
class Dependency2{  
  
}  
  
@Configuration  
@ComponentScan//("com.in28minutes.learinspringframework.examples.a1") <= 현재패키지에서 알아서 찾음  
public class DependencyInjectionLauncherApplication {  
  
    public static void main(String[] args) {  
  
        var context =  
                new AnnotationConfigApplicationContext(DependencyInjectionLauncherApplication.class);  
        Arrays.stream(context.getBeanDefinitionNames())  
                .forEach(System.out::println);  
  
        System.out.println(context.getBean(YourBusinessClass.class));  
  
    }  
}
```
>- @Autowired를 setter로 옮겨줌


## Constructor-based
```java

import org.springframework.beans.factory.annotation.Autowired;  
import org.springframework.context.annotation.AnnotationConfigApplicationContext;  
import org.springframework.context.annotation.ComponentScan;  
import org.springframework.context.annotation.Configuration;  
import org.springframework.stereotype.Component;  
  
import java.util.Arrays;  
  
@Component  
class YourBusinessClass{  
  
    Dependency1 dependency1;  
    Dependency2 dependency2;  
  
    @Autowired  
    public YourBusinessClass(Dependency1 dependency1, Dependency2 dependency2) {  
        this.dependency1 = dependency1;  
        this.dependency2 = dependency2;  
    }  
  
    public String toString() {  
        return "Using " + dependency1 + " and " + dependency2;  
    }  
  
}  
@Component  
class Dependency1{  
  
}  
@Component  
class Dependency2{  
  
}  
  
@Configuration  
@ComponentScan//("com.in28minutes.learinspringframework.examples.a1") <= 현재패키지에서 알아서 찾음  
public class DependencyInjectionLauncherApplication {  
  
    public static void main(String[] args) {  
  
        var context =  
                new AnnotationConfigApplicationContext(DependencyInjectionLauncherApplication.class);  
        Arrays.stream(context.getBeanDefinitionNames())  
                .forEach(System.out::println);  
  
        System.out.println(context.getBean(YourBusinessClass.class));  
  
    }  
}
```
>- 생성자 injection 은 @Autowired가 필수가 아님


## Which one should you use?
- 이전 포스팅에 설명처럼 스프링 팀에서는 생성자 injection 사용을 추천한다
- Field injection은 유닛 테스트 하기 힘들다고 함 / 레거시 버전에서 많이 볼 수 있음
- "Field Injection" 검색