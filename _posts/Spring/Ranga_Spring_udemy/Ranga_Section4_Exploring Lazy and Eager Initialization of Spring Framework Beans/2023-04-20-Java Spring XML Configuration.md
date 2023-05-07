---

layout: single
title: " [Spring] Java Spring XML Configuration "
categories: Spring
tag: [Java,"[Spring] Java Spring XML Configuration","[Spring] Annotations vs XML configuration"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Java Spring XML Configuration
/ XML Configuration /

![](https://i.imgur.com/Z1ToSiJ.png)

```java
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
       xmlns:context="http://www.springframework.org/schema/context" xsi:schemaLocation="  
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd"> <!-- bean definitions here -->  
  
    <bean id="name" class="java.lang.String">  
        <constructor-arg value="Ranga" />  
    </bean>  
    <bean id="age" class="java.lang.Integer">  
        <constructor-arg value="35" />  
    </bean>  
    <!--   <context:component-scan  
            base-package="com.in28minutes.learnspringframework.game"/>     -->    <bean id="game" class="com.in28minutes.learnspringframework.game.PacmanGame"/>  
  
    <bean id="gameRunner"  
          class="com.in28minutes.learnspringframework.game.GameRunner">  
        <constructor-arg ref="game" />  
    </bean>  
</beans>
```

```java

import java.util.Arrays;  
  
import org.springframework.context.support.ClassPathXmlApplicationContext;  
  
import com.in28minutes.learnspringframework.game.GameRunner;  
  
public class XmlConfigurationContextLauncherApplication {  
  
    public static void main(String[] args) {  
  
        try (var context =  
                     new ClassPathXmlApplicationContext("contextConfiguration.xml")) {  
  
            Arrays.stream(context.getBeanDefinitionNames())  
                    .forEach(System.out::println);  
  
            System.out.println(context.getBean("name"));  
  
            System.out.println(context.getBean("age"));  
  
            context.getBean(GameRunner.class).run();  
  
        }  
    }  
}
```

![](https://i.imgur.com/yRGmj5U.png)

## Annotations vs XML configuration

| Heading                   | Annotations                                                        | XML Configuration           |
| ------------------------- | ------------------------------------------------------------------ | --------------------------- |
| Ease of use               | Very Easy (defined close to source - class,method and/or variable) | Cumbersome                  |
| Short and concise         | Yes                                                                | No                          |
| Clean POJOs               | No. POJOs are polluted with Spring Annotations                     | Yes. No change in Java code |
| Easy to Maintain          | Yes                                                                | No                          |
| Usage Frequency           | Alomost all recent projects                                        | Rarely                      |
| Recommendation            | Either of them is fine BUT be consistent                           | Do NOT mix both             |
| Debugging <br> difficulty | Hard                                                               | Medium                            |


## 정리

>- XML 로 bean을 주입하고 북치고 장구치고 가능하다.
>- 사실 이전에는 XML로 작업했다고 한다.
>- 하지만 옛날 방법이며 요즘에는 잘 사용하지 않는다고 한다.
>- 나름대로 XML 의 장점도 위와 같이 어느정도 있다.
>- 대부분 Annotation을 사용하지만 디버깅이 어렵다는 단점도 있다.
>- 둘 다 좋은 방법이다. 섞어 쓰지만 않으면 된다.


