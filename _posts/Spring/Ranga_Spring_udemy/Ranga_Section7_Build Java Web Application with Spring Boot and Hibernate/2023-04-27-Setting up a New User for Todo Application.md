---

layout: single
title: " [Spring] Setting up a New User for Todo Application "
categories: Spring
tag: [Java,"[BIG][Spring] Java Web Application with Spring and Hibernate","[Spring] Setting up a New User for Todo Application","[Spring] UserDetails","[Spring] createNewUser","[Spring] HttpSecurity","[Spring] withDefaults()","[Spring] @Entity","[Spring] SecurityFilterChain","[Spring] authorizeHttpRequests","[Spring] JpaRepository"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Java Web Application with Spring and Hibernate (10)

/ UserDetails / createNewUser / HttpSecurity / withDefaults() / @Entity / SecurityFilterChain /  authorizeHttpRequests / JpaRepository

## Setting up a New User

### before SpringSecurityConfiguration

```java

@Configuration  
public class SpringSecurityConfiguration {  
    @Bean  
    public InMemoryUserDetailsManager createUserDetailsManager() {  
        Function<String, String> passwordEncoder  
                = input -> passwordEncoder().encode(input);  
        UserDetails userDetails = User.builder()  
                .passwordEncoder(passwordEncoder)  
                .username("in28minutes")  
                .password("dummy")  
                .roles("USER","ADMIN")  
                .build();  
        return new InMemoryUserDetailsManager(userDetails);  
    }  
    @Bean  
    public PasswordEncoder passwordEncoder() {  
        return new BCryptPasswordEncoder();  
    }  
}
```

### after SpringSecurityConfiguration
```java
@Configuration
public class SpringSecurityConfiguration {
	@Bean
	public InMemoryUserDetailsManager createUserDetailsManager() {
		UserDetails userDetails1 = createNewUser("in28minutes", "dummy");
		UserDetails userDetails2 = createNewUser("ranga", "dummydummy");
		return new InMemoryUserDetailsManager(userDetails1, userDetails2);
	}
	private UserDetails createNewUser(String username, String password) {
		Function<String, String> passwordEncoder
		= input -> passwordEncoder().encode(input);
		UserDetails userDetails = User.builder()
									.passwordEncoder(passwordEncoder)
									.username(username)
									.password(password)
									.roles("USER","ADMIN")
									.build();
		return userDetails;
	}
	@Bean
	public PasswordEncoder passwordEncoder() {
		return new BCryptPasswordEncoder();
	}
}
```

## Spring Boot starter Data JPA and H2

### Dependency 추가
```java
<dependency>  
   <groupId>org.springframework.boot</groupId>  
   <artifactId>spring-boot-starter-data-jpa</artifactId>  
</dependency>  
  
<dependency>  
   <groupId>com.h2database</groupId>  
   <artifactId>h2</artifactId>  
   <scope>runtime</scope>  
</dependency>
```

## h2
### resources/application.properties
- h2 연결
- 콘솔에 있는 url 복사
```java
spring.datasource.url=jdbc:h2:mem:testdb
```

### SpringSecurityConfiguration
- add SecurityFilterChain
```java
	//All URLs are protected
	//A login form is shown for unauthorized requests
	//CSRF disable
	//Frames
	@Bean
	public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
		http.authorizeHttpRequests(
				auth -> auth.anyRequest().authenticated());
		http.formLogin(withDefaults());
		http.csrf().disable();
		http.headers().frameOptions().disable();
		
		return http.build();
	}
}
```
>- 스프링 시큐리티를 적용하면 CSRF 기능이 동작한다.
>- 이 기능 때문에 h2 콘솔창이 비활성화 된다. (h2콘솔이 스프링과 상관 없는 일반 어플리케이션이라 그렇다고 한다. + 모든 url이 보호되기 때문에 어떤 url 이든 로그인이 안되어 있으면 로그인 창 부터 나옴)
>- SecurityFilterChain
>	- 웹 요청이 들어올 때마다 HTTP 서블릿 요청과 일치시킬 수 있음
>- HttpSecurity 에서 author을 정의해줘야함 -> authorizeHttpRequests
>- withDefaults() 는 Customizer에서 static 메서드 사용
>	- http.formLogin(withDefaults()); 를 통해서 로그인 되지 않는 폼을 체크 (아이디, 비번등)
>	- 참고 : https://wikidocs.net/162150
>	- 참고 : https://velog.io/@kose/SecurityFilterChain-%EB%8B%A4%EC%A4%91-%ED%95%84%ED%84%B0-%EA%B4%80%EB%A6%AC
>- CSRF 를 사용하지 않으니 http.headers().frameOptions() 를 통해 frame도 disable로 

## JPA Bean 데이터 베이스에 연결
- JPA를 사용하면 빈을 데이터베이스에 매핑할 수 있다.
- @Entity 를 추가하면 된다.
- Todo가 Entity라고 하면 이 빈이 데이터베이스 테이블에 매핑이 된다는 뜻이다.

### before Todo.java
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
    @Size(min=10, message="Enter at least 10 characters")  
    private String description;  
    private LocalDate targetDate;  
    private boolean done;  
  
....
  
}
```

```java

