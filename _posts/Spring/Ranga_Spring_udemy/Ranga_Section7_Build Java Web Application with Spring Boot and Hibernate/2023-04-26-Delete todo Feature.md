---

layout: single
title: " Delete todo Feature "
categories: Spring
tag: [Java,"[BIG] Java Web Application with Spring and Hibernate","Delete todo Feature","@RequestParam"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Java Web Application with Spring and Hibernate (6)

/ Predicate /

## Add delete in View

### listTodos.jsp
```java
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>

<html>
	<head>
		<link href="webjars/bootstrap/5.1.3/css/bootstrap.min.css" rel="stylesheet" >
		<title>List Todos Page</title>		
	</head>
	<body>
		<div class="container">
			<h1>Your Todos</h1>
			<table class="table">
				<thead>
					<tr>
						<th>id</th>
						<th>Description</th>
						<th>Target Date</th>
						<th>Is Done?</th>
						<th></th>
						<th></th>
					</tr>
				</thead>
				<tbody>		
					<c:forEach items="${todos}" var="todo">
						<tr>
							<td>${todo.id}</td>
							<td>${todo.description}</td>
							<td>${todo.targetDate}</td>
							<td>${todo.done}</td>
							<td> <a href="delete-todo?id=${todo.id}" class="btn btn-warning">Delete</a>   </td>
							<td> <a href="update-todo?id=${todo.id}" class="btn btn-success">Update</a>   </td>
						</tr>
					</c:forEach>
				</tbody>
			</table>
			<a href="add-todo" class="btn btn-success">Add Todo</a>
		</div>
		<script src="webjars/bootstrap/5.1.3/js/bootstrap.min.js"></script>
		<script src="webjars/jquery/3.6.0/jquery.min.js"></script>
		
	</body>
</html>
```
>- a href="delete-todo?id=${todo.id}" <= ?id = {} 를 사용하면 컨트롤러의 delete-todo로 이동하고 파라미터 id 값을 갖고 올 수 있다.

### TodoController.java
```java
@RequestMapping("delete-todo")  
public String deleteTodo(@RequestParam int id) {  
    //Delete todo  
    todoService.deleteById(id);  
    return "redirect:list-todos";  
}
```

### TodoService.java
```java
public void deleteById(int id) {  
    //todo.getId() == id  
    // todo -> todo.getId() == id  
    Predicate<? super Todo> predicate = todo -> todo.getId() == id;  
    todos.removeIf(predicate);  
}
```
>- Predicate<T>
>	- 타입 T 인자를 받는다.
>	- ArrayList.removeIf 사용 -> 해당 예제에 람다식을 사용한다고 한다.
>	- 참고 : https://codechacha.com/ko/java-collections-arraylist-removeif/

![](https://i.imgur.com/iiMcQ8Q.png)

![](https://i.imgur.com/ZUdw7M0.png)
