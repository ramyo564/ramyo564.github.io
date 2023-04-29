---

layout: single
title: " Show Update Todo Page "
categories: Spring
tag: [Java,"[BIG] Java Web Application with Spring and Hibernate","Show Update Todo Page","stream().filter().findFist()","findFirst() vs findAny()","filedset","bootstrap-datepicker","스프링 날짜 포맷"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Java Web Application with Spring and Hibernate (7)

/ findFirst() vs findAny() / filedset / bootstrap-datepicker / 

## Update

- Update 를 누르면 id 값으로 description 데이터 갖고 오기

### listTidis.jsp
```java
<td> <a href="update-todo?id=${todo.id}" class="btn btn-success">Update</a>   </td>
```
>- 이전과 마찬가지로 파라미터로 id 값으로 넘겨줌

###  TodoService.java
```java
public Todo findById(int id) {  
    Predicate<? super Todo> predicate = todo -> todo.getId() == id;  
    Todo todo = todos.stream().filter(predicate).findFirst().get();  
    return todo;  
}
```
>- 아이디 값을 todo에 넣어서 리턴 해줌
>- Stream을 직렬로 처리할 때 `findFirst()`와 `findAny()`는 동일한 요소를 리턴하며, 차이점이 없다. 하지만 Stream을 병렬로 처리할 때는 차이가 있다.
>- get() 을 써줘야 마무리
>- 참고 : https://futurecreator.github.io/2018/08/26/java-8-streams/
>	- 필터 매소드 : https://developer-talk.tistory.com/730
>	- findFirst() vs findAny() : https://codechacha.com/ko/java8-stream-difference-findany-findfirst/#3-findfirst%EC%99%80-findany%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90 , https://has3ong.github.io/programming/java-streamintro3/


### TodoController.java
```java
@RequestMapping("update-todo")  
public String showUpdateTodoPage(@RequestParam int id, ModelMap model) {  
    Todo todo = todoService.findById(id);  
    model.addAttribute("todo", todo);  
    return "todo";  
}
```
>- todo에 id 값 만을 반환하기 때문에 클릭한 값만 뷰로 나온다.
>- ![](https://i.imgur.com/Dw6u2dg.png)
>- ![](https://i.imgur.com/in60ym4.png)

## Save changes

### TodoController.java
```java
@RequestMapping(value="update-todo", method = RequestMethod.POST)
	public String updateTodo(ModelMap model, @Valid Todo todo, BindingResult result) {
		
		if(result.hasErrors()) {
			return "todo";
		}
		
		String username = (String)model.get("name");
		todo.setUsername(username);
		todoService.updateTodo(todo);
		return "redirect:list-todos";
	}
```
>- 위에 로직과 같다
>- 유효성 검사를 한 후
>- updateTodo 메소드를 통해 todo를 넘겨준다.


### TodoService.java
```java
public void updateTodo(@Valid Todo todo) {
		deleteById(todo.getId());
		todos.add(todo);
	}
```

>- 이전 데이터는 삭제해주고 
>- 새로운 데이터를 추가해준다.
>- ![](https://i.imgur.com/jcGtN6D.png)
>	- 하지만 날짜가 없다.

### 메이븐에 dependency 추가

```java
1.  <dependency>
2.  <groupId>org.webjars</groupId>
3.  <artifactId>bootstrap-datepicker</artifactId>
4.  <version>1.9.0</version>
5.  </dependency>
```

### todo.jsp
```java
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>  
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>  
  
  .....
<html>  

         <h1>Enter Todo Details</h1>  
         <form:form method="post" modelAttribute="todo">  
  
            <fieldset class="mb-3">  
               <form:label path="description">Description</form:label>  
               <form:input type="text" path="description" required="required"/>  
               <form:errors path="description" cssClass="text-warning"/>  
            </fieldset>  
  
            <fieldset class="mb-3">  
               <form:label path="targetDate">Target Date</form:label>  
               <form:input type="text" path="targetDate" required="required"/>  
               <form:errors path="targetDate" cssClass="text-warning"/>  
            </fieldset>  
  
            <form:input type="hidden" path="id"/>  
            <form:input type="hidden" path="done"/>  
            <input type="submit" class="btn btn-success"/>  
  
         </form:form>  
  
.....
  
      <script type="text/javascript">  
      $('#targetDate').datepicker({  
          format: 'yyyy-mm-dd'  
      });  
      </script>  
   </body>  
</html>
```

>- filedset 태그로 묶어준다.
>- bootstrap-datepicker 를 이용해서 현재 날짜를 갖고 온다.
#### Before
![](https://i.imgur.com/FINMGYC.png)

#### After
![](https://i.imgur.com/y4QZWj0.png)

### 날짜 포맷 맞추기

#### resources/application.properties
```java
spring.mvc.format.date=yyyy-MM-dd
```
- ![](https://i.imgur.com/y33mi1T.png)

### 날짜 하드코딩 변경

#### Before
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


#### After
```java
@RequestMapping(value="add-todo", method = RequestMethod.POST)  
public String addNewTodo(ModelMap model, @Valid Todo todo, BindingResult result) {  
    if(result.hasErrors()){  
        return "todo";  
    }  
    String username = (String)model.get("name");  
    todoService.addTodo(username, todo.getDescription(),  
            todo.getTargetDate(), false);  
    return "redirect:list-todos";  
}
```
>- 하드 코딩으로 되어있던 LocalDate.now().plusYears(1) 부분을 수정해주면 지금 날짜로 잘 갖고 온다.