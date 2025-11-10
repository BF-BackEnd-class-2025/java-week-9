# 🎯 Understanding DTOs (Data Transfer Objects)

In Spring Boot applications, a **DTO (Data Transfer Object)** is a class used to **send and receive data** in your API **without exposing
your database models (Entities)**.

DTOs help keep your application:

- ✅ Clean
- ✅ Secure
- ✅ Easy to maintain

---

## ❓ Why Not Use Entities Directly?

Entities represent your **database tables**. They often contain:

- `@Entity`, `@Column`, and other JPA annotations
- Relationships (`@OneToMany`, `@ManyToOne`)
- Internal or sensitive fields (e.g., passwords)
- Fields that should **not** be publicly modifiable

### Problems When Returning Entities Directly

| Problem                                          | Why It Matters                           |
|--------------------------------------------------|------------------------------------------|
| Sensitive data may leak                          | e.g., sending passwords or internal IDs  |
| Frontend becomes tightly coupled to DB structure | Changing DB breaks the frontend          |
| Harder validation                                | Users could modify fields they shouldn’t |
| Security risk                                    | Exposes how your DB is structured        |

👉 **DTOs solve all of these problems.**

---

## ✅ What Is a DTO?

A DTO is a **simple Java class** that contains **only the fields you want to accept or return** in your API.

### Example Request DTO (Client → API)

```java
public class ContactRequestDTO 
{
    private String name;
    private String phone;
    private String email;
    // getters & setters...
}
```

### Example Response DTO (API → Client)

```java
public class ContactResponseDTO 
{
    private Long id;
    private String name;
    private String phone;
    private String email;
    // getters & setters...
}
```

📝 *Notice:*
The **request** DTO has **no `id`**, because the client does not choose IDs.
The **response** DTO has **`id`**, because the client needs it.

---

## 🧠 Benefits of Using DTOs

| Benefit                       | Explanation                                   |
|-------------------------------|-----------------------------------------------|
| 🔐 **Security**               | Hide internal database structure              |
| 🎨 **Clean API Design**       | Define exactly what the frontend sees         |
| 🔄 **Flexibility**            | DB changes don’t break your API               |
| ✅ **Better Validation**       | Use `@NotBlank`, `@Email`, etc. on DTOs       |
| 🧱 **Separation of Concerns** | Controller deals with API data, not DB models |

---

## 🔄 Mapping Between Entity and DTO

### Option 1: Manual Mapping (Recommended for beginners)

```java
public Contact toEntity(ContactRequestDTO dto) 
{
    Contact c = new Contact();
    c.setName(dto.getName());
    c.setPhone(dto.getPhone());
    c.setEmail(dto.getEmail());
    return c;
}

public ContactResponseDTO toDTO(Contact entity) 
{
    ContactResponseDTO dto = new ContactResponseDTO();
    dto.setId(entity.getId());
    dto.setName(entity.getName());
    dto.setPhone(entity.getPhone());
    dto.setEmail(entity.getEmail());
    return dto;
}
```

### Option 2: ModelMapper (Auto-mapping)

```java
Contact contact = modelMapper.map(dto, Contact.class);
```

### Option 3: MapStruct (Best for real projects)

```java
@Mapper(componentModel = "spring")
public interface ContactMapper 
{
    Contact toEntity(ContactRequestDTO dto);
    ContactResponseDTO toDto(Contact entity);
}
```

---

## 🏗️ Example Controller Using DTOs

```java
@PostMapping
public ResponseEntity<ContactResponseDTO> create(@Valid @RequestBody ContactRequestDTO dto) 
{

    Contact entity = ContactMapper.toEntity(dto);
    Contact saved = service.create(entity);

    ContactResponseDTO response = ContactMapper.toDTO(saved);

    return ResponseEntity.status(201).body(response);
}
```

---

## 📁 Recommended Folder Structure

```
src/main/java/com/example/project/
 ├─ model/
 │   └─ Contact.java              ← Database Entity
 ├─ dto/
 │   ├─ ContactRequestDTO.java    ← Incoming request body
 │   └─ ContactResponseDTO.java   ← Outgoing response body
 └─ mapper/
     └─ ContactMapper.java        ← Converts Entity ↔ DTO
```

---

# ✅ Real DTO Examples (Your Projects)

## 1) 🎓 Student API DTOs

**StudentRequestDTO**, **StudentResponseDTO**, and **StudentMapper**
(Already provided in full above — ready to use)

## 2) ☎️ Contact API DTOs

**ContactRequestDTO**, **ContactResponseDTO**, and **ContactMapper**
(Also provided above)

---

# ⭐ Final Summary

| Concept      | Meaning                              |
|--------------|--------------------------------------|
| **DTO**      | Defines what your API sends/receives |
| **Entity**   | Represents your database table       |
| **Mapper**   | Converts Entity ↔ DTO                |
| **Why DTOs** | Security, clarity, flexibility       |

DTOs protect your backend and make your API clean and professional.

---
