# 📘 Dependency Injection in Spring Boot (`@Autowired` & Constructor Injection)

Spring Boot uses **Dependency Injection (DI)** to manage object creation and wiring.
Instead of manually creating objects with `new`, Spring injects the required dependencies from its **IoC container**.

---

## 🚀 What is `@Autowired`?

* `@Autowired` is an annotation that tells Spring to **automatically inject a dependency**.
* Spring looks for a matching **bean** (class managed by Spring) and injects it where required.

### Example: Field Injection

```java
@RestController
public class MyController {

    @Autowired
    private UserRepository userRepository;  // injected automatically

    @GetMapping("/users")
    public List<User> getUsers() {
        return userRepository.findAll();
    }
}
```

✔ Simple and quick
❌ Harder to test (you can’t pass mock objects easily).
❌ Breaks immutability (field is not `final`).

---

## 🔧 Constructor Injection (Recommended)

With constructor injection, dependencies are injected via the constructor instead of directly on fields.

```java
@RestController
public class MyController {

    private final UserRepository userRepository;  // immutable dependency

    // Constructor Injection
    public MyController(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    @GetMapping("/users")
    public List<User> getUsers() {
        return userRepository.findAll();
    }
}
```

✔ Promotes **immutability** (dependencies can be `final`).
✔ **Easier to test** (you can pass mock objects in unit tests).
✔ Works better with **mandatory dependencies**.
✔ No `@Autowired` needed if there’s only one constructor (Spring 4.3+).

---

## 🔄 Setter Injection (Optional/When Needed)

Dependencies are injected via a setter method.

```java
@RestController
public class MyController {

    private UserRepository userRepository;

    @Autowired
    public void setUserRepository(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
```

✔ Useful when the dependency is **optional**.
❌ Not recommended for required dependencies (object is mutable).

---

## 🏆 When to Use Each

| Injection Type              | When to Use                                                                                           |
| --------------------------- | ----------------------------------------------------------------------------------------------------- |
| **Field Injection**         | Quick prototypes, small demos, legacy codebases. Avoid in production.                                 |
| **Constructor Injection** ✅ | Best for required dependencies. Preferred in modern Spring projects. Keeps code testable & immutable. |
| **Setter Injection**        | Use when a dependency is **optional** or needs to be changed after creation.                          |

---

## 🎯 Key Takeaways

* `@Autowired` is **not required** if you’re using a **single constructor**.
* Prefer **constructor injection** for most cases.
* Use **setter injection** only when the dependency is optional.
* Avoid field injection in production code (good for quick demos).


