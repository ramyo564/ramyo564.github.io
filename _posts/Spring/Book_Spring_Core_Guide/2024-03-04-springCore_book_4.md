---
layout: single
title: "[북 스터디] 스프링 핵심가이드 (4)"
categories: Spring
tags:
  - Spring
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
# (4) 스프링 부트 - Spring Data JPA

## JPQL

JPQL 은 JPA Query Language의 줄임말로 JPA에서 사용할 수 있는 쿼리를 의미한다. JPQL의 문법은 SQL과 매우 비슷하지만 SQL에서 테이블이나 칼럼의 이름을 사용하는 것과는 달리 JPQL은 인티티 객체를 대상으로 수행하는 쿼리이기 때문에 매핑된 엔티티의 이름과 필드의 이름을 사용한다는 점이 다르다.    

![](https://i.imgur.com/KfqsYVp.png)

## 쿼리메서드 

리포지토리는 JpaRepository를 상속받는 것만으로도 다양한 CRUD 메서드를 제공한다. 하지만 이러한 기본 메서드들은 식별자 기반으로 생성되어 결국 별도의 메서드를 정의해서 사용하는 경우가 많다.    

### 쿼리 메서드 생성

![](https://i.imgur.com/gIi75xJ.png)

By는 서술어의 시작을 나타내는 구분자 역할을 한다. 서술어 부분은 검색 및 정렬 조건을 지정하는 영역이다. 기본적으로 엔티티의 속성으로 정의할 수 있고, AND나 OR를 사용해 조건을 확장하는 것도 가능하다.   

- find...by
- read...by
- get...by
- query...by
- search...by
- stream...by

... 로 표시한 영역에는 도메인(엔티티)를 표현할 수 있다. 그러나 리포지토리에서 이미 도메인을 설정한 후에 메서드를 사용하기 때문에 중복으로 판단해 생략하기도 한다. 리턴 타입으로는 Collection 이나 Stream에 속한 하위 타입을 설정할 수 있다.     

> `find...By` 키워드를 활용한 쿼리 메서드  


```java
// find..By
Optional<Product> findByNumber(Long number);  
List<Product> findAllByName(String name);  
Product queryByNumber(Long number);
```


> `exists...By`
> 특정 데이터가 존재하는지 확인하는 키워드다. 리턴 타입으로는 boolean 타입을 사용한다.    

```java
// exists..By 
boolean existsByNumber(Long number);
```

> `count...By`
> 조회 쿼리를 수행한 후 쿼리 결과로 나온 레코드의 개수를 리턴한다.   

```java
// count..By
long countByName(String name);
```

> `delete...By`, `remove...By`
> 삭제 쿼리를 수행한다. 리턴 타입이 없거나 삭제한 횟수를 리턴한다.   

```java
// delete..By, remove..By
void deleteByNumber(Long number);  
long removeByName(String name);
```

> `...First<number>...` `...Top<number>...`
> 쿼리를 통해 조회된 결괏값의 개수를 제한하는 키워드다. 두 키워드는 동일한 동작을 수행하며, 주제와 By 사이에 위치한다. 일반적으로 이 키워드는 한 번의 동작으로 여러 건을 조회할 때 사용되며, 단 건으로 조회하기 위해서는 `<number>`를 생략하면 된다.   

```java
// …First<number>…, …Top<number>… 
List<Product> findFirst5ByName(String name);  
List<Product> findTop10ByName(String name);
```

### 쿼리 메서드의 조건자 키워드

JPQL의 서술어 부분에서 사용할 수 있는 몇 가지 조건자 키워드가 있다.   

#### `Is`    
값의 일치를 조건으로 사용하는 조건자 키워드다. 생략되는 경우가 많으며 `Equals` 와 동일한 기능을 수행한다.   

```java 
// Is, Equals (생략 가능)
// Logical Keyword : IS , Keyword Expressions : Is, Equals, (or no keyword)  
// findByNumber 메소드와 동일하게 동작  
Product findByNumberIs(Long number);  
Product findByNumberEquals(Long number);
```

#### `(Is)Not`     
값의 불일치를 조건으로 사용하는 조건자 키워드다. Is는 생략하고 Not 키워드만 사용할 수도 있다.   

```java
// (Is)Not
Product findByNumberIsNot(Long number);  
Product findByNumberNot(Long number);
```

#### `(Is)Null, (Is)NotNull`    
값이 null 인지 검사하는 조건자 키워드    

```java
// (Is)Null, (Is)NotNull
List<Product> findByUpdatedAtNull();  
List<Product> findByUpdatedAtIsNull();  
List<Product> findByUpdatedAtNotNull();  
List<Product> findByUpdatedAtIsNotNull();
```

#### `(Is)True, (Is)False`    
boolean 타입으로 지정된 칼럽값을 확인하는 키워드다. Product 엔티티에 boolean 타입을 사용하는 칼럼이 없다면 에러가 발생한다.    

```java
// (Is)True, (Is)False
Product findByisActiveTrue();
Product findByisActiveIsTrue();
Product findByisActiveFalse();
Product findByisActiveIsFalse();
```

#### `And, Or`
여러 조건을 묶을 때 사용   

```java
Product findByNumberAndName(Long number, String name);  
Product findByNumberOrName(Long number, String name);
```

#### `(Is)GreaterThan, (Is)LessThan, (Is)Between`
숫자나 datetime 칼럼을 대상으로 한 비교 연산에 사용할 수 있는 조건자 키워드다. GreaterThan, LessThan 키워드는 비교 대상에 대한 초과/미만의 개념으로 배교 연산을 수행하고, 경곗값을 포함하려면 Equal 키워드를 추가하면 된다.   

```java
// (Is)GreaterThan, (Is)LessThan, (Is)Between
List<Product> findByPriceIsGreaterThan(Long price);  
List<Product> findByPriceGreaterThan(Long price);  
List<Product> findByPriceGreaterThanEqual(Long price);  
List<Product> findByPriceIsLessThan(Long price);  
List<Product> findByPriceLessThan(Long price);  
List<Product> findByPriceLessThanEqual(Long price);  
List<Product> findByPriceIsBetween(Long lowPrice, Long highPrice);  
List<Product> findByPriceBetween(Long lowPrice, Long highPrice);
```

#### `(Is)Like, (Is)Containing, (Is)StartingWith, (Is)EndingWith`
칼럽값에서 일부 일치 여부를 확인하는 조건자 키워드다. SQL 쿼리문에서 값의 일부를 포함하는 값을 추출할 떄 사용하는 `%` 키워드와 동일한 역할을 하는 키워드다. 자동으로 생성되는 SQL문을 보면 Containing 키워드는 문자열의 양 끝 StartingWith 키워드는 문자열의 앞, EndingWith 키워드는 문자열의 끝에 `%` 가 배치된다. 여기서 별도로 고려해야 하는 키워드는 Like 키워드인데 이 키워드는 코드 수준에서 메서드를 호출하면서 전달하는 값에 `%` 를 명시적으로 입력해줘야한다.   

```java
// (Is)Like, (Is)Containing, (Is)StartingWith, (Is)EndingWith 

List<Product> findByNameLike(String name);  
List<Product> findByNameIsLike(String name);  
  
List<Product> findByNameContains(String name);  
List<Product> findByNameContaining(String name);  
List<Product> findByNameIsContaining(String name);  
  
List<Product> findByNameStartsWith(String name);  
List<Product> findByNameStartingWith(String name);  
List<Product> findByNameIsStartingWith(String name);  
  
List<Product> findByNameEndsWith(String name);  
List<Product> findByNameEndingWith(String name);  
List<Product> findByNameIsEndingWith(String name);
```

## 정렬과 페이징 처리
애플리케이션에서 자주 사용되는 정렬과 페이징 처리는 앞서 소개한 쿼리 메서드를 작성하는 방법을 기반으로 수행할 수 있다. 물론 기본 쿼리 메서드인 이름을 통한 정렬과 페이징 처리도 가능하지만 다른 방법들도 많이 쓴다.   

### 정렬 처리하기
일반적인 쿼리문에서 정렬을 사용할 때는 ORDER BY 구문을 사용한다. 쿼리메서드도 정렬 기능에 동일한 키워드가 사용된다.   

```java
// Asc : 오름차순, Desc : 내림차순 
List<Product> findByNameOrderByNumberAsc(String name);  
List<Product> findByNameOrderByNumberDesc(String name);
```

위와 같이 기본 쿼리 메서드를 작성한 후 OrderBy 키워드를 삽입하고 정렬하고자 하는 칼럼과 오름차순/내림차순을 설정하면 정렬이 수행된다. 2번 줄의 쿼리 메서드를 보면 '상품정보를 이름으로 검색한 후 상품번호로 오름차순 정렬을 수행' 한다는 뜻이다. 오른차순으로 정렬하려면 Asc 키워드를, 내림차순으로 정렬하려면 Desc 키워드를 사용한다.   

![](https://i.imgur.com/4CV6gHF.png)

13번 줄에 order by 구문이 포함돼 있고 메서드 이름에 나와 있는 것처럼 Number에 대해 올므차순으로 정렬하고 있다.   

다른 쿼리 메서드들은 조건 구문에서 조건을 여러 개 사용하기 위해 And 와 Or 키워드를 사용했다. 하지만 정렬 구문은 And나 Or키보드를 사용하지 않고 밑의 코드와 같이 우선순위를 기준으로 차례대로 작성하면 된다.   

```java
 // 여러 정렬 기준 사용하기, And를 붙이지 않음
 List<Product> findByNameOrderByPriceAscStockDesc(String name);
```

메서드는 먼저 Price를 기준으로 오름차순으로 정렬한 후 후순위로 재고수량을 기준으로 내림차순정렬을 수행한다. 이렇게 작성한 메서드가 호출되면 하이버네이트에서는 다음과 같이 쿼리를 작성한다.   

![](https://i.imgur.com/Q7K1t5I.png)
![](https://i.imgur.com/O4XVFAU.png)

이렇게 쿼리 메서드의 이름에 정렬 키워드를 삽입해서 정렬을 수행하는 것도 가능하지만 메서드의 이름이 길어질수록 가독성이 떨어지는 문제가 생긴다. 이를 해결하기 위해 아래와 같이 매개변수를 활용해 정렬할 수도 있다.   

```java
// 매개변수를 활용한 정렬 방법
List<Product> findByName(String name, Sort sort);
```

매개변수를 활용한 쿼리 메서드를 사용하면 쿼리 메서드를 정의하는 단계에서 코드가 줄어드는 장점이 있지만 호출하는 위치에서는 여전히 정렬 기준이 길어져 가독성이 떨어진다.   

### 페이징 처리
페이징 처리란 데이터베이스의 레코드를 개수로 나눠 페이지로 구분하는 것을 의미한다. JPA에서는 이 같은 페이징 처리를 위해 Page 와 Pageable을 사용한다.

```java
Page<Product> findByName(String name, Pageable pageable);
```

위와 같이 리턴 타입으로 Page를 설정하고 매개변수에는 Pageable 타입의 객체를 정의한다. 해당 메서드를 사용하기 위해서는 아래와 같이 호출한다.  

```java
//예제
Page<Product> productPage = productRepository.findByName("펜", PageRequest.of(0,2));
```

위 코드에서 메서드를 호출할 때 리턴 타입으로 Page 객체를 받아야 하기 때문에 `Page<Product>` 로 타입을 선언해서 객체를 리턴받는다. 그리고 Pageable 파라미터를 전달하기 위해 PageRequest 클래스를 사용하고 PageRequest는 Pageable의 구현체다.  
일반적으로 PageRequest 는 of 메서드를 통해 PageRequest 객체를 생성한다. of 메서드는 매개변수에 따라 다양한 형태로 오버로딩돼 있는데 다음과 같은 매개변수 조합이 지원된다.  

| of 메서드                                                  | 매개변수 설명                            | 비고                                    |
| ------------------------------------------------------- | ---------------------------------- | ------------------------------------- |
| of(int page, int size)                                  | 페이지 번호(0부터 시작), 페이지당 데이터 개수        | 데이터를 정렬하지 않음                          |
| of(int page, int size, Sort)                            | 페이지 번호, 페이지당 데이터 개수, 정렬            | sort에 의해 정렬                           |
| of(int page, int size, Direction, String... properties) | 페이지 번호, 페이지당 데이터 개수, 정렬 방향, 속성(칼럼) | Sort.by(direction,properties) 에 의해 정렬 |

![](https://i.imgur.com/lHsdNQM.png)

예제의 메서드가 호출될 떄 하이버네이트에서 생성하는 쿼리는 위와 같다. 쿼리 로그를 보면 select 쿼리에 limit 쿼리가 포함돼 있는 것을 볼 수 있다. 만약 페이지 번호를 0이 아닌 1이상의 숫자를 설정하면 offset 키워드도 포함되어 레코드 목록을 구분해서 가져오게 된다. 이렇게 리턴받은 객체를 출력하면 다음과 같은 출력 결과를 확인할 수 있다.    

![](https://i.imgur.com/xSxJrVL.png)

Page 객체를 그대로 출력하면 해당 객체의 값을 보여주지 않고 위와 같이 몇 번째 페이지에 해당하는지만 확인 할 수 있다. 세부적인 값을 보려면 아래와 같이 작성하면 된다.  

```java
Page<Product> productPage = productRepository.findByName("펜", PageRequest.of(0,2));
System.out.println(productPage.getContent());
```

getContent() 를 사용하면 배열 형태로 값이 출력된다.
## @Query 어노테이션
데이터베이스에서 값을 가져올 때는 메서드의 이름만으로 쿼리 메서드를 생성할 수도 있고 이번 절에서 살펴볼 `@Query` 오노테이션을 사용해 직접 JPQL 을 작성할 수도 있다.  
JPQL 을 사용하면 JPA 구현체에서 자동으로 쿼리 문장을 해석하고 실행하게 된다. 만약 데이터베이스를 다른 데이터베이스로 변경할 일이 없다면 직접 해당 데이터베이스에 특화된 SQL을 작성할 수 있으며, 주로 튜닝된 쿼리를 사용하고자 할 떄 직접 SQL을 작성한다.  
JPQL을 사용해서 상품정보를 조회하는 메서드를 리포지토리에 추가한다.   

```java
@Query("SELECT p FROM Product p WHERE p.name = ?1")  
List<Product> findByName(String name);
```

1번 줄과 같이 `@Query` 어노테이션을 사용해 JPQL 형식의 쿼리문을 작성한다. 참고로 쿼리문에서 SQL 예약어에 해당하는 단어는 대소문자 상관 없다. FROM 뒤에서 엔티티 타입을 지정하고 별침을 사용한다(AS 생략 가능) WHERE 문에서는 SQL과 마찬가지로 조건을 지정한다. 조건문에서 사용한 `?1` 은 파라미터를 전달받이 위한 인자에 해당한다. 1은 첫 번째 파라미터를 의미한다. 하지만 이 같은 방식을 사용할 경우 파라미터의 순서가 바뀌면 오류가 발생할 가능성이 있어 `@Param` 어노테이션을 사용하는 게 좋다.   

```java
@Query("SELECT p FROM Product p WHERE p.name = :name")  
List<Product> findByNameParam(@Param("name") String name);
```

`@Query` 를 사용하면 엔티티 타입이 아니라 원하는 칼럼의 값만 추출할 수 있다.    

```java
@Query("SELECT p.name, p.price, p.stock FROM Product p WHERE p.name = :name")  
List<Object[]> findByNameParam2(@Param("name") String name);
```

이때 메서드에서는 Object 배열의 리스트 형태로 리턴 타입을 지정해야 한다.
## QueryDSL 적용

메서드의 이름을 기반으로 생성하는 JPQL 의 한계는 `@Query` 어노테이션을 통해 대부분 해소할 수 있지만 직접 문자열을 입력하기 때문에 컴파일 시점에 에러를 잡지 못하고 런타임 에러가 발생할 수 있다.    
쿼리의 문자열이 잘못된 경우에는 애플리케이션이 실행된 후 로직이 실행되고 나서야 오류를 발견할 수 있다. 이러한 이유로 개발 환경에서는 문제가 없는 것처럼 보이다가 실제 운영 환경에 애플리케이션을 배포하고 나서 오류가 발견되는 리스크를 유발한다.    

이와 같은 문제를 해결하기 위해 사용되는 것이 QueryDSL 이다. 문자열이 아니라 코드로 쿼리를 작성할 수 있도록 도와준다.

### QueryDSL 
QueryDSL 은 정적 타입을 이용해 SQL 과 같은 쿼리를 생성할 수 있도록 지원하는 프레임워크다. 문자열이나 XML 파일을 통해 쿼리를 작성하는 대신 QueryDSL 이 제공하는 플루언트 (Fluent) API를 활용해 쿼리를 생성할 수 있다.   

### QueryDSL 장점

>- IDE가 제공하는 코드 자동 완성 기능을 사용할 수 있따.
>- 문법적으로 잘못된 쿼리를 허용하지 않는다. 따라서 정상적으로 활용된 QueryDSL 은 문법 오류를 발생시키지 않는다.
>- 고정된 SQL 쿼리를 작성하지 않기 떄문에 동적으로 쿼리를 생성할 수 있다.
>- 코드로 작성하므로 가독성 및 생산성이 향상된다.
>- 도메인 타입과 프로퍼티를 안전하게 참조할 수 있다.

자세한 사용방법은 공문 또는 해당 스터디 책 참고
```
// 한글판 (4.0.1)
http://querydsl.com/static/querydsl/4.0.1/reference/ko-KR/html_single/
// 영문
https://querydsl.com/static/querydsl/4.4.0/reference/html/index.html
```
## JPA Auditing

각 데이터가 누가, 언제 데이터를 생성했고 변경했는지 감시한다는 의미로 사용된다. 앞에서 작성한 코드를 보면 알 수 있듯이 엔티티 클래스에는 공통적으로 들어가는 필드가 있다. 예를 들어 생성일자, 변경일자 같은게 있다. 대표적으로 많이 상죵되는 필드는 다음과 같다.   

- 생성 주체
- 생성 일자
- 변경 주체
- 변경 일자

이러한 필드들은 매번 엔티티를 생성하거나 변경할 때마다 값을 주입해야 하는 번거로움이 있다. 이 같은 번거로움을 해소하기 위해 SpringDataJPA 에서는 이러한 값을 자동으로 넣어주는 기능을 제공한다.

### JPA Auditing 활성화

main() 메서드가 있는 클래스에 `@EnableJpaAuditing`을 추가해준다.

```java
@EnableJpaAuditing  
@SpringBootApplication  
public class AdvancedJpaApplication {  
  
    public static void main(String[] args) {  
        SpringApplication.run(AdvancedJpaApplication.class, args);  
    }  
  
}
```

위와 같이 어노테이션을 추가하면 정상적으로 기능이 활성화되지만 테스트코드 실행에서는 오류가 발생할 수 있다. 예를 들어 `@WebMvcTest` 어노테이션을 지정해서 테스트를 소행하는 코드를 작성하면 애플리케이션 클래스를 호출하는 과정에서 예외가 발생할 수 있따. 이 같은 문제를 해결하기 위해 아래와 같이 별도의 Configuration 클래스를 생성해서 애플리케이션 클래스의 기능과 분리해서 활성화 할 수 있다. (Configuration 클래스를 별도로 생성하는걸 권장)   

```java
@Configuration  
@EnableJpaAuditing  
public class JpaAuditingConfiguration {  
  
}
```

Configuration 클래스를 별도로 지정했다면 main 클래스에서의 어노테이션은 지워줘야한다.   

### BaseEntity 

코드의 중복을 없애기 위해서는 각 엔티티에 공통으로 들어가게 되는 칼럼(필드)을 하나의 클래스로 빼는 작업을 수행하는 게 좋다 아직 생성 주체와 변경 주체는 활용할 일이 없기 떄문에 제외하고 먼저 생성 일자와 변경일자만 추가해서 아래와 같이 만들어준다.    

```java
@Getter  
@Setter  
@ToString  
@MappedSuperclass  
@EntityListeners(AuditingEntityListener.class)  
public class BaseEntity {  
  
    @CreatedDate  
    @Column(updatable = false)  
    private LocalDateTime createdAt;  
  
    @LastModifiedDate  
    private LocalDateTime updatedAt;  
  
}
```

- `@MappedSuperclass ` : JPA의 엔티티 클래스가 상속받을 경우 자식 클래스에게 매핑 정보를 전달한다.
- `@EntityListeners` : 엔티티를 데이터베이스에 적용하기 전후로 콜백을 요청할 수 있게 하는 어노테이션이다.
- `AuditingEntityListener` : 엔티티의 Auditing 정보를 주입하는 JPA 엔티티 리스터 클래스다.
- `@CreatedDate` : 데이터 생성 날짜를 자동으로 주입하느 어노테이션이다.
- `@LastModifiedDate` 데이터 수정 날짜를 자동으로 주입하는 어노테이션이다.

```java
@Entity  
@Getter  
@Setter  
@NoArgsConstructor  
@ToString(callSuper = true)  
@EqualsAndHashCode(callSuper = true)  
@Table(name = "product")  
public class Product extends BaseEntity {  
  
    @Id  
    @GeneratedValue(strategy = GenerationType.IDENTITY)  
    private Long number;  
  
    @Column(nullable = false)  
    private String name;  
  
    @Column(nullable = false)  
    private Integer price;  
  
    @Column(nullable = false)  
    private Integer stock;  
  
}
```

`@ToString(callSuper = true), @EqualsAndHashCode(callSuper = true)` 여기서 적용한 callSuper 속성은 부모클래스의 필드를 포함하는 역할을 수행한다. 또한 이렇게 설정하면 매번 LocalDateTime.now() 메서드를 사용해 시간을 주입하지 않아도 자동으로 값이 생성할 수 있다.   

>JPA Auditing 기능에는 `@CreatedBy`, `@ModifiedBy` 어노테이션도 존재한다. 누가 엔티티를 생성하고 수정했는지 자동으로 값을 주입하는 기능이다. 이 기능을 사용하려면 AuditorAware 를 스프링 빈으로 등록할 필요가 있다.