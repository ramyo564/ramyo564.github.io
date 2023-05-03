---

layout: single
title: " [Spring] JDBC to Spring JDBC to JPA to Spring Data JPA "
categories: Spring
tag: [Java,"JDBC to Spring JDBC to JPA to Spring Data JPA"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# JDBC to Spring JDBC to JPA to Spring Data JPA
/ JDBC / Spring JDBC / JPA / Spring Data JPA

- Jdbc
	- Write a lot od SQL queries (delete from todo where id=?)
	- And write a lot of Java code
- Spring JDBC
	- Write a lot od SQL queries (delete from todo where id=?)
	- BUT less Java code
- JPA
	- Do NOT worry about queries
	- Just Map Entities to Tables
- Spring Data JPA
	- Even more simple than JPA
	- Don't need to worry about entity manager as well

## Spring JDBC

```java

@Repository  
public class CourseJdbcRepository {  
    @Autowired  
    private JdbcTemplate springJdbcTemplate;  
    private static String INSERT_QUERY =  
            """  
                insert into course (id, name, author)                values(?, ?,?);                """;  
  
    private static String DELETE_QUERY =  
            """  
                delete from course                where id = ?                """;  
    private static String SELECT_QUERY =  
            """  
                select * from course                where id = ?                """;  
    public void insert(Course course) {  
        springJdbcTemplate.update(INSERT_QUERY,  
                course.getId(), course.getName(), course.getAuthor());  
    }  
    public void deleteById(long id) {  
        springJdbcTemplate.update(DELETE_QUERY,id);  
    }  
    public Course findById(long id) {  
        //ResultSet -> Bean => Row Mapper =>  
        return springJdbcTemplate.queryForObject(SELECT_QUERY,  
                new BeanPropertyRowMapper<>(Course.class), id);  
    }  
}
```



## JPA
```java

@Repository  
@Transactional  
public class CourseJpaRepository {  
    @PersistenceContext // <- @Autowired 보다 더 구체적임  
    private EntityManager entityManager;  
    public void insert(Course course) {  
        entityManager.merge(course);  
    }  
    public Course findById(long id) {  
        return entityManager.find(Course.class, id);  
    }  
    public void deleteById(long id) {  
        Course course = entityManager.find(Course.class, id);  
        entityManager.remove(course);  
    }  
}
```


## Spring Data JPA
```java  
import java.util.List;  
import org.springframework.data.jpa.repository.JpaRepository;  
import com.in28minutes.springboot.learnjpaandhibernate.course.Course;  
public interface CourseSpringDataJpaRepository extends JpaRepository<Course, Long>{  
// Long은 아이디 타입
  
}
```


## Course
```java

@Entity  
public class Course {  
  
    @Id  
    private long id;  
    @Column(name="name") // 객체와 이름이 같은 경우는 생략해도 된다.  
    private String name;  
  
    private String author;  
  
    public Course() {  
  
    }  
  
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
  
    public void setId(long id) {  
        this.id = id;  
    }  
  
    public void setName(String name) {  
        this.name = name;  
    }  
  
    public void setAuthor(String author) {  
        this.author = author;  
    }  
  
    @Override  
    public String toString() {  
        return "Course [id=" + id + ", name=" + name + ", author=" + author + "]";  
    }  
  
    // constructor  
    // getters    // toString  
}
```

### Custom Spring Data JPA

```java

public interface CourseSpringDataJpaRepository extends JpaRepository<Course, Long>{  
  
    List<Course> findByAuthor(String author);  
    List<Course> findByName(String name);  
  
}
```

```java

@Component  
public class CourseCommandLineRunner implements CommandLineRunner{  

    @Autowired  
    private CourseSpringDataJpaRepository repository;  
  
    @Override  
    public void run(String... args) throws Exception {  

        System.out.println(repository.findByAuthor("in28minutes"));  
        System.out.println(repository.findByAuthor(""));  
  
        System.out.println(repository.findByName("Learn AWS Jpa!"));  
        System.out.println(repository.findByName("Learn DevOps Jpa!"));  
  
  
    }  
  
}
```

## 정리

>- 똑같은 기능을 가각 구현했을 때의 차이다.
>- Spring Data JPA도 매직이다.
>- Spring Data JPA 내에서 따로 커스텀 매서드를 만들고 북치고 장구치고 다 할 수 있다.

## Hibernate vs JPA
- JPA defines the specification. It is an API,
- JPA is like an interface
	- How do you define entities? -> by adding an add entity annotation (@Entity)
	- How do you define a primary key? -> (@Id)
	- How do you map attributes? -> (@Column)
	- Who manages the entities? -> Entity manager
- Hibernate is one of the popular implementations of JPA
- Using Hibernate directly would result in a lock in to Hibernate
	- There are other JPA implementations (Toplink, for example)