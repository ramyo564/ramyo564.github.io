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