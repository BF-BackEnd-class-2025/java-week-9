# JPA Entities & Object–Relational Mapping (ORM)

Now that the application is connected to PostgreSQL, we can define **entities**.  
Entities represent **tables in the database**, and each instance of an entity represents a **row** in the table.

Spring Data JPA uses **ORM (Object–Relational Mapping)** to translate between **Java objects** and **database records** automatically.

---

## 1. What is an Entity?

- An **entity** is a Java class mapped to a **database table**
- Fields in the class become **columns**
- Each entity **must have a primary key**

To make a class an entity, we use the `@Entity` annotation.

---

## 2. Example: Create a `Student` Entity

Create a new package named:

```

src/main/java/com/example/demo/model

````

Inside it, create `Student.java`:

```java
package com.example.demo.model;

import jakarta.persistence.*;

@Entity
@Table(name = "students") // Optional: specify table name
public class Student 
{

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY) // Auto-increment ID
    private Long id;

    private String name;
    private int age;
    private String email;

    public Student() {}
    public Student(String name, int age, String email) 
    {
        this.name = name;
        this.age = age;
        this.email = email;
    }

    // Getters and setters

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }
}
````

---

## 3. Key JPA Annotations

| Annotation              | Purpose                                                 |
|-------------------------|---------------------------------------------------------|
| `@Entity`               | Marks class as a database entity                        |
| `@Table(name = "...")`  | (Optional) Defines custom table name                    |
| `@Id`                   | Marks the primary key field                             |
| `@GeneratedValue`       | Controls how the ID is generated (e.g., auto-increment) |
| `@Column(name = "...")` | (Optional) Customize column names and properties        |

---

## 4. How Hibernate Creates the Table

When the app runs, Hibernate reads this class and generates an SQL table similar to:

```sql
create table students (
  id serial primary key,
  name varchar(255),
  age integer,
  email varchar(255)
);
```

This happens because we set:

```properties
spring.jpa.hibernate.ddl-auto=update
```

> Note: In production environments, table schemas are usually managed manually — not generated automatically.

---

## 5. Adding Validation (Optional but Recommended)

Spring Boot supports validation annotations such as:

```java
@NotBlank
@Email
@Min(value = 18)
```

Example:

```java
private @NotBlank String name;
private @Min(1) int age;
private @Email String email;
```

To enable validation, ensure:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation</artifactId>
</dependency>
```

is included.

---

## ✅ Summary

You now know how to:

* Create **entity classes**
* Map class fields to **database table columns**
* Configure primary keys using `@Id` and `@GeneratedValue`
* Let Hibernate automatically generate database tables

This prepares us for the next step: interacting with these tables using **repositories**.

---

