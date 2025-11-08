# CRUD Operations with Spring Data JPA Repositories

Now that we have an **Entity**, we need a way to interact with the database.  
Spring Data JPA allows us to do this **without writing SQL**.  
We simply create a **Repository interface**, and Spring generates all the logic automatically.

---

## 1. What Is a Repository?

A **Repository** is an interface that provides methods to:
- Insert (Create)
- Retrieve (Read)
- Update (Update)
- Delete (Delete)

This is known as **CRUD**.

In Spring Data JPA, we create repositories by extending **`JpaRepository`**.

---

## 2. Create a Repository

Create a new package:

```

src/main/java/com/example/demo/repository

````

Inside it, create `StudentRepository.java`:

```java
package com.example.demo.repository;

import com.example.demo.model.Student;
import org.springframework.data.jpa.repository.JpaRepository;
import java.util.List;

public interface StudentRepository extends JpaRepository<Student, Long> 
{
    // Example of custom query method
    List<Student> findByName(String name);
}
````

### Why does this work?

Because `JpaRepository<Student, Long>` provides ready-to-use methods like:

| Method           | Action                    |
|------------------|---------------------------|
| `save(entity)`   | Insert or update a record |
| `findAll()`      | Get all records           |
| `findById(id)`   | Get one record by ID      |
| `deleteById(id)` | Delete a record by ID     |

And Spring generates query methods automatically based on method names (e.g., `findByName`).

---

## 3. Using the Repository in a Service

We don’t call repositories directly from controllers.
Instead, we follow the **Service Layer** pattern (clean architecture).

Create:

```
src/main/java/com/example/demo/service
```

Then `StudentService.java`:

```java
package com.example.demo.service;

import com.example.demo.model.Student;
import com.example.demo.repository.StudentRepository;
import org.springframework.stereotype.Service;
import java.util.List;

@Service
public class StudentService 
{

    private final StudentRepository repo;

    public StudentService(StudentRepository repo) 
    {
        this.repo = repo;
    }

    public List<Student> getAllStudents() {
        return repo.findAll();
    }

    public Student getStudentById(Long id) {
        return repo.findById(id).orElse(null);
    }

    public Student addStudent(Student student) {
        return repo.save(student);
    }

    public Student updateStudent(Long id, Student data) {
        Student existing = repo.findById(id).orElse(null);
        if (existing != null) {
            existing.setName(data.getName());
            existing.setAge(data.getAge());
            existing.setEmail(data.getEmail());
            return repo.save(existing);
        }
        return null;
    }

    public void deleteStudent(Long id) {
        repo.deleteById(id);
    }
}
```

---

## 4. Exposing CRUD via a Controller (Preview)

This will be covered in detail in the next lesson, but for now:

```java
@RestController
@RequestMapping("/api/students")
public class StudentController {

    private final StudentService service;

    public StudentController(StudentService service) {
        this.service = service;
    }

    @GetMapping
    public List<Student> getAllStudents() {
        return service.getAllStudents();
    }

    @PostMapping
    public Student createStudent(@RequestBody Student s) {
        return service.addStudent(s);
    }
}
```

---

## 5. Test the API Using Postman

| Method | Endpoint             | Body (JSON)                                             | Description       |
|--------|----------------------|---------------------------------------------------------|-------------------|
| GET    | `/api/students`      | —                                                       | Get all students  |
| POST   | `/api/students`      | `{ "name": "Ana", "age": 22, "email": "ana@mail.com" }` | Add student       |
| GET    | `/api/students/{id}` | —                                                       | Get student by ID |
| PUT    | `/api/students/{id}` | JSON object                                             | Update existing   |
| DELETE | `/api/students/{id}` | —                                                       | Delete student    |

---

## ✅ Summary

You now know how to:

* Create a **Repository** using `JpaRepository`
* Perform CRUD operations *without writing SQL*
* Follow best structure using a **Service layer**
* Prepare data to be exposed via **REST endpoints**

---

