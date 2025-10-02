
# Simple Spring Boot Application Example

## Overview
In this example, we will be creating a simple Spring Boot Application. The application demonstrates the basic structure of a Spring Boot project with `controller`, `service`, and `model` layers.

---

## Project Structure
Now you would be able to see a main class. In the same directory create **controller**, **model**, and **service** packages respectively as shown in the figure below:

<img width="608" height="535" alt="image" src="https://github.com/user-attachments/assets/c23cb229-5d5a-43b0-90f3-272d381e022f" />

---

## Application Properties
In the `resources` directory you will find **application.properties** where you need to set the server port as follows:

```properties
server.port=8080
````

You can change the port number if desired. This setting ensures that your Spring Boot application will listen on the configured port.

---

## Model Directory

**Purpose:**
The `model` directory holds your **data structures** or **domain objects**. These are simple Java classes that represent entities in your application, such as users, products, or orders.

**Usage in Spring Boot:**
Models encapsulate the data the application works with. They are often used by controllers and services to pass structured data around.

**Example:**

```java
package com.dockerBasic.docker_Intro.model;

public class User {
    private Long id;
    private String name;
    private String email;

    // Constructors
    public User() {}
    public User(Long id, String name, String email) {
        this.id = id;
        this.name = name;
        this.email = email;
    }

    // Getters and Setters
    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }

    public String getName() { return name; }
    public void setName(String name) { this.name = name; }

    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }
}
```

---

## Service Directory

**Purpose:**
The `service` directory contains **business logic** and core functionality. It acts as a middle layer between controllers (API layer) and models (data layer).

**Usage in Spring Boot:**
Services handle operations on models, ensuring controllers only deal with HTTP request/response and not with the business logic. This separation makes your code modular, maintainable, and testable.

**Example:**

```java
package com.dockerBasic.docker_Intro.service;

import com.dockerBasic.docker_Intro.model.User;
import org.springframework.stereotype.Service;
import java.util.List;
import java.util.ArrayList;

@Service
public class UserService {
    private final List<User> users = new ArrayList<>();

    public List<User> getAllUsers() {
        return users;
    }

    public void addUser(User user) {
        users.add(user);
    }
}
```

---

## Controller Directory

**Purpose:**
The `controller` directory handles **HTTP requests and responses**. It acts as the entry point for web clients or REST APIs.

**Usage in Spring Boot:**
Controllers expose endpoints, delegate business logic to services, and return results to the client. Typically, they are annotated with `@RestController` for REST APIs or `@Controller` for MVC applications.

**Example:**

```java
package com.dockerBasic.docker_Intro.controller;

import com.dockerBasic.docker_Intro.model.User;
import com.dockerBasic.docker_Intro.service.UserService;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/users")
public class UserController {

    private final UserService userService;

    public UserController(UserService userService) {
        this.userService = userService;
    }

    @GetMapping
    public List<User> getUsers() {
        return userService.getAllUsers();
    }

    @PostMapping
    public void addUser(@RequestBody User user) {
        userService.addUser(user);
    }
}
```

---

## Summary

| Directory      | Role                                           |
| -------------- | ---------------------------------------------- |
| **Model**      | Represents data/entities used in the app       |
| **Service**    | Contains business logic & operations on models |
| **Controller** | Exposes REST endpoints / handles HTTP requests |

This structure ensures a clean separation of concerns and makes your Spring Boot application maintainable and scalable.

---

