# Connecting Spring Boot to PostgreSQL (Using JPA)

In this section, we set up a Spring Boot project and configure it to connect to a **PostgreSQL** database using **Spring Data JPA**.  
The goal is to prepare the application so that it can store and retrieve data from a real database.

---

## 1. Add Required Dependencies

Make sure your `pom.xml` includes:

- **Spring Web**
- **Spring Data JPA**
- **PostgreSQL driver**
- (Optional) **Spring Boot DevTools** for auto-restart during development.

```xml
<dependencies>
    <!-- Spring Web -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <!-- Spring Data JPA -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>

    <!-- PostgreSQL Driver -->
    <dependency>
        <groupId>org.postgresql</groupId>
        <artifactId>postgresql</artifactId>
        <scope>runtime</scope>
    </dependency>

    <!-- Optional: DevTools -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-devtools</artifactId>
        <optional>true</optional>
    </dependency>
</dependencies>
````

If you used **Spring Initializer**, these may already be included.

---

## 2. Create a PostgreSQL Database

Open your PostgreSQL client (pgAdmin, DBeaver, or Terminal) and create a database:

```sql
CREATE DATABASE school_db;
```

You may also create a user and assign privileges if needed.

---

## 3. Configure `application.properties`

Inside `src/main/resources/application.properties`, add your database connection details:

```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/school_db
spring.datasource.username=your_username
spring.datasource.password=your_password

# JPA / Hibernate settings
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect
```

### What these settings mean:

| Property                        | Description                                             |
|---------------------------------|---------------------------------------------------------|
| `spring.datasource.url`         | Database URL & name                                     |
| `spring.jpa.hibernate.ddl-auto` | Controls table auto-creation (`update`, `create`, etc.) |
| `spring.jpa.show-sql`           | Prints SQL statements to console (useful for debugging) |
| `dialect`                       | Tells Hibernate how to generate SQL for PostgreSQL      |

> During teaching, explain that `ddl-auto=update` is helpful in development, but **not recommended** for production.

---

## 4. Run the Application

Start the application (e.g., click the **Run** button in IntelliJ).

If everything is correct, you should see logs confirming the database connection, such as:

```
HHH000204: Processing PersistenceUnitInfo
HHH000412: Database -> PostgreSQL
Hibernate: create table ...
```

If errors occur, check:

* Database name, username, password
* PostgreSQL is running
* Port `5432` is open

---

## 5. Verifying the Connection (Optional Quick Test)

Add a simple REST controller to confirm the application runs:

```java
@RestController
public class TestController 
{
    @GetMapping("/")
    public String home() 
    {
        return "Database connection is ready!";
    }
}
```

Visit:

```
http://localhost:8080/
```

If you see the message → ✅ Your project is successfully connected to PostgreSQL.

---

## ✅ Summary

By completing this step, we have:

* Added JPA and PostgreSQL dependencies
* Created a PostgreSQL database
* Configured connection details in Spring Boot
* Verified the application starts and connects

---

