---

layout: single
title: " Spring Profiles "
categories: Spring
tag: [Java,"Spring Profiles","@ConfigurationProperties"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Spring Profiles
/ Profiles / @ConfigurationProperties

- Applications have different environments: Dev, QA, Stage, Prod, ...
- Different environments need different configuration:
	- Different Databases
	- Different Web Services
- How can you provide different configuration for different environments?
	- Profiles : Environment specific configuration
- How can you define externalized configuration for your application?

- resources/application.properties
![](https://i.imgur.com/yZiwBSS.png)

```java
//currency-service.url=  
//currency-service.username=  
//currency-service.key=  
  
import org.springframework.boot.context.properties.ConfigurationProperties;  
import org.springframework.stereotype.Component;  
  
@ConfigurationProperties(prefix = "currency-service")  
@Component  
public class CurrencyServiceConfiguration {  
  
    private String url;  
    private String username;  
    private String key;  
    public String getUrl() {  
        return url;  
    }  
    public void setUrl(String url) {  
        this.url = url;  
    }  
    public String getUsername() {  
        return username;  
    }  
    public void setUsername(String username) {  
        this.username = username;  
    }  
    public String getKey() {  
        return key;  
    }  
    public void setKey(String key) {  
        this.key = key;  
    }  
}
```

```java

import org.springframework.beans.factory.annotation.Autowired;  
import org.springframework.web.bind.annotation.RequestMapping;  
import org.springframework.web.bind.annotation.RestController;  
@RestController  
public class CurrencyConfigurationController {  
    @Autowired  
    private CurrencyServiceConfiguration configuration;  
    @RequestMapping("/currency-configuration")  
    public CurrencyServiceConfiguration retrieveAllCourses() {  
        return configuration;  
    }  
}
```

## 정리
- *.properties , *.yml 파일에 있는 property를 자바 클래스에 값을 가져와서(바인딩) 사용할 수 있게 해주는 어노테이션
- 개념만 이해했고 이걸 어떻게 활용하는지는 정확하게 이해가 아직 안간다.
- 확실하게 더 이해가면 여기에 추가

참고 : https://programmer93.tistory.com/47 , https://velog.io/@max9106/Spring-Boot-%EC%99%B8%EB%B6%80%EC%84%A4%EC%A0%95-4xk69h8o50


