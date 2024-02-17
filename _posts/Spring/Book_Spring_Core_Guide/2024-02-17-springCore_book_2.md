---
layout: single
title: "[북 스터디] 스프링 핵심가이드 (2)"
categories: Spring
tags:
  - Spring
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
# (2) 스프링 부트 - API 작성법

## GET API 만들기

>@RestController와 @RequestMapping을 붙여서 내부에 선언되는 메서드에서 사용할 공통 URL을 설정한다.


```java
@RestController
@RequestMapping("/api/v1/get-api")
public class GetController {

}
```

### @RequestMapping 으로 구현하기

`@RequestMapping` 를 별다른 설정 없이 선언하면 HTTP의 모든 요청을 받는다.  
여기서 GET 요청만 받기 위해서는 RequestMethod.GET으로 설정을 해주면 된다.  

```java


    @RequestMapping(value = "/hello", method = RequestMethod.GET)
    public String getHello() {

        return "Hello World";
    }
```


스프링 4.3 이후로는 새로 나온 어노테이션 (`@GetMapping`,`@PutMapping`,`@PostMapping`,`@DeleteMapping`) 을 사용하기 때문에 더 이상 `@RequestMapping` 를 사용하지 않는다.  

### 매개변수가 없는 GET 메서드 구현

```java
@RestController
@RequestMapping("/api/v1/get-api")
public class GetController {

    @GetMapping(value = "/name")
    public String getName() {

        return "HoHi";
    }
```

### @PathVariable 을 활용한 GET 메서드 구현

URL자체에 값을 담아 요청한다.  

```java
    @GetMapping(value = "/variable1/{variable}")
    public String getVariable1(@PathVariable String variable) {
        return variable;
    }
```

URL에 `{}` 로 표시된 위치의 값을 받아 요청한다.  
값을 간단히 전달할 때 주로 사용하는 방법이며 GET 요청에서 많이 사용된다.   

`@GetMapping` 어노테이션의 값으로 URL을 입력할 때 중괄호를 사용해 어느 위치에서 값을 받을지 지정해야한다. 또한 메서드의 매개변수와 그 값을 연결하기 위해 `@PathVariable` 을 명시하고 해당 변수와 `@GetMapping` 에서 지정된 변수의 이름을 동일하게 맞춰야 한다.  

```java
   // http://localhost:8080/api/v1/get-api/variable2/{String 값}
    @GetMapping(value = "/variable2/{variable}")
    public String getVariable2(@PathVariable("variable") String var) {
    //public String getVariable2(@PathVariable(value = "variable") String var) 
        return var;
    }
```

### @RequestParam을 활용한 GET 메서드 구현

쿼리식으로 값을 담아서 요청은 URI 에서 `?` 를 기준으로 우측에 `{키}={값}` 형태로 구성된 요청을 전송한다. `@RequestParam` 을 명시해서 쿼리 값과 매핑하면 된다.  

```java
// http://localhost:8080/api/v1/get-api/request2?name=atue&email=tnkgr.fla@gmail.com&organization=thigun
    
    @GetMapping(value = "/request1")
    public String getRequestParam1(
        @RequestParam String name,
        @RequestParam String email,
        @RequestParam String organization) {
        return name + " " + email + " " + organization;
    }
```

만약 쿼리 스트링에 어떤 값이 들어올지 정확하지 않다면 Map 객체도 활용가능하다.

```java
    // http://localhost:8080/api/v1/get-api/request2?key1=value1&key2=value2
    @GetMapping(value = "/request2")
    public String getRequestParam2(@RequestParam Map<String, String> param) {
        StringBuilder sb = new StringBuilder();

        param.entrySet().forEach(map -> {
            sb.append(map.getKey() + " : " + map.getValue() + "\n");
        });

        return sb.toString();
    }

```

만약 회원 가입과 관련된 API 같은 경우 필수입력이 아닌 값에는 데이터를 입력하지 않고 이러한 경우에 매개변수 항목이 일정하지 않을 수 있다.  이럴 때 Map 객체로 받는 게 효율적이다.

> URI 와 URL의 차이
>- URL 은 우리가 흔히 말하는 웹 주소를 의미하며, 리소스가 어디에 있는지 알려주기 위한 경로를 의미한다. 반면 URI는 특정 리소스를 식별할 수 있는 식별자를 의미한다.
>- 웹에서는 URL을 통해 리소스가 어느 서버에 위치해 있는지 알 수 있으며, 그 서버에 접근해서 리소스에 접근하기 위해서는 대부분 URI가 필요하다.


### DTO 객체를 활용한 GET 메서드 구현

