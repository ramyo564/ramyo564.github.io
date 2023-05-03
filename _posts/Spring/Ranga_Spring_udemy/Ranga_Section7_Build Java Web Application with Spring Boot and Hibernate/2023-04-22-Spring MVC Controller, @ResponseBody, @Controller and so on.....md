---

layout: single
title: " [Spring] Spring MVC Controller, @ResponseBody, @Controller "
categories: Spring
tag: [Java,"Spring MVC Controller","[BIG][Spring] Java Web Application with Spring and Hibernate","@ResponseBody","@Controller","JSP","@RequestParam","@RequestMapping","Java Web Application with Spring and Hibernate"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Java Web Application with Spring and Hibernate (1)
/ @ResponseBody / @Controller / @RequestMapping / @RequestParam

## Spring MVC Controller, @ResponseBody, @Controller

```java
@Controller  
public class SayHelloController {  
  
    //"say-hello" => "Hello! What are you learning today?"  
  
    //say-hello    // http://localhost:8080/say-hello    
    @RequestMapping("say-hello")  
    @ResponseBody  
    public String sayHello() {  
        return "Hello! What are you learning today?";  
    }  
}
```
>- @Service, @Component 같은걸로 bean 인걸 알려줘야함
>	- 여기서는 유저의 요청 처리 다음 뷰에 객체를 넘겨주는 역할을 하는 컨트롤러 콜 -> @Controller 사용
>- @RequestMapping <- URL 주소
>- 브라우저로 어떤걸 보여줄지 -> @ResponseBody


참고 : https://cheershennah.tistory.com/179 <- @ResponseBody

## Enhancing Spring MVC Controller to provide HTML response

### HTML Super Hard code

```java
@RequestMapping("say-hello-html")  
@ResponseBody  
public String sayHelloHtml() {  
    StringBuffer sb = new StringBuffer();  
    sb.append("<html>");  
    sb.append("<head>");  
    sb.append("<title> My First HTML Page - Changed</title>");  
    sb.append("</head>");  
    sb.append("<body>");  
    sb.append("My first html page with body - Changed");  
    sb.append("</body>");  
    sb.append("</html>");  
  
    return sb.toString();  
}
```

![](https://i.imgur.com/p6f4pTx.png)


### Using JSP

```java
application.properties

#server.port=8081  
spring.mvc.view.prefix=/WEB-INF/jsp/  
spring.mvc.view.suffix=.jsp  
logging.level.org.springframework=debug
```

```java
pom.xml

<dependency>  
   <groupId>org.apache.tomcat.embed</groupId>  
   <artifactId>tomcat-embed-jasper</artifactId>  
</dependency>
```

```java

// "say-hello-jsp" => sayHello.jsp  
// /src/main/resources/META-INF/resources/WEB-INF/jsp/sayHello.jsp  
// /src/main/resources/META-INF/resources/WEB-INF/jsp/welcome.jsp  
// /src/main/resources/META-INF/resources/WEB-INF/jsp/login.jsp  
// /src/main/resources/META-INF/resources/WEB-INF/jsp/todos.jsp  

@RequestMapping("say-hello-jsp")  
//@ResponseBody  
public String sayHelloJsp() {  
    return "sayHello";  
}

```

![](https://i.imgur.com/k6e40rp.png)


![](https://i.imgur.com/8idMMvP.png)

>- @ResponseBody  를 사용할 경우 return에 있는 값을 다이렉트로 갖고 온다.
>	- 이 경우 그냥 sayHello 만 출력됨
>- 환경 설정을 맞춰주면 return에 파일이름.jsp 의 파일이름을 넣으면 해당 jsp 파일을 반환해준다.



### Login JSP

```java
package com.in28minutes.springboot.myfirstwebapp.login;  
  
import org.springframework.stereotype.Controller;  
import org.springframework.web.bind.annotation.RequestMapping;  
  
  
@Controller  
public class LoginController {  
  
    ///login => com.in28minutes.springboot.myfirstwebapp.login.LoginController => login.jsp  
  
    @RequestMapping("login")  
    public String gotoLoginPage() {  
        return "login";  
    }  
}
```

>- Spring 이 찾을 수 있도록 @Controller 사용해줘야함
>- 아까 패키지에 login.jsp 파일 내용 작성후 접속하면 끝

![](https://i.imgur.com/1XnoQba.png)




### How does Web Work?

![](https://i.imgur.com/E8p69hM.png)

- A : Browser sends a request
	- HttpRequest
- B : Server handles the request
	- Your Spring Boot Web Application
- C : Server returns the response
	- HttpResponse

### Peek into History - Model 1 Arch.

![](https://i.imgur.com/dmpinBL.png)

- All CODE in Views (JSPs, ....)
	- View logic
	- Flow logic
	- Queries to databases
- Disadvantages:
	- VERY complex JSPs
	- ZERO separation of concerns
	- Difficult to maintain

### Peek into History - Model 2 Arch.

![](https://i.imgur.com/52FMBXa.png)

- How about separating concerns?
	- Model : Data to genereate the view
	- View : Show information to user
	- Controller : Controls the flow
- Advantage : Simpler to maintain
- Concern:
	- Where to implement common features to all controllers?
		- You have a number if services
		- There might be common features that you would want to implement acreoss all these services
		- For example, if you want to implement authentication.
		- The authentication logic is similar across all the servlet
		- How do you implement it in common across all this servlet?
		- That's where we graduated into something called a friend controller pattern. -> ***Front Controller***

#### Model2 Architecure - Front Controller

![](https://i.imgur.com/n8kC0Pa.png)

- Concept : All requests flow into a central controller
	- Called as Front Controller
- Front Controller controls flow to Controller's and View's
	- Common features can be implemented in the Front Controller
	- ![](https://i.imgur.com/HTLBAcM.png)
- A : Receives HTTP Request
- B : Processes HTTP Request
	- B1 : Identifies correct Controller method
		- Based on request URL
			- /login => LoginController.gotoLoginPage
	- B2 : Executes Controller method
		- Puts data into model
		- Returns Model and View Name
	- B3 : Identifies correct View
		- Using ViewResolver
			- /WEB-INF/jsp/login.jsp
	- B4 : Executes view
- C : Returns HTTP Response




## QueryParams using RequestParam and First Look at Model

```java
package com.in28minutes.springboot.myfirstwebapp.login;

import org.springframework.stereotype.Controller;
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;


@Controller
public class LoginController {
	
	///login => com.in28minutes.springboot.myfirstwebapp.login.LoginController => login.jsp
	
	//http://localhost:8080/login?name=Ranga
	//Model
	@RequestMapping("login")
	public String gotoLoginPage(@RequestParam String name, ModelMap model) {
		model.put("model_name", name);
		System.out.println("Request param is " + name); //NOT RECOMMENDED FOR PROD CODE
		return "login";
	}
}
```

```java
<html>  
   <head>  
      <title> Login Page</title>  
   </head>  
   <body>  
      Welcome to the login page ${model_name}!  <-----
   </body>  
</html>
```

![](https://i.imgur.com/SZcAylG.png)

>- 컨트롤러에서 파라미터를 받으면 뷰로 넘겨줌
>- JSP에 있는 내용을 보여주기 위해서 Model이 필요함
>- Model에 넣으면 View가 알아서 픽업해줌
>	- 파라미터의 name 을 모델에서  jsp에 있는 name(model_name) 픽업