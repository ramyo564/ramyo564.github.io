---
layout: single
title: "[Account PJ] 핀테크 트러블 슈팅"
categories: 개발일기
tags:
  - EnableGlobalMethodSecurity
toc: true
toc_sticky: true
author_profile: false
sidebar:
---

@EnableGlobalMethodSecurity

```java
@EnableMethodSecurity
```

https://stackoverflow.com/questions/74910066/enableglobalmethodsecurity-is-deprecated-in-the-new-spring-boot-3-0


```java
  
@Slf4j  
@Configuration  
@EnableWebSecurity  
@EnableGlobalMethodSecurity(prePostEnabled = true)  
@RequiredArgsConstructor  
public class SecurityConfiguration extends WebSecurityConfigurerAdapter {  
  
    private final JwtAuthenticationFilter authenticationFilter;  
  
    //security 땜빵용  
    @Override  
    protected void configure(HttpSecurity http) throws Exception {  
        http  
                .httpBasic().disable()  
                .csrf().disable()  
                .sessionManagement().sessionCreationPolicy(  
                        SessionCreationPolicy.STATELESS)  
                .and()  
                .authorizeRequests()  
                .antMatchers(  
                        "/**/signup-customer",  
                        "/**/signin-customer",  
                        "/**/signup-owner",  
                        "/**/signin-owner"  
  
                )  
                .permitAll()  
                .and()  
                .addFilterBefore(  
                        this.authenticationFilter  
                        , UsernamePasswordAuthenticationFilter.class);  
  
    }  
  
    @Override  
    public void configure(final WebSecurity web) throws Exception {  
        //db 접근은 인증정보 필요 없게 함  
        web.ignoring()  
                .antMatchers("/h2-console/**");  
    }  
  
}
```


https://uriu.tistory.com/435


```java
@Override  
public void configure(final WebSecurity web) throws Exception {  
    //db 접근은 인증정보 필요 없게 함  
    web.ignoring()  
            .antMatchers("/h2-console/**");  
}
```

```java
@Bean  
public WebSecurityCustomizer webSecurityCustomizer() {  
    return (web) -> web.ignoring().requestMatchers(  
            new AntPathRequestMatcher("/h2-console/**"));  
}
```
https://covenant.tistory.com/277

https://www.inflearn.com/questions/992358/websecuritycustomizer-%EC%99%80-securityfilterchain-%EC%B0%A8%EC%9D%B4%EC%A0%90%EC%97%90-%EB%8C%80%ED%95%B4-%EC%97%AC%EC%AD%99%EA%B3%A0%EC%8B%B6%EC%96%B4%EC%9A%94



https://stackoverflow.com/questions/53539728/how-to-validate-date-in-the-format-mm-dd-yyyy-in-spring-boot


//일반적인 예외처리 ㄱㄱ  
// https://velog.io/@gongmeda/Validation-%EC%98%88%EC%99%B8-%ED%95%B8%EB%93%A4%EB%9F%AC%EB%A1%9C-%EC%9D%91%EB%8B%B5-%ED%8F%AC%EB%A7%B7-%EC%88%98%EC%A0%95%ED%95%98%EA%B8%B0



```
jakarta.servlet.ServletException: Handler dispatch failed: java.lang.NoClassDefFoundError: javax/xml/bind/DatatypeConverter
```

https://devthriver.tistory.com/13


## csrf 문제

403 
https://sedangdang.tistory.com/303

dependencies {
    testImplementation 'org.springframework.security:spring-security-test'
}

