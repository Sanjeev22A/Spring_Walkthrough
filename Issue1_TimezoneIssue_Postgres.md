# Fixing Timezone Issues in Spring Boot + PostgreSQL + Docker

When working with **Spring Boot**, **PostgreSQL**, and **Docker**, you might encounter **timezone mismatch errors** (e.g., timestamps not being stored/retrieved correctly).  
This guide shows how to properly configure timezone across all layers of the stack.

---

## 1. Configure `application.properties`

In your Spring Boot project, update `src/main/resources/application.properties`:

```properties
# Force Hibernate to use the correct JDBC timezone
spring.jpa.properties.hibernate.jdbc.time_zone=Asia/Kolkata

# PostgreSQL datasource configuration
spring.datasource.url=jdbc:postgresql://localhost:6969/health_station?currentSchema=public&stringtype=unspecified
spring.datasource.username=your_db_user
spring.datasource.password=your_db_password
````

---

## 2. Set Timezone in PostgreSQL Database

Inside PostgreSQL, set the correct timezone:

```sql
-- Set timezone for the current session
SET TIMEZONE = 'Asia/Kolkata';

-- Make timezone permanent in postgresql.conf
SHOW config_file;

-- Edit the file (path from above command) and set:
timezone = 'Asia/Kolkata';
```

Restart PostgreSQL after changes:

```bash
sudo systemctl restart postgresql
```

---

## 3. Configure Timezone in Docker Environment

If you are running PostgreSQL inside Docker, make sure timezone is passed as environment variables.

### `.env` file

```env
TZ=Asia/Kolkata
PGTZ=Asia/Kolkata
```

### `docker-compose.yml`

```yaml
services:
  postgres:
    image: postgres:latest
    container_name: my_postgres
    restart: always
    env_file: .env
    environment:
      - POSTGRES_USER=your_user
      - POSTGRES_PASSWORD=your_password
      - POSTGRES_DB=health_station
    ports:
      - "6969:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data
```

---

## 4. Set JVM Timezone in Spring Boot Application

To make sure your **Spring Boot app** runs with the same timezone:

### Option 1: Set in `main` method

```java
@SpringBootApplication
public class MyApplication {
    public static void main(String[] args) {
        // Force JVM to use Asia/Kolkata
        TimeZone.setDefault(TimeZone.getTimeZone("Asia/Kolkata"));
        SpringApplication.run(MyApplication.class, args);
    }
}
```

### Option 2: Pass as JVM Argument

When running via Docker or locally:

```bash
java -Duser.timezone=Asia/Kolkata -jar app.jar
```

---

## 5. Verification

1. Check JVM timezone:

   ```java
   System.out.println(TimeZone.getDefault());
   ```

2. Check PostgreSQL timezone:

   ```sql
   SHOW TIMEZONE;
   ```

3. Insert and fetch a timestamp to confirm consistency.

---

## ✅ Best Practices

* Always keep **Spring Boot, PostgreSQL, and Docker** on the same timezone.
* Prefer **`Asia/Kolkata`** or **UTC** across all layers to avoid mismatches.
* Store timestamps in UTC and convert them to local timezone in the frontend (recommended for distributed systems).

---

## Summary

To fix timezone errors:

1. Configure `hibernate.jdbc.time_zone` in `application.properties`.
2. Set `timezone` in PostgreSQL (`postgresql.conf`).
3. Pass `TZ` and `PGTZ` env vars in Docker.
4. Force JVM timezone in `main` or JVM args.

This ensures all timestamps are consistent and avoids subtle bugs in time-sensitive applications.

---