>  DTO란?
>  - DTO는 Data Transfer Object의 약자로, 다른 레이어 간의 데이터 교환에 활용된다. 각 클래스 및 인터페이스를 호출하면서 전달하는 매개변수로 사용되는 데이터 객체다.
>  - DTO는 데이터를 교환하는 용도로만 사용되는 객체다. 때문에 별도의 로직이 포함되지 않는다.

> DTO 와 VO
> - DTO 와 VO(Value Object) 의 역할을 서로 엄밀하게 구분하지 않고 사용할 때가 많은데 역할과 사용법에 차이가 있다.
> - VO는 데이터 그 자체로 의미가 있는 객체다. 가장 특징적인 부분은 읽기전용(Read-Only)로 설계한다는 점이다. 즉 VO는 값을 변경할 수 없게 만들어 데이터의 신뢰성을 유지해야한다.
> - DTO는 데이터를 전송을 위한 데이터 컨테이너로 볼 수 있다. 즉 애플리케이션 내부에서 사용되는 것이 아니라 다른 서버(시스템)으로 전달하는 경우에 사용된다.

## POST API 만들기

웹 애플리케이션을 통해 데이터베이스 등의 저장소에 리소스를 저장할 때 사용되는 API 다.   
GET API 에서는 URL의 경로나 파라미터에 변수를 넣어 요청을 하지만 POST API 에서는 저장하고자 하는 리소스나 값을 HTTP 바디(body) 에 담아 서버에 전달한다. 그래서 URI가 GET API에 비해 간단하다.  

```java
@RestController  
@RequestMapping("/api/v1/post-api")  
public class PostController {

}
```

### @RequestMapping 으로 구현하기

```java
@RequestMapping(value = "/domain", method = RequestMethod.POST)  
public String postExample(){  
    return "Hello Post API";  
}
```

### @RequestMapping를 활용한 POST 메서드 구현

```java
@PostMapping(value = "/member")  
public String postMember(@RequestBody Map<String, Object> postData) {  
    StringBuilder sb = new StringBuilder();  
  
    postData.entrySet().forEach(map -> {  
        sb.append(map.getKey() + " : " + map.getValue() + "\n");  
    });  
  
    return sb.toString();  
}
```

`Map` 객체는 요청을 통해 어떤 값이 들어오게 될지 특정하기 어려울 때 활용하기 딱 좋다. 요청 메세지에 들어갈 값이 정해져 있다면 `@RequestBody` DTO객체를 매개변수로 삼아 작성할 수있다.   

```java

@PostMapping(value = "/mem2")  
public String postMemberDto(@RequestBody Dto dto) {  
    return dto.toString();  
}
```


## PUT API 만들기

PUT Api는 데이터베이스 같은 저장소에 존재하는 리소스 값을 웹 애플리케이션 서버를 통해 업데이트한다.  

```java
@RestController  
@RequestMapping("/api/v1/put-api")  
public class PutController {

}
```

### @RequestBody를 활용한 PUT 메서드 구현

```java

// http://localhost:8080/api/v1/put-api/member  
@PutMapping(value = "/member")  
public String postMember(@RequestBody Map<String, Object> putData) {  
    StringBuilder sb = new StringBuilder();  
  
    putData.entrySet().forEach(map -> {  
        sb.append(map.getKey() + " : " + map.getValue() + "\n");  
    });  
  
    return sb.toString();  
}
```

만약 서버에 들어오는 요청에 담겨 있는 값이 정해져 있는 경우는 DTO 객체를 활용해서 구현한다.   

```java
// http://localhost:8080/api/v1/put-api/member1  
@PutMapping(value = "/member1")  
public String postMemberDto1(@RequestBody MemberDto memberDto) {  
    return memberDto.toString();  
}  
  
// http://localhost:8080/api/v1/put-api/member2  
@PutMapping(value = "/member2")  
public MemberDto postMemberDto2(@RequestBody MemberDto memberDto) {  
    return memberDto;  
}
```

Dto1의 메서드 같은 경우 Content-Type 이  text/plain이며 일반 문자열로 반환되지만 Dto2 같은 경우는 JSON 형식으로 전달된다. 또한 `@RestController`어노테이션이 지정된 클래스는 `@ResponseBody` 를 생략할 수 있는데 자동으로 값을 JSON과 같은 형식으로 변환해서 전달하는 역할을 수행한다.
### ResponseEntity를 활용한 PUT 메서드 구현

```java
public class ResponseEntity<T> extends HttpEntity<T>{
	private final Object status;
	...
	..
	.
	
}
```

