---

layout: single
title: " [Spring] Spring Stereotype Annotations - Component and more "
categories: Spring
tag: [Java,"[Spring] Spring Stereotype Annotations - Component and more","[Spring] @Service","[Spring] @Controller","[Spring] @Repository"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Spring Stereotype Annotations - Component and more
/ @Component / @Repository / @Service / @Controller / 

- @Component - Generic annotation applicable for any class
	- Base for all Spring Stereotype Annotations
	- Specializations of @Component:
		- @Service - Indicates that an annotated class has business logic
		- @Controller - Indicates that an annotated class is a "Controller" (e.g. a web controller)
			- Used to define controllers in your web applications and REST API
		- @Repository - Indicates that an annotated class is used to retrieve and/or manipulate data in a database
- What should you use?
	- (RECOMMENDATION) Use the most specific annotation possible
	- Why?
		- By using a specific annotation, you are giving more information to the framework about your intentions
		- You can use AOP at later point to add additional behavior
			- Example : For @Repository, Spring automatically wires in JDBC Exception translation features

## 예제

```java

import org.springframework.stereotype.Component;  
import org.springframework.stereotype.Repository;  
  
//@Component  
@Repository  <-
public class MySQLDataService implements DataService{  
    @Override  
    public int[] retrieveData() {  
  
        return new int[] {1, 2, 3, 4, 5};  
    }  
}
```


```java

import org.springframework.stereotype.Component;  
import org.springframework.stereotype.Service;  
  
import java.util.Arrays;  
//@Component  
@Service  <-
public class BusinessCalculationService {  
    private DataService dataService;  
    public BusinessCalculationService(DataService dataService){  
        super();  
        this.dataService = dataService;  
    }  
    public int findMax() {  
        return Arrays.stream(dataService.retrieveData())  
                .max().orElse(0);  
    }  
}
```