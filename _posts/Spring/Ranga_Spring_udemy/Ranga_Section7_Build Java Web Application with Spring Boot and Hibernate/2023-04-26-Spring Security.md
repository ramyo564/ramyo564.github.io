---

layout: single
title: " Spring Security "
categories: Spring
tag: [Java,"Java Web Application with Spring and Hibernate","Spring Security","InMemoryUserDetailsManager","PasswordEncoder","BCryptPasswordEncoder","User","UserDetails","SecurityContextHolder.getContext().getAuthentication()"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Java Web Application with Spring and Hibernate (9)

/ Spring Security / InMemoryUserDetailsManager / PasswordEncoder / BCryptPasswordEncoder / User / UserDetails / 


## Change hard code

### before
```java
private AuthenticationService authenticationService;  
  
public LoginController(AuthenticationService authenticationService) {  
    super();  
    this.authenticationService = authenticationService;  
}

@RequestMapping(value="/",method = RequestMethod.GET)  
public String gotoLoginPage() {  
  
    return "welcome";  
}
@RequestMapping(value="login",method = RequestMethod.POST)  
//login?name=Ranga RequestParam  
public String gotoWelcomePage(@RequestParam String name,  
                              @RequestParam String password, ModelMap model) {  
  
    if(authenticationService.authenticate(name, password)) {  
  
        model.put("name", name);  
        model.put("password", password);  
        //Authentication  
        //name - in28minutes        //password - dummy  
        return "welcome";  
  
    }  
    model.put("errorMessage", "Invalid Credentials! ");  
    return "login";  
}
```
>- Plus, Delete AuthenticationService.java, login.jsp


### after
```java
package com.in28minutes.springboot.myfirstwebapp.login;

import org.springframework.stereotype.Controller;
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.SessionAttributes;

@Controller
@SessionAttributes("name")
public class WelcomeController {

	@RequestMapping(value="/",method = RequestMethod.GET)
	public String gotoWelcomePage(ModelMap model) {
		model.put("name", "in28minutes");
		return "welcome";
	}
}
```
>- Change Controller name

## Plus security dependency
```java
<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-security</artifactId>
		</dependency>
```

![](https://i.imgur.com/OpZ7z9T.png)
>- dependency 추가후에는 어느 페이지를 가든 로그인이 필요함
>- 콘솔에 비밀번호 자동으로 생성됨

## Conifituring Spring Security with Custom User and Password Encoder

```java

@Configuration
public class SpringSecurityConfiguration {
	//LDAP or Database
	//In Memory 
	//InMemoryUserDetailsManager
	//InMemoryUserDetailsManager(UserDetails... users)
	@Bean
	public InMemoryUserDetailsManager createUserDetailsManager() {
		Function<String, String> passwordEncoder
				= input -> passwordEncoder().encode(input);
		UserDetails userDetails = User.builder()
									.passwordEncoder(passwordEncoder)
									.username("in28minutes")
									.password("dummy")
									.roles("USER","ADMIN")
									.build();
		return new InMemoryUserDetailsManager(userDetails);
	}
	@Bean
	public PasswordEncoder passwordEncoder() {
		return new BCryptPasswordEncoder();
	}
}
```
>- @Configuration을 선언해서 직접적으로 Bean 알아서 생성
>- InMemoryUserDetailsManager 을 사용해서 내장되어 있는 User details 사용
>- passwordEncoder() 을 통해서 dummy 패스워드를 인코딩해서 메모리에 저장한다.


## Refactoring and Removing Hardcoding of User id

### before WelcomeController.java
```java
@RequestMapping(value="/",method = RequestMethod.GET)  
public String gotoWelcomePage(ModelMap model) {  
    model.put("name", "in28minutes");  
    return "welcome";  
}
```

### after WelcomeController.java
```java
@RequestMapping(value="/",method = RequestMethod.GET)  
public String gotoWelcomePage(ModelMap model) {  
    model.put("name", getLoggedinUsername());  
    return "welcome";  
}  
  
private String getLoggedinUsername() {  
    Authentication authentication =  
            SecurityContextHolder.getContext().getAuthentication();  
    return authentication.getName();  
}
```
>- SecurityContextHolder.getContext().getAuthentication();  를 통해서 로그인에 성공한 유저 이름을 그냥 갖고옴 


### before TodoController.java

```java
@RequestMapping("list-todos")  
public String listAllTodos(ModelMap model) {  
    List<Todo> todos = todoService.findByUsername("in28minutes");  
    model.addAttribute("todos", todos);  
  
    return "listTodos";  
}
```


### after TodoController.java

```java
@RequestMapping("list-todos")  
public String listAllTodos(ModelMap model) {  
    String username = (String)model.get("name");   
List<Todo> todos = todoService.findByUsername(username);  
    model.addAttribute("todos", todos);  
  
    return "listTodos";  
}
```
>- 웰컴 컨트롤러에서 작업한 모델 name 값을 username 객체에 저장하고 갖고옴

### before TodoService.java
```java
public List<Todo> findByUsername(String username){   
    return todos;  
}
```

### after TodoService.java
```java
public List<Todo> findByUsername(String username){
		Predicate<? super Todo> predicate = 
				todo -> todo.getUsername().equalsIgnoreCase(username);
		return todos.stream().filter(predicate).toList();
	}
```
>- username 이 매칭될 경우에 실행됨

## username을 활용하기 위해 다시 리팩
- 모델 안에 username을 세팅해놓았기 때문에
- 웰컴컨트롤러가 아닌 유저가 바로 Todo 컨트롤러를 거친다면 세션이 생성이 안된다.
- 따라서 Spring security에서 바로 세션 값을 갖고 오는 방법을 권장한다.

### before TodoController.java
```java
@RequestMapping("list-todos")  
public String listAllTodos(ModelMap model) {  
    String username = (String)model.get("name");  
    List<Todo> todos = todoService.findByUsername(username);  
    model.addAttribute("todos", todos);  
  
    return "listTodos";  
}
```

### after TodoController.java
```java
@RequestMapping("list-todos")  
public String listAllTodos(ModelMap model) {  
    String username = getLoggedInUsername(model);  
    List<Todo> todos = todoService.findByUsername(username);  
    model.addAttribute("todos", todos);  
  
    return "listTodos";  
}

private String getLoggedinUsername() {  
    Authentication authentication =  
            SecurityContextHolder.getContext().getAuthentication();  
    return authentication.getName();  
}
```

