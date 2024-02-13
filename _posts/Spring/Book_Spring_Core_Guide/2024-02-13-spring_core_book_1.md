---
layout: single
title: "[북스터디] 스프링 핵심가이드(1)"
categories: Spring
tags:
  - Spring
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
# (1) 스프링 부트 

- 1장 스프링 부트란?
- 2장 개발에 앞서 앞면 좋은 기초 지식

## 스프링 부트란?

- 스프링 프레임워크
	- 제어 역전 (IOC)
	- 의존성 주입 (DI)
	- 관점 지향 프로그래밍 (AOP)
	
- 스프링 프레임워크 vs 스프링 부트
	- 의존성 관리
	- 자동설정
	- 내장 AWS
	- 모니터링

### 스프링 프레임워크

> 핵심가치 : 애플리케이션 개발에 필요한 기반을 제공해서 개발자가 비즈니스 로직 구현에만 집중할 수 있게끔 하는 것

#### 제어 역전 (IOC)

기존의 일반적인 자바 개발의 경우 사용자가 객체를 선언하고 해당 객체의 의존성을 생성한 후 객체에서 제공하는 기능을 사용한다. 즉 객체를 생성하고 사용하려는 일련의 작업을 개발자가 직접 제어하는 구조다.   

하지만 **제어 역전 (IOC; Inversion of Control)** 은 객체의 생명 주기 관리를 외부에 위임한다. 객체의 관리를 컨테이너에 맡겨 제어권이 넘어가고 이를 제어 역전이라고 부르며 제어 역전을 통해 의존성 주입(DI; Dependency Injection), 관점 지향 프로그래밍 (AOP; Aspect-Oriented Programming) 등이 가증해 진다.  

스프링을 사용하면 객체의 제어권을 컨테이너로 넘기기 떄문에 개발자는 비즈니스 로직을 작성하는데 더욱 집중할 수 있다.

#### 의존성 주입 (DI)

제어 역전의 방법 중 하나로, 사용할 객체를 직접 생성하지 않고 외부 컨테이너가 생성한 객체를 주입받아 사용하는 방식을 의미한다.  

스프링에서 의존성을 주입받는 세가지 방법이 있다.

- 생성자를 통한 의존성 주입
- 필드 객체 선언을 통한 의존성 주입
- setter 메서드를 통한 의존성 주입

스프링에서 @Autowired 라는 어노테이션을 통해 의존성을 주입 받을 수 있는데 스프링 4.3이후 버전은 생성자를 통한 의존성 주입시 해당 어노테이션은 생량할 수 있다.  

>기존 자바의 개발방식

```java
@RestController
public class NoDIController {
	private MyService service = new MyServiceImpl();

	@GetMapping("/asd/asd")
	public String getHello(){
		return service.getHello();
	}
}
```

> 생성자를 통한 의존성 주입

```java
@RestController
public class DIController {
	MyService service;

	@Autowired
	public DIController(MyService myService){
		this.myService = myService;
	}

	@GetMapping("/asd/asd")
	public String getHello(){
		return myService.getHello();
	}
}

```

> 필드 객체 선언을 통한 의존성 주입

```java
@RestController
public class FieldInjectionController {
	

	@Autowired
	private MyService service;
}

```

> setter 메서드를 통한 의존성 주입

```java
@RestController
public class SetterInjectionController {

	MyService myService;

	@Autowired
	public void setMyService(MyService myService){
		this.myService = myService;
	}
}
```


#### 관점 지향 프로그래밍 (AOP) 

AOP는 스프링에서 아주 중요한 특징이다.   
자바에서 객체지향 프로그래밍 (OOP; Object Oriented Programming) 의 개념이 있는데 AOP를 OOP의 대체 개념이 아니라 OOP를 더욱 잘 사용하도록 돕는 개념이다.   

> OOP는 각 기능을 재사용 가능한 개별 객체로 구성해 프로그래밍을 한다는걸 의미한다.
> - 추상화 (abstraction)
> - 캡슐화 (encapsulation)
> - 상속 (inheritance)
> - 다형성 (polymorphism)

