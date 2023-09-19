---
layout: single
title: "[Spring] 회원정보 등록 구현"
categories: Spring
tag: [Java,MySQL]
toc: true
toc_sticky: true
author_profile: false
sidebar:
     

---
# 회원정보 등록 구현 with MySQL
MySQL 맛보기 (2)

## 회원정보 관리
- 이메일, 닉네임, 생년월일을 입력받아 저장한다.
- 닉네임은 10자를 초과할 수 없다.
- 회원은 닉네임을 변결할 수 있다.
	- 회원의 닉네임 변경이력을 조회 할 수 있어야한다. 


CREATE DATABASE 문은 MySQL 데이터베이스를 생성하는 SQL 명령문입니다.

your_database_name은 실제 생성할 데이터베이스의 이름을 나타내며, 여기에 원하는 이름을 넣어주면 됩니다.

CHARACTER SET은 데이터베이스 내의 문자열 데이터에 대한 문자 집합을 설정하는 부분입니다. utf8mb4는 유니코드 문자를 저장하기 위한 넓은 범위의 문자 집합입니다. 이를 사용하면 다국어 문자를 지원할 수 있습니다.

COLLATE는 문자열의 비교 및 정렬 방식을 설정하는 부분입니다. utf8mb4_unicode_ci는 대소문자를 구분하지 않는(Uni)코드(UTF) 문자열 비교(CI: Case-Insensitive)를 의미합니다. 이를 사용하면 대소문자를 구분하지 않고 문자열을 비교하거나 정렬할 수 있습니다.

이러한 설정을 통해 데이터베이스의 문자열 데이터를 다양한 언어와 문자셋에서 정확하게 처리할 수 있도록 설정할 수 있습니다.

장고 초기화

```
DROP DATABASE your_database_name;
CREATE DATABASE your_database_name CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

```