---
layout: single
title: "[북 스터디] 스프링 핵심가이드 (1)"
categories: Spring
tags:
  - Spring
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
# (1) 스프링 부트 - 스프링 프레임워크 vs 스프링 부트 / 레이어드 아키텍쳐 / 디자인 패턴

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

> 핵심가치 : 애플리케이션 개발에 필요한 기반을 제공해서 개발자가 비즈니스 로직 구현에만 집중할 수 있겠끔 하는 것

#### 제어 역전 (IOC)

기존의 일반적인 자바 개발의 경우 사용자가 객체를 선언하고 해당 객체의 의존성을 생성한 후, 객체에서 제공하는 기능을 사용한다. 즉 객체를 생성하고 사용하려는 일련의 작업을 개발자가 직접 제어하는 구조다.   

하지만 **제어 역전 (IOC; Inversion of Control)** 은 객체의 생명 주기 관리를 외부에 위임한다. 객체의 관리를 컨테이너에 맡겨 제어권이 넘어가는 걸 제어 역전이라고 부른다. 또한 제어 역전을 통해 의존성 주입(DI; Dependency Injection), 관점 지향 프로그래밍 (AOP; Aspect-Oriented Programming) 등이 가능해 진다.  

스프링을 사용하면 객체의 제어권을 컨테이너로 넘기기 때문에 개발자는 비즈니스 로직을 작성하는데 더욱 집중할 수 있다.

#### 의존성 주입 (DI)

제어 역전의 방법 중 하나로, 사용할 객체를 직접 생성 X -> 외부 컨테이너가 생성한 객체를 주입받아 사용하는 방식을 의미한다.  

스프링에서 의존성을 주입받는 세가지 방법이 있다.

- 생성자를 통한 의존성 주입
- 필드 객체 선언을 통한 의존성 주입
- setter 메서드를 통한 의존성 주입

스프링에서 @Autowired 라는 어노테이션을 통해 의존성을 주입 받을 수 있는데 스프링 4.3이후 버전은 생성자를 통한 의존성 주입시 해당 어노테이션은 생략할 수 있다.  

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
자바에서 객체지향 프로그래밍 (OOP; Object Oriented Programming) 의 개념이 있는데 AOP는 OOP의 대체 개념이 아니라 OOP를 더욱 잘 사용하도록 돕는 개념이다.   

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

여기서 어플리케이션 로직에서 로깅과 트랜잭션등의 부가기능이 필요할 수 있는데 이 때 상품 정보 등록 기능과 상품정보 조회 기능은 각각의 로직으로 구현되지만 유지보수 목적이나 데이터베이스 접근을 위해 작성된 로깅과 트랜잭션은 동일한 기능을 수행할 확률이 높다.  

즉 핵심 기능을 구현한 두 로직에 동일한 코드가 포함된다는 것을 의미한다.  
AOP 관점에서는 부가 기능은 핵심 기능이 어떤 기능인지와는 무관하게 로직이 수행되기 전 또는 후에 수행되기만 하므로 *여러 비즈니스 로직에서 반복되는 부가 기능을 하나의 공통 로직으로 처리하도록 모듈화해 삽입하는 방식을 AOP라고 한다.*

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

각 라이브러리의 기능과 관련해서 자주 사용되고 서로 호환되는 버전의 모듈 조합을 제공해준다.   

> `spring-boot-starter` 의 여러 라이브러리를 함께 사용할 때는 의존성이 겹칠 수 있다. 이로 인해 버전 충돌이 발생할 수 있는데, 의존성 조합 충돌 문제가 없도록 `spring-boot-starter-parent` 가 검증된 조합을 알아서 제공해준다.  


#### 자동설정

