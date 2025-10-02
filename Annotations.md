# Spring Boot Annotations Reference

This README provides an overview of commonly used **Spring Boot annotations**, explaining their purpose and usage with examples. Understanding these annotations is crucial for building REST APIs and Spring Boot applications efficiently.

---

## 1. @SpringBootApplication

**Purpose:**  
Marks the main class of a Spring Boot application. It is a combination of:

- `@Configuration` – Allows defining beans
- `@EnableAutoConfiguration` – Enables auto-configuration
- `@ComponentScan` – Scans for components, services, and controllers

**Example:**

```java
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}


---

## 2. @RestController

**Purpose:**
Defines a controller that handles HTTP requests and returns JSON/XML responses directly (instead of views).

**Example:**

```java
package com.example.demo.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {

    @GetMapping("/hello")
    public String hello() {
        return "Hello, Spring Boot!";
    }
}
```

**Key Points:**

* Combines `@Controller` and `@ResponseBody`
* Used for RESTful APIs

---

## 3. @RequestMapping and Specific HTTP Mappings

**Purpose:**
Maps HTTP requests to controller methods.

**Types:**

* `@RequestMapping` – General-purpose, can specify method type
* `@GetMapping` – Handles HTTP GET
* `@PostMapping` – Handles HTTP POST
* `@PutMapping` – Handles HTTP PUT
* `@DeleteMapping` – Handles HTTP DELETE

**Example:**

```java
@RestController
@RequestMapping("/users")
public class UserController {

    @GetMapping
    public List<User> getAllUsers() {
        return List.of(new User(1L, "Alice"));
    }

    @PostMapping
    public void addUser(@RequestBody User user) {
        // Add user logic
    }
}
```

---

## 4. @Service

**Purpose:**
Marks a class as a **service** that contains business logic. Spring will detect it during component scanning and manage it as a bean.

**Example:**

```java
package com.example.demo.service;

import org.springframework.stereotype.Service;

@Service
public class UserService {

    public String getWelcomeMessage() {
        return "Welcome to the Spring Boot service!";
    }
}
```

---

## 5. @Repository

**Purpose:**
Marks a class as a **data repository**. Typically used for interacting with databases. Provides exception translation automatically.

**Example:**

```java
package com.example.demo.repository;

import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface UserRepository extends JpaRepository<User, Long> {
}
```

---

## 6. @Entity (Model / Domain Objects)

**Purpose:**
Defines a class as a JPA entity mapped to a database table.

**Example:**

```java
package com.example.demo.model;

import jakarta.persistence.Entity;
import jakarta.persistence.Id;

@Entity
public class User {
    @Id
    private Long id;
    private String name;
    private String email;

    // Constructors, getters, setters
}
```

---

## 7. @Autowired

**Purpose:**
Injects dependencies automatically. Commonly used in controllers, services, or repositories.

**Example:**

```java
@RestController
public class UserController {

    private final UserService userService;

    @Autowired
    public UserController(UserService userService) {
        this.userService = userService;
    }

    @GetMapping("/message")
    public String getMessage() {
        return userService.getWelcomeMessage();
    }
}
```

---

## 8. Other Useful Annotations

| Annotation       | Purpose                                            |
| ---------------- | -------------------------------------------------- |
| `@Component`     | Generic Spring-managed component                   |
| `@Configuration` | Marks class as a configuration provider            |
| `@Value`         | Injects values from properties file                |
| `@PathVariable`  | Extracts values from URI path                      |
| `@RequestParam`  | Extracts query parameters from URL                 |
| `@CrossOrigin`   | Enables CORS for REST APIs                         |
| `@Transactional` | Defines transactional boundaries for DB operations |

**Example with @PathVariable and @RequestParam:**

```java
@GetMapping("/users/{id}")
public User getUserById(@PathVariable Long id, @RequestParam(defaultValue="true") boolean active) {
    // Logic to return user by id and active status
}
```

---

## 9. Summary

1. **Controller Layer** – Handles HTTP requests (`@RestController`, `@RequestMapping`, `@GetMapping`, `@PostMapping`)
2. **Service Layer** – Contains business logic (`@Service`)
3. **Repository Layer** – Handles database operations (`@Repository`, `@Entity`, JPA)
4. **Dependency Injection** – Managed by Spring (`@Autowired`)
5. **Configuration & Properties** – Managed by `@SpringBootApplication` and `@Value`

Using these annotations, Spring Boot provides a clean, modular, and maintainable architecture for web applications and RESTful APIs.

---


