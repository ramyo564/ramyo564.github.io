---

layout: single
title: " Adding a Navigation Bar and Implementing JSP Fragments "
categories: Spring
tag: [Java,"[BIG] Java Web Application with Spring and Hibernate","Adding a Navigation Bar and Implementing JSP Fragments","jspf","Navigation, Header, Footer 분할"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Java Web Application with Spring and Hibernate (8)

/ jspf /

## 부트스트랩 네비게이션

```java
<nav class="navbar navbar-expand-md navbar-light bg-light mb-3 p-1">  
    <a class="navbar-brand m-1" href="https://courses.in28minutes.com">in28Minutes</a>  
    <div class="collapse navbar-collapse">  
        <ul class="navbar-nav">  
            <li class="nav-item"><a class="nav-link" href="/">Home</a></li>  
            <li class="nav-item"><a class="nav-link" href="/list-todos">Todos</a></li>  
        </ul>  
    </div>  
    <ul class="navbar-nav">  
        <li class="nav-item"><a class="nav-link" href="/logout">Logout</a></li>  
    </ul>  
</nav>
```

## Navigation, Header, Footer 분할

![](https://i.imgur.com/iLkj0Rp.png)

### listTodos.jsp
```java
<%@ include file="common/navigation.jspf" %>
```
>- 이런 템플릿 사용법은 다 비슷비슷 하다.
>- 자주 쓰는 탬플릿은 따로 보관해서 랜더링해서 끼워 맞추면 된다.