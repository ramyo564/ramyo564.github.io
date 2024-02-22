---
layout: single
title: "[북 스터디] 스프링 핵심가이드 (3)"
categories: Spring
tags:
  - Spring
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
# (3) 스프링 부트 - 데이터 베이스 연동

## ORM

![](https://i.imgur.com/tWDBEFo.png)

ORM 을 이용하면 쿼리문 작성이 아닌 코드(메서드)로 데이터를 조작 할 수 있다.

### 장점

- ORM을 사용하면서 데이터베이스 쿼리를 객체지향적으로 조작할 수 있다.
	- 쿼리문을 작성하는 양이 현저히 줄어 개발 비용이 줄어든다.
	- 객체지향적으로 데이터베이스에 접근할 수 있어 코드의 가독성이 높아진다.
- 재사용 및 유지보수가 편리하다.
	- ORM을 통해 매핑된 객체는 모두 독립적으로 작성되어 있어 재사용이 용이하다.
	- 객체들은 각 클래스로 나뉘어 있어 유지보수가 수월하다.
- 데이터베이스에 대한 종속성이 줄어든다.
	- ORM을 통해 자동 생성된 SQL문은 객체를 기반으로 데이터베이스 테이블을 관리하기 때문에 데이터베이스에 종속적이지 않다.
	- 데이터베이스를 교체하는 상황에서도 비교적 적은 리스크를 부담한다.

### 단점

- ORM만으로 온전한 서비스를 구현하기에는 한계가 있다.
	- 복잡한 서비스의 경우 직접 쿼리를 구현하지 않고 코드로 구현하기 어렵다.
	- 복잡한 쿼리를 정확한 설계 없이 ORM만으로 구성하게 되면 속도 저하 등의 성능 문제가 발생할 수 있다.
- 애플리케이션의 객체 관점과 데이터베이스의 관계 관점의 불일치가 발생한다.
	- 세분성 (Granularity) : ORM의 자동 설계 방법에 따라 데이터베이스에 있는 테이블의 수와 애플리케이션의 엔티티(Entity) 클래스의 수가 다른 경우가 생긴다 (클래스가 테이블의 수보다 많아질 수 있다.)
	- 상속성 (Inheritance) : RDBMS 에는 상속이라는 개념이 없다.
	- 식별성 (Identity) : RDBMS 는 기본키 (primary key) 로 동일성을 정의한다. 하지만 자바는 두 객체의 값이 같아도 다르다고 판단할 수 있다 -> 식별과 동일성의 문제
	- 연관성 (Associations) : 객체지향 언어는 객체를 참조함으로써 연관성을 나타내지만 RDBMS 에서는 외래키(foreign key)를 삽입함으로써 연관성을 표현한다. 또한 객체지향 언어에서는 객체를 참조할 떄는 방향성이 존재하지만 RDBMS에서는 왜래키를 삽입하는 것은 양뱡향의 관계를 가지기 때문에 방향성이 없다.
	- 탐색 (Navigation) : 자바와 RDBMS 는 어떤 값(객체)에 접근하는 방식이 다르다. 자바에서는 특정 값에 접근하기 위해 객체 참조 같은 연결 수단을 활용한다. 이 방식은 객체를 연결하고 또 연결해서 접근하는 그래프 형태의 접근 방식이다. (예: 어떤 멤버의 회사 주소를 구하기 위해 `member.getOrganization().getAddress()` 와 같이 접근할 수 있다.) 반면 RDBMS 에서는 쿼리를 최소화하고 조인 (JOIN) 을 통해 여러 테이블을 로드하고 값을 추출하는 접근 방식을 채택하고 있다.


## JPA

![](https://i.imgur.com/CZeA1du.png)

JPA(Java Persistence API) 는 자바 진영의 ORM 기술 표준으로 채택된 인터페이스 모음집이다. ORM이 큰 개념이라면 JPA는 더 구체화된 스펙이 포함된다. JPA 는 실제로 동작하는게 아닌 어떻게 동작해야 하는지 매커니즘을 정리한 표준 명세로 생각하면 된다.

![](https://i.imgur.com/fxyexWr.png)

JPA 의 메커니즘을 보면 내부적으로 JDBC를 사용한다. 개발자가 직접 JDBC를 구현하면 SQL에 의존하게 되는 문제 등이 있어 개발의 효율성이 떨어지는데 JPA는 이 같은 문제점을 보완해서 개발자 대신 적절한 SQL을 생성하고 데이터 베이스를 조작해서 객체를 자동 매핑하는 역할을 수행한다.  

JPA 기반의 구현체는 대표적으로 하이버네이트, 이클립스 링크, 데이터 뉴클리어스가 있으며 그중 가장 많이 사용되는 구현체는 하이버네이트다.
## 하이버네이트

하이버네이트는 자바의 ORM 프레임워크로 JPA가 정의하는 인터페이스를 구현하고 있는 JPA 구현체중 하나다. 하지만 스프링에서는 Spring Data JPA라고 해당 기능을 편하게 사용하도록 모듈화해서 JPA를 직접 사용할 일은 잘 없다.

### Spring Data JPA

Spring Data JPA 는 CRUD 처리에 필요한 인터페이스를 제공하며, 하이버네이트의 엔티티 매니저를 직접 다루지 않고 리포지토리를 정의해 사용함으로써 스프링이 적합한 쿼리를 동적으로 생성하는 방식으로 데이터를 조작한다.  

![](https://i.imgur.com/MiGIzrh.png)

## 영속성 컨텍스트

영속성 컨텍스트 (Persistence Context) 는 애플리케이션과 데이터베이스 사이에서 엔티티와 레코드의 괴리를 해소하는 기능과 객체를 보관하는 기능을 수행한다. 앤티티 객체가 용속성 컨텍스트에 들어오면 JPA는 엔티티 객체의 매핑 정보를 데이터베이스에 반영하는 작업을 수행한다. 이처럼 앤티티 객체가 영속성 컨텍스트에 들어와 JPA의 관리 대상이 되는 시점부터는 해당 객체를 영속 객체(Persistence Object)라고 부른다.  

![](https://i.imgur.com/aM8edsv.png)

영속성 컨텍스트는 세션 단위의 생명주기를 가진다. 데이터베이스에 접근하기 위한 세션이 생성되면 영속성 컨텍스트가 만들어지고, 세션이 종료되면 영속성 컨텍스트도 없어진다. 엔티티 매니저는 이러한 일련의 과정에서 영속성 컨텍스트에 접근하기 위한 수단으로 사용된다.

### 엔티티 매니저

엔티티 매니저(Entity Manager)는 이름 그대로 엔티티를 관리하는 객체다. 엔티티 매니저는 데이터베이스에 접근해서 CRUD 작업을 수행한다. Spring Data JPA 를 사용하면 리포지토리를 사용해서 데이터베이스에 접근하는데 아래와 같이 매니저를 사용한다.

![](https://i.imgur.com/93dZgVO.png)

엔티티 매니저는 앤티티 매니저 팩토리(EntityManagerFactory)가 만든다. 앤티티 매니저 팩토리는 데이터베이스에 대응하는 객체로서 스프링 부트에서는 자동 설정 기능이 있기 때문에 application.properties 에서 작성한 최소한의 설정만으로도 동작하지만 JPA의 구현체중 하나인 하이버네이트에서는 persistence.xml이라는 설정파일을 구성하고 사용해야하는 객체다

![](https://i.imgur.com/EAJzGzc.png)
![](https://i.imgur.com/akxSBnZ.png)

8번 줄과 같이 persistence-unit을 설정하면 해당 유닛의 이름을 가진 앤티티 매니저 팩토리가 생성된다. 앤티티 매니저 팩토리는 애플리케이션에서 단 하나만 생성되며 모든 엔티티가 공유해서 사용한다.  

앤티티 매니저 팩토리로 생성된 앤티티 매니저는 앤티티를 영속성 컨텍스트에 추가해서 영속 객체로 만드는 작업을 수행하고, 영속성 컨텍스트와 데이터베이스를 비교하면서 실제 데이터베이스를 대상으로 작업을 수행한다.  

### 엔티티의 생명주기

- 비영속(New)
	- 영속성 컨텍스트에 추가되지 않은 엔티티 객체의 상태를 의미한다.
- 영속(Managed)
	- 영속성 컨텍스트에 의해 엔티티 객체가 관리되는 상태다.
- 준영속(Detached)
	- 영속성 컨텍스트에 의해 관리되던 엔티티 객체가 컨텍스트와 분리된 상태다.
- 삭제(Removed)
	- 데이터베이스에서 레코드를 삭제하기 위해 영속성 컨텍스트에 삭제 요청을 한 상태다.

## 데이터베이스 연동

`applications.properties`

```applications.properties
spring.datasource.driverClassName=org.mariadb.jdbc.Driver  
spring.datasource.url=jdbc:mariadb://localhost:3306/springboot  
spring.datasource.username=flature  
spring.datasource.password=aroundhub12#  
  
spring.jpa.hibernate.ddl-auto=update  
spring.jpa.show-sql=true  
spring.jpa.properties.hibernate.format_sql=true
```

마지막 3개의 줄은 하이버네이트 활성화에 관한 설정이다. ddl-auto는 의미 그대로 데이터 베이스를 자동으로 조작하는 옵션이다.   

- create : 애플리케이션이 가동되고 SessionFactory가 실행될 때 기존 테이블을 지우고 새로 생성한다. 
- create-drop : create와 동일한 기능을 수행하나 애플리케이션을 종료하는 시점에 테이블을 지운다.
- update : SessionFactory 가 실행될 때 객체를 검사해서 변경된 스키마를 갱신한다. 기존에 저장된 데이터는 유지된다.
- validate : update 처럼 객체를 검사하지만 스키마는 건드리지 않는다. 검사 과정에서 데이터베이스의 테이블 정보와 객체의 정보가 다르면 에러가 발생한다.
- none : ddl-auto 기능을 사용하지 않는다.

운영 환경에서는 create, create-drop, update 기능을 사용하지 않는데 그 이유는 데이터베이스에 축적된 데이터를 지워버릴 수도 있고, 사람의 실수로 객체의 정보가 변경됐을 때 운영 환경의 데이터베이스 정보까지 변경될 수 있기 때문이다.  

운영 환경에서는 대체로 validate나 none을 사용하며 개발환경에서는 create 또는 update를 사용하는 편이다.

나머지 줄은 쿼리 출력문과 포매팅 설정이다.

## 엔티티 설계

Spring Data JPA를 사용하면 데이터베이스에 테이블을 생성하기 위해 직접 쿼리를 작성할 피룡가 없다. 이 기능을 가능하게 하는 게 엔티티다. JPA에서 엔티티는 데이터베이스의 테이블에 대응하는 클래스다.   
엔티티에는 데이터 베이스에 쓰일 테이블과 칼럼을 정의한다. 엔티티에 어노테이션을 사용하면 테이블간의 연관관계를 정의할 수 있다.

![](https://i.imgur.com/BU7vI2p.png)


```java
import java.time.LocalDateTime;  
import javax.persistence.Column;  
import javax.persistence.Entity;  
import javax.persistence.GeneratedValue;  
import javax.persistence.GenerationType;  
import javax.persistence.Id;  
import javax.persistence.Table;  
  
import lombok.*;  
  
@Entity  
@Getter  
@Setter  
@NoArgsConstructor  
@AllArgsConstructor  
@EqualsAndHashCode  
@ToString(exclude = "name")  
@Table(name = "product")  
public class Product {  
  
    @Id  
    @GeneratedValue(strategy = GenerationType.IDENTITY)  
    private Long number;  
  
    @Column(nullable = false)  
    private String name;  
  
    @Column(nullable = false)  
    private Integer price;  
  
    @Column(nullable = false)  
    private Integer stock;  
  
    private LocalDateTime createdAt;  
  
    private LocalDateTime updatedAt;  
  
}
```

application.properties 에 정의한 spring.jpa.hibernate.ddl-auto 의 값을 create 같ㅇ느 테이블을 생성하는 옵션으로 설정하면 쿼리문을 작성하지 않아도 데이터베이스에 테이블이 자동으로 만들어진다.  

- `@Entity`
	- 해당 클래스가 엔티티임을 명시하기 위한 어노테이션. 클래스 자체는 테이블과 일대일로 매칭되며 해당 클래스의 인스턴스는 매핑되는 테이블에서 하나의 레코드를 의미함
- `@Table`
	- 특별한 경우가 아니면 필요없다.
	- `@Table` 어노테이션을 사용할 떄는 클래스의 이름과 테이블의 이름을 다르게 지정해야 하는 경우다.
	- 이를 지정하지 않을 경우 테이블의 이름과 클래스의 이름이 동일하다는 의미며 서로 다른 이름을 사용하려면 `@Table(name = 값)` 형태로 데이터베이스의 테이블명을 명시해주면 도니다.
- `@Id`
	- 엔티티 클래스의 필드는 테이블의 칼럼과 매핑된다. `@Id` 어노테이션이 선언된 필드는 테이블의 기본값 역할로 사용된다. 모든 엔티티는 `@Id` 어노테이션이 꼭 필요하다.
- `@GeneratedValue`
	- 일반적으로 `@Id` 와 함께 사용된다. 해당 필드의 값을 어떤 방식으로 자동생성할지 결정할 때 사용 한다.
		- GeneratedValue를 사용하지 않음(직접 할당)
			- 애플리케이션에서 자체적으로 고유한 기본값을 생성할 경우 사용하는 방식
			- 내부에 정해진 규칙에 의해 기본값을 생성하고 식별자로 사용
		- AUTO
			- `@GeneratedValue`의 기본 설정값
			- 기본값을 사용하는 데이터베이승제 맞게 자동 생성한다.
		- IDENTITY
			- 기본값 생성을 데이터베이스에 위임하는 방식이다.
			- 데이터베이스의 AUTO_INCREMENT를 사용해 기본값을 생성한다.
		- SEQUENCE
			- `@SequenceGenerator` 어노테이션으로 식별자 생성기를 설정하고 이를 통해 값을 자동 주입받는다.
			- SequenceGenerator 를 정의할 때는 name, sequenceName, allocationSize를 활용한다.
			- `@GeneratedValue` 에 생성기를 결정한다.
		- TABLE
			- 어떤 DBMS를 사용하더라도 동일하게 동작하기를 원할 경우 사용한다.
			- 식별자로 사용할 숫자의 보관 테이블을 별도로 생성해서 엔티티를 생성할 때마다 값을 갱신하며 사용한다.
			- `@TableGenerator` 어노테이션으로 테이블 정보를 설정한다.
- `@Column`
	- 엔티티 클래스의 필드는 자동으로 테이블 칼럼으로 매핑된다.
		- name : 데이터베이스의 칼럼명을 설정하는 속성이다. 명시하지않으면 필드명으로 지정된다.
		- nullable : 레코드를 생성할 때 칼럼 값에 null 처리가 가능한지를 명시하는 속성이다.
		- length : 데이터베이스에 저장하는 데이터의 최대 길이를 설정한다.
		- unique : 해당 컬럼을 유니크로 설정한다.
- `@Transient`
	- 엔티티 클래스에는 선언돼 있는 필드지만 데이터베이스에서는 필요 없을 경우 이 어노테이션을 사용해 데이터 베이스에서 사용하지 않게 살 수 있다.
## 리포지토리 인터페이스 설계

Spring Data JPA는 JpaRepository 를 기반으로 더욱 쉽게 테이터베이스를 사용할 수 있는 아키텍처를 제공한다.
### 리포지토리 인터페이스 생성

지금 리포지토리(Repository)는 Spring Data JPA가 제공하는 인터페이스다. 엔티티를 데이터베이스의 테이블과 구조를 생성하는 데 사용했다면 리포지토리는 엔티티가 생성한 데이터베이스에 접근하는 데 사용된다.

```java
public interface ProductRepository extends JpaRepository<Product, Long> {

}
```

![](https://i.imgur.com/RtP07zn.png)

ProductRepository 가 JpaRepository 를 상속받을 때는 대상 엔티티와 기본값 타입을 지정해야 한다. 위와 같이 대상 엔티티를 Product로 설정하고 해당 엔티티의 `@Id` 필드 타입인 Long 을 설정하면 된다.  

또한 상속받은 쿼리문을 사용가능하다.  
![](https://i.imgur.com/H3UPZhq.png)

### 리포지토리 메서드의 생성 규칙

일반적으로 CRUD에서 따로 생성해서 사용하는 메서드는 대부분 Read부분에 해당하는 Select 쿼리 뿐이다.  
엔티티를 저장하거나 갱신 또는 삭제할 때는 별도의 규칙이 필요하지 않기 때문이다. 다만 리포지토리에서 기본적으로 제공하는 조회 메서드는 기본값으로 단일 조회하거나 전체 엔티티를 조회하는 것만 지원하고 있기 때문에 필요에 따라 다른 조회 메서드가 필요하다.   

- FindBy : SQL 문의 where 절 , findBy 뒤에 엔티티의 필드값을 입력해서 사용
	- findByName(String name)
- AND, OR : 조건을 여러 개 설정하기 위해 사용
	- `findByNameAndEmail(String name, String email)`
- Like / NotLike : SQL 문의 like와 동일한 기능을 수행, 특정 문자를 포함하는지의 여부를 조건으로 추가 -> 비슷한 키워드로 Containing, contains, isContaing 이 있다.
- StartWith / StartingWith : 특정 키워드로 시작하는 문자열 조건을 설정
- EndsWith / EndiingWith : 특정 키워드로 끝나는 문자열 조건을 설정
- IsNull / IsNotNull : 레코드 값이 Null이거나 Null이 아닌 값을 검색
- True / False : 타입의 레코드를 검색할 때 사용
- Before / After : 시간을 기준으로 값을 검색
- LessThan / GreaterThan : 특정 숫자 값을 기준으로 대소 비교
- Between : 두 개의 값(숫자) 사이의 데이터 조회
- OrderBy : SQL 문의 order by와 동일 기능
	- 예를 들어 가격순으로 이름을 조회한다면
	- `List<Product> findByNameOrderByPriceAsc(String name);`
- countBy : SQL 문의 count와 동일한 기능 


## DAO 설계

DAO (Data Access Object) 는 데이터베이스에 접근하기 위한 로직을 관리하기 위한 객체이다. 데이터를 조작하는 기능은 이 DAO 객체가 수행한다. 다만 스프링 데이터 JPA에서 DAO의 개념은 리포지토리가 대체하고 있다.  

실제 업무에 필요한 비즈니스 로직 개발에서는 데이터를 다루는 중간 계층을 두는 게 유지보수 측면에서 용이한 경우가 많다.   

객체지향적인 설계에서는 서비스와 비즈니스 레이어를 분리하고 서비스 레이어에서는 서비스 로직을 수행하고 비즈니스 레이어에서는 비즈니스 로직을 수행해야 한다는 의견도 많다고 한다.  

> DAO vs 리포지토리
> - DAO와 리포지토리는 역할이 비슷하다. 그렇기 때문에 논쟁도 많지만 실제로 리포지토리는 Spring Data JPA에서 제공하는 기능이기 때문에 기존의 스프링 프레임워크나 스프링 MVC의 사용자는 리포지토리라는 개념을 사용하지 않고 DAO 객체로 데이터베이스에 접근했다. 

### DAO 클래스 생성

DAO 클래스는 일반적으로 `인터페이스-구현체` 로 생성한다. DAO 클래스는 의존성 결합을 낮추기 위한 디자인 패턴이며 서비스 레이어에 DAO 객체를 주입받을 때 인터페이스를 선언하는 방식으로 구성할 수 있다.

![](https://i.imgur.com/x0bgkcU.png)

```java
package com.springboot.jpa.data.dao;  
  
import com.springboot.jpa.data.entity.Product;  
  {  
  
    Product insertProduct(Product product);  
  
    Product selectProduct(Long number);  
  
    Product updateProductName(Long number, String name) throws Exception;  
  
    void deleteProduct(Long number) throws Exception;  
  
}
```

일반적으로 데이터베이스에 접근하는 메서드는 리턴 값으로 데이터 객체를 전달한다. 이때 데이터 객체를 엔티티 객체로 전달할지, DTO 객체로 전달할지에 대해서는 개발자마다 의견이 다르다.   

일반적인 설계 원칙에서 엔티티 객체는 데이터베이스에 접근하는 계층에서만 사용하도록 정의한다. 다른 계층으로 데이터를 전달할 때는 DTO 객체를 사용하지만 회사나 부서마다 의견 차이가 있으므로 기존에 정해진 원칙에 따라 진행하는게 좋다.  

```java
 
public class ProductDAOImpl implements ProductDAO {  
  
    private ProductRepository productRepository;  
  
    @Autowired  
    public ProductDAOImpl(ProductRepository productRepository) {  
        this.productRepository = productRepository;  
    }  
  
    @Override  
    public Product insertProduct(Product product) {  
        Product savedProduct = productRepository.save(product);  
  
        return savedProduct;  
    }  
  
    @Override  
    public Product selectProduct(Long number) {  
        Product selectedProduct = productRepository.getById(number);  
  
        return selectedProduct;  
    }  
  
    @Override  
    public Product updateProductName(Long number, String name) throws Exception {  
        Optional<Product> selectedProduct = productRepository.findById(number);  
  
        Product updatedProduct;  
        if (selectedProduct.isPresent()) {  
            Product product = selectedProduct.get();  
  
            product.setName(name);  
            product.setUpdatedAt(LocalDateTime.now());  
  
            updatedProduct = productRepository.save(product);  
        } else {  
            throw new Exception();  
        }  
  
        return updatedProduct;  
    }  
  
    @Override  
    public void deleteProduct(Long number) throws Exception {  
        Optional<Product> selectedProduct = productRepository.findById(number);  
  
        if (selectedProduct.isPresent()) {  
            Product product = selectedProduct.get();  
  
            productRepository.delete(product);  
        } else {  
            throw new Exception();  
        }  
    }  
}
```

`ProductDAOImpl` 클래스를 스프링이 관리하는 빈으로 등록하려면 `@Component` 또는 `@Service` 를 지정해야한다. 빈으로 등록된 객체는 다른 클래스가 인터페이스를 가지고 의존성을 주입받을 때 이 구현체를 찾아 주입하게 된다.   

마찬가지로 DAO 객체에서도 데이터베이스에 접근하기 위해 리포지토리 인터페이스를 사용해 의존성을 주입받아야한다.

```java
private ProductRepository productRepository;  
  
@Autowired  
public ProductDAOImpl(ProductRepository productRepository) {  
    this.productRepository = productRepository;  
}
```

이렇게 리포지토리를 정의하고 생성자를 통해 의존성을 주입 받으면 된다.   

`selectProduct()` 메서드가 사용한 리포지토리의 메서드는 `getById()` 다.  

- `getById()`
	- 내부적으로 `EntityManager` 의 `getReference()` 메서드를 호출한다. `getReference()` 메서드를 호출하면 프락시 객체를 리턴한다. 실제 퀴리는 프락시 객체를 통해 최초로 데이터에 접근하는 시점에 실행된다. 이때 데이터가 존재하지 않는 경우에는 `EntityNotFoundException` 이 발생한다. 

```Java
@Override
public T getById(ID id){
	Assert.notNull(id, ID_MUST_NOT_BE_NULL);
	return em.getReference(getDomainClass(), id);
}
```

- `findById()`
	- 내부적으로 `EntityManager` 의 `find()` 메서드를 호출한다. 이 메서드는 영속성 컨텍스트의 캐시에서 값을 조회한 후 영속성 컨텍스트에 값이 존재하지 않는다면 실제 데이터베이스에 데이터를 조회한다. 리턴 값으로 `Optional` 객체를 전달한다.

```java
@Override
public Optional<T> findById(ID id){

	Assert.notNull(id, ID_MUST_NOT_BE_NULL);
	Class<T> domainType = getDomainClass();

	if (metadata == null){
		return Optional.ofNullable(em.find(domainType, id));
	}

	LockModeType type = metadata.getLockModeType();

	Map<String, Object> hints = new HashMap<>();
	getQueryHints().withFetchGraphs(em).forEach(hints::put);

	return Optional.ofNullable(type == null ? em.find(domainType, id, hints) : em.find(domainType, id, type, hints));

}
```

### UpdateProductName() 

```java
    @Override  
    public Product updateProductName(Long number, String name) throws Exception {  
        Optional<Product> selectedProduct = productRepository.findById(number);  
  
        Product updatedProduct;  
        if (selectedProduct.isPresent()) {  
            Product product = selectedProduct.get();  
  
            product.setName(name);  
            product.setUpdatedAt(LocalDateTime.now());  
  
            updatedProduct = productRepository.save(product);  
        } else {  
            throw new Exception();  
        }  
  
        return updatedProduct;  
    }  
```

JPA는 값을 갱신할 때 update라는 키워드를 사용하지 않는다. 여기서는 영속성 컨텍스트를 활용해 값을 갱신하는데, `find()` 메서드를 통해 데이터베이스에서 값을 가져오면 가져온 객체가 영속성 컨텍스트에 추가된다. 영속성 컨텍스트가 유지되는 상황에서 객체의 값을 변경하고 다시 `save()` 를 실행하면 JPA에서는 더티 체크(Dirty Check) 라고 하는 변경 감지를 수행한다.  

```java
@Transactional
@Override
public <S extends T> S save(S entity){
	Assert.notNull(entity, "Entity must not be null.");

	if (entityInformation.isNew(entity)){
		em.persist(entity);
		return entity;
	} else {
		return em.merge(entity);
	}
}
```

`@Transactional` 이 지정되어 있으면 매서드 내 작업을 마칠 경우 자동으로 `flush()` 실행한다. 이 과정에서 변경이 감지되면 대상객체에 해당하는 데이터베이스의 레코드를 업게이트하는 쿼리가 실행된다.    

### deleteProduct() 

```java
@Override  
public void deleteProduct(Long number) throws Exception {  
    Optional<Product> selectedProduct = productRepository.findById(number);  
  
    if (selectedProduct.isPresent()) {  
        Product product = selectedProduct.get();  
  
        productRepository.delete(product);  
    } else {  
        throw new Exception();  
    }  
}
```

데이터베이스의 레코드를 삭제하기 위해서는 삭제하고자 하는 레코드와 매핑된 영속 객체를 영속성 컨텍스트에 가져와야한다. `deleteProduct()` 메서드는 `findById()` 메서드를 통해 객체를 가져오는 작업을 수행하고 `delete()` 메서드를 통해 해당 객체를 삭제하게끔 삭제 요청을 한다.  

```java
@Override
@Transactional
@SuppressWarnings("unchecked")
public void delete(T entity) {

	Assert.notNull(entity, "Entity must not be null!!");

	if (entityInformation.isNew(entity)){
		return;
	}

	Class<?> type = ProxyUtils.getUserClass(entity);
	T existing = (T) em.find(type, entityInformation.getId(entity));
	// if the entity to be deleted doesn't exist, delete is a NOOP
	if (existing == null){
		return;
	}
	em.remove(em.contains(entity) ? entity:em.merge(entity));
}
```

SimpleJpaRepository의 `delete()` 메서드는 `em.remove(em.contains(entity) ? entity:em.merge(entity));` 에서 `delete()` 메서드로 전달받은 엔티티가 영속성 컨텍스트에 있는지 파악하고, 해당 엔티티를 영속성 컨텍스트에 영속화하는 작업을 거쳐 데이터베이스의 레코드와 매핑한다. 그렇게 매핑된 영속 객체를 대상으로 삭제 요청을 수행하는 메서드를 실행해 작업을 마치코 커밋단계에서 삭제를 진행한다.
## DAO 연동을 위한 컨트롤러와 서비스 설계

위에서 설계한 구성 요소들을 클라이언트의 요청과 연결을 위해 컨트롤러와 서비스를 생성해야한다.   
이를 위해 먼저 DAO의 메서드를 호출하고 그 외 ㅣㅂ즈니스 로직을 수행하는 서비스 레이어를 생성한 후 컨트롤러를 생성한다.

### 서비스 클래스 만들기

서비스 레이어에서는 도메인 모델을 활용해 애플리케이션에서 제공하는 핵심 기능을 제공한다. 핵심기능을 구현하려면 세부 기능을 정의해야한다.    
세부기능이 모여 핵심기능이 된다.    

도메인을 활용한 세부 기능들은 비즈니스 레이어의 로직에서 구현하고, 서비스 레이어에서는 기능들을 종합해서 핵심 기능을 전달하도록 구성하는 경우가 대표적이다.

> 서비스 레이어에서 비즈니스 로직을 처리하는 아키텍처로 ㄱㄱ

![](https://i.imgur.com/mzscgse.png)

```java
@Data  
@NoArgsConstructor  
@AllArgsConstructor  
@ToString  
@Builder  
public class ProductDto {  
  
    private String name;  
  
    private int price;  
  
    private int stock;  
  
}
```

> 빌드메서드
> - 빌드 메서드는 빌더(Builder) 패턴을 따르는 메서드다. 데이터 클래스를 사용할 때 생성자로 초기화할 경우 모든 필드에 값을 넣거나 null을 명시적으로 사용해야한다. 이러한 단점을 보완하기 위해 나온 패턴이 빌드패턴이며, 이패턴을 이용하면 필요한 데이터만 설정할 수 있어 유연성을 확보할 수 있다. 


![](https://i.imgur.com/UJkCKqZ.png)
![](https://i.imgur.com/gFonCE8.png)

기본적인 CRUD의 기능을 호출하기 위해 간단한 메서드를 만들어준다.   

```java
public interface ProductService {  
  
    ProductResponseDto getProduct(Long number);  
  
    ProductResponseDto saveProduct(ProductDto productDto);  
  
    ProductResponseDto changeProductName(Long number, String name) throws Exception;  
  
    void deleteProduct(Long number) throws Exception;  
  
}
```

서비스에서는 클라이언트가 요청한 데이터를 절적하게 가공해 컨트롤러에게 넘기는 역할을 한다.  
위의 코드를 보면 리턴 타입이 DTO 객체다. DAO 객체에서는 엔티티 타입을 사용하는 것을 고려하면 서비스 레이어에서 DTO 객체와 엔티티 객체를 각 레이어에 변환해서 전달하는 역할도 수행한다고 볼 수 있다. 다만 이부분은 얼마든지 바뀔 수 있다.  

데이터베이스와 밀접한 관련이 있는 데이터 엑세스 레이어까지는 엔티티 객체를 사용하고, 클라이언트와 가까워지는 다른 레이어에서는 데이터를 교환하는 데 DTO 객체를 사용하는 것이 일반적이다.  

![](https://i.imgur.com/YWPM5GE.png)

DAO의 사이에서 엔티티 데이터를 전달하는 것으로 표현했지만 회사나 개발 그룹 내 규정에 따라 DTO를 사용하기도 한다. 위 구조는 각 레이어 사이의 큰 데이터의 전달을 표현된 거고 단일 데이터나 소량의 데이터를 전달하는 경우 DTO나 엔티티를 사용하지 않기도 한다.  

```java
@Service  
public class ProductServiceImpl implements ProductService {  
  
    private final ProductDAO productDAO;  
  
    @Autowired  
    public ProductServiceImpl(ProductDAO productDAO) {  
        this.productDAO = productDAO;  
    }  
  
    @Override  
    public ProductResponseDto getProduct(Long number) {  
        Product product = productDAO.selectProduct(number);  
  
        ProductResponseDto productResponseDto = new ProductResponseDto();  
        productResponseDto.setNumber(product.getNumber());  
        productResponseDto.setName(product.getName());  
        productResponseDto.setPrice(product.getPrice());  
        productResponseDto.setStock(product.getStock());  
  
        return productResponseDto;  
    }  
  
    @Override  
    public ProductResponseDto saveProduct(ProductDto productDto) {  
        Product product = new Product();  
        product.setName(productDto.getName());  
        product.setPrice(productDto.getPrice());  
        product.setStock(productDto.getStock());  
        product.setCreatedAt(LocalDateTime.now());  
        product.setUpdatedAt(LocalDateTime.now());  
  
        Product savedProduct = productDAO.insertProduct(product);  
  
        ProductResponseDto productResponseDto = new ProductResponseDto();  
        productResponseDto.setNumber(savedProduct.getNumber());  
        productResponseDto.setName(savedProduct.getName());  
        productResponseDto.setPrice(savedProduct.getPrice());  
        productResponseDto.setStock(savedProduct.getStock());  
  
        return productResponseDto;  
    }  
  
    @Override  
    public ProductResponseDto changeProductName(Long number, String name) throws Exception {  
        Product changedProduct = productDAO.updateProductName(number, name);  
  
        ProductResponseDto productResponseDto = new ProductResponseDto();  
        productResponseDto.setNumber(changedProduct.getNumber());  
        productResponseDto.setName(changedProduct.getName());  
        productResponseDto.setPrice(changedProduct.getPrice());  
        productResponseDto.setStock(changedProduct.getStock());  
  
        return productResponseDto;  
    }  
  
    @Override  
    public void deleteProduct(Long number) throws Exception {  
        productDAO.deleteProduct(number);  
    }  
}
```

DAO 인터페이스를 선언하고 `@Autowired` 를 지정한 생성자를 통해 의존성을 주입받는다. 그리고 인터페이스에서 정의한 메서드를 오버라이딩한다.

```java
    private final ProductDAO productDAO;  
  
    @Autowired  
    public ProductServiceImpl(ProductDAO productDAO) {  
        this.productDAO = productDAO;  
    }  
```

DTO 객체를 생성하고 값을 넣어 초기화하는 작업을 수행하는데 이런 부분은 빌더패턴을 활용하거나 엔티티 객체나 DTO 객체 내부에 변환하는 메서드를 추가해서 간단하게 전환할 수 있다.   

저장 메서드에서는 전달받은 DTO 객체를 통해 엔티티 객체를 생성해서 초기화한 후 DAO 객체로 전달하면 된다. 다만 저장 메서드의 리턴 타입을 어떻게 ㅈ지정할지는 고민해야 한다. 일반적으로 저장 메서드는 void 타입으로 작성하거나 잡업의 성공 여부를 나타내는 boolean 타입으로 지정하는 경우가 많다.   

데이터를 조회하는 메서드에서는 테이터베이스의 인덱스를 통해 값을 찾아야하는데 void로 저장 메서드를 구현하면 클라이언트가 저장한 데이터의 인덱스 값을 알 수 없다. 그래서 데이터를 저장하면서 가져온 인덱스를 DTO에 담아 결괏값으로 클라이언트에 전달하는 코드를 작성해줘야한다.  

만약 이 같은 방식이 아니라 void 형식으로 메서드를 작성하면 조회 메서드를 추가로 구현하고 클라이언트에서 한 번 더 요청한다.  

`changeProductName()` 에서 상품정보 중 이름을 변경하는 작업을 수행한다. 이름을 변경하기 위해 먼저 클라이언트로부터 대상을 식별할 수 있는 인덱스 값과 변경하려는 이름을 받아온다. 좀 더 견고하게 코드를 작성하기 위해서는 기존 이름도 받아와 식별자로 가져온 상품정보와 일치하는지 검증하는 단계를 추가하기도 한다.  

이 기능의 핵심이 되는 비즈니스 로직은 레코드의 이름 칼럼을 변경하는 점이다. 실제 레코드 값을 변경하는 작업은 DAO에서 진행하기 때문에 서비스 레이어에서는 해당 메서드를 호출해서 결괏값만 받아온다.  

상품정보를 삭제하는 메서드는 리포지토리에서 제공하는 `delete()` 메서드를 사용할 경우 리턴받는 타입이 지정돼 있지 않기 때문에 리턴 타입을 void로 지정해 메서드를 구현해야한다.   

### 컨트롤러 생성

```java
@RestController  
@RequestMapping("/product")  
public class ProductController {  
  
    private final ProductService productService;  
  
    @Autowired  
    public ProductController(ProductService productService) {  
        this.productService = productService;  
    }  
  
    @GetMapping()  
    public ResponseEntity<ProductResponseDto> getProduct(Long number) {  
        ProductResponseDto productResponseDto = productService.getProduct(number);  
  
        return ResponseEntity.status(HttpStatus.OK).body(productResponseDto);  
    }  
  
    @PostMapping()  
    public ResponseEntity<ProductResponseDto> createProduct(@RequestBody ProductDto productDto) {  
        ProductResponseDto productResponseDto = productService.saveProduct(productDto);  
  
        return ResponseEntity.status(HttpStatus.OK).body(productResponseDto);  
    }  
  
    @PutMapping()  
    public ResponseEntity<ProductResponseDto> changeProductName(  
            @RequestBody ChangeProductNameDto changeProductNameDto) throws Exception {  
        ProductResponseDto productResponseDto = productService.changeProductName(  
                changeProductNameDto.getNumber(),  
                changeProductNameDto.getName());  
  
        return ResponseEntity.status(HttpStatus.OK).body(productResponseDto);  
  
    }  
  
    @DeleteMapping()  
    public ResponseEntity<String> deleteProduct(Long number) throws Exception {  
        productService.deleteProduct(number);  
  
        return ResponseEntity.status(HttpStatus.OK).body("정상적으로 삭제되었습니다.");  
    }  
  
}
```

컨트롤러는 클라이언트로부터 요청을 받고 해당 요청에 대해 서비스 레이어에 구현된 적절한 메서드를 호출해서 결과 값을 받는다. 이처럼 컨트롤러는 요청과 응답을 전달하는 역할만 맡는 게 좋다.

> ChangeProductNameDto 

```java
public class ChangeProductNameDto {  
  
    private Long number;  
  
    private String name;  
  
    public ChangeProductNameDto(Long number, String name) {  
        this.number = number;  
        this.name = name;  
    }  
  
    public ChangeProductNameDto() {  
    }  
  
    public Long getNumber() {  
        return this.number;  
    }  
  
    public String getName() {  
        return this.name;  
    }  
  
    public void setNumber(Long number) {  
        this.number = number;  
    }  
  
    public void setName(String name) {  
        this.name = name;  
    }  
  
}
```
## 롬복

### 생성자 자동 생성 어노테이션

- `@NoArgsConstructor` : 매개변수가 없는 생성자를 자동 생성
- `@AllArgsConstructor` : 모든 필드를 매개변수로 갖는 생성자를 자동으로 생성한다.
- `@RequiredArgsConstructor` : 필드 중 final 이나 `@NotNull` 이 설정된 변수를 매개변수로 갖는 생성자를 자동으로 생성한다.

### @EqualsAndHashCode

`@EqualsAndHashCode` 는 객체의 동등성(Equality) 와 동일성(Identity)을 비교하는 연산 메서드를 생성한다.

- equals : 두 객체의 내용이 같은지 동등성을 비교한다.
- hashCode : 두 객체가 같은 객체인지 동일성을 비교한다.

만약 부모클래스가 있어서 상속을 받는 상황이라면 부모 클래스의 필드까지 비교할 필요가 있는 경우가 발생한다. 이 경우에는 `@EqualsAndHashCode` 에서 제공하는 `callSuper` 속성을 설정하면 부모 클래스의 필드를 비교 대상에 포함할 수 있다.

```java
@Entity
@EqualsAndHashCode(callSuper = ture)
@Table(name = "product")
public class Product extends BaseEntity{
	...
	..
	.
}
```

`callSuper` 의 기본 값은 false 이며, true 일 경우 부모 객체의 값도 비교 대상에 포함된다.

> 동등성과 동일성
> - equals와 hashCode를 공부할 때 반드시 나오는 개념으로 동등성(equality)과 동일성(identity)이 있다. 동등성은 비교대상이 되는 구 객체가 가진 값이 같음을 의미하고, 동일성은 두 객체가 같은 객체임을 의미한다.
> - 두 메서드는 일반적으로 클래스 단위의 객체를 비교하는 데 사용하고 Object 클래스의 메서드를 오버라이딩해 구현한다.
> - 동등성과 동일성이 원시(primitive) 타입의 자료형과 레퍼런스(reference) 타입의 자료형에서 어떻게 사용되는지 알아보면 자바를 이해하는데 도움이 된다다. 그 중 String 타입은 특별한 사례다. String은 레퍼런스 타입이지만 원시 타입처럼 사용된다.
> - 동일성 Idenity -> 메모리 내 주소값이 같은지 비교
> - 동등성 Equality -> 논리적 지위가 동등한지 비교 -> 두 개의 객체가 같은 정보를 갖고 있음 -> 객체의 주소가 다르더라도 내용만 같으면 두 변수는 동등하다고 판별


### @Data

`@Data` 는 `@Getter/@Setter, @RequiredArgsConstructor, @ToString, @EqualsAndHashCode` 를 모두 포괄한다.  