![](https://i.imgur.com/k7gAIg3.png)

401 에러 

## 테스트 코드
모르는게 너무 많음 ㅜ 테스트코드 이것도 좀 정리해놔야함
https://velog.io/@jkijki12/Spring-MockMvc
https://everydayyy.tistory.com/103


## 토근 근데도 403

role 권한 부여 jwt 필터에서 걸러져도 권한승인이 없음
permit all 에서 변경


## equals and hashcode
https://velog.io/@wonizizi99/Spring-equals%EC%99%80-hashCode%EB%8A%94-%EC%99%9C-%EA%B0%99%EC%9D%B4-%EC%9E%AC%EC%A0%95%EC%9D%98%ED%95%B4%EC%95%BC-%ED%95%A0%EA%B9%8C

https://mangkyu.tistory.com/101


## 테스트 코드

# Introduction: Tackling the NullPointerException in Spring Security Tests

As a developer, one of the most challenging tasks I just encountered was resolving a `NullPointerException` in my Spring Security unit tests. This error often occurs when the `SecurityContext` isn't properly set up, leading to issues when your service methods rely on authentication information. Here's my journey to resolving this issue.

# The Root of the Error

The crux of the problem lies in how Spring Security handles authenticated contexts. In a typical Spring Boot application with Spring Security, certain service methods, like `createPost` in the `PostServiceImpl`, require the current user's information. During testing, without a correctly initialized `SecurityContext`, the `SecurityContextHolder` does not have the necessary authentication details. This absence results in the dreaded `NullPointerException`.

# The Solution: Embracing `@WithMockUser`

Spring Security provides a powerful annotation named `@WithMockUser` for addressing this issue in tests. This annotation helps simulate a logged-in user by mocking the authentication and user details in a test environment, thereby eliminating the need for an actual user or web request for authentication.

## What is `@WithMockUser`?

`@WithMockUser` is an annotation used in Spring Security testing to set up a mock security context. It allows you to define a fake user with specified roles and permissions, which the Spring Security context uses during the execution of the test.

## Alternatives: Considering `@WithUserDetails`

While `@WithMockUser` is convenient, there's also the `@WithUserDetails` annotation. However, using `@WithUserDetails` requires configuring `UserDetailsService`, which can be more cumbersome for simple test scenarios. Hence, for ease of use and simplicity, `@WithMockUser` often becomes the preferred choice.

@ExtendWith(MockitoExtension.class)  
@ActiveProfiles("test")  
class PostServiceImplTest {  
  
    @Mock  
    private PostRepository postRepository; // Mock PostRepository  
    @Mock  
    private UserRepository userRepository; // Mock UserRepository  
  
    @InjectMocks  
    private PostServiceImpl postService; // Inject mocks into PostServiceImpl  
  
    private LocalDateTime localDateTime;  
    private User user;  
    private Post post;  
  
    @BeforeEach  
    public void firstPostSetup() {  
        // User setup  
        user = User.builder()  
                .name("Steve Jobs")  
                .email("steve.jobs@apple.com")  
                .nickname("Steve")  
                .password("apple123")  
                .roles(singletonList(new Role(1L, "ROLE_GUEST", new ArrayList<>())))  
                .build();  
  
        // Post setup  
        post = Post.builder()  
                .title("Innovation at Apple")  
                .url("innovation-at-apple")  
                .content("Apple's approach to innovation...")  
                .shortDescription("A brief overview of Apple's innovative strategies")  
                .createdBy(user)  
                .createdOn(now)  
                .updatedOn(now)  
                .comments(new HashSet<>()) // Assuming no comments initially  
                .build();  
    }  
  
    @DisplayName("JUnit test for createPost method")  
    @Test  
    public void givenPostObject_whenCreatePost_thenReturnPostObject() {  
        // Mock behavior of userRepository and postRepository  
        given(userRepository.findByEmail(user.getEmail())).willReturn(user);  
        given(postRepository.save(any(Post.class))).willReturn(post);  
  
        PostDto savedPost = PostMapper.mapToPostDto(post); // Map Post to PostDto  
        String email = user.getEmail();  
        email = SecurityUtils.getCurrentUser().getUsername();  
        // Perform the action to be tested  
        postService.createPost(savedPost);  
  
        // Verify interactions and assert results  
        verify(postRepository).save(any(Post.class)); // Ensure postRepository.save was called  
        assertNotNull(savedPost); // Check that savedPost is not null  
    }  
}

java.lang.NullPointerException: Cannot invoke "org.springframework.security.core.Authentication.getPrincipal()" because the return value of "org.springframework.security.core.context.SecurityContext.getAuthentication()" is null  
 at net.shinyeachan.springboot.util.SecurityUtils.getCurrentUser(SecurityUtils.java:15)  
 at net.shinyeachan.springboot.service.impl.PostServiceImplTest.givenPostObject_whenCreatePost_thenReturnPostObject(PostServiceImplTest.java:81)  
 at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method)  
 at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:77)  
 at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)  
 at java.base/java.lang.reflect.Method.invoke(Method.java:568)  
... error occurs

// 1. Initializes the Spring Boot test environment.  
@SpringBootTest  
// 2. Configures the MockMvc for testing controllers.  
@AutoConfigureMockMvc  
@ExtendWith(MockitoExtension.class)  
@ActiveProfiles("test")  
class PostServiceImplTest {  
  
