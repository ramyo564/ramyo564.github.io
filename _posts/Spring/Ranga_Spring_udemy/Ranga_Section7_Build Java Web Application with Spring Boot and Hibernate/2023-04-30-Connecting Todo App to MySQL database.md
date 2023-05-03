---

layout: single
title: " [Spring] Connecting Todo App to MySQL database "
categories: Spring
tag: [Java,"[BIG][Spring] Java Web Application with Spring and Hibernate","Docker"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Java Web Application with Spring and Hibernate (13)

/ Docker /

- H2 메모리 데이터에서 탈출을 위해 도커를 사용한 MySQL를 사용해보기
- 도커를 설치한 후 아래의 명령어 실행

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
			<version>8.0.28</version> <-- 추가하니 작동됨
		</dependency>
```

>- 이전 H2 삭제하고 mysql 추가
>- 이렇게 해도 계속 오류가 나서 이것 저것 찾아보고 메이븐도 클린 빌드 하고 진짜 별에별 짓은 다했는데 ***버전을 써놓으니까 해결*** (심지어 도커랑 정확히 맞는 버전도 아님..)
>	- 윈도우 문제인지 내 노트북 문제인지는 모르겠음...

### application.properties
```java
#spring.datasource.url=jdbc:h2:mem:testdb

spring.jpa.hibernate.ddl-auto=update  
spring.datasource.url=jdbc:mysql://localhost:3306/todos  
spring.datasource.username=todos-user  
spring.datasource.password=dummytodos  
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL8Dialect
```

>- url 에 대해서 http가 아닌 jdbc를 사용 `jdbc:mysql://localhost:3306/todo`
>![](https://i.imgur.com/MboP7fv.png)

## 정리
- 이전에 연습용으로 실행했던 h2도 결국 메모리에 저장하는 방식이라 서버를 껐다 키면 모든 내용이 날라간다
- 처음에 MySQL을 로컬로 이용했는데 이번에는 도커를 사용해서 연결해서 연결했다.
- 단순히 연결해서 쉽게 사용하는데 있어서 MySQL을 로컬로 설치해서 사용하는 것보다 도커를 이용하는게 훨씬 편하고 빠르고 쉬운거 같다.