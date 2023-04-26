---

layout: single
title: " Validations with Spring Boot Starter "
categories: Spring
tag: [Java,"Java Web Application with Spring and Hibernate","Validations with Spring Boot Starter","BindingResult","@Valid","@Size"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Java Web Application with Spring and Hibernate (5)

/ @Size / @Valid / BindingResult / Validations

## 유효성 검사

![](https://i.imgur.com/0PmZ50s.png)

>- 현재 그냥 아무것도 없이 폼을 제출하면 빈 공간으로 제출이 되버린다.
>- 유효성 검사를 스프링에서는 어떻게 할까?

```java
<form method="post">  
   Description: <input type="text" name="description"/  
   required="required">  
   <input type="submit" class="btn btn-success"/>  
         </form>
```

>- 단순히 input 태그 안에 required="required" 를 추가하면 된다.
>- ![](https://i.imgur.com/wJE1rts.png)
>- 하지만 HTML 이나 자바스크립트에서 유효성 검사는 해커로부터 아주 쉽게 덮어쓸 수 있다고 한다.


## Validations with Spring Boot
- 4 Steps
	- Spring Boot Starter Validation
		- pom.xml
	- Command Bean (Form Backing Object)
		- 2-way binding(todo.jsp & TodoController.java)
	- Add Validations to Bean
		- Todo.java
	- Display Validation Errors in the View
		- todo.jsp

### 1. Spring Boot Starter Validation
```java
<dependency>  
   <groupId>org.springframework.boot</groupId>  
   <artifactId>spring-boot-starter-validation</artifactId>  
</dependency>
```

### 2. Command Bean (Form Backing Object)

#### Before TodoController.java
```java
@RequestMapping(value="add-todo", method = RequestMethod.POST)  
public String addNewTodo(@RequestParam String description, ModelMap model) {  
    String username = (String)model.get("name");  
    todoService.addTodo(username, description,  
            LocalDate.now().plusYears(1), false);  
    return "redirect:list-todos";  
}
```

#### Afrer TodoController.java
```java
@RequestMapping(value="add-todo", method = RequestMethod.POST)  
public String addNewTodo(ModelMap model, Todo todo) {  
    String username = (String)model.get("name");  
    todoService.addTodo(username, todo.getDescription(),  
            LocalDate.now().plusYears(1), false);  
    return "redirect:list-todos";  
}
```
>- 해당 방법을 통해 직접적으로 Todo Bean과 연결시켜 준다.


#### Before todo.jsp
```java
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>  
  
<html>  
   <head>  
      <link href="webjars/bootstrap/5.1.3/css/bootstrap.min.css" rel="stylesheet" >  
      <title>Add Todo Page</title>  
   </head>  
   <body>  
      <div class="container">  
         <h1>Enter Todo Details</h1>  
         <form method="post">  
            Description: <input type="text" name="description"/  
            required="required">  
            <input type="submit" class="btn btn-success"/>  
            </form>  
      </div>  
      <script src="webjars/bootstrap/5.1.3/js/bootstrap.min.js"></script>  
      <script src="webjars/jquery/3.6.0/jquery.min.js"></script>  
   </body>  
</html>
```

#### After todo.jsp
```java
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>


<html>
	<head>
		<link href="webjars/bootstrap/5.1.3/css/bootstrap.min.css" rel="stylesheet" >
		<title>Add Todo Page</title>		
	</head>
	<body>
		<div class="container">
			<h1>Enter Todo Details</h1>
			<form:form method="post" modelAttribute="todo">
				Description: <form:input type="text" path="description" 
								required="required"/>
				<form:input type="hidden" path="id"/>
				<form:input type="hidden" path="done"/>
				<input type="submit" class="btn btn-success"/>
			
			</form:form>
			
		</div>
		<script src="webjars/bootstrap/5.1.3/js/bootstrap.min.js"></script>
		<script src="webjars/jquery/3.6.0/jquery.min.js"></script>		
	</body>
</html>
```

>- 스프링 폼 태그 라이브러리를 사용하기 위해 상단에 선언해준다.
>- 형식에 맞게 폼을 변형해준다.
>- modelAttribute="todo" 등 매칭해줘야 될 것들을 맞춰준다.
>- 원하는 정보는 Description 이지만 정보를 넘겨줄 때 id와 done 정보가 없으면 에러가난다.
>- 이 부분을 처리하기 위해서 hidden으로 숨겨준다.

### Problem is
이렇게 하고 나서도 문제가 생긴다.

Error message is
>- Neither binding result nor plain target object for being to do available as request attribute

The problem is `showNewTodoPage()`
- That's the one which is called when `add-todo` is called
- This one is not setting an attribute called to do into the model

#### Before TodoController.java
```java
//GET, POST  
@RequestMapping(value="add-todo", method = RequestMethod.GET)  
public String showNewTodoPage() {  
    return "todo";  
}
```

#### After TodoController.java
```java
//GET, POST  
@RequestMapping(value="add-todo", method = RequestMethod.GET)  
public String showNewTodoPage(ModelMap model) {  
    String username = (String)model.get("name");  
    Todo todo = new Todo(0, username, "Default Desc", LocalDate.now().plusYears(1), false);  
    model.put("todo", todo);  
    return "todo";  
}
```

>- One side binding ->  `"Default Desc"` 
>- ![](https://i.imgur.com/UyQijTj.png)
>- Basically from your controller to the values which are shown in the form that one side
>- The second side is whatever values you are entering in here
>- ![](https://i.imgur.com/ZFdundd.png)
>- ![](https://i.imgur.com/CvMIQZ1.png)


## 3. Add Validations to Bean
- Todo.java
### TodoController.java
```java
@RequestMapping(value="add-todo", method = RequestMethod.POST)  
public String addNewTodo(ModelMap model, @Valid Todo todo) {  
  
    String username = (String)model.get("name");  
    todoService.addTodo(username, todo.getDescription(),  
            LocalDate.now().plusYears(1), false);  
    return "redirect:list-todos";  
}
```

### Todo.java
```java
@Size(min=10, message="Enter at least 10 characters")  
private String description;
```
>- @Size 애너테이션을 이용해 유효성 검사를 한다.
>- 컨트롤러에서 @Valid를 사용해서 Todo의 bean 유효성 검사
>- ![](https://i.imgur.com/Wu8qeWK.png)

## 4. Display Validation Errors in the View
- VIew 에서 에러페이지가 나오도록 하기 위해 JSP 를 수정

### todo.jsp
```java
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>  
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>  
  
  
<html>  
   <head>  
      <link href="webjars/bootstrap/5.1.3/css/bootstrap.min.css" rel="stylesheet" >  
      <title>Add Todo Page</title>  
   </head>  
   <body>  
      <div class="container">  
         <h1>Enter Todo Details</h1>  
         <form:form method="post" modelAttribute="todo">  
            Description: <form:input type="text" path="description"  
                        required="required"/>  
                        <form:errors path="description" cssClass="text-warning"/>  
            <form:input type="hidden" path="id"/>  
            <form:input type="hidden" path="done"/>  
            <input type="submit" class="btn btn-success"/>  
  
         </form:form>  
  
      </div>  
      <script src="webjars/bootstrap/5.1.3/js/bootstrap.min.js"></script>  
      <script src="webjars/jquery/3.6.0/jquery.min.js"></script>  
   </body>  
</html>
```
>- `<form:errors path="description" cssClass="text-warning"/> ` 추가 
>- JSP를 수정하면 부트스트랩에서 제공하는 cssClass를 사용해 css를 컨트롤 할 수 있다.

### TodoController.java
```java
@RequestMapping(value="add-todo", method = RequestMethod.POST)  
public String addNewTodo(ModelMap model, @Valid Todo todo, BindingResult result) {  
    if(result.hasErrors()){  
        return "todo";  
    }  
    String username = (String)model.get("name");  
    todoService.addTodo(username, todo.getDescription(),  
            LocalDate.now().plusYears(1), false);  
    return "redirect:list-todos";  
}
```
>- 스프링에서 제공하는 BindingResult 를 통해 오류 내용을 JSP로 보내줌
>- 조건문을 활용해서 에러가 있으면 todo 페이지에 남아 있으면서 에러메세지를 뷰를 통해 브라우저에서 보여준다.