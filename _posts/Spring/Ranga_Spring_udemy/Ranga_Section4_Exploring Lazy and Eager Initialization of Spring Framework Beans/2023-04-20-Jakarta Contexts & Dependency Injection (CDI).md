---

layout: single
title: " Jakarta Contexts & Dependency Injection (CDI) "
categories: Spring
tag: [Java,"Jakarta Contexts & Dependency Injection (CDI)","@Named","@Inject","@Resource"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Jakarta Contexts & Dependency Injection (CDI)
/ CDI / 

- Spring Framework V1 was released in 2004
- CDI specification introduced into Java EE 6 platform in December 2009
- Now called Jakarta Contexts and Dependency Injection (CDI)
- CDI is a specification (interface)
	- Spring Framework implements CDI
- Important Inject API Annotations:
	- Inject (~Autowired in Spring)
	- Named (~Component in Spring)
	- Qualifier
	- Scope
	- Singleton

```java
<dependency>  
   <groupId>jakarta.inject</groupId>  
   <artifactId>jakarta.inject-api</artifactId>  
   <version>2.0.1</version>  
</dependency>
```

```java
import java.util.Arrays;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

import jakarta.inject.Inject;
import jakarta.inject.Named;

//@Component
@Named
class BusinessService {
	private DataService dataService;

	//@Autowired
	@Inject
	public void setDataService(DataService dataService) {
		System.out.println("Setter Injection");
		this.dataService = dataService;
	}


	public DataService getDataService() {
		return dataService;
	}

	
	
}

//@Component
@Named
class DataService {
	
}

@Configuration
@ComponentScan
public class CdiContextLauncherApplication {
	
	public static void main(String[] args) {

		try (var context = 
				new AnnotationConfigApplicationContext
					(CdiContextLauncherApplication.class)) {
			
			Arrays.stream(context.getBeanDefinitionNames())
				.forEach(System.out::println);
			
			System.out.println(context.getBean(BusinessService.class)
					.getDataService());

		}
	}
}
```

## 정리
>- @Inject 는 @Autowired 와 유사하게 쓸 수 있다.
>- @Named 는 @Component 과 유사하게 쓸 수 있다.

### @Autowired

-   Field, Setter Method, Constructor에 사용 가능
-   기본적으로 타입을 기준으로 의존성을 주입
-   동일한 타입의 빈이 여러 개 존재할 경우 기본적으로 참조 변수의 이름과 동일한 빈을 찾아서 주입
-   이름을 기준으로 의존성을 주입할 때 @Qualifier 사용해서 주입될 빈 지정 가능
-   동일한 타입의 빈이 여러 개 존재할 경우 @Primary을 사용해서 주입될 빈 지정 가능

### @Resource

-   Field, Setter Method에 사용 가능합니다. (생성자에는 사용 불가능)
-   기본적으로 참조 변수의 이름과 동일한 빈이 존재하면 해당 빈을 주입해줍니다.
-   name속성을 사용해서 주입받을 빈을 지정할 수 있습니다.
-   이름으로 빈을 찾지 못하면 타입을 기준으로 의존성을 주입합니다.
-   ***Java9 이후부터는 삭제되어서 사용할 수 없습니다.***

### @Inject

@Inject 은 기본적으로 @Autowired와 동일하게 사용할 수 있으며 추가적으로 @Named을 같이 사용할 수 도 있다.

-   Field, Setter Method, Constructor에 사용 가능
-   기본적으로 타입을 기준으로 의존성을 주입
-   동일한 타입의 빈이 여러 개 존재할 경우 기본적으로 참조 변수의 이름과 동일한 빈을 찾아서 주입
-   이름을 기준으로 의존성을 주입할 때 @Qualifier을 사용해서 주입될 빈 지정 가능
-   동일한 타입의 빈이 여러 개 존재할 경우 @Primary을 사용해서 주입될 빈 지정 가능
-   이름을 기준으로 의존성을 주입할 때 @Named을 사용해서 주입될 빈 지정 가능


### @Inject vs @ Autowired 차이점

- 먼저 @Inject은 Java에서 지원하고 @Autowired는 SpringFramwork에서 지원
- 두 어노테이션의  FQCN (fully Qualified Class Name)을 보면 바로 알 수 있음
- **@Inject : javax.inject.Inject** 
- **@Autowired : org.springframework.beans.factory.annotation.Autowired**
- 그래서 @Inject은 Spring에 종속적이지 않은 어노테이션이고, @Autowired는 Spring에서만 사용 가능
- 추가적으로 @Inject은 nullable 하게 만들 수가 없음
- 무조건 빈을 주입받아야 하고 해당하는 빈이 없다면 에러가 발생
- (Spring 4.xxx , Java 8 이상 일 경우 Optional을 사용해서 할 순 있지만 어노테이션 자체에서 사용 가능한 속성은 없다.)
- 그러나 @Autowired은 @Autowired (required = false)와 같이 선언하면 참조 변수에 빈을 주입하지 못해도 에러가 발생하지 않고 null로 처리

## @Autowired vs @Resource vs @Inject

|                      | @Autowired                                             | @Resource                 | @Inject                                                                       |
| -------------------- | ------------------------------------------------------ | ------------------------- | ----------------------------------------------------------------------------- |
| FQCN                 | org.springframework.beans.factory.annotation.Autowired | javax.annotation.Resource | javax.inject.Inject                                                           |
| Target               | Field, Setter Method ,Constructor                      | Field, Setter Method      | Field, Setter Method ,Constructor                                             |
| 의존성 <br> 주입순서 | 타입 -> 이름                                           | 이름 -> 타입              | 타입 -> 이름                                                                  |
| 빈 지정 <br> 방법    | 1. @Qualifier("빈 이름") <br>  2. @Primary 사용        | @Resource(name="빈 이름") | 1. @Qualifier("빈 이름")<br> 2. @Primary 사용<br>  3. @Named(value="빈 이름") |
| Nullable             | required=false 속성 사용                               | X                         | X                                                                             |
| 비고                 | SpringFramework 안에서만 사용 가능                     | Java 9 이후로 삭제        | 특정 프레임 워크에 종속 X                                                                              |

>- @Inject 가 있다는 정도로만 일단 알고 넘어가면 될 듯 하다.
>- 나중에 더 자세히 알게 되거나 확실히 이해되면 여기에 다시 정리
>- 실제로 활용하게 되면 이 밑으로 더 자세하게 내용 정리




참조 : https://codingnojam.tistory.com/13