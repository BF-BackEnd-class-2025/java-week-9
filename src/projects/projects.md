# 10 Spring Boot + Database Project Ideas – Week 9

*(Using PostgreSQL + Spring Data JPA)*

---

## Dependency to include:

```xml
<dependencies>
   <!-- Spring Web Starter -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <!-- Spring Boot Test Starter -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>

    <!-- Spring Boot DevTools -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-devtools</artifactId>
        <optional>true</optional>
    </dependency>
    
    <!-- Spring Data JPA -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    
    <!-- Bean Validation -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-validation</artifactId>
    </dependency>

    <!-- PostgreSQL driver -->
    <dependency>
        <groupId>org.postgresql</groupId>
        <artifactId>postgresql</artifactId>
        <scope>runtime</scope>
    </dependency>
</dependencies>
```
---

## 1. 👨‍🎓 Student Records API 

**Goal:** Learn basic CRUD using JPA and PostgreSQL.

**Description:**
Create an API to store and manage student information in a database.
Each student has `id`, `name`, `age`, and `email`.

**Endpoints:** 
```

```txt
GET /students               → Get all students
GET /students/{id}          → Get a student by ID
POST /students              → Add a new student
PUT /students/{id}          → Update existing student
DELETE /students/{id}       → Delete student
```

**Learning Focus:**

* Controller → Service → Repository flow
* Mapping JSON to Entities
* `JpaRepository` standard CRUD methods

**Extension:**
Add search:

```txt
GET /students/search?name=alex
```

---

## 2. ☎️ Contact Manager API 

**Goal:** Practice validation and unique constraints.

**Description:**
Store contacts with `id`, `name`, `phone`, and **unique** `email`.

**Endpoints:**

```txt
GET /contacts
POST /contacts
GET /contacts/{id}
PUT /contacts/{id}
DELETE /contacts/{id}
```

**Learning Focus:**

* `@Email`, `@NotBlank`, `@Column(unique = true)`
* Handling validation errors correctly

**Extension:**
Search by name:

```txt
GET /contacts/search?name=john
```

---

## 3. 🛒 Inventory & Stock Manager API 

**Goal:** Work with numeric fields and computed values.

**Description:**
Store products with `id`, `productName`, `quantity`, `price`.

**Endpoints:**

```txt
GET /products
POST /products
PUT /products/{id}
DELETE /products/{id}
GET /products/value          → Total inventory value
```

**Learning Focus:**

* Using repository methods to query and compute totals
* Returning formatted numeric responses

**Extension:**
Alert if stock level is low.

---

## 4. 🍽️ Recipe Catalog API

**Goal:** Learn how to perform text search using JPA.

**Description:**
Store recipes (`title`, `ingredients`, `category`) and allow text search.

**Endpoints:**

```txt
GET /recipes
POST /recipes
GET /recipes/search?keyword=chicken  → Search by text
```

**Learning Focus:**

* Using `Containing` and `Like` queries
* Returning lists of filtered results

**Extension:**
Add category filter:

```txt
GET /recipes?category=dessert
```

---

## 5. 🏋️ Workout Log API

**Goal:** Use date fields with JPA.

**Description:**
Store daily workout entries with `exerciseName`, `sets`, `reps`, `date`.

**Endpoints:**

```txt
GET /workouts
POST /workouts
GET /workouts/today           → Only today’s workouts
```

**Learning Focus:**

* `LocalDate` in entities
* Date filtering

**Extension:**
Get workouts for a specific date:

```txt
GET /workouts?date=2025-02-01
```

---

## 6. 💰 Expense Tracker API

**Goal:** Perform filtered database queries based on date ranges.

**Description:**
Track expenses with `title`, `amount`, `category`, and `date`.

**Endpoints:**

```txt
GET /expenses
POST /expenses
GET /expenses?month=2025-03       → Filter by month
```

**Learning Focus:**

* `LocalDate` range queries
* Custom repository methods: `findByDateBetween(...)`

**Extension:**
Summary endpoint:

```txt
GET /expenses/summary
```

---

## 7. 📅 Appointment Scheduler API

**Goal:** Validate date/time and prevent overlaps.

**Description:**
Schedule appointments with `startTime`, `endTime`, `title`, and `clientName`.

**Endpoints:**

```txt
GET /appointments
POST /appointments
PUT /appointments/{id}
DELETE /appointments/{id}
```

**Learning Focus:**

* `LocalDateTime` usage
* Business logic in **Service**, not controller
* Time conflict detection

**Extension:**
Check slot availability:

```txt
GET /appointments/check?time=...
```

---

## 8. 🎬 Movie Ratings & Favorites API

**Goal:** Use custom query methods to filter and sort.

**Description:**
Store movies with `title`, `rating`, `genre`, and `isFavorite`.

**Endpoints:**

```txt
GET /movies
POST /movies
GET /movies?rating>=8
GET /movies?favorite=true
```

**Learning Focus:**

* Custom repository queries like:

```java
List<Movie> findByRatingGreaterThan(double rating);
```

**Extension:**
Sort:

```txt
GET /movies?sort=rating
```

---

## 9. 🌦️ Weather Tracker

**Goal:** Call external APIs using `RestTemplate` and store the result in DB.

**Description:**
Fetch weather data for a city from an external API and **cache it**.

**Endpoints:**

```txt
GET /weather/{city}
```

**Learning Focus:**

* JSON → DTO → Entity mapping
* Cache old results to avoid repeated API calls
* Compare timestamps before calling API again

**Extension:**
Auto-refresh weather every 30 minutes.

---

## 10. 📰 News Aggregator

**Goal:** Fetch external API data and allow saving favorites to DB.

**Description:**
Fetch live news articles and store user-favorited articles.

**Endpoints:**

```txt
GET /news                      → Live headlines (from API)
GET /news/search?query=java    → Filter API results
POST /news/favorites           → Save article to DB
GET /news/favorites            → View saved articles
```

**Learning Focus:**

* External API consumption
* Custom response mapping
* Persistence layer for favorites

**Extension:**
Allow tagging + category grouping.

---
Happy coding with Spring Boot and PostgreSQL! 🚀
