# AI Engineering Guidelines

> **Enterprise AI Coding Standards for Java, Spring, Distributed Systems, Frontend, Cloud, and Modern Software Engineering**

A comprehensive collection of engineering standards, AI coding instructions, and technology-specific knowledge designed to help developers and AI coding assistants build secure, maintainable, scalable, and production-ready software.

---

# Overview

This repository serves as an enterprise engineering handbook for both human developers and AI coding assistants.

It defines coding standards, architectural principles, security best practices, testing strategies, and technology-specific guidance for modern software development.

While inspired by broadly accepted AI-assisted software engineering practices—such as simplicity, readability, incremental development, and rigorous review—it extends those ideas with practical standards for enterprise Java ecosystems, distributed systems, cloud-native applications, and frontend development.

---

# Goals

This repository aims to:

* Produce maintainable, readable, and secure code
* Favor correctness before optimization
* Encourage small, composable, and testable components
* Reduce cognitive complexity
* Make repositories AI-friendly through consistent conventions
* Standardize engineering practices across teams

---

# Supported Technologies

## Backend

* Java 8
* Java 17
* Java 21
* Java 26
* Spring MVC
* Spring Boot
* Spring Security
* JWT
* Apache Camel
* Kafka
* RabbitMQ
* GraphQL
* gRPC
* Groovy

## Databases

* PostgreSQL
* SQL
* Redis
* Hazelcast
* Solr
* Snowflake

## Frontend

* JavaScript
* jQuery
* Angular
* React
* Vue.js
* FreeMarker (FTL)

---

# Repository Structure

```text
.
├── CLAUDE.md
├── SKILL.md
│
├── java/
├── spring/
├── integration/
├── database/
├── frontend/
├── architecture/
├── security/
├── testing/
├── templates/
└── examples/
```

---

# Engineering Philosophy

Our engineering standards are built upon a few fundamental principles:

* Keep code simple
* Prefer explicit behavior over clever implementations
* Optimize for readability
* Write tests alongside code
* Keep functions focused
* Minimize unnecessary dependencies
* Design for change
* Automate repetitive work
* Document architectural decisions
* Continuously improve through refactoring

---

# Intended Audience

This repository is designed for:

* Software Engineers
* Technical Leads
* Software Architects
* DevOps Engineers
* QA Engineers
* AI Coding Assistants

---

# Compatible AI Coding Assistants

These standards are designed to work with modern AI-assisted development tools, including:

* Claude Code
* GitHub Copilot
* Cursor
* Codex
* Cline
* Roo Code
* Gemini CLI
* Windsurf

---

# Guiding Engineering Principles

The guidance throughout this repository emphasizes:

* Clean Code
* SOLID Principles
* DRY
* KISS
* YAGNI
* Hexagonal Architecture
* Domain-Driven Design (DDD)
* Event-Driven Architecture
* Twelve-Factor App
* Secure by Default
* Test-Driven Development (where appropriate)

---

# Repository Contents

## 📘 CLAUDE.md — AI Coding Constitution

Defines the expected engineering behavior of an AI coding assistant while working within a repository.

It includes guidance for:

* AI assistant behavior
* Repository awareness
* Incremental development
* Refactoring policy
* Testing strategy
* Documentation requirements
* Security defaults
* Performance defaults
* Error handling
* Logging standards
* Code review expectations
* Architecture rules
* Git workflow
* Commit message conventions
* AI self-review checklist
* AI verification checklist

---

## 📗 SKILL.md — Technology Knowledge Base

A comprehensive technical reference that provides AI assistants with best practices and modern guidance across:

* Java
* Spring Ecosystem
* Security
* Distributed Systems
* Databases
* Frontend Frameworks
* Cloud Native Engineering
* Observability
* Performance Optimization
* AI-assisted Development

---

## 📙 Technology-Specific AI Instructions

Dedicated instruction files that teach AI assistants how to generate high-quality code for specific technologies and frameworks.

