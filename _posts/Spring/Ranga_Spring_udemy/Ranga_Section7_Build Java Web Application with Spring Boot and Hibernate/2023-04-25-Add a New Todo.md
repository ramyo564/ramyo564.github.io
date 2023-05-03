---

layout: single
title: " [Spring] Add a New Todo "
categories: Spring
tag: [Java,"[BIG][Spring] Java Web Application with Spring and Hibernate","Add a New Todo","method = RequestMethod.GET","method = RequestMethod.POST","ModelMap"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Java Web Application with Spring and Hibernate (4)

/ method = RequestMethod.POST / method = RequestMethod.GET / 

![](https://i.imgur.com/IkNq7El.png)

## add 버튼 만들기

### listTodos.jsp
```java
<a href="add-todo" class="btn btn-success">Add Todo</a>
```

### Add 버튼 추가 화면
![](https://i.imgur.com/dtuAd2f.png)

### todo.jsp
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
            Description: <input type="text" name="description"/>  
            <input type="submit" class="btn btn-success"/>  
            </form>  
      </div>  
      <script src="webjars/bootstrap/5.1.3/js/bootstrap.min.js"></script>  
      <script src="webjars/jquery/3.6.0/jquery.min.js"></script>  
   </body>  
</html>
```

![](https://i.imgur.com/NkjX1kv.png)

## 컨트롤러 연결

```java
@RequestMapping(value="add-todo", method = RequestMethod.GET)  
public String showNewTodoPage() {  
    return "todo";  
}  
  
@RequestMapping(value="add-todo", method = RequestMethod.POST)  
public String addNewTodo() {  
    return "redirect:listTodos";  
}
```

>-  처음에 listTodos.jsp에 추가했던 Add todo 버튼을 누르면  
>-  http://localhost:8080/add-todo 이동
>- 처음 페이지를 갖고 오는거니까 RequestMethod.Get 으로해서 todo.jsp가 보이게 설정
>- 여기서 제출 버튼을 누르면 리턴 값으로 설정한 listTodos.jsp 로 연결
>- 하지만 아무 정보가 남아 있지 않다.


![](https://i.imgur.com/o5e9Vqg.png)

## Redirect:

![](https://i.imgur.com/l3rqcBO.png)


```java
@RequestMapping(value="add-todo", method = RequestMethod.POST)  
public String addNewTodo() {  
    return "redirect:list-todos";  
}
```

>- 리턴 값에 redirect: {메핑이름} 
>	- {} 여기에 listTodos.jsp 의 이름이 아니라 이전에 데이터를 갖고 있던 주소를 써준다.
>	- 그러면 이전의 데이터를 유지한 화면을 보여준다.
>	- flask든 django든 기본원리는 다 똑같다

- GET의 파라미터 형식으로 정보를 전달한다는 특징을 이용하면 된다.
GET/servlet/helloServlet?userId=hello
- 하지만 중요한 정보가 URL에 그대로 노출된다는 문제점이 있고, 스프링은 이를 해결하기 위해 RedirectAttirbutes 클래스를 제공한다.
- RedirectAttribute는 리다이렉트가 발생하기 전에 모든 없어질만한 속성을 세션에 복사한다. 리다이렉션 이후에는 복사된 정보들을 세션에서 모델로 이동시킨다. 
- 헤더에 파라미터를 붙이지 않기 때문에 URL에 노출되지 않아 안정성이 더 보장된다.

[추가 설명]

- 새로운 요청이 발생할 때 HttpServletRequest 객체는 소멸 후 새롭게 생성되며, HttpSession 객체는 그대로 유지된다. 
- 즉, HttpServletRequest는 웹 브라우저에서 정보를 요청할 때 생성되고, 객체 안에 정보들이 저장된다. 
- 이 객체가 새로 생성되기 때문에 이전의 정보가 사라지는 것이고, HttpSession의 경우 유지가 되기 때문에 RedirectAttribute는 이를 이용하여 정보들을 임시 저장하고 모델에 다시 이동시킬 수 있는 것이다.

참고 : https://brightmango.tistory.com/339

## 유저로부터 데이터 받기

### TodoController.java
```java
@RequestMapping(value="add-todo", method = RequestMethod.POST)  
public String addNewTodo(@RequestParam String description, ModelMap model) {  
    String username = (String)model.get("name");  
    todoService.addTodo(username, description,  
            LocalDate.now().plusYears(1), false);  
    return "redirect:list-todos";  
}
```

>- @RequestParam 을 통해서 description 내용을 받는다.
>- ModelMap 을 통해 세션에 저장한 name을 String으로 형 변환 후username에 넣어준다.

### TodoService.java
```java
@Service  
public class TodoService {  
    private static List<Todo> todos = new ArrayList<>();  
    private static int todosCount = 0;  
    static {  
        todos.add(new Todo(++todosCount, "in28minutes","Learn AWS",  
                LocalDate.now().plusYears(1), false ));  
        todos.add(new Todo(++todosCount, "in28minutes","Learn DevOps",  
                LocalDate.now().plusYears(2), false ));  
        todos.add(new Todo(++todosCount, "in28minutes","Learn Full Stack Development",  
                LocalDate.now().plusYears(3), false ));  
        todos.add(new Todo(++todosCount, "test","test description",  
                LocalDate.now().plusYears(4), false ));  
    }  
  
    public List<Todo> findByUsername(String username){  
        return todos;  
    }  
    public void addTodo(String username, String description, LocalDate targetDate, boolean done) {  
        Todo todo = new Todo(++todosCount,username,description,targetDate,done);  
        todos.add(todo);  
    }  
}
```

>- todosCount 선언해 id 카운팅
>- 0이 출발이니 todosCount++ 가 아닌 ++todosCount 로 처리
>- addTodo 메소드를 이용해 파라미터로 받은 데이터를 리스트에 저장

- ![](https://i.imgur.com/KbYsqlH.png)
- ![](https://i.imgur.com/kAJrif0.png)
- ![](https://i.imgur.com/Ikl1ujT.png)


