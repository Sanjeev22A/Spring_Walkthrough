# 📘 ORM with Spring Boot (JPA/Hibernate)

This document explains how to link a **Spring Boot application** to a **database** using **JPA/Hibernate ORM**.
We model Java classes as database tables by using **JPA annotations**.

---

## 🚀 Steps to Connect a Spring Application to DB

1. Create a **Model Class** that represents a table.
2. Annotate it with **`@Entity`**.
3. Use **`@Table(name="...")`** to map it to a specific table.
4. Mark a primary key with **`@Id`**.
5. Define how the key is generated with **`@GeneratedValue`**.
6. Use **`@Column`** and other annotations for fine-grained control.

---

## 🏷️ Common Annotations with Examples

### 1. `@Entity`

Marks a class as a JPA entity (mapped to a table).

```java
@Entity
public class Product {
    @Id
    private Long id;
}
```

---

### 2. `@Table(name="table_name")`

Maps an entity to a specific table name.

```java
@Entity
@Table(name = "products")
public class Product {
    @Id
    private Long id;
}
```

---

### 3. `@Id`

Defines the primary key of the entity.

```java
@Id
private Long id;
```

---

### 4. `@GeneratedValue(strategy=...)`

Specifies how the primary key is generated.

```java
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)  // Auto-increment
private Long id;
```

Options:

* `AUTO` → Hibernate decides
* `IDENTITY` → Auto-increment (MySQL, PostgreSQL)
* `SEQUENCE` → Uses sequence (Oracle, PostgreSQL)
* `TABLE` → Uses a separate table to manage IDs

---

### 5. `@Column`

Maps a field to a DB column and allows constraints.

```java
@Column(name = "product_name", nullable = false, length = 100, unique = true)
private String name;
```

---

### 6. `@Transient`

Marks a field **not to be persisted** in the database.

```java
@Transient
private String tempValue;  // Ignored by DB
```

---

### 7. `@Lob`

Maps large objects (CLOB or BLOB).

```java
@Lob
private String description;  // For long text
```

---

### 8. `@Enumerated`

Maps `enum` values to DB (as `ORDINAL` or `STRING`).

```java
public enum Status { ACTIVE, INACTIVE }

@Enumerated(EnumType.STRING)
private Status status;
```

---

### 9. `@Temporal`

Used for `java.util.Date` or `java.util.Calendar` to define precision.

```java
@Temporal(TemporalType.DATE)  // Only stores date
private Date createdDate;
```

---

### 10. `@OneToMany`, `@ManyToOne`, `@OneToOne`, `@ManyToMany`

Used for **relationships** between entities.

```java
@OneToMany(mappedBy = "product")
private List<Order> orders;
```

---

## 🧩 Example: User Entity

```java
package com.dockerBasic.docker_Intro.model;

import jakarta.persistence.*;

@Entity
@Table(name="app_user")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)   // Primary Key Auto-increment
    private Long id;

    @Column(name = "name", nullable = false, length = 100)
    private String name;

    @Column(name = "age")
    private Integer age;

    // Default constructor
    public User(){}

    // Parameterized constructor
    public User(long id, String name, int age){
        this.id=id;
        this.name=name;
        this.age=age;
    }

    // Getters and Setters
    public Long getId() {
        return id;
    }
    public void setId(Long id){
        this.id=id;
    }
    public void setName(String name) {
        this.name = name;
    }
    public String getName() {
        return name;
    }
    public Integer getAge() {
        return age;
    }
    public void setAge(Integer age) {
        this.age = age;
    }
}
```