Examples include:

* Java
* Spring Boot
* Spring Security
* PostgreSQL
* Kafka
* RabbitMQ
* Apache Camel
* Redis
* Docker
* Kubernetes
* React
* Angular
* Vue
* REST APIs
* GraphQL
* gRPC

---

# Contributing

Every contribution should:

* Compile successfully
* Include appropriate tests
* Update documentation when necessary
* Pass static analysis
* Preserve backward compatibility unless intentionally changed
* Maintain readability and simplicity

---

# CLAUDE.md (Initial Version)

## Purpose

`CLAUDE.md` defines the default engineering behavior expected from an AI coding assistant working within this repository.

The assistant should consistently prioritize:

* Maintainability
* Correctness
* Security
* Readability
* Long-term software quality

---

## Core Principles

### 1. Readability First

* Write code that is easy to understand.
* Prefer descriptive names over abbreviations.
* Avoid clever implementations when a straightforward solution exists.

### 2. Simplicity

* Choose the simplest solution that satisfies current requirements.
* Avoid speculative abstractions.
* Do not introduce unnecessary frameworks or dependencies.

### 3. Incremental Development

* Prefer small, reviewable changes.
* Split large refactorings into logical commits.
* Leave the codebase in a working state after every change.

### 4. Correctness Before Performance

* Never sacrifice correctness for premature optimization.
* Measure performance before optimizing.
* Optimize only verified bottlenecks.

### 5. Testing

Every functional change should include:

* Unit tests
* Integration tests (when appropriate)
* Regression tests for defect fixes

Untested code should be considered incomplete.

---

## Code Style

### Methods

* One responsibility per method
* Prefer early returns
* Avoid deep nesting
* Keep methods concise

### Classes

* One concept per class
* Avoid God Objects
* Prefer composition over inheritance

### Naming

Use descriptive, domain-driven names.

Avoid generic names such as:

* Data
* Utils
* Helper
* Manager
* Processor

---

## Error Handling

* Never ignore exceptions
* Preserve stack traces
* Provide meaningful error messages
* Avoid empty `catch` blocks
* Use checked and unchecked exceptions consistently

---

## Logging

* Log meaningful business events
* Never log sensitive information
* Prefer structured logging
* Use parameterized log statements

---

## Security

* Never hardcode passwords, API keys, JWT secrets, or database credentials
* Validate all external input
* Follow the Principle of Least Privilege
* Protect against common web vulnerabilities

---

## Refactoring

* Improve code without changing observable behavior
* Remove duplication
* Eliminate dead code
* Continuously reduce complexity

---

## Documentation

Document:

* Architectural decisions
* Public APIs
* Complex algorithms
* Configuration
* Deployment considerations

Keep documentation synchronized with code.

---

## AI Assistant Checklist

Before proposing code, always:

* Understand the existing architecture
* Reuse existing abstractions
* Minimize unnecessary changes
* Preserve backward compatibility whenever possible
* Add or update tests
* Consider security implications
* Consider performance impacts
* Consider observability
* Consider long-term maintainability

---

## Pull Request Expectations

Every pull request should clearly describe:

* Purpose
* Design rationale
* Testing performed
* Risks
* Rollback strategy (when applicable)

---

## Prohibited Practices

Do **not**:

* Duplicate business logic
* Ignore failing tests
* Disable security checks
* Suppress warnings without explanation
* Introduce unnecessary complexity
* Commit secrets
* Commit generated binaries
* Add dependencies without justification

---

## Default AI Behavior

When generating code, the AI assistant should:

* Understand the problem before proposing changes
* Ask clarifying questions when requirements are ambiguous
* Explain significant design decisions
* Follow existing project conventions
* Produce production-quality code
* Include tests when appropriate
* Keep responses concise yet complete
* Clearly state assumptions
* Recommend incremental implementation for large changes
* Always prioritize long-term maintainability over short-term convenience
