---
layout: single
title: "[Spring] JDBC Authentication Custom tables"
categories: Spring
tag: [Java,"JDBC Authentication Custom tables","Security Schema Customization"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---
# JDBC Authentication Custom tables

## Default Spring Security Database Schema

![](https://i.imgur.com/ueEMJdO.png)

## Custom Tables
- What if we have our own custom tables?
- Our won custom column names?

![](https://i.imgur.com/NZK6jZt.png)



## For Security Schema Customization
- Tell Spring how to query your custom tables
- Provide query to find user by user name
- Provide query to find authorities / roles by user name

## Development Process
1. Create our custom tables with SQL
2. Update Spring Security Configuration
	- Provide query to find user by user name
	- Provide query to find authorities / roles by user name


### Step 1: Create our custom tables with SQL

![](https://i.imgur.com/NZK6jZt.png)

### Step 2: Update Spring Security Configuration

![](https://i.imgur.com/xwcZESO.png)

![](https://i.imgur.com/yyDdwWN.png)