AOP 는 어떤 기능을 구현할 때 그 기능을 *핵심 기능* 과 *부가 기능* 으로 구분해 각각의 관점으로 보는걸 의미한다.

핵심기능은 비즈니스 로직을 구현하는 과정에서 비즈니스 로직이 처리하려는 목적 기능을 말한다. 예를 들어 클라이언트로부터 상품 정보 등록 요청을 받아 데이터베이스에 저장하고, 그 상품 정보를 조회하는 비즈니스 로직을 구현한다면   
1. 상품 정보를 데이터베이스에 저장하고 
2. 저장된 상품 정보 데이터를 보여주는 코드
가 핵심 기능이다.   

여기서 어플리케이션 로직에서 로깅과 트랜잭션등의 부가기능이 필요할 수 있는데 이 때 상품 정보 등록 기능과 상품정보 조회 기능은 각자 로직으로 구현되지만 유지보수 목적이나 데이터베이스 접근을 위해 작성된 로깅과 트랜잭션은 동일한 기능을 수행할 확률이 높다.  

즉 핵심 기능을 구현한 두 로직에 동일한 코드가 포함된다는 것을 의미한다.  
AOP 관점에서는 부가 기능은 핵심 기능이 어떤 기능인지에 무관하게 로직이 수행되기 전 또는 후에 수행되기만 하므로 *여러 비즈니스 로직에서 반복되는 부가 기능을 하나으 ㅣ공통 로직으로 처리하도록 모듈화해 삽입하는 방식을 AOP라고 한다.*

> AOP 를 구현하는 세 가지 방법
> - 컴파일 과정에 삽입하는 방식
> - 바이트코드를 메모리에 로그하는 과정에 삽입하는 방식
> - 프락시 패턴을 이용한 방식

이 가운데 스프링은 디자인 패턴 중 하나인 프락시 패턴을 통해 AOP 기능을 제공하고 있다.  

요약하자면 스프링 AOP 목적은 OOP와 마찬가지로 모듈화해서 재사용 가능한 구성을 만드는 것이고, 모듈화된 객체를 편하게 적용할 수 있게 함으로서 비즈니스 로직을 구현하는 데만 집중할 수 있게 도와준다.  

### 스프링 프레임워크 vs 스프링 부트

> 스프링 부트를 이용하면 단독으로 실행 가능한 사용 수준의 스프링 기반 애플리케이션을 손쉽게 만들 수 있다.

- 한마디로 초기설정을 하는데 간편해진다.

#### 의존성 관리

스프링 프레임 워크에서는 모듈의 의존성을 직접 설정해줘야 했지만 스프링 부트에서는 `spring-boot-starter` 라는 의존성을 제공한다.  

각 라이브러리으 ㅣ기능과 관련해서 자주 사용되고 서로 호환되는 버전의 모듈 조합을 제공해준다.   

> `spring-boot-starter` 의 여러 라이브러리를 함께 사용할 떄는 의존성이 겹칠 수 있다. 이로 인해 버전 충돌이 발생할 수 있는데, 의존성 조합 충돌 문제가 없도록 `spring-boot-starter-parent` 가 검증된 조합을 알아서 제공해준다.  


#### 자동설정

스프링 부트는 스프링 프레임워크의 기능을 사용하기 위한 자동 설정 (Auto Configuration)을 지원한다. 자동 설정은 애플리케이션에 추가된 라이브러리를 실행하는 데 필요한 환경 설정을 알아서 찾아준다.*즉 애플리케이션을 개발하는 데 필요한 의존성을 추가하면 프레임 워크가 이를 자동으로 관리해준다.*

