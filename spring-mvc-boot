# Spring Ecosystem Engineering Guide

A production-focused guide for building modern Spring applications.

This section covers:

* Spring MVC architecture
* Spring Boot application design
* Dependency injection
* Configuration management
* Validation
* Exception handling
* Testing strategies
* Observability
* Security practices
* Production deployment standards

---

# Part 7 — Spring MVC Engineering Guide

**File:** `spring/spring-mvc.md`
**Version:** 1.0

## Purpose

Spring MVC is the foundation for building web applications and REST APIs in the Spring ecosystem.

This guide defines standards for designing clean, testable, maintainable, and secure MVC-based applications.

---

# Philosophy

Core principles:

* Controllers should remain thin
* Business logic belongs in services
* Validation must happen at application boundaries
* HTTP is only a transport layer
* Request/response models must be separated from domain models

---

# Layered Architecture

Standard structure:

```
Controller
    ↓
Service
    ↓
Repository
    ↓
Database
```

Optional layers:

```
DTO Layer
    ↓
Mapper Layer
    ↓
Validation Layer
```

Responsibilities:

| Layer      | Responsibility        |
| ---------- | --------------------- |
| Controller | HTTP handling         |
| Service    | Business logic        |
| Repository | Data access           |
| DTO        | API contract          |
| Mapper     | Object transformation |
| Validation | Input correctness     |

---

# Controllers

## Rules

Controllers must:

* Be stateless
* Contain no business logic
* Delegate processing to services
* Handle only HTTP concerns

Example:

```java
@RestController
@RequestMapping("/api/customers")
public class CustomerController {

    private final CustomerService customerService;

    public CustomerController(CustomerService customerService) {
        this.customerService = customerService;
    }

    @GetMapping("/{id}")
    public ResponseEntity<CustomerDto> getCustomer(
            @PathVariable Long id) {

        return ResponseEntity.ok(
            customerService.getCustomer(id)
        );
    }
}
```

---

# Services

## Rules

Services should:

* Contain business logic
* Remain independent of HTTP
* Be reusable across:

  * Controllers
  * Scheduled jobs
  * Messaging consumers
* Be testable without Spring context where possible

---

# DTO Design

Use DTOs for API boundaries.

Avoid exposing:

* Entities directly
* Persistence models
* Internal domain structures

Example:

```java
public record CustomerResponse(
    Long id,
    String name,
    String email
) {}
```

---

# Validation

Use Bean Validation.

Example:

```java
public record CustomerRequest(

    @NotBlank
    String name,

    @Email
    String email

) {}
```

Validation should always happen at system boundaries.

---

# Exception Handling

Use centralized exception handling.

Example:

```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<String> handleNotFound(
            Exception ex) {

        return ResponseEntity
                .status(404)
                .body(ex.getMessage());
    }
}
```

Rules:

* Never expose stack traces
* Standardize error responses
* Handle exceptions consistently

---

# REST API Design

Follow REST principles.

Rules:

* Use nouns, not verbs
* Use correct HTTP methods
* Use meaningful status codes
* Keep URL patterns consistent

Examples:

```
GET     /customers/{id}

POST    /customers

PUT     /customers/{id}

DELETE  /customers/{id}
```

---

# Standard Error Response

Example:

```json
{
  "timestamp": "2026-01-01T10:00:00Z",
  "status": 404,
  "error": "Not Found",
  "message": "Customer not found",
  "path": "/api/customers/1"
}
```

---

# Logging

Log:

* Minimal request information
* Errors with stack traces
* Important business events

Avoid logging:

* Passwords
* Tokens
* Sensitive user data

---

# Security Integration

Spring MVC applications should integrate with:

* Spring Security
* JWT authentication
* Role-based authorization

Controllers should not implement security rules manually.

---

# Testing Strategy

Recommended:

| Type              | Purpose             |
| ----------------- | ------------------- |
| `@WebMvcTest`     | Controller testing  |
| Unit Tests        | Service testing     |
| `@SpringBootTest` | Integration testing |

Prefer service-level testing over controller-heavy testing.

---

# Performance Guidelines

Avoid:

* Heavy controller logic
* N+1 database queries
* Unbounded queries

Use:

