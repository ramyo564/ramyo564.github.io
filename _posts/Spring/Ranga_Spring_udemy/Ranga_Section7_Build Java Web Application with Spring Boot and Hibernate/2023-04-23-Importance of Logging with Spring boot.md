---

layout: single
title: " [Spring] Importance of Logging with Spring boot "
categories: Spring
tag: [Java,"Importance of Logging with Spring boot","Logging"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Importance of Logging with Spring boot
/ Logging /

```java
logging.level.some.path=debug
logging.level.some.other.path=error
logging.file.name=logfile.log

private Logger logger = LofferFactory.getLogger(this.getClass());
logger.info("postConstruct");

```

- Knowing what to log is an essential skill to be a great programmer
- Spring Boot makes logging easy
	- spring-boot-starter-logging
- Default : Logback with SLF4j


```java
resources/application.properties

#server.port=8081  
spring.mvc.view.prefix=/WEB-INF/jsp/  
spring.mvc.view.suffix=.jsp  
logging.level.org.springframework=info  
logging.level.com.in28minutes.springboot.myfirstwebapp=debug
```

```java
package com.in28minutes.springboot.myfirstwebapp.login;  
  
import org.slf4j.Logger;  
import org.slf4j.LoggerFactory;  
import org.springframework.stereotype.Controller;  
import org.springframework.ui.ModelMap;  
import org.springframework.web.bind.annotation.RequestMapping;  
import org.springframework.web.bind.annotation.RequestParam;  
  
  
@Controller  
public class LoginController {  
  
    private Logger logger = LoggerFactory.getLogger(getClass());  
    @RequestMapping("login")  
    public String gotoLoginPage(@RequestParam String name, ModelMap model) {  
        model.put("model_name", name);  
        logger.debug("Request param is {}", name);  
        logger.info("I want this printed at info level");  
        logger.warn("I want this printed at warn level");  
  
        System.out.println("Request param is " + name); //NOT RECOMMENDED FOR PROD CODE  
        return "login";  
    }  
}
```


