---
layout: single
title: "[Database] Relational data model"
categories: Database
tags:
  - Database
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
# 관계형 데이터 모델

## 데이터 모델링

![](https://i.imgur.com/7zqaFmC.png)


### 데이터 모델링 3단계
- 개념적 데이터 모델링
	- 현실세계를 추상화하여 중요 데이터를 개념 세계로 추출해 가능 과정
	- 결과물로 개념적 데이터 모델 (객체 - 관계 (E-R) 모델)
		- ERD
- 논리적 데이터 모델링
	- 개념 세계의 데이터를 데이터베이스가 저장할 구조로 변환하는 과정
	- 결과물로 관계 데이터 모델
- 물리적 데이터 모델링
	- 논리 데이터 모델이 실제 데이터베이스 저장소에 저장되는 저장 구조
	- (테이블, 컬럼)로 변경

![](https://i.imgur.com/ZDXvouy.png)

## 관계 데이터 모델

- 개체에 대한 데이터를 저장하는 논리적 구조 - 릴레이션 (2차원의 테이블 구조)
![](https://i.imgur.com/S4Eq9Jd.png)

- null 은 비어있는 empty 개념이 아님

### 릴레이션의 특성

- 튜플의 유일성 : 동일한 튜플이 존재할 수 없다.
- 튜플의 무순서 : 튜플 사이의 순서는 무의미
- 속성(attribute) 의 무순서 : 속성 사이의 순서는 무의미
- 속성의 원자성(atomic) : attribute 값으로 하나 (나누어지지 않는) 값만 가짐


### Key

![](https://i.imgur.com/D2U0oi9.png)

- 릴레이션에 튜플을 구별하는 역할을 하는 속성 또는 속성의 집합
- Super key: 튜플을 구별하기 위해 유일성을 제공할 수 있는 속성 또는 속성ㅈ의 집합
	- {ID}, {ID,name}
- Candidate key : super key 중에서 개수가 가장 적은 키
	- {ID}
- Primary key : candidate key 중에서 디자인을 고려하여 선택된 키 (관계를 고려하거나 데이터 타입을 고려했을 때 가장 적당한 키)
- Foreign key : 다른 릴레이션의 primary key 를 참조하는 속성 또는 속성의 집합

### 관계 데이터 모델의 제약 조건

- 도메인 무결성 제약조건(domain inegrity constraint)
	- 릴레이션 내의 튜플들이 각 속성의 도메인에 지정된 값만 가져야한다.
- 개체 무결성 제약조건(entity integrity constraint = primary key constraint)
	- 기본키를 구성하는 모든 속성은 null을 가질 수 없다.
- 참조 무결성 제약조건 (referential integrity constraint = foreign key constraint)
	- Foreign key는 참조하는 릴레이션의 primary key 속성 값 중 하나여야 한다 (null 가능)


## 관계 개수

모든 DBMS 는 데이터 처리를 위해 하나 이상의 데이터 언어를 제공     

**Formal query language**
- 수학기호(notation)을 사용하여 데이터 처리를 기술한 언어
- 새로운 언어의 개념과 유용성을 검증하는 기준
- 관계 대수 (Relational algebra)

**Commercial language**
- 수학적인 원리를 기반으로 사용하기 쉽게 만들어진 언어
- 관계 대수를 만들어진 모든 질의가 표현 가능 -Relationally complete
- SQL

### 관계 대수 연산자 (Relational Algebra Operations)
- 피연산자로 하나 또는 두 개의 릴레이션 (Unary and binary operations)
- 각 연산자의 연산 결과는 새로운 릴레이션-연산의 합성(compose)&체이닝(chaining) 가능

### 연산자 종류
- select 
- project 
- union 
- difference 
- instersection 
- cartesian product 
- natural join
- theta join
- outer join

![](https://i.imgur.com/hwdCeI6.png)

[참고 출처](https://inpa.tistory.com/entry/DB-%F0%9F%93%9A-%EA%B4%80%EA%B3%84-%EB%8C%80%EC%88%98-%EA%B4%80%EA%B3%84-%ED%95%B4%EC%84%9D-SQL-%F0%9F%95%B5%EF%B8%8F-%EC%A0%95%EB%A6%AC)

해당 내용이 아직 확실히 와 닿지는 않지만 다음부터 SQL에 적용하면서 어떻게 작동되는지 생각하면서 공부하면 딱 좋을 거 같다.