* Pagination
* Optimized queries
* Proper projections

---

# AI Coding Checklist

When generating Spring MVC code:

* Keep controllers thin
* Use DTOs for API boundaries
* Add validation annotations
* Use centralized exception handling
* Avoid business logic in controllers
* Follow REST conventions

---

# Part 8 — Spring Boot Engineering Guide

**File:** `spring/spring-boot.md`
**Version:** 1.0

---

# Purpose

Spring Boot provides opinionated defaults for building production-ready applications.

This guide defines standards for scalable and maintainable Spring Boot systems.

---

# Philosophy

Principles:

* Convention over configuration
* Externalized configuration
* Stateless services
* Simple architecture
* Production readiness by default

---

# Project Structure

Recommended:

```
com.example.app

├── config
├── controller
├── service
├── repository
├── domain
├── dto
├── mapper
├── exception
├── security
├── client
└── util
```

---

# Configuration

Use:

```
application.yml
```

Support:

* Environment-specific configuration
* External secrets
* Feature flags
* Logging configuration

Avoid:

* Hardcoded configuration values

---

# Dependency Injection

Always prefer constructor injection.

Example:

```java
@Service
public class OrderService {

    private final OrderRepository repository;

    public OrderService(OrderRepository repository) {
        this.repository = repository;
    }
}
```

Avoid:

```java
@Autowired
private OrderRepository repository;
```

---

# REST API Standards

Follow Spring MVC rules:

Required:

* DTO usage
* Validation
* Correct HTTP responses
* Consistent errors

---

# Exception Handling

Use:

```java
@RestControllerAdvice
```

Never return:

* Stack traces
* Internal exceptions
* Database errors

---

# Security

Integrate:

* Spring Security
* JWT authentication
* Role-based access control

Ensure:

* Stateless authentication
* Secure password storage
* Proper CORS configuration
* CSRF protection when required

---

# Spring Profiles

Recommended profiles:

```
dev
test
staging
production
```

Never mix environment configurations.

---

# Logging

Use structured logging.

Include:

* Request ID
* User context
* Service name
* Operation name

Avoid excessive production logs.

---

# Spring Actuator

Enable:

* Health checks
* Metrics
* Application information

Production rules:

* Secure actuator endpoints
* Avoid exposing sensitive data

---

# Database Integration

Use Spring Data JPA carefully.

Avoid:

* N+1 queries
* Large uncontrolled queries

Prefer:

* Pagination
* Projections
* Explicit transactions

---

# Transactions

Use:

```java
@Transactional
```

Guidelines:

* Apply at service layer
* Avoid unnecessary transactions
* Understand propagation behavior

---

# Scheduling

Use Spring Scheduler for:

* Batch processing
* Periodic jobs

Avoid:

* Long-running blocking tasks

---

# Messaging Integration

Supported systems:

* Kafka
* RabbitMQ

Requirements:

* Idempotent consumers
* Retry policies
* Dead-letter queues

---

# Testing Strategy

Recommended:

| Type              | Purpose            |
| ----------------- | ------------------ |
| Unit Tests        | Business logic     |
| `@WebMvcTest`     | MVC testing        |
| `@DataJpaTest`    | Repository testing |
| `@SpringBootTest` | Full integration   |

Mock external dependencies.

---

# Performance

Focus areas:

* Connection pooling
* Query optimization
* Caching
* Async processing
* Thread management
* Virtual threads where appropriate

---

# Observability

Production applications should include:

* Metrics
* Distributed tracing
* Structured logging
* Health checks

Common tooling:

* Micrometer
* OpenTelemetry
* Spring Actuator

---

# AI Coding Checklist

When generating Spring Boot code:

* Use constructor injection
* Keep services stateless
* Externalize configuration
* Add validation
* Implement exception handling
* Avoid unnecessary annotations
* Include production readiness:

  * Logging
  * Metrics
  * Security

---

# Next Step — Part 9

Next topic:

## Spring Security + JWT Engineering Guide

Topics:

* Authentication flows
* JWT architecture
* Role-based access control
* Security filters
* OAuth2 concepts
* OWASP practices
* Secure API design

Future ecosystem topics:

* Kafka
* Apache Camel
* RabbitMQ
* GraphQL
* gRPC
