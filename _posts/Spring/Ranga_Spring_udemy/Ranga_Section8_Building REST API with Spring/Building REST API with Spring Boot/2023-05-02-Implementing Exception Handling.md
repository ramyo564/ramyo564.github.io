---

layout: single
title: " [Spring] Implementing Exception Handling "
categories: Spring
tag: [Java,"[BIG][Spring] Building REST API with Spring Boot","HTTP 에러코드","@ControllerAdvice","@ExceptionHandler"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Building REST API with Spring Boot (5)

/ HTTP 에러코드 / @ControllerAdvice / @ExceptionHandler / 

## 에러코드 정리


| HTTP ERROR CODE |              DESCRIPTION                                         |
| --------------- | ----------------------------------------------------- |
| 200             | 에러없이 성공적으로 페이지를 불러오거나 데이터를 전송 |
| 400             | Bad Request로써, 요청 실패-문법상 오류가 있어서 서버가 요청 사항을 이해하지 못함                                        |
| 404             | Not Found, 문서를 찾을 수 없음->클라이언트가 요청한 문서를 찾지 못한 경우에 발생함 (URL을 잘 살펴보기)                               |
| 405             | Method not allowed, 메소드 허용 안됨-> Request 라인에 명시된 메소드를 수행하기 위한 해당 자원의 이용이 허용되지 않았을 경우 발생함.    (페이지는 존재하나, 그걸 못보게 막거나 리소스를 허용안함)                                                |
| 415             |    지원되지 않는 형식으로 클라이언트가 요청을 해서 서버가 요청에 대한 승인을 거부한 오류를 의미한다.(ContentType,Content Encoding 데이터를 확인할 필요가 있다.) |
| 500             |       서버 내부 오류는 웹 서버가 요청사항을 수행할 수 없을 경우에 발생함                                                |
| 505                |     HTTP Version Not Supported                                                  |


- 참고 : https://articles09.tistory.com/5

## Implementing Exception Handlin

![](https://i.imgur.com/wK4HdHt.png)

- 현재 user가 12까지 있지 않은데 이런 경우 문서에서 찾을 수 없는 경우라
  500이 아닌 404 오류가 나와야 한다.

### UserDaoService.java
```java
public User findOne(int id) {  
	Predicate<? super User> predicate = user -> user.getId().equals(id);  
	return users.stream().filter(predicate).findFirst().get();  
}
```
>- get() 부분을 orElse(null) 로 변경해주면 된다.
>- 하지만 이것도 일종의 성공이라 200 이 나온다.
>	- ![](https://i.imgur.com/auqsQ3Y.png)

### before UserResource.java

```java
// GET /users  
@GetMapping("/users/{id}")  
public User retrieveUser(@PathVariable int id) {  
	return service.findOne(id);  
}
```

### after UserResource.java

```java
// GET /users  
@GetMapping("/users/{id}")  
public User retrieveUser(@PathVariable int id) {  
	User user = service.findOne(id);  
  
	if(user==null)  
			throw new UserNotFoundException("id : " + id);  
		  
		return user;  
}
```
>- 이전에 리턴으로 id 값만 줬던걸 orElse를 통해 null 값이 나오면 id 값을 보여준다.
>- UserNotFoundException 을 통해 메세지 전달.

### UserNotFoundException.java
```java
package com.in28minutes.rest.webservices.restfulwebservices.user;  
  
import org.springframework.http.HttpStatus;  
import org.springframework.web.bind.annotation.ResponseStatus;  
  
@ResponseStatus(code = HttpStatus.NOT_FOUND)  
public class UserNotFoundException extends RuntimeException {  
	public UserNotFoundException(String message) {  
		super(message);  
	}  
}
```

>- ![](https://i.imgur.com/zXKu1Kn.png)
>- id 도 12로 잘 나오고 404 도 정상적으로 출력


## Implementing Generic Exception Handling for all Resources

### Custom structre for exception response

#### ErrorDetails.java
```java
import java.time.LocalDateTime;  
public class ErrorDetails {  
	private LocalDateTime timestamp;  
	private String message;  
	private String details;  
public ErrorDetails(LocalDateTime timestamp, String message, String details) {  
	super();  
	this.timestamp = timestamp;  
	this.message = message;  
	this.details = details;  
}  
public LocalDateTime getTimestamp() {  
	return timestamp;  
}  
public String getMessage() {  
	return message;  
}  
public String getDetails() {  
	return details;  
}  
  
}
```

#### CustomizedResponseEntityExceptionHandler.java

```java
@ControllerAdvice  
public class CustomizedResponseEntityExceptionHandler extends ResponseEntityExceptionHandler{  
@ExceptionHandler(Exception.class)  
public final ResponseEntity<ErrorDetails> handleAllExceptions(Exception ex, WebRequest request) throws Exception {  
	ErrorDetails errorDetails = new ErrorDetails(LocalDateTime.now(),  
	ex.getMessage(), request.getDescription(false));  
	  
	return new ResponseEntity<ErrorDetails>(errorDetails, HttpStatus.INTERNAL_SERVER_ERROR);  
}  
@ExceptionHandler(UserNotFoundException.class)  
public final ResponseEntity<ErrorDetails> handleUserNotFoundException(Exception ex, WebRequest request) throws Exception {  
	ErrorDetails errorDetails = new ErrorDetails(LocalDateTime.now(),  
	ex.getMessage(), request.getDescription(false));  
	return new ResponseEntity<ErrorDetails>(errorDetails, HttpStatus.NOT_FOUND);  
	  
}  
}
```
>- ResponseEntityExceptionHandler
>	- This is the standard class which handles all spring MVC raised exceptions and it returns formatted error details
>- @ControllerAdvice
>	- Specialization of `@Component` for classes that declare `@ExceptionHandler`, `@InitBinder`, or `@ModelAttribute` methods to be shared across multiple @Controller classes.
>- 오버라이딩을 통해 ResponseEntityExceptionHandler 을 extends를 통해 갖고 옴
>- @ExceptionHandler(Exception.class) 는 모든 예외를 갖고 옴
>- 유저 아이디가 없을 경우 실행되게 UserNotFoundException를 주입
>	- ![](https://i.imgur.com/Q0BPkmA.png)

>	- 404 오류 번호, 아이디 값이 null일 때 메세지 및  ErrorDetails 클래스에서 지정해두었던 정보들 확인
