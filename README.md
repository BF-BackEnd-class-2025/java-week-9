# Week 9: Spring Boot + Database (Spring Data JPA)

This week introduces how to build **database-powered applications** using **Spring Boot** and **Spring Data JPA**.  
Students will learn how to connect a Spring Boot project to a relational database (PostgreSQL), define data models,
and perform CRUD operations following a clean architectural structure.

---

## 📚 Learning Objectives

By the end of this week, students will be able to:

- Configure a Spring Boot application to connect to a **PostgreSQL** database
- Use **Spring Data JPA** to map Java classes to database tables
- Perform **CRUD operations** using repository interfaces
- Apply the **Controller → Service → Repository → Model** project structure
- Test database-backed API endpoints using tools like **Postman**

---

## 🧠 Key Concepts

### 1. Database Configuration in Spring Boot
- Adding required dependencies
- Setting database connection properties
- Understanding autoconfiguration and Hibernate integration

### 2. JPA Entities & ORM (Object–Relational Mapping)
- Defining model classes as entities
- Managing primary keys and field mappings
- Understanding how tables and objects correspond

### 3. Spring Data JPA Repositories
- Using repository interfaces to handle database operations
- Built-in CRUD methods and query generation

### 4. Layered Architecture

```txt
controller/   → Handles HTTP requests (REST endpoints)
service/      → Business logic layer
repository/   → Database communication (JPA)
model/        → Data representation (entities)
```

### 5. Testing & Tools
- Using Postman to test API endpoints
- Viewing logs to track SQL statements
- Managing database tables using pgAdmin or DBeaver

---

## 🧰 Recommended Tools

| Tool              | Purpose                                |
|-------------------|----------------------------------------|
| IntelliJ IDEA     | Develop & run Spring Boot projects     |
| PostgreSQL        | Relational database used this week     |
| pgAdmin / DBeaver | GUI clients for exploring the database |
| Postman           | Test REST API endpoints                |
| Spring Initializr | Generate Spring Boot skeleton projects |

---

## 🏗️ Example Project Structure

```txt
spring-boot-db-demo/
├── controller/
├── service/
├── repository/
├── model/
├── resources/
│   └── application.properties
└── pom.xml
```

---

