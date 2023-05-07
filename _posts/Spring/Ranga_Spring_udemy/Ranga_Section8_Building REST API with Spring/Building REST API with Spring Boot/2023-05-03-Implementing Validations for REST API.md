---

layout: single
title: " [Spring] Implementing Validations for REST API "
categories: Spring
tag: [Java,"[BIG][Spring] Building REST API with Spring Boot","[Spring] @Valid","[Spring] @Size","[Spring] @Past","[Spring] getErrorCount()","[Spring] getFieldError().getDefaultMessage()","[Spring] handleMethodArgumentNotValid","[Spring] ResponseEntity"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Building REST API with Spring Boot (7)

/ @Valid / @Size / @Past / getErrorCount() / getFieldError().getDefaultMessage() / handleMethodArgumentNotValid / ResponseEntity

## Implementing Validations for REST API
- 현재 유효성 검사 기능이 없기 때문에 Empty 값도 그대로 입력된다.
- 이를 해결하기 위해 유효성 검사를 사용하자.

### pom.xml
```java
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-validation</artifactId>
</dependency>
```

#### UserResource.java
```java
    @PostMapping("/users")
    public ResponseEntity<User> createUser(@Valid @RequestBody User user) {
        User savedUser = service.save(user);
        URI location = ServletUriComponentsBuilder.fromCurrentRequest()
                .path("/{id}")
                .buildAndExpand(savedUser.getId())
                .toUri();
        return ResponseEntity.created(location).build();
    }
```
>- @Valid 로 유효성 검사
>- 추가로 유저에서 생성되는 빈에도 유효성 에노테이션을 달아주면 된다.

#### User.java
```java
public class User {
	private Integer id;
	@Size(min=2, message = "Name should have atleast 2 characters")
	private String name;
	@Past(message = "Birth Date should be in the past")
	private LocalDate birthDate;
	public User(Integer id, String name, LocalDate birthDate) {
		super();
		this.id = id;
		this.name = name;
		this.birthDate = birthDate;
	}
	public Integer getId() {
		return id;
	}
	public void setId(Integer id) {
		this.id = id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public LocalDate getBirthDate() {
		return birthDate;
	}
	public void setBirthDate(LocalDate birthDate) {
		this.birthDate = birthDate;
	}
	@Override
	public String toString() {
		return "User [id=" + id + ", name=" + name + ", birthDate=" + birthDate + "]";
	}
}

```
>- @Size 는 말 그대로 제시하는 사이즈에 충족하지 않으면 에러를 출력한다.
>- @Past 로컬 날짜보다 과거가 아니면 에러를 출력한다.


![](https://i.imgur.com/FdInysl.png)
> - 현재 오류 번호도 확실하게 출력되지만 원인이 무엇인지 알 수 없다.
> - 이 부분을 커스텀을 해보자.

#### CustomizedResponseEntityExceptionHandler.java
```java
    @Override
    protected ResponseEntity<Object> handleMethodArgumentNotValid(
            MethodArgumentNotValidException ex, HttpHeaders headers, HttpStatusCode status, WebRequest request) {

        ErrorDetails errorDetails = new ErrorDetails(LocalDateTime.now(),
                "Total Errors:" + ex.getErrorCount() + " First Error:" + ex.getFieldError().getDefaultMessage(), request.getDescription(false));



        return new ResponseEntity(errorDetails, HttpStatus.BAD_REQUEST);
    }
```
>- handleMethodArgumentNotValid 메소드를 오버라이딩해서 사용하면 된다.
>- HttpStatus.BAD_REQUEST 는 상태코드 400을 리턴한다.
>- MethodArgumentNotValidException으로 만든 객체 ex를 사용하여 에러가 몇 개인지 그리고 그중에서 첫 번째 에러는 무엇인지 사용자에게 알려주도록 하여 사용자가 제대로 사용할 수 있겠금 해준다.

## 에러가 2개 발생했을 때

![](https://i.imgur.com/QHjip9D.png)
>- 현재 유효성 검사에서 이름과 생년월일 2개가 있다.
>- 총 에러가 2개인지 보여주고 그중에서 첫 번째를 출력해서 보여준다.

![](https://i.imgur.com/LOinSIY.png)
>- 에러를 고치면 위와 같이 총 에러 갯수와 해당 에러의 원인을 출력해준다.