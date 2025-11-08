# Step-by-Step: Testing Your Application

## 1. Start PostgreSQL

Make sure your PostgreSQL server is running.

If using pgAdmin:

- Open pgAdmin → Login → Check database `school_db` exists.

If using DBeaver:

- Connect → Test connection → Should say **Success**.

---

## 2. Run Your Spring Boot Application

From IntelliJ:

**Click Run** → Look for these logs:

```bash
Tomcat started on port 8080
Connected to PostgreSQL
Hibernate: create table ...
```

If you see errors → check `application.properties`.

---

## 3. Open Postman

We will test the API by sending HTTP requests.

### ✅ **Endpoint 1: Get All Students**

| Method | URL                                  |
|--------|--------------------------------------|
| GET    | `http://localhost:8080/api/students` |

Expected Response (empty list first time):

```json
[]
```

---

### ✅ **Endpoint 2: Add a Student**

| Method | URL                                  |
|--------|--------------------------------------|
| POST   | `http://localhost:8080/api/students` |

Select:

- Body → **raw**
- Type → **JSON**

Send:

```json
{
  "name": "Sam",
  "age": 25,
  "email": "sam@mail.com"
}
```

Expected Response:

```json
{
  "id": 1,
  "name": "Sam",
  "age": 25,
  "email": "sam@mail.com"
}
```

---

### ✅ Verify in the Database

In pgAdmin or DBeaver:

```sql
SELECT * FROM students;
```

You should now see:

| id | name | age | email                               |
|----|------|-----|-------------------------------------|
| 1  | Sam  | 25  | [sam@mail.com](mailto:sam@mail.com) |

---

### ✅ **Endpoint 3: Get Student by ID**

| Method | URL                                    |
|--------|----------------------------------------|
| GET    | `http://localhost:8080/api/students/1` |

Expected:

```json
{
  "id": 1,
  "name": "Sam",
  "age": 25,
  "email": "sam@mail.com"
}
```

---

### ✅ **Endpoint 4: Update Student**

| Method | URL                                    |
|--------|----------------------------------------|
| PUT    | `http://localhost:8080/api/students/1` |

Body:

```json
{
  "name": "Sam Updated",
  "age": 26,
  "email": "sam.updated@mail.com"
}
```

Expected Response:

- Student updated in DB

---

### ✅ **Endpoint 5: Delete Student**

| Method | URL                                    |
|--------|----------------------------------------|
| DELETE | `http://localhost:8080/api/students/1` |

After sending, check DB again:

```sql
SELECT * FROM students;
```

→ Record should be gone ✅

---

## 🧠 Common Testing Mistakes & Fixes

| Issue                               | Explanation              | Fix                                           |
|-------------------------------------|--------------------------|-----------------------------------------------|
| `415 Unsupported Media Type`        | Postman didn't send JSON | Select **raw → JSON**                         |
| `404 Not Found`                     | URL incorrect            | Check controller `@RequestMapping` path       |
| `org.postgresql.util.PSQLException` | Wrong DB password        | Fix in `application.properties`               |
| `Table not created`                 | ddl-auto disabled        | Ensure `spring.jpa.hibernate.ddl-auto=update` |

---

## 🎯 Testing Is Successful When

- ✔ Endpoints return correct JSON
- ✔ Database updates after POST/PUT/DELETE
- ✔ No errors appear in logs

---
