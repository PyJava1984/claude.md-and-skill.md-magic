# Enterprise AI Engineering Skills Guide

**Version:** 1.0

---

# Purpose

The **Enterprise AI Engineering Skills Guide** defines the engineering knowledge, coding standards, architectural principles, technology expectations, and AI-assisted development practices used throughout this repository.

It serves as a shared reference for both **software engineers** and **AI coding assistants**, ensuring code generated or reviewed by AI aligns with enterprise software engineering best practices.

## Objectives

* Produce readable, maintainable, and production-ready code
* Encourage consistent engineering practices across teams
* Support AI coding assistants with clear repository conventions
* Standardize architecture, testing, security, documentation, and operational practices
* Promote long-term maintainability over short-term convenience

---

# Engineering Principles

Every engineering decision should strive for:

* **Simplicity**
* **Correctness**
* **Readability**
* **Testability**
* **Maintainability**
* **Observability**
* **Security**
* **Performance** (only after correctness)
* **Backward compatibility**, where appropriate

> **Guiding Principle:** Avoid unnecessary abstraction and speculative design. Build only what is needed today while keeping the codebase adaptable for tomorrow.

---

# Universal Coding Standards

## Naming

Use descriptive, domain-specific names that clearly communicate intent.

### Preferred

```java
CustomerOrderService
InvoiceRepository
UserRegistrationController
```

### Avoid

```text
Util
Helper
Manager
Common
Misc
Processor
```

Always use **ubiquitous language** from the business domain consistently throughout the codebase.

---

## Functions

Every function should:

* Have a single responsibility
* Be concise and focused
* Minimize nesting
* Prefer early returns
* Avoid hidden side effects
* Be easy to test independently

---

## Classes

Each class should represent a single concept or responsibility.

Prefer:

* Composition over inheritance
* Constructor injection for dependencies
* High cohesion
* Explicit dependencies

Avoid:

* God classes
* Feature envy
* Excessive coupling

---

# Error Handling

Applications should fail predictably and provide actionable diagnostics.

Guidelines:

* Fail fast
* Never ignore exceptions
* Preserve stack traces
* Provide meaningful error messages
* Separate business exceptions from infrastructure failures
* Use domain-specific exception types where appropriate

---

# Logging

Applications should log meaningful operational information.

Log:

* Business events
* Security events
* Integration failures
* Retry attempts
* Startup configuration summaries

Do **not** log:

* Passwords
* API keys
* Authentication tokens
* Secrets
* Sensitive personal information unless required and adequately protected

Use structured logging and correlation IDs whenever possible.

---

# Architecture Principles

Select the simplest architecture that satisfies the system requirements.

Preferred architectural styles include:

* Layered Architecture
* Hexagonal Architecture (Ports & Adapters)
* Clean Architecture
* Event-Driven Architecture
* Modular Monoliths
* Microservices (only when justified)

Architecture should emphasize:

* Loose coupling
* Clear boundaries
* Independent deployability (where applicable)
* Domain-focused design

---

# Design Principles

Apply established software engineering principles:

* SOLID
* DRY
* KISS
* YAGNI
* Separation of Concerns
* Dependency Inversion
* Encapsulation
* High Cohesion
* Low Coupling

---

# Java

## Supported Versions

* Java 8
* Java 17
* Java 21
* Java 26

## General Guidance

Prefer:

* Immutable objects
* Records where available
* Sealed classes when appropriate
* `Optional` for optional return values instead of `null`
* Streams only when they improve readability
* Virtual Threads (Java 21+) for I/O-bound workloads
* Structured Concurrency for coordinating concurrent tasks

Avoid:

* Premature optimization
* Overly complex stream pipelines
* Mutable shared state

---

# Spring Ecosystem

Supported technologies include:

* Spring Framework
* Spring MVC
* Spring Boot
* Spring Security
* Spring Data
* Spring Validation
* Spring Transactions

Engineering recommendations:

* Constructor injection
* Stateless services
* Externalized configuration
* Secrets outside source control
* Consistent exception handling
* Health checks
* Metrics
* Graceful shutdown
* Minimal framework magic

---

# Security

Every application should be secure by default.

Minimum expectations:

* Principle of Least Privilege
* Input validation
* Output encoding
* Strong authentication
* Secure password storage
* JWT validation
* Secret management outside source control
* OWASP Top 10 awareness
* Security headers
* HTTPS by default

---

# SQL

Write SQL that is:

* Readable
* Parameterized
* Indexed appropriately
* Optimized using execution plans
* Protected from SQL injection

Prefer:

* Explicit joins
* Meaningful aliases
* Well-documented complex queries

---

# PostgreSQL

Recommended practices:

* Appropriate indexing
* Execution plan analysis
* Correct transaction usage
* Normalization unless justified otherwise
* Monitoring slow queries
* Table partitioning for large datasets