![](https://i.imgur.com/rvCGhID.png)


여기서 `@SpringBootApplication` 은 다음 세 개의 어노테이션을 합쳐놓은 구성이다.   
`@SpringBootConfiguration`
`@EnableAutoConfiguration`
`@ComponentScan`

`@SpringBootConfiguration` 하위에 또 여러가지의 어노테이션이 합쳐져있다.  

아무튼 스프링 부트 애플리케이션이 실행되면 우선 `@ComponentScan` 어노테이션이 `@Component` 시리즈 어노테이션이 붙은 클래스를 찾고 빈(bean) 을 등록한다. ㅊ 
이후 `@EnableAutoConfiguration` 어노테이션을 통해 `spring-boot-autoconfigure` 패키지 안에 `spring.factories` 파일을 추가해 다양한 자동설정이 일부 조건을 거쳐 적용된다.  
`org.springframework.boot.autoconfigure.EnableAutoConfiguration` 하단에 많은 자동 설정이 되고 이 설정은 각 파일에 설정된 `@Conditional` 의 조건을 충족할 경우 빈에 등록되고 애플리케이션에 자동 반영된다.  

> `@Component` 시리즈 어노테이션에서 '시리즈'는`@Component` 어노테이션이 포괄하는 어노테이션들을 통칭하기 위해 사용한 표현이다. 이러한 `@Component` 시리즈 어노테이션의 대표적인 예는 아래와 같다.
> - `@Controller`
> - `@ResController`
> - `@Service`
> - `@Repository`
> - `@Configuration`

#### 내장 AWS 

스프링 부트의 각 웹 애플리케이션에는 WAS(Web Application Server) 가 존재한다. 웹 애플리케이션을 개발할 때 가장 기본이 되는 의존성인 'spring-boot-starter-web' 의 경우 톰캣을 내장한다.  

스프링 부트의 자동 설정 기능은 톰캣에도 적용되므로 특별한 설정 없이도 톰캣을 실행할 수 있다. 필요에 따라 톰캣이 아닌 다른 웹 서버 (Jetty , Undertow 등)으로 대체할 수도 있다.  

#### 모니터링 

개발이 끝나고 서비스를 운영하는 시기에는 해당 시스템이 상죵하는 스레드, 메모리, 세션 등의 주요 요소등을 모니터링 해야한다. 스프링 부트에는 Spring Boot Actuator 라는 자체 모니터링 도구가 있다.  

## 개발에 앞서 알면 좋은 기초 지식

> 애플리케이션이 '어떻게 동작하고 왜 이렇게 구성되는지' 생각하며 실습

### 서버간 통신

쇼핑몰을 서비스 단위로 개발한다고 가정할 시 해당 쇼핑몰에 주문, 상품올리기, 게시판등 여러가지의 기능을 하나의 애플리케이션에 통합했다는 뜻이다.  
이럴 경우 하나의 기능을 업데이트할 때도 전체 사이트가 멈춰야한다.  
그만큼 개발에 보수적인 입장이 되고 서비스 자체의 규모도 커지기 때문에 서비스를 구동하는데 걸리는 시간도 길어진다.    

이 같은 문제를 해결하기 위해 나온 것이 마이크로 서비스 아키텍셔 (MSA; Microservice Architecture) 다.   

![](https://i.imgur.com/sfEXeLD.png)

단일 서비스로 구성된 A 사이트는 내부메서드 호출등을 통해 원하는 자원을 가져와 상용할 수 있지만 서비스 기능별로 구분해서 B 포털 사이트와 같이 독립적인 애플리케이션을 개발하게 되면 각 서비스 간에 통신해야 하는 경우가 발생하다.  
예를 들어 사용자가 블로그 기능을 상죵하기 위해 로그인을 거쳐야만 하는 상황이 있는데 이를 *서버 간 통신* 이라고 말한다.  

서버간 통신은 한 서버가 다른 서버에 통신을 요청하는 것을 의미하며, 한 대는 서버, 다른 한 대는 클라이언트가 되는 구조다.  

가장 많이 사용 되는 통신 방식 트로토콜은 HTTP/HTTPS 방식이다.  

### 스프링 부트의 동작 방식

스프링 부트에서 spring-boot-starter-web 모듈을 사용하면 기본적으로 톰캣ㅇ즐 상죵하는 스프링 MVC 구조를 기반으로 동작한다.  


서블릿(Servlet) 은 클라이언트의 요청을 처리하고 결과를 반환하는 자바 웹 프로그래밍 기술이다. 일반적으로 서블릿은 서블릿 컨테이너 (Servlet Container) 에서 관리한다. 서블릿 컨테이너는 서블릿 인스턴스(Servlet Instance) 를 생성하고 관리하는 역할을 수행하는 주체로서 톰캣은 WAS의 역할과 서블릿 컨테이너의 역할을 수행하는 대표적인 컨테이너다.   

- 서블릿 객체를 생성, 초기화, 호출, 종료하는 생명주기를 관리한다.
- 서블릿 객체는 싱글톤 패턴으로 관리된다.
- 멀티 스레딩을 지원한다. 

	스프링에서 DispatcherServlet이 서블릿의 역할을 수행하는데 일반적으로 스프링은 톰캣을 임베드해 사용한다. 그렇기 때문에 서블릿 컨테이너와 DispatcherServlet은 자동 설정된 web.xml의 설정값을 공유한다.  



![](https://i.imgur.com/1EX82tI.png)

(1) DispatcherServlet 으로 요청(HttpServletRequest) 가 들어오면 DispatcherServlet은 핸들러 매핑(Handler Mapping) 을 통해 요청 URI에 매핑된 핸들러를 탐색한다. 여기서 핸들러는 컨트롤러(Controller) 를 의미한다.   
(2) 핸들러 어댑터 (HandlerAdapter) 로 컨트롤러를 호출한다.
(3) 핸들러 어댑터에 컨트롤러의 응답이 돌어오면 ModelAndView로 응답을 가공해 반환한다.
(4) 뷰 형식으로 리턴하는 컨트롤러를 사용할 때는 뷰 리졸버(View Resolver)를 통해 (View)를 받아 리턴한다.  

핸들러 매핑은 요청 정보를 기준으로 어떤 컨트롤러를 상죵할지 선정하는 인터페이스다. 핸들러 매핑 인터페이스는 여러 구현체를 가지며, 대표적인 구현체 클래스는 다음과 같다.

> BeanNameUrlHandlerMapping
> - 빈 이름을 URL로 사용하는 매핑 전략이다.
> - 빈을 정의할 때 슬래시('/') 가 들어가면 매핑 대상이된다.
> - 예) @Bean("/hello")

> ControllerClassNameHandlerMapping
> - URL과 일치하는 클래스 이름을 갖는 빈을 컨트롤러로 사용하는 전략이다.
> - 이름 중 Controller를 제외하고 앞부분에 작성된 suffix를 소문자로 매핑한다.

> SimpleUrlHandlerMapping
> - URL 패턴에 매핑된 컨트롤러를 사용하는 전략이다.

> DefaultAnnotationHandlerMapping
> - 어노테이션으로 URL과 컨트롤러를 매핑하는 방법이다.


뷰 리졸버는 뷰의 렌더링 역할을 담당하는 뷰 객체를 반환한다.   

![](https://i.imgur.com/1ktHZxe.png)

뷰가 없는 REST 형식의 `@ResponseBody` 를 사용해 MessageConverter를 거쳐 JSON 형식으로 변환해 응답하는 방식은 아래와 같다.   

![](https://i.imgur.com/H3QmbaS.png)

여기서 MessageConverter 는 요청과 응답에 대해 Body 값을 변환하는 역할을 수행한다. 스프링 부트의 자동 설정 내역을 보면 HttpMessageConverter 인터페이스를 사용하고 있다.  

![](https://i.imgur.com/cAFkqHG.png)

HttpMessageConverter 인터페이스를 빈으로 등록하는 걸 확인할 수 있다. 해당 인터페이스를 기반으로 하는 구현체 클래스는 다양하며, Content-Type 을 참고해 Converter를 선정한다.  
스프링 부트에서는 자동 설정되기 떄문에 별도의 설정이 필요하지 않다

> 책에서는 심도 있는 개발을 위해서는 스프링의 동작 원리를 파악해야 된다고 강조하고 있다. 

### 레이어드 아키텍쳐

#### 디자인 패턴

##### 디자인 패턴의 종류

##### 생성 패턴

##### 구조 패턴

##### 행위 패턴

#### REST API
- REST API
	- REST 란?
	- REST API란?
	- REST의 특징
	- REST의 URI 설계 규칙