    @Mock  
    private PostRepository postRepository;  
    @Mock  
    private UserRepository userRepository;  
  
    @InjectMocks  
    private PostServiceImpl postService;  
  
    private User user;  
    private Post post;  
  
    private LocalDateTime now = LocalDateTime.now();  
  
    @BeforeEach  
    public void setup() {  
        // User setup  
        user = User.builder()  
                .name("Steve Jobs")  
                .email("steve.jobs@apple.com")  
                .nickname("Steve")  
                .password("apple123")  
                .roles(singletonList(new Role(1L, "ROLE_GUEST", new ArrayList<>())))  
                .build();  
  
        // Post setup  
        post = Post.builder()  
                .title("Innovation at Apple")  
                .url("innovation-at-apple")  
                .content("Apple's approach to innovation...")  
                .shortDescription("A brief overview of Apple's innovative strategies")  
                .createdBy(user)  
                .createdOn(now)  
                .updatedOn(now)  
                .comments(new HashSet<>()) // Assuming no comments initially  
                .build();  
  
    }  
    @Test  
    // 3. Mocks a user with specified username and role.  
    @WithMockUser(username = "steve.jobs@apple.com", roles = "GUEST")  
    public void givenPostDto_whenCreatePost_thenPostIsSaved() {  
        // given : PostDto setup  
        PostDto postDto = PostDto.builder()  
                .title("Innovation at Apple")  
                .url("innovation-at-apple")  
                .content("Apple's approach to innovation...")  
                .shortDescription("A brief overview of Apple's innovative strategies")  
                .comments(emptySet())  
                .build();  
  
        // when : Perform the action to be tested  
        postService.createPost(postDto);  
  
        // then : Verify interactions and assert results  
        verify(postRepository).save(any(Post.class));  
    }  
}

## Conclusion

The `[@WithMockUser](http://twitter.com/WithMockUser)` annotation from Spring Security is an effective tool for simplifying security contexts in unit tests. It enables easy testing of secured methods without cumbersome setup or the need for an actual user in the system, ensuring that your security logic is thoroughly tested and reliable.

## References:

[11. Testing Method Security (spring.io)](https://docs.spring.io/spring-security/site/docs/5.0.x/reference/html/test-method.html)

[Spring Security 단위테스트 :: 레이피엘의 블로그 (tistory.com)](https://reiphiel.tistory.com/entry/spring-security-unit-test)


## 에러처리 하지만 안나옴

![](https://i.imgur.com/z1S7wqn.png)

[[Spring Security] 스프링 시큐리티를 적용하고 예외가 발생했을 때 403 Forbidden이 발생하는 원인과 처리 방안 (tistory.com)](https://colabear754.tistory.com/182)

https://velog.io/@shwncho/Spring-security-6-Controller-test-403-Forbidden-%EC%97%90%EB%9F%ACfeat.-csrf

JWT 토큰
https://csy7792.tistory.com/177

![](https://i.imgur.com/UL2pYcJ.png)

https://wonit.tistory.com/631
스테틱 메서드는 재정의가 불가능!
[[Spring] ❌ Static 메소드를 모킹하지 말자 (velog.io)](https://velog.io/@betterfuture4/Static-%EB%A9%94%EC%86%8C%EB%93%9C%EB%A5%BC-mocking%ED%95%98%EC%A7%80-%EB%A7%90%EC%9E%90-feat.-LocalDate.nowclock)

static이 아니라 컴포넌트로 해결


## 토큰 예외처리
object 매퍼로처리하면 해결됨 -> 안됌!

![](https://i.imgur.com/dpzDNX8.png)

https://wonowdaily.tistory.com/75


https://jhkimmm.tistory.com/29


DTO 사용이유
https://velog.io/@witwint/Spring-DTO%EC%9D%98-%EC%82%AC%EC%9A%A9%EC%9D%B4%EC%9C%A0



--------------------------------------------------------

금요일 5시까지 트러블 슈팅써야함

스웨거 쓸지말지

늦어도 수요일까지는 리뷰 요청해야함 -> 늦어도 화요일 -> 
어떤 프로젝트 만들고 어떤 기능을 만들려고한다 초안 완성되고 꼭 확인받기
보통 4번 한다고 함 -> 빠르게 실패하자

기획먼저 -> 피드백 받고 erd 작성

슬랙 -> 디스코드? -> 글 정리하기



