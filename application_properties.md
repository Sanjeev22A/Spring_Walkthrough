# Spring Boot Application Properties Explained

This document explains the `application.properties` configuration for the **Dockerized Spring App** that uses **Spring Boot + PostgreSQL + Hibernate (JPA)**.

---

## 1. Application Settings

```properties
spring.application.name=Dockerized_Spring_App
server.port=8000
spring.profiles.active=default
````

* **spring.application.name**: Sets the application name (`Dockerized_Spring_App`).
  Useful in logs, tracing, and when using Spring Cloud or microservices.

* **server.port**: Defines which port the Spring Boot application will run on (`8000`).
  Default is `8080`, but this overrides it.

* **spring.profiles.active**: Activates a profile (`default`).
  Profiles allow different configurations (e.g., `dev`, `test`, `prod`).

---

## 2. Debug & Logging Settings

```properties
debug=true

logging.level.org.hibernate.SQL=DEBUG
logging.level.org.hibernate.type.descriptor.sql.BasicBinder=TRACE
```

* **debug=true**: Enables Spring Boot debug mode, printing additional startup info.
  Helps when troubleshooting beans, config, and auto-configuration.

* **logging.level.org.hibernate.SQL=DEBUG**: Logs all SQL queries executed by Hibernate.
  Very useful for development but should be disabled in production for performance/security.

* **logging.level.org.hibernate.type.descriptor.sql.BasicBinder=TRACE**:
  Logs parameter binding values (e.g., what’s being passed into `?` placeholders in SQL).
  Example: `binding parameter [1] as [VARCHAR] - 'John'`.

---

## 3. Database Configuration

```properties
spring.datasource.url=jdbc:postgresql://localhost:6969/health_station?currentSchema=public&stringtype=unspecified
spring.datasource.username=sandy
spring.datasource.password=sandy
spring.datasource.driver-class=org.postgresql.Driver
```

* **spring.datasource.url**:
  Connection URL for PostgreSQL:

  * `localhost:6969` → database runs on port 6969.
  * `health_station` → database name.
  * `currentSchema=public` → sets default schema to `public`.
  * `stringtype=unspecified` → ensures string handling compatibility with Hibernate.

* **spring.datasource.username** / **password**: Credentials for connecting to the database (`sandy / sandy`).

* **spring.datasource.driver-class**: Explicitly sets the driver (`org.postgresql.Driver`).
  Usually auto-detected, but good to be explicit.

---

## 4. Timezone Setting

```properties
spring.jpa.properties.hibernate.jdbc.time_zone=Asia/Kolkata
```

* Forces Hibernate to **store/retrieve timestamps in `Asia/Kolkata` timezone**.
* Without this, you may face mismatches where database stores UTC but app assumes local time.

---

## 5. JPA / Hibernate Settings

```properties
spring.jpa.database-platform=org.hibernate.dialect.PostgreSQLDialect
spring.jpa.hibernate.ddl-auto=create
spring.jpa.show-sql=true
```

* **spring.jpa.database-platform**: Sets the SQL dialect (`PostgreSQLDialect`).
  Tells Hibernate how to generate SQL optimized for PostgreSQL.

* **spring.jpa.hibernate.ddl-auto=create**: Schema generation strategy. Options:

  * `none` → Do nothing.
  * `validate` → Validate schema matches entities.
  * `update` → Update schema automatically without dropping data.
  * `create` → Drop and recreate schema on each startup (⚠️ **data loss on restart**).
  * `create-drop` → Same as `create`, but also drops schema on shutdown.

  Here it is set to `create` (good for dev/testing, risky for prod).

* **spring.jpa.show-sql=true**: Logs SQL statements to console.
  Works alongside `logging.level.org.hibernate.SQL=DEBUG`.

---

## ✅ Summary of Best Practices

* Use `ddl-auto=update` or `validate` in production (instead of `create`).
* Disable `debug=true` and SQL logging in production for performance/security.
* Always set `hibernate.jdbc.time_zone` for consistency.
* Use **Spring Profiles** (`dev`, `test`, `prod`) to manage different environments.

---

## Example: Switching to Production

For production, you might use:

```properties
spring.profiles.active=prod
server.port=8080
spring.jpa.hibernate.ddl-auto=validate
debug=false
logging.level.org.hibernate.SQL=ERROR
```

This way you don’t drop data or expose SQL logs.

---

