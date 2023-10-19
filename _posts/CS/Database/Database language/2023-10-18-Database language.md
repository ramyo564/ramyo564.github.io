---
layout: single
title: "[Database] Database language"
categories: Database
tags:
  - Database
toc: true
toc_sticky: true
author_profile: false
sidebar:
---
# SQL 데이터베이스 언어

## SQL 분류

- DDL (data definition language)
	- 테이블을 생성하고 변경, 제거하는 기능을 제공
- DML (data manipulation language)
	- 테이블에 새 데이터를 삽입하거나, 테이블에 저장된 데이터를 수정, 삭제하는 기능을 제공
- SELECT
	- 테이블 데이터를 조회하는 기능을 제공
- DCL (data control language)
	- 보안을 위해 데이터에 대한 접근 및 사용권한을 조절하는 기능을 제공


![](https://i.imgur.com/3o6XQqJ.png)

## DDL 데이터 정의
- 데이터베이스 구조를 정의하고 변경하는 기능을 제공하는 언어
- CREATE: 새로운 데이터베이스 오브젝트들을 생성 (schema, table view 등)
- ALTER: 존재하는 오브젝트의 정의를 변경
- DROP: 존재하는 오브젝트를 데이터베이스에서 삭제

### 데이터 타입

![](https://i.imgur.com/3dMz7kE.png)

![](https://i.imgur.com/bIwpN3H.png)

### Create table syntax

![](https://i.imgur.com/zSj3HtP.png)

- default clause
	- `<default value>:= DEFAULT<value>`
		- INSERT 문에 컬럼에 대한 값이 지정되지 않았을 때 null 대신에 `<value>` 사용
		- credit INT DEFAULT 0
		- contact char(20) DEFAULT 'james'
- constraint clause
	- `<integrity constraint>:={UNIQUE|<primary key>|<reference constraint>|<check constraint>}`
		- UNIQUE : 대체 키를 지정하고 유일성을 가지는 컬럼임을 지정
		- PRIMARY KEY : ROW를 식별할 수 있는 기본 키 (unique + not null)
		- `<reference constraint>`: 참조하는 테이블을 지정하는 외래키 설정 
		- `<check constraint>`: column 값의 기능 domain 지정
- CREATE TABLE LIKE
	- `CREATE TABLE<table name> <like_table_clause> <like_table_clause>:= LIKE<table_name>[WITH DATA]`
		- ex) CREATE TABLE customer_backup LIKE customer WITH DATA;
		- 기존의 테이블을 복사
- ALTER TABLE
	- 새로운 컬럼 추가
		- `ALTER TABLE<table_name>ADD<column_name><column_type>[not null][<default_value>]`
			- ex) ALTER TABLE customer ADD reg_data DATE;
	- 기존 컬럼 삭제
		- `ALTER TABLE <table_name> DROP COLUMN <column_name>`
			- ex) ALTER TABLE customer drop age;
	- 새로운 제약조건 추가
		- `ALTER TABLE <table_name> ADD CONSTRAINT <constraint_name><contraints>`
			- ALTER TABLE customer ADD CONSTRAINT set_pri_key PRIMARY KEY (id)
	- 제약조건 삭제
		- `ALTER TABLE <table_name> DROP CONTRAINT set_pri_key;`
- DROP TABLE
	- 테이블 데이터 및 catalog 삭제
	- `DROP TABLE <table_name>`

## SELECT 데이터 조회
- SELECT
	- 테이블에서 원하는 데이터를 검색하는 SQL
		- Syntax
			- `SELECT [DISTINCT]{*|<column_name>,...} FROM <table list>;`
		- Example
			- `SELECT DISTINCT dept_name from instructor;`
				- DISTNCT : 결과 테이블에서 중복된 레코드를 제거하는 키워드
		- SELECT clause 는 산술식 (arithmetic expression) 을 포함 가능
			- `+,-, * and /`
			- SELECT ID, name, salary/12 from employee;
- with WHERE
				- Syntax
					- `SELECT [DISTINCT]{*|<column_name>,...} FROM <table list>[WHERE condition];`
					- Condition은 비교 연산자 `(=,<>,<,>,<=,=>)` 와 논리 연산자 (AND, OR, NOT)
				- Example
					- `SELECT name from employee WHERE dept_name = 'SALES' and salary > 100000;`
				- LIKE keyword
					- 문자열 컬럼인 경우 부분적인 매칭으로 조건 검색 가능
					- `%` : 0개 이상의 문자 , `_` : 1개의 문자
						- `SELECT * from employee where name like '김%';`
						- `SELECT * from employee where name like '김정_';`
- NULL Value
	- null은 empty 가 아닌 unknown
	- null 값과 산술 연산 및 비교 연산 결과는 null
		- 5 + null returns null
		- null > 5 returns null
		- null = null returns null
	- null 과 논리 연산 결과
		- OR : (null OR true) = ture, (null or false) = null, (null or null) = null
		- AND : (true AND null) = null, (false AND null) = false, (null and null) = null
		- NOT : (NOT null) = null
	- null 값을 가진 row을 찾기 위한 조건식 - IS NULL
		- SELECT id, name, FROM customer WHERE age IS NULL;
- SELECT 의 검색 결과를 정렬하기 위해 ORDER BY 키워드 사용
	- Syntax
		- `SELECT[DISTINCT]{*|<column_name>,...} FROM <table list> [WHERE condition][ORDER BY <column_name>,...[ASC|DESC]];`
		- ASC: 오름 차순 정렬 (default), DESC: 내림 차순 정렬
	- Example
		- SELECT id, name, age, address FROM customer ORDER BY age DESC;
		- SELECT id, name, dept_name, salary FROM employee ORDER BY salary;
		- SELECT id, product, quantity, order_date FROM sales_orders ORDER BY order_date ASC, quantity DESC;
- Aggregation function
	- 특정 컬럼의 값을 통계적으로 계산한 결과를 보여주는 SQL 함수
		- COUNT : 컬럼 값의 개수 (중복된 것도 모두 셈)(중복된 값을 없애려면 distinct를 앞에 써서 걸러야함)
		- MAX : 컬럼 값의 최대값
		- MIN : 컬럼 값의 최소값
		- SUM : 컬럼 값의 합계
		- AVG : 컬럼 값의 평균
	- COUNT, MAX, MIN 은 LOB 타입을 제외한 모든 타입에서 사용 가능 SUM, AVG는 숫자 데이터만 가능
		- SELECT sum (salary) FROM employee WHERE dept_name = 'SALES';
		- SELECT count (distinct id) FROM sales_orders WHERE product = 'computer';
- Null value in aggregation function
	- SELECT sum (salary) FROM employee;
		- 만약 컬럼 값이 null 이면 합을 계산할 때 무시
		- 만약 컬럼 값이 모두 null 이면 null 이 출력
	- MAX, MIN, SUM, AVG 경우 null 이 아닌 값으로만 계산
		- 모두 null 인 경우 null을 리턴
	- COUNT : null 이 아닌 값의 개수 출력
		- 컬럼이 모두 null 인 경우 0 리턴
	- 테이블에 레코드가 없는 경우
		- sum, avg, min, max : return null
		- count : return 0
- GROUP BY
	- 테이블에서 특정 컬럼의 값이 같은 rows를 모아 그룹을 만들고, 그룹별로 검색하기 위해 사용되는 keyword
	- 그룹에 대한 조건을 추가하려면 HAVING 키워드를 이용
		- WHERE 조건 : 레코드를 grouping 하기 전에 조건을 검색
		- Having 조건 : 레코드를 grouping 후에 그룹에 대한 조건을 검색
	- Syntax
		- `SELECT [DISTINCT] {*|<column_name>,...} FROM <table_list> [WHERE condtion] [GROUP BY <column_name list> [HAVING condition]] [ORDER BY <column_name list> [ASC|DESC]]`
	-  Aggregation function 을 제외한 SELECT 문의 컬럼은 GROUP BY 리스트에 포함된 컬럼들만 가능
		- SELECT dept_name, ID, avg(salary) from employee group by dept_name;
			- Return error: SELECT list is not in GROUP BY clause and contains nonaggregated column

![](https://i.imgur.com/IrGVJNQ.png)

### set

![](https://i.imgur.com/jPWQg2s.png)

![](https://i.imgur.com/sCsbQZj.png)

![](https://i.imgur.com/Ddvh7hR.png)

### join

![](https://i.imgur.com/a16kC0E.png)

![](https://i.imgur.com/Kxvmcbz.png)

![](https://i.imgur.com/0S0vhJx.png)

#### outer join

![](https://i.imgur.com/Ptvdyhj.png)


![](https://i.imgur.com/qHzMDxG.png)

![](https://i.imgur.com/qlXefjr.png)


![](https://i.imgur.com/pjgFtLK.png)

![](https://i.imgur.com/Q7Tg8Kt.png)

![](https://i.imgur.com/XaPfF4O.png)

## DML 데이터 조작

- INSERT
	- Syntax
		- `INSERT <table_name>[(<column_name>,...)]VALUES(value,...)`
	- Example
		- `INSERT student values ('20210001', 'James', 'Computer', 1);`
		- `INSERT student (id, name, dept_name,grade) VALUES('20210001','James','Computer',1)`
		- `INSERT student (id, name, dept_name) VALUES ('20210001','James','Computer') -set grade value as null`
		- ![](https://i.imgur.com/y8uypry.png)
	- INSERT INTO SELECT
		- 다른 테이블에서 질의 결과를 삽입하는 구문
		- `INSERT INTO <table_name>[(<column_name>,...)]<select clause>`
	- Example
		- `INSERT INTO student SELECT * from new_students;`
- DELETE
	- 테이블에서 쿼리에 해당되는 row를 지우는 sql
	- Syntax
		- `DELETE FROM <table_name> [WHERE condition]`
	- Example
		- DELETE FROM student; -> 모든 rows 삭제
		- DELETE FROM student WHERE dept_name = 'Computerl
		- DELETE FROM student WHERE dept_name in (select dept_name from department where building = 'SEL01')
- UPDATE
	- 테이블에서 저장된 데이터를 변경하는 SQL
	- Syntax
		- `UPDATE <table_name> SET <column_name> = value[WHERE condition]`
	- Example
		- `UPDATE employee set salary = salary * 1.07;`
		- `UPDATE employee set salary = salary * 1.07 WHERE hire_date < '2020.01.01';`
		- `UPDATE employee set salary = salary * 1.07 WHERE dept_name in (SELECT * from department WHERE location = 'SEOUL';