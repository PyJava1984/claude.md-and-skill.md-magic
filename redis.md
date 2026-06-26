# Database Layer — Redis Engineering Guide

**File:** `database/redis.md`
**Version:** 1.0

---

# Purpose

Redis is an in-memory data store designed for:

* High-speed data access
* Caching
* Distributed coordination
* Temporary state management

Redis should be treated as a performance layer, not the primary system of record.

---

# Philosophy

Core principles:

* Redis is not a primary database
* Data should be considered disposable
* Every key should have an expiration strategy
* Keep data models simple
* Optimize for speed and availability

---

# Redis Architecture Role

Typical usage:

```text
Application

     |
     ↓

Redis Cache

     |
     ↓

Primary Database
```

Redis improves performance but should not replace durable storage.

---

# Common Use Cases

Redis is commonly used for:

## Caching

Examples:

* Frequently accessed data
* Expensive query results
* API responses

---

## Session Storage

Examples:

* User sessions
* Authentication state
* Temporary user data

---

## Rate Limiting

Examples:

* API request limits
* Abuse prevention
* Traffic control

---

## Distributed Locks

Examples:

* Prevent duplicate processing
* Coordinate workers

---

## Pub/Sub Messaging

Examples:

* Real-time notifications
* Lightweight events

Avoid using it as a replacement for reliable messaging systems.

---

# Redis Data Structures

Choose structures based on access patterns.

---

# Strings

Use for:

* Simple values
* Counters
* Tokens

Example:

```text
customer:count = 1000
```

---

# Hashes

Use for:

* Object-like structures
* Small related fields

Example:

```text
customer:1

name = John

status = ACTIVE
```

---

# Lists

Use for:

* Queues
* Ordered collections

Example:

```text
email_queue
```

---

# Sets

Use for:

* Unique collections
* Membership checks

Examples:

```text
active_users
```

---

# Sorted Sets

Use for:

* Rankings
* Leaderboards
* Time-based ordering

Examples:

```text
game_scores
```

---

# Caching Strategy

## Rules

Every cache entry should have:

* Defined TTL
* Clear ownership
* Invalidation strategy

Never create:

```text
Permanent cache entries
```

---

# Cache-Aside Pattern

Recommended pattern:

```text
Application

     |
     ↓

Check Redis

     |
     ↓

If Missing

     |
     ↓

Load Database

     |
     ↓

Store in Redis
```

---

# Example

Spring Redis:

```java
redisTemplate.opsForValue()
    .set(
        "customer:1",
        customer,
        Duration.ofMinutes(10)
    );
```

---

# Cache Invalidation

Cache invalidation is one of the hardest distributed system problems.

---

# Strategies

## TTL Expiration

Data expires automatically.

Good for:

* Temporary data
* Read-heavy systems

---

## Explicit Invalidation

Delete cache after updates.

Example:

```text
Update Customer

        ↓

Delete customer cache
```

---

## Write-Through Cache

Application updates:

```text
Database

+

Redis
```

together.

Useful when:

* Strong cache consistency is required

---

# Distributed Locks

Redis can coordinate distributed work.

Example:

```text
Worker A

    obtains lock

Worker B

    waits
```

---

# Lock Rules

Always:

* Set expiration
* Handle failure cases
* Keep lock duration short

Avoid:

```text
Infinite locks
```

---

# Rate Limiting

Common algorithms:

## Fixed Window

Example:

```text
100 requests / minute
```

---

## Sliding Window

More accurate time-based control.

---

## Token Bucket

Allows controlled bursts.

---

# Performance Guidelines

Optimize Redis usage:

## Keep Values Small

Avoid:

* Large serialized objects
* Huge JSON blobs

---

## Use Pipelining

For multiple operations:

```text
Command 1

Command 2

Command 3
```

sent together.

Benefits:

* Lower network overhead
* Better throughput

---

# Persistence

Redis supports:

## RDB Snapshots

Periodic snapshots.

Advantages:

* Faster recovery
* Smaller files

Trade-off:

* Possible data loss between snapshots

---

## AOF Logs

Append-only persistence.

Advantages:

* More durable

Trade-off:

* More storage
* More write overhead

---

# Persistence Decisions

Choose based on:

* Data importance
* Recovery requirements
* Performance needs

---

# Security

Production requirements:

* Enable authentication
* Restrict network access
* Protect credentials
* Avoid exposing Redis publicly

---

# Monitoring

Track:

* Memory usage
* Evictions
* Hit/miss ratio
* Latency
* Connection count

---

# Redis Anti-Patterns

Avoid:

## Using Redis as Primary Database

Bad:

```text
Application

   ↓

Redis only
```

---

## Missing TTLs

Problems:

* Memory growth
* Stale data

---

## Long-Lived Locks

Problems:

* Deadlocks
* Blocking workers

---

## Large Value Storage

Avoid storing:

* Large documents
* Entire database objects

---

## Overusing Pub/Sub

Redis Pub/Sub does not guarantee:

* Message persistence
* Replay
* Delivery guarantees

Use Kafka/RabbitMQ when reliability is required.

---

# Production Checklist

Before deployment:

* Define TTL strategy
* Monitor memory usage
* Select correct data structures
* Define cache invalidation rules
* Secure Redis access
* Monitor cache effectiveness

---

# AI Coding Checklist

When generating Redis usage:

* Always include TTL
* Do not use Redis as system of record
* Define invalidation strategy
* Choose correct Redis data structures
* Avoid large object storage
* Consider failure scenarios

---

# Next Step — Database Layer

Upcoming guides:

```text
database/

├── hazelcast.md
├── solr.md
└── snowflake.md
```

Topics:

* Distributed caching
* Search architecture
* Data analytics platforms
* Large-scale data processing
