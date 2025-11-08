# Controller–Service–Repository Architecture

Now that we have **Entities** and **Repositories**, we can expose our data through REST API endpoints.  
To keep the project clean and maintainable, we use a **layered architecture**.

---

## 1. Why Use Layers?

Instead of writing everything in the controller, we separate responsibilities:

```

Controller  →  Handles HTTP requests (input/output)
Service     →  Business logic (rules & processing)
Repository  →  Database access (CRUD operations)
Model       →  Entity representing data

```

This makes the application easier to:
- Understand
- Maintain
- Test
- Extend later

---

## 2. Project Structure Overview

```

src/main/java/com/example/demo/
├── controller/
│   └── StudentController.java
├── service/
│   └── StudentService.java
├── repository/
│   └── StudentRepository.java
├── model/
│   └── Student.java

````

---

## 3. Service Layer (Recap)

We already created `StudentService.java`.  
This layer talks to the **Repository** and performs logic.

```java
@Service
public class StudentService {

    private final StudentRepository repo;

    public StudentService(StudentRepository repo) {
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
        return repo.findById(id)
                .map(existing -> {
                    existing.setName(data.getName());
                    existing.setAge(data.getAge());
                    existing.setEmail(data.getEmail());
                    return repo.save(existing);
                })
                .orElse(null);
    }

    public void deleteStudent(Long id) {
        repo.deleteById(id);
    }
}
````

---

## 4. Create the Controller

Create `StudentController.java` in:

```
src/main/java/com/example/demo/controller
```

```java
package com.example.demo.controller;

import com.example.demo.model.Student;
import com.example.demo.service.StudentService;
import org.springframework.web.bind.annotation.*;
import java.util.List;

@RestController
@RequestMapping("/api/students")
public class StudentController {

    private final StudentService service;

    public StudentController(StudentService service) {
        this.service = service;
    }

    // GET all students
    @GetMapping
    public List<Student> getStudents() {
        return service.getAllStudents();
    }

    // GET one student
    @GetMapping("/{id}")
    public Student getStudentById(@PathVariable Long id) {
        return service.getStudentById(id);
    }

    // POST new student
    @PostMapping
    public Student createStudent(@RequestBody Student student) {
        return service.addStudent(student);
    }

    // PUT update student
    @PutMapping("/{id}")
    public Student updateStudent(@PathVariable Long id, @RequestBody Student student) {
        return service.updateStudent(id, student);
    }

    // DELETE student
    @DeleteMapping("/{id}")
    public void deleteStudent(@PathVariable Long id) {
        service.deleteStudent(id);
    }
}
```

---

## 5. API Endpoints Summary

| Method   | Endpoint             | Description           |
|----------|----------------------|-----------------------|
| `GET`    | `/api/students`      | Get all students      |
| `GET`    | `/api/students/{id}` | Get one student by ID |
| `POST`   | `/api/students`      | Create a student      |
| `PUT`    | `/api/students/{id}` | Update a student      |
| `DELETE` | `/api/students/{id}` | Delete a student      |

---

