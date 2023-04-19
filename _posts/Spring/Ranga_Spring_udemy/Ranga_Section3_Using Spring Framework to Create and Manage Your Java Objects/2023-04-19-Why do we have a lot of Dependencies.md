---

layout: single
title: "Why do we have a lot of Dependencies?"
categories: Spring
tag: [Java,"Starting Spring Framework","Why do we have a lot of Dependencies?","Dependencies"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Starting Spring Framework ( 8 )
/ Dependencies /

- In Game Runner Hello World App, we have very few classes
- BUT Real World applications are much more complex:
	- Multiple Layers (Web, Business, Data etc)
	- Each layer is dependent on the layer below it!
		- Data Layer class is a dependency of Business Layer class
	- There are thousands of such dependencies in every application!
- With Spring Framework:
	- INSTEAD of FOCUSING on objects, their dependencies and wiring
		- You can focus on the business logic of your application!
	- Spring Framework manages the lifecycle of objects:
		- Mark components using annotations : @Component (and othes...)
		- Mark dependencies using @Autowired
		- Allow Spring Framework to do it's magic

## Exercise - BusinessCalculationService

![](https://i.imgur.com/Yz3ufnS.png)

### RealWorldSpringLauncherApplication

```java
import org.springframework.context.annotation.AnnotationConfigApplicationContext;  
import org.springframework.context.annotation.ComponentScan;  
import org.springframework.context.annotation.Configuration;  
  
import java.util.Arrays;  
  
  
@Configuration  
@ComponentScan  
public class RealWorldSpringLauncherApplication {  
  
    public static void main(String[] args) {  
  
        var context =  
                new AnnotationConfigApplicationContext(RealWorldSpringLauncherApplication.class);  
        Arrays.stream(context.getBeanDefinitionNames())  
                .forEach(System.out::println);  
        System.out.println(  
                context.getBean(BusinessCalculationService.class).findMax()  
        );  
  
    }  
  
}
```

>- AnnotationConfigApplicationContext는 자바 설정에서 정보를 읽어와 빈 객체를 생성, 관리한다.  
>- getBeanDefinitionNames는 등록된 빈을 모두 출력한다.
>- @Primary 로 MongoDbDataService가 설정 되어 있으니 거기에서 BusinessCalculationService에서 인터페이스를 통해 설정한 데이터를 갖고 오고 만들어 둔 findMax 함수를 통해 max 값을 갖고 온다.



### DataService interface
```java
package com.in28minutes.learinspringframework.examples.c1;  
  
public interface DataService {  
    int[] retrieveData();  
}
```


### BusinessCalculationService
```java
  
import org.springframework.stereotype.Component;  
  
import java.util.Arrays;  
@Component  
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

### MongoDbDataService
```java

import org.springframework.context.annotation.Primary;  
import org.springframework.stereotype.Component;  
  
@Component  
@Primary  
public class MongoDbDataService implements DataService{  
    @Override  
    public int[] retrieveData() {  
        return new int[] {11, 22, 33, 44, 55};  
    }  
}
```


### MySQLDataService

```java

import org.springframework.stereotype.Component;  
  
@Component  
public class MySQLDataService implements DataService{  
    @Override  
    public int[] retrieveData() {  
  
        return new int[] {1, 2, 3, 4, 5};  
    }  
}
```

### 출력

![](https://i.imgur.com/iNcwKoM.png)


