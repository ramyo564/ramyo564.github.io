---

layout: single
title: " [Spring] Connecting Database with Spring "
categories: Spring
tag: [Java,"[BIG][Spring] Java Web Application with Spring and Hibernate","[Spring] JpaRepository"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Java Web Application with Spring and Hibernate (11)

/ JpaRepository /

## Changing hard code

### before TodoControllerJpa.java
```java
@RequestMapping(value="add-todo", method = RequestMethod.POST)  
public String addNewTodo(ModelMap model, @Valid Todo todo, BindingResult result) {  
  
    if(result.hasErrors()) {  
        return "todo";  
    }  
  
    String username = getLoggedInUsername(model);  
    todoService.addTodo(username, todo.getDescription(),  
            LocalDate.now().plusYears(1), false);  
    return "redirect:list-todos";  


...
delete <---
	@RequestMapping("delete-todo")  
	public String deleteTodo(@RequestParam int id) {  
	    //Delete todo  
	  
	    todoService.deleteById(id);  
	    return "redirect:list-todos";  
	  
	}

update <---
		@RequestMapping(value="update-todo", method = RequestMethod.GET)  
	public String showUpdateTodoPage(@RequestParam int id, ModelMap model) {  
	    Todo todo = todoService.findById(id);  
	    model.addAttribute("todo", todo);  
	    return "todo";  
	}
	
	@RequestMapping(value="update-todo", method = RequestMethod.POST)  
	public String updateTodo(ModelMap model, @Valid Todo todo, BindingResult result) {  
	  
	    if(result.hasErrors()) {  
	        return "todo";  
	    }  
	  
	    String username = getLoggedInUsername(model);  
	    todo.setUsername(username);  
	    todoService.updateTodo(todo);  
	    return "redirect:list-todos";  
	}

}

```


### after TodoControllerJpa.java
```java
@RequestMapping(value="add-todo", method = RequestMethod.POST)  
public String addNewTodo(ModelMap model, @Valid Todo todo, BindingResult result) {  
  
    if(result.hasErrors()) {  
        return "todo";  
    }  
  
    String username = getLoggedInUsername(model);  
    todo.setUsername(username);  
	todoRepository.save(todo);
  //  todoService.addTodo(username, todo.getDescription(),  
  //          todo.getTargetDate(), todo.isDone());  
    return "redirect:list-todos";  

	delete <-----
	
	@RequestMapping("delete-todo")  
	public String deleteTodo(@RequestParam int id) {  
	    //Delete todo  
	    todoRepository.deleteById(id);  
	    return "redirect:list-todos";  
	  
	}
	
	update <---
	
	@RequestMapping(value="update-todo", method = RequestMethod.GET)  
	public String showUpdateTodoPage(@RequestParam int id, ModelMap model) {  
	    Todo todo = todoRepository.findById(id).get();  
	    model.addAttribute("todo", todo);  
	    return "todo";  
	}

	@RequestMapping(value="update-todo", method = RequestMethod.POST)  
	public String updateTodo(ModelMap model, @Valid Todo todo, BindingResult result) {  
	  
	    if(result.hasErrors()) {  
	        return "todo";  
	    }  
	  
	    String username = getLoggedInUsername(model);  
	    todo.setUsername(username);  
	    todoRepository.save(todo);  
	    //todoService.updateTodo(todo);  
	    return "redirect:list-todos";  
	}

}
```
>- 시간 표시 같은 경우 로컬이 아닌 자동으로 되게 변경
>- todoService 이걸 다시 todoRepository로 변경
>- update 같은 경우는 기존과 다르게 .get() 을 따로 한 번 더 사용해줘야함

## 정리
- JpaRepository 덕분에 따로 클래스를 만들어서 매서드를 만들 필요가 없어져서 이전 보다 편하게 작업 가능하다
- 근데 아직까지는 써보면서 느낀건 그냥 간단하게 만드는건 플라스크나 장고가 짱인거 같다.
- 대용량 트레픽 발생에서는 장고가 쓰레기라고 들었는데 인스타그램이나 넷플릭스등 기타 여러 대형사이트들도 장고베이스가 많아서 이건 그냥 실력차이인거 같다
	- 플라스크는 확실히 설정할 게 많아서 그렇다 쳐도 장고는 이런거 몰라도 어느정도 가능하다.
	  스프링을 공부하면서 느낀 점은 이런 내부 동작을 모르면 장고 쓰면서 오류나거나 문제 생겼을 때 해결 못 할거 같다.