# ✅ Validation in Controllers: `@Valid` vs `@Validated` + Global Exception Handling

Validation in Spring ensures that the data received from the client (JSON input, form data, etc.) meets the expected requirements
before it’s processed or stored in the database.

Spring Boot integrates with **Jakarta Bean Validation** (hibernate-validator under the hood).

---

## 1. `@Valid` — Validate **Request Body** Objects

`@Valid` is used to trigger validation on **Java objects**, typically DTOs or Entities passed through `@RequestBody`.

### Example

```java
@PostMapping
public ResponseEntity<Contact> create(@Valid @RequestBody Contact contact) 
{
    Contact saved = service.create(contact);
    return ResponseEntity.status(201).body(saved);
}
```

If the object has validation annotations (like `@NotBlank`, `@Email`), Spring automatically checks them.

If validation fails → Spring throws:

```
MethodArgumentNotValidException
```

---

## 2. `@Validated` — Validate **Path Variables & Request Parameters**

`@Validated` works similarly to `@Valid`, but **also enables validation at the method level**, including:

* `@RequestParam`
* `@PathVariable`
* Validation groups

### Example

```java
import jakarta.validation.constraints.Min;
import org.springframework.validation.annotation.Validated;

@RestController
@RequestMapping("/contacts")
@Validated   // <--- enables validation on parameters
public class ContactController {

    @GetMapping("/{id}")
    public ResponseEntity<Contact> getById(@PathVariable @Min(1) Long id) 
    {
        Contact contact = service.getById(id);
        return (contact != null)
                ? ResponseEntity.ok(contact)
                : ResponseEntity.notFound().build();
    }
}
```

If ID is less than 1, Spring throws:

```
ConstraintViolationException
```

---

## 3. When to Use Which?

| Situation                                     | Use          | Reason                             |
|-----------------------------------------------|--------------|------------------------------------|
| Validating `@RequestBody` JSON object         | `@Valid`     | Field-level model validation       |
| Validating `@PathVariable` or `@RequestParam` | `@Validated` | Enables method-level validation    |
| Working with validation groups                | `@Validated` | Supports groups, `@Valid` does not |

---

## 4. ✅ Global Exception Handling (Clean JSON Errors)

Spring controllers should not return validation exceptions directly.
We handle them **centrally** using `@RestControllerAdvice`.

Create:

```
src/main/java/.../exception/GlobalExceptionHandler.java
```

### Handle `@Valid` errors (RequestBody validation)

```java
@RestControllerAdvice
public class GlobalExceptionHandler 
{
    @ResponseStatus(HttpStatus.BAD_REQUEST)
    @ExceptionHandler(MethodArgumentNotValidException.class)
    public Map<String, String> handleValidationErrors(MethodArgumentNotValidException ex) 
    {
        Map<String, String> errors = new HashMap<>();
        ex.getBindingResult().getFieldErrors().forEach(error ->
                errors.put(error.getField(), error.getDefaultMessage())
        );
        return errors;
    }
}
```

**Example error response:**

```json
{
  "email": "Email must be valid",
  "name": "Name cannot be blank"
}
```

---

### Handle `@Validated` param errors (path variables / request params)

```java
@ResponseStatus(HttpStatus.BAD_REQUEST)
@ExceptionHandler(ConstraintViolationException.class)
public Map<String, String> handleConstraintViolation(ConstraintViolationException ex) 
{
    Map<String, String> errors = new HashMap<>();
    ex.getConstraintViolations().forEach(violation ->
            errors.put(
                violation.getPropertyPath().toString(),
                violation.getMessage()
            )
    );
    return errors;
}
```

---

## 5. Summary Table

| Validation Type     | Annotation   | Works On                         | Exception Type                    | Global Handler                |
|---------------------|--------------|----------------------------------|-----------------------------------|-------------------------------|
| Request Body Object | `@Valid`     | `@RequestBody` DTO/entity        | `MethodArgumentNotValidException` | `handleValidationErrors()`    |
| Path / Query Param  | `@Validated` | `@PathVariable`, `@RequestParam` | `ConstraintViolationException`    | `handleConstraintViolation()` |

---

## ✅ Best Practice

Always:

- ✔ Use `@Valid` on DTOs / request bodies
- ✔ Use `@Validated` on controllers if you want param validation
- ✔ Use **one global exception handler** to keep controllers clean

---
