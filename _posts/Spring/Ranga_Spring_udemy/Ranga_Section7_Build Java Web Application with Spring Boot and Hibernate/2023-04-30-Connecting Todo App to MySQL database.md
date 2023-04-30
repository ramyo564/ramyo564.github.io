---

layout: single
title: " Connecting Todo App to MySQL database "
categories: Spring
tag: [Java,"[BIG] Java Web Application with Spring and Hibernate","Docker"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Java Web Application with Spring and Hibernate (13)

/ Docker /

```text
docker run --detach --env MYSQL_ROOT_PASSWORD=dummypassword --env MYSQL_USER=todos-user --env MYSQL_PASSWORD=dummytodos --env MYSQL_DATABASE=todos --name mysql --publish 3306:3306 mysql:8-oracle
```
>-  위 코드로 가상환경으로 MYSQL 알아서 생성해줌

### pom.xml

```java
<!--       <dependency>-->  
<!--         <groupId>com.h2database</groupId>-->  
<!--         <artifactId>h2</artifactId>-->  
<!--         <scope>runtime</scope>-->  
<!--      </dependency>-->

<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
		</dependency>
```
>- 이전 H2 삭제하고 mysql 추가

```xml
<dependency>
<groupId>com.mysql</groupId>
<artifactId>mysql-connector-j</artifactId>
</dependency>
```
### application.properties
```java
#spring.datasource.url=jdbc:h2:mem:testdb

spring.jpa.hibernate.ddl-auto=update  
spring.datasource.url=jdbc:mysql://localhost:3306/todos  
spring.datasource.username=todos-user  
spring.datasource.password=dummytodos  
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL8Dialect
```