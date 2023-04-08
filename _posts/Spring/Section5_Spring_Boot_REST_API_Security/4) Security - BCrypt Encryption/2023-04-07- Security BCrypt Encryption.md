---
layout: single
title: "Security - BCrypt Encryption"
categories: Spring
tag: [Java,"Security - BCrypt Encryption",]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# Spring Security Team Recommendation
- Spring Security recommends using the popular bcrypt algorithm
- bcrypt
	- Performs one-way encrypted hashing
	- Adds a random salt to the password for additional protection
	- Includes support to defeat brute force attacks

## Bcrypt Additional Information
- Why you should use bcrypt to hash passwords
	- https://danboterhoven.medium.com/why-you-should-use-bcrypt-to-hash-passwords-af330100b861
- Detailed bcrypt algorithm analysis
	- https://en.wikipedia.org/wiki/Bcrypt
- Password hashing - Best Practices
	- https://crackstation.net/hashing-security.htm

## How to get a Bcrypt password
- You have a plaintext password and you want to encrypt using bcrypt
	- Option 1: Use a website utility to perform the encryption
	- Option 2: Write a Java code to perform the encryption

## Development Process
1. Run SQL Script that contains encrypted passwords
	- Modify DDL for password, field, length should be 68

### Spring Security Password Storage
- In Spring Security, passwords are stored using a specific format

![](https://i.imgur.com/M3Xvt1v.png)


### Modify DDL for Password Field

![](https://i.imgur.com/ExqgTPR.png)


### Step 1: Develop SQL Script to setup database tables

![](https://i.imgur.com/9HUH1JT.png)


#### Spring Security Login Process

![](https://i.imgur.com/f0BVPXW.png)


>1. Retrieve password from db for the user
>2. Read the encoding algorithm id (bcrypt etc)
>3. For case of bcrypt, encrypt plaintext password from login form (using salt from db password)
>4. Compare encrypted password from login form WITH encrypted password from db
>5. If there is a match, login succesful
>6. If no match, login NOT successful

>- NOTE: 
>- The password from db is NEVER decrypted
>- Because bcrypt is a one-way encryption algorithm