스프링 부트는 스프링 프레임워크의 기능을 사용하기 위한 자동 설정 (Auto Configuration)을 지원한다. 자동 설정은 애플리케이션에 추가된 라이브러리를 실행하는 데 필요한 환경 설정을 알아서 찾아준다.*즉 애플리케이션을 개발하는 데 필요한 의존성을 추가하면 프레임 워크가 이를 자동으로 관리해준다.*

![](https://i.imgur.com/rvCGhID.png)


여기서 `@SpringBootApplication` 은 다음 세 개의 어노테이션을 합쳐놓은 구성이다.   
`@SpringBootConfiguration`
`@EnableAutoConfiguration`
`@ComponentScan`

`@SpringBootConfiguration` 하위에 또 여러가지의 어노테이션이 합쳐져있다.  

아무튼 스프링 부트 애플리케이션이 실행되면 우선 `@ComponentScan` 어노테이션이 `@Component` 시리즈 어노테이션이 붙은 클래스를 찾고 빈(bean) 을 등록한다. 
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

개발이 끝나고 서비스를 운영하는 시기에는 해당 시스템이 사용하는 스레드, 메모리, 세션 등의 주요 요소등을 모니터링 해야한다. 스프링 부트에는 Spring Boot Actuator 라는 자체 모니터링 도구가 있다.  

## 개발에 앞서 알면 좋은 기초 지식

> 애플리케이션이 '어떻게 동작하고 왜 이렇게 구성되는지' 생각하며 실습

### 서버간 통신

쇼핑몰을 서비스 단위로 개발한다고 가정할 시 이 말은 해당 쇼핑몰에 주문, 상품올리기, 게시판등 여러가지의 기능을 하나의 애플리케이션에 통합했다는 뜻이다.  

이럴 경우 하나의 기능을 업데이트할 때도 전체 사이트가 멈춰야한다.  
그만큼 개발에 보수적인 입장이 되고 서비스 자체의 규모도 커지기 때문에 서비스를 구동하는데 걸리는 시간도 길어진다.    

이 같은 문제를 해결하기 위해 나온 것이 마이크로 서비스 아키텍셔 (MSA; Microservice Architecture) 다.   

![](https://i.imgur.com/sfEXeLD.png)

단일 서비스로 구성된 A 사이트는 내부메서드 호출등을 통해 원하는 자원을 가져와 상용할 수 있지만 서비스 기능별로 구분해서 B 포털 사이트와 같이 독립적인 애플리케이션을 개발하게 되면 각 서비스 간에 통신해야 하는 경우가 발생하다.  
예를 들어 사용자가 블로그 기능을 사용하기 위해 로그인을 거쳐야만 하는 상황이 있는데 이를 *서버 간 통신* 이라고 말한다.  

서버간 통신은 한 서버가 다른 서버에 통신을 요청하는 것을 의미하며, 한 대는 서버, 다른 한 대는 클라이언트가 되는 구조다.  

가장 많이 사용되는 통신 방식 프로토콜은 HTTP/HTTPS 방식이다.  

### 스프링 부트의 동작 방식

스프링 부트에서 spring-boot-starter-web 모듈을 사용하면 기본적으로 톰캣을 사용하는 스프링 MVC 구조를 기반으로 동작한다.  


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

핸들러 매핑은 요청 정보를 기준으로 어떤 컨트롤러를 사용할지 선정하는 인터페이스다. 핸들러 매핑 인터페이스는 여러 구현체를 가지며, 대표적인 구현체 클래스는 다음과 같다.

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
스프링 부트에서는 자동 설정되기 때문에 별도의 설정이 필요하지 않다

> 책에서는 심도 있는 개발을 위해서는 스프링의 동작 원리를 파악해야 된다고 강조하고 있다. 

### 레이어드 아키텍쳐

레이어드 아키택처란 애플리케이션의 컴포넌트를 유사 관심사를 기준으로 레이어드로 묶어 수평적으로 구성한 구조를 의미한다. 레이어드 아키텍처는 여러 방면에서 쓰이는 개념이며, 어떻게 설계하느냐에 따라 용어와 계층의 수가 달라진다.    

일반적으로 레이어드 아키텍처라 하면 3계층 또는 4계층 구성을 의미한다. 차이는 인프라(데이터 베이스) 레이어의 추가 여부로 결정된다.    


![](https://i.imgur.com/J9vcIDA.png)

#### 프레젠테이션 계층

- 애플리케이션의 최상단 계층으로, 클라이언트의 요청을 해석하고 응답하는 역할을 한다.
- UI 나 API를 제공한다.
- 프레젠테이션 계층은 별도의 비즈니스 로직을 포함하고 있지 않으므로 비즈니스 계층으로 요청을 위임하고 받은 결과를 응답하는 역할만 수행한다.

#### 비즈니스 계층

- 애플리케이션이 제공하는 기능을 정의하고 세부 작업을 수행하는 도메인 객체를 통해 업무를 위임하는 역할을 수행한다.
- DDD (Domain-Driven Design) 기반의 아키텍처에서는 비즈니스 로직에 도메인이 포함되기도 하고, 별도로 도메인 계층을 두기도 한다.

#### 데이터 접근 계층

- 데이터베이스에 접근하는 일련의 작업을 수행한다.


레이어드 아키텍처는 하나의 애플리케이션에도 적용되지만 애플리케이션 간의 관계를 설명하는 데도 사용할 수 있다. 레이어드 아키텍처 기반 설계는 다음과 같은 특징을 가진다.   

- 각 레이어는 가장 가까운 하위 레이어의 의존성을 주입받는다.
- 각 레이어는 관심사에 따라 묶여 있으며, 다른 레이어의 역할을 침범하지 않는다.
	- 각 컴포넌트의 역할이 명확하므로 코드의 가독성과 기능 구현에 유리하다.
	- 코드의 확장성도 좋아진다.
- 각 레이어가 독립적으로 작성되면 다른 레이어와의 의존성을 낮춰 단위 테스트에 용이하다.

스프링 부트는 별도의 설정 없이 spring-boot-starter-web의 의존성을 사용할 때 기본적으로 스프링 MVC 구조를 띠게 되며 대체로 아래와 같은 아키텍처를 이룬다.  

![](https://i.imgur.com/W2mNOv0.png)

spring MVC 는 model-view-controller의 구조로 view 와 controller는 프레젠테이션 계층 영역이며 model은 비즈니스와 데이터 접근 계층의 영역으로 구분할 수 있다.   
다만 스프링 MVC 모델로 레이어드 아키텍처를 구현하기 위해서는 역할을 세분화해야한다.   
비즈니스 계층에 서비스를 배치해 엔티티와 같은 도메인 객체의 비즈니스 로직을 조합하도록 하고 데이터 접근 계층에는 DAO(Spring Data JPA 에서는 Repository) 를 배치해 도메인을 관리한다.

스프링의 레이어드 아키텍처는 다음과 같다.

- 프레젠테이션 계층
	- 상황에 따라 유저 인터페이스(UI; User Interface) 계층이라고 한다.
	- 클라이언트와의 접점이 된다.
	- 클라이언트로부터 데이터와 함계 요청을 받고 처리 결과를 응답으로 전달하는 역할을 한다.
	
- 비즈니스 계층
	- 상황에 따라 서비스(Service) 계층이라고 한다.
	- 핵심 비즈니스 로직을 구현하는 영역이다.
	- 트랜잭션 처리나 유효성 검사 등의 작업도 수행한다.
	
- 데이터 접근 계층
	- 상황에 따라 영속(Persistence) 계층이라고 한다.
	- 데이터베이스에 접근해야 하는 작업을 수행한다.
	- 그림에 DAO는 Spring Data JPA에서 리포지토리가 DAO 역할을 대신한다.

> 레이어드 아키텍처는 일반적인 계층 구조를 기반으로 필요에 따라 조금씩 변형해 사용한다. 가장 중요한 부분은 비즈니스 계층 영역인데, 비즈니스 로직을 어디서 담당할지 결정하고 설계하는 게 좋다. 비즈니스 로직은 도메인 계층에서 담당하는 게 일반적이다.

> 스프링에서 JPA 를 사용하면 @Entity를 정의한 클래스가 도메인 객체가 되며, 이곳에서 비즈니스 로직을 설계하면 좋다. 다만 서비스 레이어에서 비즈니스 로직을 담당하는 경우도 있으므로 이러한 역할과 책임을 잘 구분해서 설계해야 한다. 상황에 맞는 설계 방식을 알아두면 좋다.

#### 디자인 패턴

> 디자인 패턴은 소프트웨어를 설계할 때 자주 발생하는 문제들을 해결하기 위해 고안된 해결책이다.

##### 디자인 패턴의 종류

디자인 패턴을 구체화해서 정리한 대표적인 분류 방식으로 GoF 디자인 패턴이 있다.

![](https://i.imgur.com/779sBQr.png)

> GoF 디자인 패턴은 생성 패턴, 구조 패턴, 행위 패턴의 총 세가지로 구분된다.
> -생성패턴
> 	- 객체 생성에 사용되는 패턴으로, 객체를 수정해도 호출부가 영향을 받지 않게 한다.
> -구조패턴
> 	- 객체를 조합해서 더 큰 구조를 만드는 패턴이다.
> -행위패턴
> 	- 객체 간의 알고리즘이나 책임 분배에 관한 패턴이다.
> 	- 객체 하나로는 수행할 수 없는 작업을 여러 객체를 이용해 작업을 분배한다. 결합도 최소화를 고려할 필요가 있다.

##### 생성 패턴

- 추상 팩토리 : 구체적인 클래스를 지정하지 않고 상황에 맞는 객체를 생성하기 위한 인터페이스를 제공하는 패턴
- 빌더 : 객체의 생성과 표현을 분리해 객체를 생성하는 패턴
- 팩토리 메서드 : 객체 생성을 서브클래스로 분리해서 위임하는 패턴
- 프로토타입 : 원본 객체를 복사해 객체를 생성하는 패턴
- 싱글톤 : 한 클래스마다 인스턴스를 하나만 생성해서 인스턴스가 하나임을 보장하고 어느 곳에서도 접근할 수 있게 제공하는 패턴

##### 구조 패턴

- 어댑터 : 클래스의 인터페이스를 의도하는 인터페이스로 변환하는 패턴
- 브리지 : 추상화와 구현을 분리해서 각각 독립적으로 변형케 하는 패턴
- 컴포지트 : 여러 객체로 구성된 복합 객체와 단일 객체를 클라이언트에서 구별 없이 다루는 패턴
- 데코레이터 : 객체의 결함을 통해 기능을 동적으로 유연하게 확장할 수 있게 하는 패턴
- 퍼사드 : 서브시스템의 인터페이스 집합들에 하나의 통합된 인터페이스를 제공하는 패턴
- 플라이 웨이트 : 특정 클래스의 인스턴스 한 개를 가지고 여러 개의 가상 인스턴스를 제공할 때 사용하는 패턴
- 프락시 : 특정 객체를 직접 참조하지 않고 해당 객체를 대행(프락시) 하는 객체를 통해 접근하는 패턴

##### 행위 패턴

- 책임연쇄 : 요청 처리 객체를 집합으로 만들어 결합을 느슨하게 만드는 패턴
- 커맨드 : 실행될 기능을 캠슐화해서 주어진 여러 기능을 실행하도록 클래스를 설계하는 패턴
- 인터프리터 : 주어진 언어의 문법을 위한 표현 수단을 정의하고 해당 언어로 구성된 문장을 해석하고 패턴
- 이터레이터 : 내부 구조를 노출하지 않으면서 해당 객체의 집합 원소에 순차적으로 접근하는 방법을 제공하는 패턴
- 미디에이터 : 한 집합에 속한 객체들의 상호작용을 캡슐화하는 객체를 정의한 패턴
- 메멘토 : 객체의 상태 정보를 저장하고 필요에 따라 상태를 복원하는 패턴
- 옵저버 : 객체의 상태 변화를 관찰하는 관찰자들, 즉 옵저보 목록을 객체에 등록해 상태가 변할 때마다 메서드 등을 통해 객체가 직접 옵저버에게 통지하게 하는 디자인 패턴
- 스테이트 : 상태에 따라 객체가 행동을 변경하게 하는 패턴
- 스트래티지 : 행동을 클래스로 캡슐화해서 동적으로 행동을 바꿀 수 있게 하는 패턴
- 탬플릿 메서드 : 일정 작업을 처리하는 부분을 서브클래스로 캡슐화해서 전체 수행 구조는 바꾸지 않으면서 특정 단계만 변경해서 수행하는 패턴
- 비지터 : 실제 로직을 가지고 있는 객체(visitor)가 로직을 적용할 객체(element)를 방문하며 실행하는 패턴


#### REST API

##### REST란?

먼저 REST랑 'Representational State Transfer'의 약자로 WWW 웹과 같은 분산 하이퍼미디어 시스템 아키텍처의 한 형식이다.  
주고 받는 자원 Resource에 이름을 규정하고 URI에 명시해 HTTP 메서드 (GET, POST, PUT, DELETE) 를 통해 해당 자원의 상태를 주고 받는 것을 의미한다.

##### REST API란?

먼저 API는 "Application Programming Interface" 의 약자로 애플리케이션에서 제공하는 인터페이스를 의미한다. API를 통해 서버 또는 프로그램 사이를 연결할 수 있다. 즉 REST 아케텍처를 따르는 시스템/애플리케이션 인터페이스라 볼 수 있으며 REST 아키텍처를 구현하는 웹 서비스를 RESTful 하다 라고 표현한다.


##### REST의 특징

- 유니폼 인터페이스 -> 일관된 인터페이스 -> HTTP 표준 전송 규약을 따름 -> 프로그래밍 언어, 플랫폼, 기술에 종속되지 않음 -> 다양하게 호환 가능
- 무상태성 -> stateless -> 서버에 상태정보를 따로 보관하거나 관리 X -> 세션이나 쿠키 정보 별도 보관 X -> 클라이언트가 몇 명이든 각 요청은 개발로 처리 -> 불필요한 정보 관리 X -> 비즈니스 로직의 자유도가 높으며 설계가 단순
- 캐시 가능성 -> HTTP의 캐싱 기능 적용 O -> 서버의 트랜잭션 부하가 줄어 효율적이며 사용자 입장에서 성능 개선
- 레이어 시스템 -> REST 서버는 네트워크 상의 여러 계층으로 구성될 수 있지만 서버의 복잡도와 관계없이 클라이언트는 서버와 연결되는 포인트만 알면 됨
- 클라이언트-서버 아키텍처 -> REST 서버는 API를 제공하고 클라이언트는 사용자 정보를 관리하는 구조로 분리해 설계한다. 이 구성은 서로에 대한 의존성을 낮추는 기능을 함


##### REST URI 규칙

- URI 마지막에 /를 포함하지 않는다.
- `_ ` 대신 `-` 를 사용한다.
- URL에는 행위(동사)가 아닌 결과(명사)를 포함한다.
	- X) `http://localhost.com/delete-product/123`
	- O) `http://localhost.com/product/123`
- URI는 소문자로 작성해야 한다.
	- 일부 웹 서버의 운영체제는 대소문자를 구별한다.
- 파일 확장자는 URI에 포함하지 않는다.
	- HTTP에서 제공하는 Accept 헤더를 사용하는 게 좋다.