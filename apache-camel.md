# Integration Layer — Apache Camel Engineering Guide

**File:** `integration/apache-camel.md`
**Version:** 1.0

---

# Purpose

Apache Camel is an integration framework based on Enterprise Integration Patterns (EIP).

It is used for:

* Connecting heterogeneous systems
* Routing messages
* Transforming data
* Mediating communication between services

Camel should be treated as an integration layer, not a business logic layer.

---

# Philosophy

Core principles:

* Integration logic should be declarative
* Routes should be small and composable
* Business logic belongs in services
* Camel should act as a mediation layer
* Prefer readability over complex DSL chains

---

# Camel Architecture Overview

Basic flow:

```text
Source System

      |
      ↓

Camel Route

      |
      ↓

Transformation / Routing

      |
      ↓

Target System
```

---

# Core Concepts

| Concept   | Description             |
| --------- | ----------------------- |
| Route     | Message flow definition |
| Endpoint  | Source or destination   |
| Processor | Message processing step |
| Exchange  | Message container       |
| Component | External connector      |

---

# Components

Camel connects systems through components:

Examples:

```text
HTTP

JMS

Kafka

RabbitMQ

File

Database

REST
```

Components provide connectivity while routes define behavior.

---

# Route Design Principles

## Rules

A route should:

* Have one responsibility
* Be easy to understand
* Avoid excessive processing steps
* Delegate business logic externally
* Externalize configuration

---

# Example Route

```java
from("direct:orderCreated")

    .routeId("order-created-route")

    .log("Processing order ${body}")

    .bean(orderService, "processOrder")

    .to("kafka:order-events");
```

---

# Route Responsibilities

A route should handle:

* Routing
* Transformation
* Validation
* Integration concerns

A route should not handle:

* Business decisions
* Complex calculations
* Domain workflows

---

# Enterprise Integration Patterns (EIP)

Apache Camel implements many classic patterns:

* Content-Based Router
* Message Translator
* Splitter
* Aggregator
* Dead Letter Channel
* Retry Policies
* Recipient List
* Wire Tap

---

# Content-Based Routing

Used when routing depends on message content.

Example:

```java
from("direct:orders")

    .choice()

        .when(simple("${body.amount} > 1000"))

            .to("direct:highValueOrders")

        .otherwise()

            .to("direct:standardOrders");
```

---

# Message Transformation

Purpose:

Convert one message format into another.

Examples:

```text
JSON → XML

API Model → Internal Model

External Event → Domain Event
```

---

# Transformation Rules

Prefer:

* Dedicated mapper classes
* Serialization libraries
* Reusable transformers

Avoid:

* Large inline expressions
* Complex route transformations

---

# Error Handling

Define global error handling.

Example:

```java
onException(Exception.class)

    .maximumRedeliveries(3)

    .redeliveryDelay(1000)

    .to("log:error");
```

---

# Error Handling Strategy

Classify failures:

| Type      | Action                  |
| --------- | ----------------------- |
| Temporary | Retry                   |
| Permanent | Route to error handling |
| Unknown   | Log and alert           |

---

# Dead Letter Channel

Use when messages cannot be processed.

Benefits:

* Prevent message loss
* Improve observability
* Allow manual recovery

---

# Splitter Pattern

Used to break large messages into smaller units.

Example:

```java
from("direct:batch")

    .split(body())

    .to("bean:itemProcessor");
```

Common use cases:

* Batch processing
* File processing
* Bulk API calls

---

# Aggregator Pattern

Used to combine messages.

Requirements:

* Correlation identifier
* Completion strategy
* Timeout handling

Example concepts:

```text
Order Items

      ↓

Aggregate

      ↓

Complete Order
```

---

# Logging Strategy

Log at route boundaries.

Recommended:

* Incoming message metadata
* Processing milestones
* Errors

Avoid:

* Sensitive payload logging
* Credentials
* Personal information

---

# Performance Guidelines

Optimize:

## Route Complexity

Avoid:

* Huge routes
* Excessive processors

---

## Streaming

Use streaming for:

* Large files
* Large message sets

---

## Thread Management

Tune:

* Thread pools
* Consumer concurrency
* Async processing

---

# Security

Secure integration flows.

Requirements:

* Secure endpoints
* Validate incoming messages
* Sanitize external inputs
* Restrict internal routes

---

# Endpoint Security

Protect:

```text
HTTP endpoints

Message brokers

External connectors
```

Use:

* TLS
* Authentication
* Authorization

---

# Camel Anti-Patterns

Avoid:

## Business Logic Inside Routes

Bad:

```text
Route

  ↓

100 lines of business rules
```

Better:

```text
Route

  ↓

Service Layer

  ↓

Business Logic
```

---

## Extremely Long Routes

Avoid:

```text
A → B → C → D → E → F → G → H
```

Prefer:

Small reusable routes.

---

## Hardcoded Endpoints

Avoid:

```java
.to("http://production-server/api")
```

Prefer:

External configuration.

---

## Uncontrolled Retry Loops

Avoid:

```text
Retry forever
```

Always define:

* Maximum attempts
* Backoff strategy
* Failure handling

---

## Mixing Multiple Integration Concerns

Avoid one route handling:

* API calls
* Messaging
* File processing
* Database synchronization

Separate responsibilities.

---

# Production Checklist

Before deployment:

* Routes are small and readable
* Error handling exists
* Retry strategy is defined
* Endpoints are externalized
* Sensitive data is protected
* Monitoring is enabled

---

# AI Coding Checklist

When generating Apache Camel routes:

* Keep routes small
* Use EIP patterns correctly
* Extract business logic into services
* Define error handling
* Add retry policies
* Avoid complex DSL chains
* Externalize configuration
* Prefer readability over cleverness

---

# Next Step — Integration Layer (Advanced)

Upcoming guides:

```text
integration/

├── graphql.md
└── grpc.md
```

Topics:

* API design
* Contract-first development
* High-performance RPC
* Schema evolution
* Service communication patterns
