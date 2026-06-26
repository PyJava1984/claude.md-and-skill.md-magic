# Architecture Layer — Clean Architecture Guide

**File:** `architecture/clean-architecture.md`
**Version:** 1.0

---

# Purpose

Clean Architecture defines strict dependency rules to ensure systems remain:

* Testable
* Maintainable
* Independent of frameworks
* Independent of databases
* Independent of UI technologies

The goal is to build systems where business rules remain stable while external technologies can change.

---

# Philosophy

Core principles:

* Business logic is the center of the system
* Frameworks are implementation details
* External systems should be replaceable
* Dependencies must point inward
* Testing should not require infrastructure

---

# Architecture Overview

```text id="a7p4mz"
        Presentation

             ↓

        Application

             ↓

          Domain

             ↑

     Infrastructure
```

---

# Layers

---

# 1. Domain Layer (Core)

The Domain Layer contains the most important business concepts.

Responsibilities:

* Entities
* Value Objects
* Domain Rules
* Business Behavior

---

# Rules

The Domain Layer:

* Has no external dependencies
* Does not know about databases
* Does not know about frameworks
* Does not know about APIs

---

# Example

```text id="k8m2vp"
Customer

Order

Payment

Invoice
```

These represent business concepts.

---

# Domain Layer Must NOT Contain

Avoid:

* Database calls
* REST controllers
* Framework annotations
* Messaging logic

---

# 2. Application Layer

The Application Layer defines system behavior.

Responsibilities:

* Use cases
* Application workflows
* Orchestration logic
* Interfaces (ports)

---

# Use Cases

Use cases represent actions the system performs.

Examples:

```text id="p3v9kx"
CreateCustomer

ProcessPayment

GenerateInvoice
```

---

# Use Case Rules

Use cases:

* Coordinate domain objects
* Define application flow
* Remain framework independent

---

# Example Flow

```text id="m7q2cw"
Create Order Use Case

        ↓

Validate Rules

        ↓

Save Order

        ↓

Publish Event
```

---

# 3. Infrastructure Layer

Infrastructure contains external implementations.

Examples:

* Databases
* Messaging systems
* External APIs
* File systems
* Framework adapters

---

# Responsibilities

Infrastructure implements interfaces defined by the application layer.

Example:

Application:

```text id="u6n8pz"
CustomerRepository
(interface)
```

Infrastructure:

```text id="r4x7mv"
PostgresCustomerRepository
(implementation)
```

---

# 4. Presentation Layer

The Presentation Layer handles external communication.

Examples:

* REST APIs
* Controllers
* UI logic
* Request mapping

---

# Rules

Presentation should:

* Receive input
* Validate transport concerns
* Call use cases
* Return responses

---

# Avoid

Controllers containing:

* Business rules
* Database logic
* Complex workflows

---

# Dependency Rule

The most important rule:

> Dependencies must point inward.

---

# Correct Direction

```text id="z8q3ny"
UI

 ↓

Application

 ↓

Domain
```

Infrastructure:

```text id="h4m8ks"
Infrastructure

        ↓

Application

        ↓

Domain
```

---

# Domain Independence

The Domain Layer should never depend on:

* Spring
* Hibernate
* Databases
* REST
* Messaging frameworks

---

# Entities

Entities contain:

* Business rules
* State
* Behavior
* Identity

---

# Good Entity

Example:

```text id="v5q9mk"
Order

- addItem()
- calculateTotal()
- cancel()
```

---

# Avoid Anemic Domain Models

Bad:

```text id="c2x7pw"
Order

- getters
- setters
- no behavior
```

Business logic should live where the business concept exists.

---

# Value Objects

Value Objects represent concepts without identity.

Examples:

```text id="m3k8vz"
Money

EmailAddress

Address
```

They should be:

* Immutable
* Self-validating

---

# Use Cases

Use cases:

* Represent application behavior
* Are framework independent
* Coordinate domain logic

---

# Example

```text id="w6n2qa"
PlaceOrderUseCase

        ↓

Order Entity

        ↓

Order Repository Port
```

---

# Interfaces (Ports & Adapters)

Interfaces create boundaries.

Use interfaces to:

* Decouple infrastructure
* Improve testing
* Replace implementations easily

---

# Example

Port:

```java id="d7x3mv"
interface PaymentGateway {

    PaymentResult charge();

}
```

Adapter:

```java id="f5q8mz"
StripePaymentGateway
```

---

# Framework Independence

Avoid placing framework details in core logic.

---

# Avoid

## Framework Annotations in Domain

Example:

```java id="j8m4pv"
@Entity
class Customer
```

Prefer keeping persistence concerns outside the domain.

---

# Avoid

## Direct Database Calls

Bad:

```text id="q9k2mw"
Business Logic

       ↓

Database
```

---

# Testing Strategy

Each layer has a different testing approach.

---

# Domain Layer

Testing:

* Unit tests
* Business rule tests

No infrastructure required.

---

# Application Layer

Testing:

* Use case tests
* Mock external dependencies

---

# Infrastructure Layer

Testing:

* Integration tests
* Database tests
* External system tests

---

# Presentation Layer

Testing:

* API tests
* Controller tests
* UI tests

---

# Anti-Patterns

---

# Fat Controllers

Problem:

Controllers become application services.

Solution:

Move logic into use cases.

---

# Anemic Domain Models

Problem:

Entities only store data.

Solution:

Put business behavior inside domain objects.

---

# Business Logic in Infrastructure

Problem:

Database adapters become business services.

Solution:

Keep infrastructure focused on technical concerns.

---

# Framework Coupling

Problem:

Business rules depend on technology.

Solution:

Move framework details outward.

---

# Tight Database Coupling

Problem:

Changing databases requires rewriting business logic.

Solution:

Use repository interfaces.

---

# AI Coding Checklist

When generating architecture:

* Ensure dependency direction is correct
* Keep domain logic pure
* Move framework logic outward
* Use interfaces for external dependencies
* Keep use cases independent
* Design for testability
* Avoid coupling business rules to infrastructure

---

# Architecture Principle Summary

```text id="n7m5vx"
Domain Rules

      ↓

Application Behavior

      ↓

External Implementations
```

A clean architecture keeps business value stable while technology evolves.