//Database (MySQL) 
//Static List of todos => Database (H2, MySQL)

//JPA
// Bean -> Database Table

@Entity
public class Todo {

	public Todo(int id, String username, String description, LocalDate targetDate, boolean done) {
		super();
		this.id = id;
		this.username = username;
		this.description = description;
		this.targetDate = targetDate;
		this.done = done;
	}

	@Id
	@GeneratedValue
	private int id;

	private String username;
	
	@Size(min=10, message="Enter atleast 10 characters")
	private String description;
	private LocalDate targetDate;
	private boolean done;
....

}
```


>- Primary key로 @Id를 선언해서 사용하면 된다.
>- 시퀀스를 사용해서 Id 값을 생성하기 위해서 @GeneratedValue를 사용해주면 된다.
>	- 뭔말이냐면 id 값을 자동으로 생성해주는 에노테이션이다.
>- @Column이나 @Entitiy도 (name= ) 을통해 이름을 컨트롤 할 수 있다.
>- 스프링에서 @Entitiy가 탐색 되면 바로 Todo 클래스 빈을 만든다.

### before h2
![](https://i.imgur.com/2DCUkBb.png)

### after h2
![](https://i.imgur.com/oxrVpUw.png)

## sql문 

### /src/main/resources/data.sql

```java
insert into todo (ID, USERNAME, DESCRIPTION, TARGET_DATE, DONE)
values(10001,'in28minutes', 'Get AWS Certified', CURRENT_DATE(), false);

insert into todo (ID, USERNAME, DESCRIPTION, TARGET_DATE, DONE)
values(10002,'in28minutes', 'Get Azure Certified', CURRENT_DATE(), false);

insert into todo (ID, USERNAME, DESCRIPTION, TARGET_DATE, DONE)
values(10003,'in28minutes', 'Get GCP Certified', CURRENT_DATE(), false);

insert into todo (ID, USERNAME, DESCRIPTION, TARGET_DATE, DONE)
values(10004,'in28minutes', 'Learn DevOps', CURRENT_DATE(), false);
```

>-  sql에서는 '' 작은 따옴표를 사용한다.


### /src/main/resources/application.properties
```java
spring.jpa.defer-datasource-initialization=true
```

>- @Entitiy 가 sql 파일보다 먼저 실행된다.
>- 따라서 테이블이 생성되면 그 후에 @Entity가 실행 될 수 있게 환경 설정을 위와 같이 해줘야 한다.
>- 그럼 다음과 같이 잘 작동된다.
>- ![](https://i.imgur.com/9OmNGZX.png)

## Creating TodoRepository & Connecting List Todos page from H2 database

- Repository allows you to perorm actions on entities
### /src/main/java/com/in28minutes/springboot/myfirstwebapp/todo/TodoRepository.java
```java

public interface TodoRepository extends JpaRepository<Todo, Integer>{
	public List<Todo> findByUsername(String username);
}
```
>- JpaRepository< 1 , 2 >
>	- 1은 어떤 빈을 관리할 건지
>	- 2는 아이디 필드의 타입을 뭘로 할 건지
>- Spring data JPA가 자동으로 username을 찾는 메소드를 생성해준다.

### before Todo.java
```java
@Entity  
public class Todo {   
    public Todo(int id, String username, String description, LocalDate targetDate, boolean done) {  
        super();  
        this.id = id;  
        this.username = username;  
        this.description = description;  
        this.targetDate = targetDate;  
        this.done = done;  
    }
```

### after Todo.java
```java
@Entity  
public class Todo {  
    public Todo(){  <----
    }  
    public Todo(int id, String username, String description, LocalDate targetDate, boolean done) {  
        super();  
        this.id = id;  
        this.username = username;  
        this.description = description;  
        this.targetDate = targetDate;  
        this.done = done;  
    }
```
>- Entity 에 대한 디폴트 생성자를 추가해줌


### before TodoControllerJpa.java
```java
@Controller  
@SessionAttributes("name")  
public class TodoControllerJpa {  
    public TodoControllerJpa(TodoService todoService) {  
        super();  
        this.todoService = todoService;  
    }  
    private TodoService todoService;  
    
@RequestMapping("list-todos")  
public String listAllTodos(ModelMap model) {  
    String username = getLoggedInUsername(model);  
    List<Todo> todos = todoService.findByUsername(username);  
    model.addAttribute("todos", todos);  
    return "listTodos";  
}
```

### after TodoControllerJpa.java
```java
@Controller  
@SessionAttributes("name")  
public class TodoControllerJpa {  
  
    public TodoControllerJpa(TodoService todoService, TodoRepository todoRepository) {  
        super();  
        this.todoService = todoService;  <----
        this.todoRepository = todoRepository;  
    }  
    private TodoService todoService;  <----
    private TodoRepository todoRepository;

@RequestMapping("list-todos")  
public String listAllTodos(ModelMap model) {  
    String username = getLoggedInUsername(model);  
    List<Todo> todos = todoRepository.findByUsername(username);  <---
    model.addAttribute("todos", todos);  
  
    return "listTodos";  
}
```

>-  이렇게 하면 static에 있는 정보가 아닌 h2 database로 부터 데이터를 갖고 온다.

