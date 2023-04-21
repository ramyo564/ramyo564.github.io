---

layout: single
title: " Spring MVC Controller, @ResponseBody, @Controller "
categories: Spring
tag: [Java,"Spring MVC Controller, @ResponseBody, @Controller"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Spring MVC Controller, @ResponseBody, @Controller
/ @ResponseBody / @Controller / @RequestMapping /

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