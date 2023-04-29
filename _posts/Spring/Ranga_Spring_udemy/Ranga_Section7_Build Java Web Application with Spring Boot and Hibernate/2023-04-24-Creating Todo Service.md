---

layout: single
title: " Creating Todo Service "
categories: Spring
tag: [Java,"[BIG] Java Web Application with Spring and Hibernate","Creating Todo Service","@SessionAttributes",JSTL,Bootstrap]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Java Web Application with Spring and Hibernate (3)

/ @SessionAttributes / bootstrap / JSTL

## Creating Todo Service
- list 만들어서 jsp에 뿌리고 모델,컨트롤러, 뷰 연결하기

#### listTodos.jsp
```java
<html>  
   <head>  
      <title>List Todos Page</title>  
   </head>  
   <body>  
       <div>Welcometo in28minutes>div>  
       <div>Your Todos are ${todos}</div>  
   </body>  
</html>
```

#### TodoService.java
```java

@Service  
public class TodoService {  
  
    private static List<Todo> todos = new ArrayList<>();  
  
    static {  
        todos.add(new Todo(1, "in28minutes","Learn AWS",  
                LocalDate.now().plusYears(1), false ));  
        todos.add(new Todo(2, "in28minutes","Learn DevOps",  
                LocalDate.now().plusYears(2), false ));  
        todos.add(new Todo(3, "in28minutes","Learn Full Stack Development",  
                LocalDate.now().plusYears(3), false ));  
        todos.add(new Todo(4, "test","test description",  
		        LocalDate.now().plusYears(4), false ));
    }  
  
    public List<Todo> findByUsername(String username){  
        return todos;  
    }  
}
```

#### TodoController
```java

@Controller  
@SessionAttributes("name")  
public class TodoController {  
  
    public TodoController(TodoService todoService) {  
        super();  
        this.todoService = todoService;  
    }  
  
    private TodoService todoService;  
  
  
    @RequestMapping("list-todos")  
    public String listAllTodos(ModelMap model) {  
        List<Todo> todos = todoService.findByUsername("adad");  
        model.addAttribute("todos", todos);  
  
        return "listTodos";  
    }  
  
}
```

>- findByUsername 매서드에 임의의 값(adad)을 넣어도 현재는 return 값이 todo 그 자체이기 때문에 상관없이 아이디 1,2,3,4 의 정보를 다 불러온다.
- ![](https://i.imgur.com/BYEPW8s.png)


#### Todo.java
```java

//Database (MySQL)  
//Static List of todos => Database (H2, MySQL)  
  
public class Todo {  
  
    public Todo(int id, String username, String description, LocalDate targetDate, boolean done) {  
        super();  
        this.id = id;  
        this.username = username;  
        this.description = description;  
        this.targetDate = targetDate;  
        this.done = done;  
    }  
  
    private int id;  
    private String username;  
    private String description;  
    private LocalDate targetDate;  
    private boolean done;  
  
    public int getId() {  
        return id;  
    }  
  
    public void setId(int id) {  
        this.id = id;  
    }  
  
    public String getUsername() {  
        return username;  
    }  
  
    public void setUsername(String username) {  
        this.username = username;  
    }  
  
    public String getDescription() {  
        return description;  
    }  
  
    public void setDescription(String description) {  
        this.description = description;  
    }  
  
    public LocalDate getTargetDate() {  
        return targetDate;  
    }  
  
    public void setTargetDate(LocalDate targetDate) {  
        this.targetDate = targetDate;  
    }  
  
    public boolean isDone() {  
        return done;  
    }  
  
    public void setDone(boolean done) {  
        this.done = done;  
    }  
  
    @Override  
    public String toString() {  
        return "Todo [id=" + id + ", username=" + username + ", description=" + description + ", targetDate="  
                + targetDate + ", done=" + done + "]";  
    }  
  
}
```

클래스 제네릭 : https://gangnam-americano.tistory.com/47
Static : https://mangkyu.tistory.com/47 , https://coding-factory.tistory.com/524

## Request vs Model vs Session

### Session vs Request Scopes
- All requests from browser are handled by our web application deployed on a server
- Request Scope: Active for a single request ONLY
	- Once the response is sent back, the request attributes will be removed from memory
	- These cannot be used for future requests
	- Recommended for most use cases
- Session Scope: Details stored across multiple requests
	- Be careful about what you store in session (Takes additional memory as all details are stored on server)

### example

- ![](https://i.imgur.com/Jcy2eeG.png)
- ![](https://i.imgur.com/EsnZ3p2.png)
- 현재 name 부분은 in28minutes 라는 정보를 갖고 있다.
- manage는 위 부분의  list-todos 으로 연결해놨다.
- list-todos에 name 정보를 써 넣었지만 해당 정보는 안 넘어오는걸 볼 수 있다.
- ![](https://i.imgur.com/8lyLLZD.png)
- ![](https://i.imgur.com/vkb5ebi.png)
- 위에 나온것처럼 디폴트는 해당 페이지에서 넘어가면 정보가 없어짐으로
- Session을 이용해야 데이터를 길게 가져갈 수 있음
- 해당 컨트롤러에 @SessionAttributes 을 사용하면 Session을 이용해서 데이터를 유지할 수 있다.
- ![](https://i.imgur.com/qdNV7xD.png)
- ![](https://i.imgur.com/HZMeXLX.png)
- ![](https://i.imgur.com/xvMo77a.png)

## JSTL

- 자바코드를 html태그형식으로 간편하게 사용하기 위해 나온 라이브러리

참고 : https://velog.io/@psj0810/JSTL%EC%9D%B4%EB%9E%80-JSTL-%EA%B8%B0%EC%B4%88%EC%82%AC%EC%9A%A9%EB%B2%95 , https://hackersstudy.tistory.com/42

### JSTL Dependencies 추가 
```java
<dependency>  
   <groupId>jakarta.servlet.jsp.jstl</groupId>  
   <artifactId>jakarta.servlet.jsp.jstl-api</artifactId>  
</dependency>  
  
<dependency>  
   <groupId>org.eclipse.jetty</groupId>  
   <artifactId>glassfish-jstl</artifactId>  
</dependency>
```

### 부트스트랩 & 제이쿼리 Dependencies 추가

```java
<dependency>  
   <groupId>org.webjars</groupId>  
   <artifactId>bootstrap</artifactId>  
   <version>5.1.3</version>  
</dependency>  
  
<dependency>  
   <groupId>org.webjars</groupId>  
   <artifactId>jquery</artifactId>  
   <version>3.6.0</version>  
</dependency>
```

#### listTodo.jsp
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
               </tr>  
            </thead>  
            <tbody>  
               <c:forEach items="${todos}" var="todo">  
                  <tr>  
                     <td>${todo.id}</td>  
                     <td>${todo.description}</td>  
                     <td>${todo.targetDate}</td>  
                     <td>${todo.done}</td>  
                  </tr>  
               </c:forEach>  
            </tbody>  
         </table>  
  
      </div>  
      <script src="webjars/bootstrap/5.1.3/js/bootstrap.min.js"></script>  
      <script src="webjars/jquery/3.6.0/jquery.min.js"></script>  
  
   </body>  
</html>
```

>- JSTL 문법은 위에 참고란 링크를 참고
>- 제이쿼리는 더 이상 쓸 이유가 없다고 하지만 계속 지겹게 나오는거 보면 그냥 찍먹 수준으로 보는게 좋은거 같다.
>- 진자든 타임리프든 사용법만 알면 다 비슷비슷한거 같다.

![](https://i.imgur.com/IkNq7El.png)
