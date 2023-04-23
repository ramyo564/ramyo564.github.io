---

layout: single
title: " Spring MVC Controller, @ResponseBody, @Controller "
categories: Spring
tag: [Java,"Spring MVC Controller","@ResponseBody","@Controller","JSP","@RequestParam","@RequestMapping"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Spring MVC Controller, @ResponseBody, @Controller
/ @ResponseBody / @Controller / @RequestMapping / @RequestParam

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
- A : Browser sends a request
	- HttpRequest
- B : Server handles the request
	- Your Spring Boot Web Application
- C : Server returns the response
	- HttpResponse



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
		model.put("name", name);
		System.out.println("Request param is " + name); //NOT RECOMMENDED FOR PROD CODE
		return "login";
	}
}
```