---
layout: single
title: "[Spring] How can I list all beans managed by Spring Framework?"
categories: Spring
tag: [Java,"[BIG][Spring] Starting Spring Framework","@Primary","@Qualifier","IoC","@Component","@Bean","@Component vs @Bean"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Starting Spring Framework (4)
/ AutoWiring / @Primary / @Qualifier / @Bean  / @Component

## How can I list all beans managed by Spring Framework?
- ![](https://i.imgur.com/IQzOS1h.png)
>- 위와 같이 스트림을 이용해서 관리되는 Bean 목록을 가져와 볼 수 있다.


## What if multiple matching beans are available?
- @Primary , @Qualifier 를 사용한다.
- 태그 "@Primary","@Qualifier" 에 Component에 대해서 정리해둠

### 그럼 @Bean 이랑 @Component 의 차이는 무엇일까?

- 우선 스프링 MVC 에서는 `@Controller`, `@Service`, `@Repository` 등으로 빈으로 등록할 수 있으며, configuration 관련 객체들은 `@Bean`과 `@Component` 으로 스프링 컨테이너에 객체를 빈으로 등록할 수 있다.

>- @Bean 은 메소드 레벨에서 선언하며, 반환되는 객체(인스턴스)를 개발자가 수동으로 Bean을 등록하는 어노테이션이다.
>- 반면 @Component는 클래스 레벨에서 선언함으로써 스프링이 런타임시에 컴포넌트 스캔을 하여 자동으로 빈을 찾고 등록하는 에노테이션이다.

#### @Bean 사용 예제
```java
@Configuration  
public class AppConfig {  
   @Bean  
   public MemberService memberService() {  
      return new MemberServiceImpl();  
   }  
}
```

#### @Component 사용 예제
```java
@Component  
public class Utility {  
   // ...  
}
```

#### 정리


| @Component                                       | @Bean                                                  |
| ------------------------------------------------ | ------------------------------------------------------ |
| 클래스에 사용                                    | 메소드에 사용                                          |
| 개발자가 직접 컨트롤이 가능한 내부 클래스에 사용 | 개발자가 컨트롤이 불가능한 외부 라이브러리 사용시 사용 |




출처 : https://youngjinmo.github.io/2021/06/bean-component/