---

# Redis

Appropriate use cases include:

* Caching
* Distributed locking (with care)
* Rate limiting
* Session storage

Recommendations:

* Define explicit TTL values
* Plan cache invalidation strategies
* Monitor cache hit ratios

---

# Kafka

Design messaging systems for resilience.

Guidelines:

* Idempotent consumers
* Explicit retry policies
* Dead-letter topics
* Schema versioning
* Consumer lag monitoring
* Reasonable message sizes

---

# RabbitMQ

Recommended scenarios:

* Work queues
* Reliable messaging
* Background task processing

Prefer:

* Durable queues
* Explicit acknowledgments
* Dead-letter exchanges where appropriate

---

# Apache Camel

Design routes that are:

* Small
* Reusable
* Easy to test
* Easy to monitor

Prefer:

* Route templates
* Centralized error handling
* Externalized endpoint configuration

Avoid monolithic route definitions.

---

# GraphQL

Best practices:

* Design around domain models
* Prevent N+1 query issues
* Implement pagination
* Validate inputs
* Version schemas cautiously

---

# gRPC

Prefer:

* Strong service contracts
* Backward-compatible schema evolution
* Deadlines and timeouts
* Streaming only when beneficial
* Consistent error mapping

---

# Solr

Recommendations:

* Well-designed schemas
* Appropriate analyzers
* Efficient indexing
* Query optimization
* Replication planning
* Continuous monitoring

---

# Hazelcast

Use Hazelcast for:

* Distributed caching
* Shared state
* Cluster coordination

Minimize mutable shared objects and unnecessary distributed state.

---

# Snowflake

Best practices:

* Cost-aware query design
* Proper clustering strategies
* Appropriate warehouse sizing
* Secure data sharing
* Role-based access control

---

# Frontend

## JavaScript

Prefer:

* ES6+
* Modules
* Async/Await
* Strict linting
* Modern build tooling

Avoid unnecessary global state.

---

## jQuery

Maintain existing implementations when necessary.

Avoid introducing new jQuery-based solutions unless modernization is not feasible.

---

## Angular

Prefer:

* Standalone Components
* Reactive Forms
* RxJS best practices
* Lazy Loading
* Strong typing
* OnPush change detection where appropriate

---

## React

Prefer:

* Functional Components
* Hooks
* Composition
* Minimal Context usage
* State management proportional to application complexity

---

## Vue

Prefer:

* Composition API
* Single File Components
* Reactive programming patterns
* Clear separation of concerns

---

# Templates

## FreeMarker (FTL)

Templates should focus exclusively on presentation.

Avoid:

* Business logic
* Complex calculations

Always escape output appropriately.

---

# Groovy

Use Groovy primarily for:

* Build automation
* DSLs
* Administrative tooling
* Automation scripts

Keep production logic predictable and maintainable.

---

# Testing

Adopt the Testing Pyramid:

1. Unit Tests
2. Integration Tests
3. End-to-End Tests

Tests should be:

* Fast
* Deterministic
* Independent
* Repeatable

Mock only external dependencies.

---

# Performance

Measure before optimizing.

Focus optimization efforts on:

* Database performance
* Memory allocation
* Network latency
* Serialization overhead
* Thread contention

Document significant optimizations and benchmark results.

---

# Observability

Applications should provide:

* Structured logging
* Metrics
* Health endpoints
* Distributed tracing
* Correlation IDs

Operational visibility is a core engineering requirement, not an optional enhancement.

---

# Documentation

Documentation is part of the software deliverable.

Maintain:

* Architecture diagrams
* API documentation
* Architecture Decision Records (ADRs)
* Deployment guides
* Operational runbooks

Ensure documentation evolves alongside the codebase.

---

# AI-Assisted Development

When working with AI coding assistants:

* Understand the existing codebase before making changes
* Preserve architectural consistency
* Explain significant assumptions
* Prefer incremental, reviewable changes
* Generate or update tests
* Update documentation when behavior changes
* Avoid unnecessary dependencies
* Recommend refactoring only when it provides measurable value

---

# Code Review Checklist

Before merging, verify:

* ✅ Correctness
* ✅ Readability
* ✅ Security
* ✅ Test coverage
* ✅ Performance impact
* ✅ Error handling
* ✅ Logging
* ✅ Backward compatibility
* ✅ Documentation updates
* ✅ Dependency justification

---

# Continuous Improvement

This guide is a living document.

Review and update it regularly to incorporate:

* New Java language features
* Framework improvements
* Evolving security guidance
* Cloud-native best practices
* AI-assisted development techniques
* Lessons learned from production systems

Continuous improvement ensures the repository remains aligned with modern enterprise engineering practices while enabling both developers and AI assistants to deliver high-quality software consistently.
