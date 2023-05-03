---

layout: single
title: " [Spring] Build a Hello world API with Spring Boot "
categories: Spring
tag: [Java,"Build a Hello world API with Spring Boot","arrays.asList()"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Build a Hello world API with Spring Boot
/ arrays.asList() /

```java

public class Course {
	private long id;
	private String name;
	private String author;

	public Course(long id, String name, String author) {
		super();
		this.id = id;
		this.name = name;
		this.author = author;
	}

	public long getId() {
		return id;
	}

	public String getName() {
		return name;
	}

	public String getAuthor() {
		return author;
	}

	@Override
	public String toString() {
		return "Course [id=" + id + ", name=" + name + ", author=" + author + "]";
	}

}
```

```java
import java.util.Arrays;
import java.util.List;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class CourseController {
	
	@RequestMapping("/courses")
	public List<Course> retrieveAllCourses() {
		return Arrays.asList(
				new Course(1, "Learn AWS", "in28minutes"),
				new Course(2, "Learn DevOps", "in28minutes"),
				new Course(3, "Learn Azure", "in28minutes"),
				new Course(4, "Learn GCP", "in28minutes")
				
				);
	}

}
```

- ![](https://i.imgur.com/nwmPVXD.png)


## 정리
>- 일반 배열을 ArrayList로 변환한다.
>- `List<String> list = Arrays.asList(arr);`


### Arrays.asList
- Arrays.asList()는 Arrays의 private 정적 클래스인 ArrayList를 리턴한다.
- **java.util.ArrayList 클래스와는 다른 클래스**이다.
- java.util.Arrays.ArrayList 클래스는 set(), get(), contains() 메서드를 가지고 있지만  **원소를 추가하는 메서드는 가지고 있지 않기 때문에 사이즈를 바꿀 수 없다.**
- asList()를 사용해서 List 객체를 만들 때 새로운 배열 객체를 만드는 것이 아니라,  **원본 배열의 주소값**을 가져오게 된다.  
- 따라서 asList()를 사용해서 내용을 수정하면 원본 배열도 함께 바뀌게 되고  원본 배열을 수정하면 그 배열로 만들어뒀던 asList()를 이용한 List 내용도 바뀌게 된다.  
- 이러한 이유 때문에 Arrays.asList()로 만든 List에 **새로운 원소를 추가하거나 삭제 할 수 없다.**  
- 따라서 Arrays.asList()는 배열의 내용을 수정하려고 할 때 List로 바꿔서 편리하게 사용하기 위함.

참조 : https://m.blog.naver.com/roropoly1/221140156345, https://tecoble.techcourse.co.kr/post/2020-05-18-ArrayList-vs-Arrays.asList/ , https://velog.io/@bahar-j/%EC%9E%90%EB%B0%94%EC%9D%98-Arrays.asList