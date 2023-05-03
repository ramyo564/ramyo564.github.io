---

layout: single
title: " [Spring] Creating a Login Form "
categories: Spring
tag: [Java,"[BIG][Spring] Java Web Application with Spring and Hibernate","@Service","html pre 태그"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Java Web Application with Spring and Hibernate (2)

/ @Service / @RequestMapping / @RequestParam /`<pre>`

- 자바에서 폼 만들고 아이디랑 비번 받아서 페이지 넘기는 연습

## Creating a Login Form

```java
AuthenticationService

@Service  
public class AuthenticationService {  
    public boolean authenticate(String username, String password) {  
        boolean isValidUserName = username.equalsIgnoreCase("in28minutes");  
        boolean isValidPassword = password.equalsIgnoreCase("dummy");  
        return isValidUserName && isValidPassword;  
    }  
}
```

>- 유저네임과 패스워드를 true false로 반환 값 만듦

```java

LoginController

@Controller  
public class LoginController {  
    private AuthenticationService authenticationService;  
    public LoginController(AuthenticationService authenticationService) {  
        super();  
        this.authenticationService = authenticationService;  
    }  
    @RequestMapping(value="login",method = RequestMethod.GET)  
    public String gotoLoginPage() {  
        return "login";  
    }  
    @RequestMapping(value="login",method = RequestMethod.POST)  
    //login?name=Ranga RequestParam  
    public String gotoWelcomePage(@RequestParam String name,  
                                  @RequestParam String password, ModelMap model) {  
        if(authenticationService.authenticate(name, password)) {  
            model.put("name", name);  
            model.put("password", password);  
            //Authentication  
            //name - in28minutes            
            //password - dummy  
            return "welcome";  
        }  
        model.put("errorMessage", "Invalid Credentials! ");  
        return "login";  
    }  
}
```

>- @RequestParam -> 파라미터 가져옴
>- @RequestMapping -> 요청이 들어왔을 때 컨트롤러에서 매핑해주고 그 컨트롤러를 실행시켜 응답을 받음
>	- 참고 : https://tecoble.techcourse.co.kr/post/2021-06-18-spring-request-mapping/
>- ModelMap
>	- ModelMap 은 데이터만 저장한다고 한다. ModelAndView 는 ViewPage 도 함께 저장한다고 하는데 지금은 그냥 그런게 있다 정도로 넘어감
>	- ModelMap 에서 좀 알아볼겸 찾아보는데 put이 아닌  addAttribute 가 나와서 좀 더 찾아봄
>		- addAttributes implies check for not null in attribute name

```java
     /*  Add the supplied attribute under the supplied name.
	     @param attributeName the name of the model attribute (never <code>null</code>)
	     @param attributeValue the model attribute value (can be <code>null</code>)
	*/
    public ModelMap addAttribute(String attributeName, Object attributeValue) {
        Assert.notNull(attributeName, "Model attribute name must not be null");
        put(attributeName, attributeValue);
        return this;
    }
```


>- 참고 : https://stackoverflow.com/questions/15742262/modelmap-put-v-s-modelmap-addattribute
>- 참고 : https://javaoop.tistory.com/56


```java
welcome.jsp

<html>  
   <head>  
      <title>Welcome Page</title>  
   </head>  
   <body>  
      <div>Welcome to in28minutes</div>  
      <div>Your Name: ${name}</div>  
      <div>Your Password: ${password}</div>  
   </body>  
</html>
```

```java
login.jsp

<html>  
   <head>  
      <title>Login Page</title>  
   </head>  
   <body>  
      Welcome to the login page!  
  
      <pre>${errorMessage}</pre>  
      <form method="post">  
         Name: <input type="text" name="name">  
         Password: <input type="password" name="password">  
         <input type="submit">  
      </form>  
  
   </body>  
</html>
```