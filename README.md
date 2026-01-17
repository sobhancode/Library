# Library Management System (Monolithic)

## 1. Overview

A single Spring Boot application that manages users, books, borrowing, returns, fines, notifications, and security. This project is ideal to showcase **real-world backend skills** with authentication, authorization, business logic, scheduling, and deployment.

---

## 2. Tech Stack

* **Backend:** Spring Boot
* **Security:** Spring Security + JWT
* **Database:** MySQL / PostgreSQL
* **ORM:** Spring Data JPA + Hibernate
* **Email:** JavaMailSender (SMTP)
* **Scheduling:** @Scheduled
* **Build Tool:** Maven
* **Containerization:** Docker
* **Deployment:** AWS EC2

---

## 3. Core Features

### Authentication & Authorization

* User registration & login
* JWT-based authentication
* Role-based access control

  * `ROLE_ADMIN`
  * `ROLE_USER`

### Book Management (Admin)

* Add new books
* Update book details
* Delete books
* Manage available copies

### User Operations

* Search books (title, author, ISBN)
* Borrow books
* Return books
* View borrowed books & fines

### Fine Management

* Fine calculated for overdue books
* Fine amount configurable (e.g. ₹5/day)

### Email Notifications

* Borrow confirmation email
* Due-date reminder email
* Overdue notification email

### Scheduled Jobs

* Daily job to check overdue books
* Auto-calculate fines
* Trigger email notifications

---

## 4. Database Design (Entities)

### User

* id
* name
* email
* password
* role
* enabled

### Book

* id
* title
* author
* isbn
* totalCopies
* availableCopies

### BorrowRecord

* id
* user (ManyToOne)
* book (ManyToOne)
* borrowDate
* dueDate
* returnDate
* fineAmount
* status (BORROWED / RETURNED / OVERDUE)

### Role (Optional – if normalized)

* id
* name

---

## 5. API Design (REST)

### Auth APIs

* `POST /api/auth/register`
* `POST /api/auth/login`

### Book APIs

* `POST /api/books` (ADMIN)
* `PUT /api/books/{id}` (ADMIN)
* `DELETE /api/books/{id}` (ADMIN)
* `GET /api/books`
* `GET /api/books/search`

### Borrow APIs

* `POST /api/borrow/{bookId}`
* `POST /api/return/{borrowId}`
* `GET /api/my-borrows`

### Admin APIs

* `GET /api/admin/overdue-records`

---

## 6. Business Logic Highlights

* Prevent borrowing if no copies available
* One user cannot borrow the same book twice
* Auto-update available copies
* Fine calculation logic:

  * `(currentDate - dueDate) * finePerDay`

---

## 7. Scheduled Job

```java
@Scheduled(cron = "0 0 1 * * ?")
public void checkOverdueBooks() {
    // find overdue records
    // update fine
    // send email notification
}
```

Runs daily at **1 AM**.

---

## 8. Security Flow

1. User logs in
2. JWT token generated
3. Token sent in `Authorization: Bearer <token>`
4. JWT filter validates token
5. Role-based access enforced

---

## 9. Docker Setup

* Dockerfile for Spring Boot app
* application-prod.yml for AWS
* MySQL/Postgres container (optional)

---

## 10. AWS Deployment

* EC2 instance (Amazon Linux / Ubuntu)
* Docker installed
* App exposed via port 8080
* Security group configured

---

## 11. What This Project Demonstrates

* Real-world **monolithic architecture**
* Secure REST APIs
* Strong **business logic**
* Scheduled background jobs
* Email integration
* Database relationships
* Production-ready deployment

---

# Library
Built a Library Management System using Spring Boot with JWT security, role-based access, scheduled jobs for overdue tracking, fine calculation, email notifications, Dockerized deployment on AWS EC2.
