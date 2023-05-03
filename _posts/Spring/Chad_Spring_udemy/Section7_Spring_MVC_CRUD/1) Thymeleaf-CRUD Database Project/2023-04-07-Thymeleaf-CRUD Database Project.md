---
layout: single
title: "[Spring] Thymeleaf-CRUD Database Project"
categories: Spring
tag: [Java,"Thymeleaf-CRUD"]
toc: true
toc_sticky: true
author_profile: false
sidebar:

---

# Thymeleaf CRUD - Real Time Project

## Application Requirements
- Create a Web UI for the Employee Directory
- Users should be able to
	- Get a list of employees
	- Add a new employee
	- Update an employee
	- Delete an employee

## Real-Time Project

![](https://i.imgur.com/APMJn5t.png)


## Application Architecture

![](https://i.imgur.com/I1xla57.png)

## Project Set Up
- We will extend our existing Employee project and add DB integration
- Add ***EmployeeService***, ***EmployeeRepository*** and ***Employee*** entity
	- Available in one of our previous projects
	- We created all of this code already from scratch ... so we'll just copy / paste it
- Allows us to focus on creating ***EmployeeController*** and Thymeleaf templates 

## Development Process
1. Get list of employess
2. Add a new employee
3. Update an existing employee
4. Delete an existing employee

### Get list of employees

```java
@Controller  
@RequestMapping("/employees")  
public class EmployeeController {  
   private EmployeeService employeeService;  
   public EmployeeController(EmployeeService theEmployeeService) {  
      employeeService = theEmployeeService;  
   }  
   // add mapping for "/list"  
   @GetMapping("/list")  
   public String listEmployees(Model theModel) {  
      // get the employees from db  
      List<Employee> theEmployees = employeeService.findAll();  
      // add to the spring model  
      theModel.addAttribute("employees", theEmployees);  
      return "employees/list-employees";  
   }
```

![](https://i.imgur.com/beyoFuz.png)


![](https://i.imgur.com/bBxDsqf.png)
> Auto redirect to another URL


### Add a new employee

- New Add Employee button for list-employees.html
	- ![](https://i.imgur.com/HStXInH.png)

- Create HTML form for new employee
	- ![](https://i.imgur.com/UkwU9Fz.png)

- Process form data to save employee
	- ![](https://i.imgur.com/RySocU6.png)

#### Step 1: New "Add Employee" button
- Add Employee button will href link to
	- request mapping /employees/showFormForAdd
	- ![](https://i.imgur.com/UWNEBNI.png)


##### Showing Form
In your Spring Controller
- Before you show the form, you must add a ***model attribute***
- This is an object that will hold form data for the ***data binding***

##### Controller code to show form

![](https://i.imgur.com/pNVAy7P.png)



##### Thymeleaf and Spring MVC Data Binding
- Thymeleaf has special expressions for binding Spring MVC form data
- Automatically setting / retrieving data from a Java object

##### Thymeleaf Expressions
- Thymeleaf expressions can help you build the HTML form

| Expression | Description                                      |
| ---------- | ------------------------------------------------ |
| th:action  | Location to send form data                       |
| th:object  | Reference to model attribute                     |
| th:field   | Bind input field to a property on model attribue |
| ...        | ...                                                 |

#### Step 2: Create HTML form for new employee

![](https://i.imgur.com/7UWEj2m.png)

![](https://i.imgur.com/oKyinlp.png)
> - When form is loaded, will call:
> 	- will call:
> 	- employee.getFirstName()
> 	- employee.getLastName
> 		- Call getter methods to populate form fields initially
> - When form is submitted,
> 	- will call:
> 		- employee.setFirstName(...)
> 		- employee.setLastName(....)
> 			- Call setter methods to populate
> 			  Java object with form data
> - Add controller request mapping for /employees/save


#### Step 3: Process form data to save employee

```java
@Controller  
@RequestMapping("/employees")  
public class EmployeeController {  
  
   private EmployeeService employeeService;  
  
   public EmployeeController(EmployeeService theEmployeeService) {  
      employeeService = theEmployeeService;  
   }
   
@PostMapping("/save")  
public String saveEmployee(@ModelAttribute("employee") Employee theEmployee){  
   // save the employee  
   employeeService.save(theEmployee);  
  
   // use a redirect to prevent duplicate submissions  
   return "redirect:/employees/list";  
}
```

![](https://i.imgur.com/bykfsxW.png)

![](https://i.imgur.com/A7ycgRG.png)

### Update an existing employee

1. "Update" button
2. Pre-populate the form
3. Process form data

#### Step 1: "Update" button

![](https://i.imgur.com/WbWjCCV.png)

>- Each row has an Update link
>	- current employee id embedded in link
>- When clicked
>	- will load the employee from database
>	- prepopulate the form

- Update button includes employee id

![](https://i.imgur.com/FyTZamD.png)


#### Step 2: Pre-populate Form

![](https://i.imgur.com/998ZPdK.png)

![](https://i.imgur.com/tkfPork.png)

>- When form is loaded,
>	- will call:
>		- employee.getFirstName()
>		- employee.getLastName()
>- This is how form is pre-populated


#### Step 3: Process form data to save employee
- No need for new code.. we can resue our existing code
- Works the same for add or update

### Delete Employee

1. Add "Delete" button/link on page
   ![](https://i.imgur.com/FYuAtMT.png)
2. Add controller code for "Delete"
   ![](https://i.imgur.com/ekJsDgi.png)


#### Step 1: "Delete" button

![](https://i.imgur.com/FYuAtMT.png)
>- Each row has a Delete button/link
>	- current employee id embedded in link
>- When clicked
>	- prompt user
>	- will delete the employee from database

- Delete button includes employee id
  
![](https://i.imgur.com/f8Ffe7I.png)

#### Step 2: Add controller code for delete
![](https://i.imgur.com/qUFcLqt.png)
