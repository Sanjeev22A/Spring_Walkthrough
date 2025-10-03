# 📘 Spring Data JPA Repository Guide

In Spring Boot, **repositories** are the way we interact with the database.
Instead of writing SQL queries manually, we use the **Spring Data JPA** abstraction which automatically provides CRUD (Create, Read, Update, Delete) operations.

---

## 🚀 What is `JpaRepository`?

* `JpaRepository<T, ID>` is an interface provided by Spring Data JPA.
* `T` → the entity type (`User` in our case).
* `ID` → the type of the entity’s primary key (`Long` here).
* By extending this interface, you get **ready-to-use methods** like:

  * `save(entity)` → Insert/Update
  * `findById(id)` → Fetch by primary key
  * `findAll()` → Fetch all records
  * `deleteById(id)` → Delete by primary key
  * `count()` → Count rows

---

## 📝 Example Repository

```java
package com.dockerBasic.docker_Intro.repository;

import com.dockerBasic.docker_Intro.model.User;
import org.springframework.data.jpa.repository.JpaRepository;

public interface UserRepository extends JpaRepository<User, Long> {
    // Built-in CRUD operations are inherited
    // Long represents the type of the primary key
    // User represents the entity
}
```

With just this interface, you can already perform **CRUD operations** without writing SQL queries.

---

## ✨ Adding Custom Query Methods

Spring Data JPA allows **derived query methods**.
Just follow the naming conventions, and Spring automatically generates the queries.

### 1. Find by a Field

```java
List<User> findByName(String name);
```

👉 Fetches all users with the given name.

---

### 2. Find by Multiple Fields

```java
User findByNameAndAge(String name, Integer age);
```

👉 Fetches a user with both name and age matching.

---

### 3. Find with Comparisons

```java
List<User> findByAgeGreaterThan(int age);
List<User> findByAgeLessThan(int age);
List<User> findByAgeBetween(int min, int max);
```

👉 Fetches users based on age conditions.

---

### 4. Find with Sorting

```java
List<User> findAllByOrderByNameAsc();
List<User> findAllByOrderByAgeDesc();
```

👉 Fetches users sorted by name or age.

---

### 5. Custom JPQL/SQL Queries (`@Query`)

You can also write **manual queries** using `@Query`.

```java
@Query("SELECT u FROM User u WHERE u.name = ?1")
List<User> searchByName(String name);
```

Or native SQL:

```java
@Query(value = "SELECT * FROM app_user u WHERE u.age >= ?1", nativeQuery = true)
List<User> findUsersOlderThan(int age);
```

---

## 🔧 Adding Update/Delete Queries

* For update/delete operations with custom queries, you must use `@Modifying` and `@Transactional`.

```java
@Modifying
@Transactional
@Query("UPDATE User u SET u.age = ?2 WHERE u.name = ?1")
int updateUserAgeByName(String name, int age);

@Modifying
@Transactional
@Query("DELETE FROM User u WHERE u.name = ?1")
int deleteUserByName(String name);
```

---

## ✅ Example Usage in Service Layer

```java
@Service
public class UserService {
    
    @Autowired
    private UserRepository userRepository;

    public List<User> getAllUsers() {
        return userRepository.findAll();
    }

    public User addUser(User user) {
        return userRepository.save(user);
    }

    public void deleteUser(Long id) {
        userRepository.deleteById(id);
    }
}
```

---

## 🎯 Key Takeaways

* Extending `JpaRepository<User, Long>` gives you **CRUD for free**.
* Add custom finder methods by following **Spring Data JPA naming conventions**.
* Use `@Query` for **custom JPQL or SQL queries**.
* Use `@Modifying` + `@Transactional` for **update/delete queries**.


