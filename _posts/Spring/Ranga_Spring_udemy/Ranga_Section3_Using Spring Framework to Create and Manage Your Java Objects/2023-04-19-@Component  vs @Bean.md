---
layout: single
title: "[Spring] @Component  vs @Bean"
categories: Spring
tag: [Java,"[BIG][Spring] Starting Spring Framework","[Spring] @Component vs @Bean"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Starting Spring Framework (7)
/ @Component  vs @Bean /


| Heading            | @Component                                                          | @Bean                                                                                 |
| ------------------ | ------------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| Where?             | Can be usedon any Java class                                       | Typically used on methods in Spring Configuration classes <br> `@Configuration`                             |
| Ease of use        | Very easy. Just add an annotation                                   | You write all the code                                                                |
| Autowiring         | Yes - Field, Setter or Constructor Injection                        | Yes - method call or method parameters                                                |
| Who creates beans? | Spring Framework                                                    | You write bean creation code                                                          |
| Recommended For    | Instantiating Beans for Your Own Application.<br>    `Code : @Component` | 1: Custom Business Logic.<br> 2: Instantiating Beans for 3rd -party   libraries: `@Bean` |



