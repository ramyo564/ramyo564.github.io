---

layout: single
title: " [Spring] Spring Annotations and Concepts "
categories: Spring
tag: [Java,"[Spring] Spring Annotations and Concepts","[Spring] Annotations and  Concepts"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Spring Annotations
/ Annotations /

| Annotation                                              | Description                                                                                                                                                                                                      |
| ------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| @Configuration                                          | Indicates that a class declares one or more @Bean methods and may be processed by the Spring container to generate bean definitions                                                                              |
| @ComponentScan                                          | Define specific packages to scan for components. if specific packages are not defined, scanning will occur from the package of the class that declares this annotation                                           |
| @Bean                                                   | Indicates that a method produces a bean to be managed by the Spring container                                                                                                                                    |
| @Component                                              | Indicates that an annotated class is a "component"                                                                                                                                                               |
| @Service                                                | Specialization of @Component indicating that an annotated class has business logic                                                                                                                               |
| @Controller                                             | Specialization of @Component indicating that an annotated class is a "Controller" (e.g. a web controller). Used to define controllers in your web applications and REST API                                      |
| @Repository                                             | Specialization of @Component indicating that an annotated class is used to retrieve and/or manipulate data in a database                                                                                         |
| @Primary                                                | Indicates that a bean should be given preference when multiple candidates are qualified to autowire a single-valued dependecy                                                                                    |
| @Qualifier                                              | Used on a field or parameter as a qualifier for candidate beans when autowiring                                                                                                                                  |
| @Lazy                                                   | Indicates that a bean has to be lazily initialized. Absence of @Lazy annotation will lead to eager initialization.                                                                                               |
| @Scope (Value= ConfigurableBeanFactory.SCOPE_PROTOTYPE) | Defines a bean to be prototype - a new instance will be created every time you refer to the bean. Default scope is a singleton - one instance per IOC container.                                                 |
| @PostConstruct                                          | Identifies the method that will be executed after dependency injection is done to perform any initialization                                                                                                     |
| @PreDestory                                             | Identifies the method that will receive the callback notification to signal that the instance is in the process of being removed by the container. Typically used to release resources that it has been holding. |
| @Named                                                  | Jakarta Contexts & Dependency Injection (CDI) Annotation similar to @Component                                                                                                                                    |
| @Inject                                                 | Jakarta Contexts & Dependency Injection (CDI) Annotation similar to @Autowired                                                                                                                                                                                                                 |

## Spring Concepts

| Concept              | Description                                                                                                                                                               |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Dependency Injection | Identify beans, their dependencies and wire them together (provices IOC - Inversion of Control)                                                                           |
| Constr.injection     | Dependencies are set by creating the Bean using its Constructor                                                                                                           |
| Setter injection     | Dependencies are set by calling setter methods on your beans                                                                                                              |
| Field injection      | No setter or constructor. Dependency is injected using reflection                                                                                                         |
| IOC Container        | Spring IOC Context that manages Spring Beans & their lifecycle                                                                                                            |
| Bean Factory         | Basic Spring IOC Container                                                                                                                                                |
| Application Context  | Advanced Spring IOC Container with enterprise-specific features - Easy to use in web applications with internationalization features and good integration with Spring AOP |
| Spring Beans         | Objects managed by Spring                                                                                                                                                 |
| Auto-wiring          | Process of wiring in dependencies for a Spring Bean                                                                                                                                                                          |



