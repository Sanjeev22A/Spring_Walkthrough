---

# Initializing a Spring Boot Project

This document describes how to initialize a Spring Boot project using **Spring Initializr**.

---

## 1. Using Spring Initializr

To initialize the project, I used the **Spring Initializr** tool:

![Spring Initializr Screenshot](https://github.com/user-attachments/assets/4b0cc9de-441f-42f9-87ea-44ca23e22da7)

---

## 2. Project Setup Steps

### **a) Project Type**

* Choose the build system:

  * **Maven** (selected in this case)
  * Gradle (alternative)

### **b) Language**

* Select the language for your project:

  * **Java** (selected)
  * Kotlin or Groovy (alternatives)

### **c) Spring Boot Version**

* Pick a version of Spring Boot.
* ⚠️ **Be careful to choose a stable version** for production.

---

## 3. Project Metadata

### **Group**

* Acts as a namespace for the project.
* Common examples: `com.example`, `com.mycompanyname`.

### **Artifact**

* The name of the build artifact (**JAR** or **WAR**) produced when you build the project.
* Also part of your **Maven coordinates**:

  ```
  groupId : artifactId : version
  ```

### **Name**

* The human-readable name of the project.

### **Description**

* A short description of the project.

### **Package Name**

* The base package for your Java classes.
* The main class will be placed here.
* Example:
  If package name = `com.mycompany.orders`
  Then main class is

  ```java
  package com.mycompany.orders;

  @SpringBootApplication
  public class OrdersApplication {
      public static void main(String[] args) {
          SpringApplication.run(OrdersApplication.class, args);
      }
  }
  ```

### **Packaging**

* Defines how the project is packaged:

  * **JAR** → Standalone application (default, runnable with `java -jar`).
  * **WAR** → For deployment into an external servlet container (e.g., Tomcat).
 
### **Dependencies**

* The additional libraries a project needs:

  * Just Type and select the dependencies via **ADD DEPENDENCIES** button.

### Installation
 * Click on Generate Button.
 * A zip file is downloaded, extract it and then open its root folder in Intellij to start with your spring project.
---


