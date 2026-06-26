# Integration Layer — RabbitMQ Engineering Guide

**File:** `integration/rabbitmq.md`
**Version:** 1.0

---

# Purpose

RabbitMQ is a message broker designed for:

* Task distribution
* Command processing
* Reliable queue-based messaging
* Asynchronous workflows

Unlike Kafka, which is optimized for event streaming, RabbitMQ is optimized for:

> Work queues and command-driven messaging.

---

# Philosophy

Core principles:

* Messages represent tasks or commands
* Delivery reliability is mandatory
* Consumers should remain stateless
* Acknowledgment is required
* Failure handling must be explicit

---

# RabbitMQ Architecture Overview

Basic flow:

```text
Producer

    |
    ↓

Exchange

    |
    ↓

Queue

    |
    ↓

Consumer Worker
```

---

# Core Concepts

| Concept  | Description                |
| -------- | -------------------------- |
| Exchange | Message routing layer      |
| Queue    | Message storage buffer     |
| Binding  | Routing relationship       |
| Consumer | Worker processing messages |

---

# Exchange Types

RabbitMQ supports multiple routing strategies.

---

# Direct Exchange

Routing:

```text
Exact routing key match
```

Example:

```text
order.created
```

Only consumers bound to that key receive the message.

---

# Topic Exchange

Routing:

```text
Pattern-based matching
```

Example:

```text
order.*
```

Matches:

```text
order.created

order.completed
```

---

# Fanout Exchange

Routing:

```text
Broadcast
```

Every bound queue receives the message.

Use cases:

* Notifications
* Broadcasting events

---

# Headers Exchange

Routing based on:

* Message attributes
* Metadata headers

Useful when routing rules are not key-based.

---

# Message Design

Messages should be:

* Small
* Self-contained
* Versioned
* Serializable

Example:

```json
{
  "taskId": "uuid",
  "type": "EMAIL_SEND",
  "payload": {
    "email": "user@example.com"
  },
  "version": 1
}
```

---

# Message Design Rules

Avoid:

* Large database snapshots
* Internal implementation details
* Non-versioned payloads

Prefer:

* Clear contracts
* Stable schemas
* Backward compatibility

---

# Producer Design

## Rules

Producers should:

* Ensure message durability
* Use persistent messages
* Confirm delivery when required
* Handle broker failures

---

# Durable Messaging

For important messages:

Enable:

```text
Persistent messages

Durable queues

Durable exchanges
```

Goal:

Messages survive broker restarts.

---

# Consumer Design

Consumers must:

* Acknowledge messages explicitly
* Handle retries
* Be idempotent
* Avoid long blocking operations

---

# Consumer Processing Flow

Recommended:

```text
Receive Message

        ↓

Validate

        ↓

Process

        ↓

ACK

```

Failure:

```text
Receive Message

        ↓

Processing Failure

        ↓

NACK

        ↓

Retry / DLQ
```

---

# Acknowledgment Strategy

RabbitMQ acknowledgments:

| Action | Meaning                |
| ------ | ---------------------- |
| ACK    | Processing successful  |
| NACK   | Retry or reject        |
| Reject | Remove or route to DLQ |

---

# Manual ACK Example

```java
channel.basicAck(
    deliveryTag,
    false
);
```

Rules:

Do not acknowledge before successful processing.

---

# Dead Letter Queue (DLQ)

DLQ is used for messages that cannot be processed.

Common cases:

* Poison messages
* Repeated failures
* Invalid payloads
* Permanent business errors

---

# DLQ Flow

```text
Main Queue

     ↓

Processing Failure

     ↓

Retry Attempts

     ↓

Dead Letter Queue
```

---

# Retry Strategy

Recommended approaches:

## Delayed Queues

Messages wait before retry.

---

## Retry Exchanges

Route failed messages through retry paths.

---

## Backoff Policies

Example:

```text
5 seconds

30 seconds

5 minutes
```

Avoid:

```text
Infinite immediate retries
```

---

# Ordering

RabbitMQ does not guarantee global ordering.

Ordering is only predictable within:

```text
Single queue

+

Single consumer
```

For strict ordering:

* Use dedicated queues
* Control consumer concurrency

---

# Performance Guidelines

Optimize:

## Prefetch

Controls:

```text
Messages delivered before acknowledgment
```

Avoid:

* Too many unprocessed messages

---

## Message Size

Keep messages:

* Small
* Focused

Large messages increase:

* Memory usage
* Network cost
* Processing latency

---

## Batching

Use batching when:

* Throughput is critical
* Ordering is not required

---

# Monitoring

Track:

## Queue Metrics

* Queue depth
* Consumer count
* Message age

## Application Metrics

* Processing latency
* Failed messages
* Retry count

---

# Reliability Patterns

Recommended patterns:

## Publisher Confirms

Ensures broker received message.

---

## Consumer Acknowledgments

Ensures successful processing.

---

## Durable Queues

Protect messages from restarts.

---

## Durable Exchanges

Preserve routing configuration.

---

# Security

Production requirements:

## Encryption

Enable:

```text
TLS
```

---

## Authentication

Use:

* RabbitMQ authentication mechanisms
* Secure credentials

---

## Authorization

Apply:

* Virtual host restrictions
* Least privilege permissions

Control:

* Publish access
* Consume access
* Queue access

---

# RabbitMQ Anti-Patterns

Avoid:

## Using RabbitMQ as Kafka Replacement

RabbitMQ:

```text
Task / Command Messaging
```

Kafka:

```text
Event Streaming
```

---

## Unbounded Queues

Problems:

* Memory pressure
* Delayed processing
* Operational risk

---

## No DLQ Strategy

Failures must be visible.

---

## Fire-and-Forget Messaging

Always confirm important messages.

---

## One Queue With Multiple Responsibilities

Avoid:

```text
everything.queue
```

Prefer:

```text
email.queue

payment.queue

notification.queue
```

---

# Production Checklist

Before deployment:

* Enable durable messaging
* Configure acknowledgments
* Add retry strategy
* Configure DLQ
* Monitor queue health
* Secure broker access
* Keep messages versioned

---

# AI Coding Checklist

When generating RabbitMQ code:

* Ensure message durability
* Use explicit ACK/NACK handling
* Implement retry strategy
* Add DLQ support
* Keep messages small
* Version message contracts
* Make consumers idempotent
* Do not use RabbitMQ as Kafka replacement

---

# Next Step — Integration Layer (Advanced)

Upcoming guides:

```text
integration/

├── apache-camel.md
├── graphql.md
└── grpc.md
```

Topics:

* Enterprise Integration Patterns (EIP)
* API design
* High-performance RPC
* Routing and transformation
* Mediation patterns
* Contract design and versioning
