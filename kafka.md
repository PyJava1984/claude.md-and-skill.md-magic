# Integration Layer — Apache Kafka Engineering Guide

**File:** `integration/kafka.md`
**Version:** 1.0

---

# Purpose

This guide defines standards for designing reliable, scalable, and maintainable event-driven systems using Apache Kafka.

Kafka is not just a messaging system.

Kafka should be treated as:

> A distributed, persistent event log.

Systems must be designed around event-driven principles.

---

# Philosophy

Core principles:

* Events represent immutable facts
* Producers should not know consumers
* Consumers must be idempotent
* Systems must tolerate failure
* Data flows asynchronously by default
* Ordering should never be assumed unless explicitly partitioned

---

# Kafka Architecture Overview

Basic flow:

```text
Producer

    |
    ↓

Kafka Topic

    |
    ↓

Consumer Group

    |
    ↓

Application Processing
```

---

# Core Concepts

| Concept        | Description                 |
| -------------- | --------------------------- |
| Topic          | Stream of events            |
| Partition      | Unit of parallel processing |
| Consumer Group | Scalability mechanism       |
| Offset         | Consumer position in stream |
| Broker         | Kafka server node           |

---

# Event Design

## Golden Rules

Events represent something that already happened.

Use past tense naming.

Good:

```text
CustomerCreated

OrderPlaced

PaymentCompleted
```

Avoid:

```text
CreateCustomer

ProcessOrder
```

Commands belong in:

* REST
* RPC
* Command systems

Kafka events describe facts.

---

# Event Structure

Recommended format:

```json
{
  "eventId": "uuid",
  "eventType": "OrderCreated",
  "timestamp": "2026-01-01T10:00:00Z",
  "version": 1,
  "data": {
    "orderId": 123,
    "customerId": 456
  }
}
```

Required fields:

| Field     | Purpose                 |
| --------- | ----------------------- |
| eventId   | Unique event identifier |
| eventType | Event name              |
| timestamp | Creation time           |
| version   | Schema version          |
| data      | Business payload        |

---

# Producer Design

## Rules

Producers should:

* Enable idempotent publishing
* Retry safely
* Use acknowledgements
* Compress large payloads
* Avoid sending unnecessary data

Recommended:

```properties
acks=all
enable.idempotence=true
```

---

# Producer Example

Spring Kafka example:

```java
KafkaTemplate<String, Object> kafkaTemplate;


public void publishOrderCreated(OrderEvent event) {

    kafkaTemplate.send(
        "order-events",
        event.getOrderId(),
        event
    );
}
```

---

# Consumer Design

Consumers must assume:

* Messages can be duplicated
* Processing can fail
* Messages can arrive after delays

---

# Consumer Rules

Always:

* Implement idempotency
* Commit offsets after successful processing
* Handle retries explicitly
* Monitor consumer lag

---

# Consumer Example

```java
@KafkaListener(
    topics = "order-events",
    groupId = "order-service"
)
public void consume(OrderEvent event) {


    if(alreadyProcessed(event.getEventId())) {
        return;
    }


    process(event);


    markProcessed(event.getEventId());
}
```

---

# Idempotency

Idempotency is a mandatory requirement.

Kafka commonly uses:

```text
At-least-once delivery
```

Meaning:

The same event may arrive multiple times.

---

# Idempotency Techniques

Common approaches:

## Deduplication Table

Store processed event IDs.

Example:

```text
processed_events

event_id
processed_time
```

---

## Event ID Tracking

Example:

```text
eventId = abc-123
```

Reject duplicates.

---

## Natural Keys

Use business identifiers:

```text
orderId
customerId
transactionId
```

---

## Upsert Operations

Prefer:

```text
INSERT OR UPDATE
```

over duplicate inserts.

---

# Partitioning Strategy

Partitions provide:

* Parallelism
* Ordering boundaries

Choose keys carefully.

---

# Good Partition Keys

Customer-based ordering:

```text
customerId
```

Result:

```text
Customer A events stay ordered
```

---

Order-based ordering:

```text
orderId
```

Result:

```text
Order lifecycle stays ordered
```

---

# Avoid

Random partition keys when ordering matters.

Kafka only guarantees ordering:

> Inside a single partition.

---

# Delivery Semantics

Kafka supports:

| Type          | Description           |
| ------------- | --------------------- |
| At-most-once  | Possible message loss |
| At-least-once | Possible duplicates   |
| Exactly-once  | Advanced and limited  |

Most enterprise systems use:

```text
At-least-once + Idempotent Consumers
```

---

# Retry Strategy

Avoid blocking consumers indefinitely.

Use:

* Retry topics
* Exponential backoff
* Dead Letter Queues

Example flow:

```text
Main Topic

     ↓

Retry Topic

     ↓

Retry Again

     ↓

DLQ
```

---

# Dead Letter Queue (DLQ)

DLQ is used when:

* Message format is invalid
* Processing permanently fails
* External dependency is unavailable

Rules:

Never silently discard messages.

---

# Schema Management

Recommended formats:

* Avro
* Protobuf
* JSON Schema

Prefer:

Strong schema contracts.

---

# Schema Rules

Always:

* Version schemas
* Maintain backward compatibility
* Avoid breaking consumers

Example:

```text
OrderCreatedV1

OrderCreatedV2
```

---

# Performance Guidelines

Optimize:

* Batch processing
* Compression
* Fetch sizes
* Serialization

Recommended compression:

```text
snappy

lz4
```

---

# Message Design

Keep messages:

* Small
* Focused
* Business meaningful

Avoid:

Large database snapshots as events.

---

# Error Handling

Classify failures.

| Error Type | Action      |
| ---------- | ----------- |
| Transient  | Retry       |
| Permanent  | DLQ         |
| Unknown    | Log + Alert |

---

# Retry Rules

Never:

```text
Infinite retry loop
```

Always include:

* Backoff
* Maximum attempts
* Failure tracking

---

# Monitoring

Monitor:

## Consumer

* Consumer lag
* Processing latency
* Error rate

## Broker

* Disk usage
* Replication health
* Partition balance

## Application

* Throughput
* Failed processing
* Retry volume

---

# Security

Enable:

## Encryption

Use:

```text
TLS
```

---

## Authentication

Use:

```text
SASL
```

---

## Authorization

Use:

```text
ACLs
```

Control:

* Topic access
* Consumer permissions
* Producer permissions

---

# Production Rules

Never:

* Expose Kafka publicly
* Share credentials
* Allow unrestricted topic access

---

# Kafka Anti-Patterns

Avoid:

## Using Kafka as RPC

Bad:

```text
Request → Kafka → Response
```

Use:

* REST
* gRPC
* RPC systems

---

## Large Monolithic Events

Avoid:

```json
{
 "everything": "from database"
}
```

---

## Tight Producer/Consumer Coupling

Consumers should evolve independently.

---

## Ignoring Schema Evolution

Breaking schemas break systems.

---

## No Idempotency

Duplicates are expected.

---

## Infinite Retries

Failures must eventually stop.

---

# AI Coding Checklist

When generating Kafka code:

* Design events as immutable facts
* Use meaningful event names
* Implement consumer idempotency
* Use correct partition keys
* Enable safe producer settings
* Add retry strategy
* Add DLQ handling
* Version schemas
* Avoid tight coupling
* Protect against duplicate messages

---

# Next Step — Integration Layer

Upcoming guides:

```text
integration/

├── rabbitmq.md
├── apache-camel.md
├── graphql.md
└── grpc.md
```

Topics:

* Messaging patterns
* Enterprise integration
* Reliability guarantees
* Async workflows
* Schema design
* Distributed system patterns