RequestEntity와 ResponseEntity는 HttpEntity를 상속받아 구현한 클래스다. 그중 ResponseEntity는 서버에 들어온 요청에 대해 응답 데이터를 구성해서 전달할 수 있게 한다.  

```java
// http://localhost:8080/api/v1/put-api/member3  
@PutMapping(value = "/member3")  
public ResponseEntity<MemberDto> postMemberDto3(@RequestBody MemberDto memberDto) {  
    return ResponseEntity  
        .status(HttpStatus.ACCEPTED)  
        .body(memberDto);  
}
```

ResponseEntity는 HttpEntity로 부터 HttpHeaders와 Body를 가지고 자체적으로 HttpStatus 를 구현한다.  
## DELETE API 만들기

데이터베이스나 캐시에 있는 리소스를 조회하고 삭제하는 역할을 한다.

```java
@RestController  
@RequestMapping("/api/v1/delete-api")  
public class DeleteController {

}
```

### @PathVariable 과 @RequestParam을 활용한 DELETE 메서드 구현

```java
// http://localhost:8080/api/v1/delete-api/{String 값}  
@DeleteMapping(value = "/{variable}")  
public String DeleteVariable(@PathVariable String variable) {  
    return variable;  
}
```

`@DeleteMapping` 어노테이션에 정의한 Value의 이름과 메서드의 매개변수 이름을 동일하게 설정해야 삭제할 값이 주입된다. 또는 `@RequestParam` 어노테이션을 통해 쿼리스트링 값도 받을 수 있다.

```java
// http://localhost:8080/api/v1/delete-api/request1?email=value  
@DeleteMapping(value = "/request1")  
public String getRequestParam1(@RequestParam String email) {  
    return "e-mail : " + email;  
}
```

## REST API 문서화 하는 방법 - Swagger

2024년 1월 기준으로 Swagger는 스프링 3.0 이상에서 정상작동을 하지 않는다 ㅜ
다시 동작하거나 다른 라이브러리 생기면 그 때 적는걸로..
## Logback 

로깅이란 애플리케이션이 동작하는 동안 시스템의 상태나 동작 정보를 시간순으로 기록하는 것을 의미한다.  
디버깅하거나 개발 이후 발생한 문제를 해결할 때 원인을 분석하는 데 꼭 필요한 요소다.  
spring-boot-starter-web 라이브러리 내부에 내장돼 있어 별도의 의존성을 추가하지 않아도 된다.  

xml 파일로 설정할 수 있으며 필요한데로 설정하면 된다.  
주기적으로 삭제하거나 관리할 수 있다.   

```java
<?xml version="1.0" encoding="UTF-8"?>  
<configuration>  
    <property name="LOG_PATH" value="./logs"/>  
  
    <!-- Appenders -->  
    <appender name="console" class="ch.qos.logback.core.ConsoleAppender">  
        <filter class="ch.qos.logback.classic.filter.ThresholdFilter">  
            <level>INFO</level>  
        </filter>        <encoder>            <pattern>[%d{yyyy-MM-dd HH:mm:ss.SSS}] [%-5level] [%thread] %logger %msg%n</pattern>  
        </encoder>    </appender>  
    <appender name="INFO_LOG" class="ch.qos.logback.core.rolling.RollingFileAppender">  
        <filter class="ch.qos.logback.classic.filter.ThresholdFilter">  
            <level>INFO</level>  
        </filter>        <file>${LOG_PATH}/info.log</file>  
        <append>true</append>  
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">  
            <fileNamePattern>${LOG_PATH}/info_${type}.%d{yyyy-MM-dd}.gz</fileNamePattern>  
            <maxHistory>30</maxHistory>  
        </rollingPolicy>        <encoder>            <pattern>[%d{yyyy-MM-dd HH:mm:ss.SSS}] [%-5level] [%thread] %logger %msg%n</pattern>  
        </encoder>    </appender>  
    <!-- TRACE > DEBUG > INFO > WARN > ERROR > OFF -->  
    <!-- Root Logger -->    <root level="INFO">  
        <appender-ref ref="console"/>  
        <appender-ref ref="INFO_LOG"/>  
    </root></configuration>
```

사용하는 방법은 Logger 객체를 각 클래스에 정의해서 사용한다.

```java
@RestController  
@RequestMapping("/api/v1/get-api")  
public class GetController {  
  
    private final Logger LOGGER = LoggerFactory.getLogger(GetController.class);  
  
   @RequestMapping(value = "/hello", method = RequestMethod.GET) 
    public String getHello() {  
        LOGGER.info("getHello 메소드가 호출되었습니다.");  
        return "Hello World";  
    }
}
```

