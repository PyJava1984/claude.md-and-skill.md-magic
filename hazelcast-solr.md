# Database Layer — Hazelcast Engineering Guide

**File:** `database/hazelcast.md`
**Version:** 1.0

---

# Purpose

Hazelcast is a distributed in-memory data grid used for:

* Distributed caching
* Shared application state
* Cluster coordination
* High-performance distributed computing

Hazelcast provides memory-based distributed structures across multiple application nodes.

It should be treated as a distributed performance layer, not a primary data store.

---

# Philosophy

Core principles:

* Shared state must be controlled
* Avoid unnecessary distributed locking
* Design for eventual consistency
* Minimize network communication
* Keep distributed operations efficient

---

# Hazelcast Architecture

Typical flow:

```text id="5v8kz2"
Application Node

        |
        ↓

Hazelcast Cluster

        |
        ↓

Distributed Data Structures
```

---

# Common Use Cases

Hazelcast is commonly used for:

## Distributed Caching

Examples:

* Frequently accessed data
* Expensive computation results
* Shared cache across services

---

## Cluster Coordination

Examples:

* Node discovery
* Shared coordination state

---

## Shared Maps

Examples:

* Distributed key-value storage
* Temporary application state

---

## Distributed Queues

Examples:

* Work distribution
* Background processing

---

## Session Replication

Examples:

* Shared user sessions
* Clustered web applications

---

# Data Structures

Use the correct structure for the problem.

---

# IMap

Distributed key-value store.

Example:

```java id="d1i7px"
IMap<String, Customer> map =
    hazelcastInstance.getMap("customers");


map.put(
    "1",
    customer
);
```

Use for:

* Cached objects
* Shared state

---

# IQueue

Distributed queue.

Use for:

* Task processing
* Worker coordination

---

# ISet

Distributed unique collection.

Use for:

* Membership tracking
* Unique values

---

# Topic

Publish/subscribe messaging.

Use for:

* Notifications
* Lightweight events

Avoid using it for critical guaranteed delivery messaging.

---

# Data Modeling

Rules:

Keep distributed objects:

* Small
* Simple
* Serializable

Avoid:

* Large object graphs
* Deep nested structures
* Heavy serialization

---

# Performance Guidelines

Optimize:

## Object Size

Avoid storing:

```text id="x3b7jo"
Large documents

Huge serialized objects

Entire database entities
```

---

## Partitioning

Hazelcast distributes data across cluster members.

Design keys carefully.

Example:

```text id="z7y2mv"
customer:123
order:456
```

Good keys improve distribution.

---

## Network Communication

Distributed calls are expensive.

Avoid:

* Frequent remote reads
* Chatty operations
* Excessive synchronization

---

# Local Caching

Use local caching when:

* Data is read frequently
* Slight staleness is acceptable

Benefits:

* Reduced network traffic
* Faster access

---

# Consistency Model

Hazelcast commonly uses:

```text id="m9b7qk"
Eventual consistency
```

Meaning:

Updates propagate across nodes over time.

---

# Strong Consistency

Strong consistency provides:

* Immediate visibility
* More predictable behavior

Trade-off:

* Higher latency
* Lower scalability

Use only when required.

---

# Distributed Locking

Hazelcast supports distributed locks.

Use carefully.

Problems:

* Reduced scalability
* Potential deadlocks
* Increased coordination cost

Prefer:

* Atomic operations
* Idempotent design
* Event-driven coordination

---

# Monitoring

Track:

* Cluster health
* Memory usage
* Partition distribution
* Operation latency
* Network traffic

---

# Security

Production requirements:

* Secure cluster communication
* Authenticate members
* Restrict client access
* Protect sensitive cached data

---

# Hazelcast Anti-Patterns

Avoid:

## Using Hazelcast as Primary Database

Bad:

```text id="z3l5xv"
Application

    ↓

Hazelcast only
```

Use:

```text id="3o5bpl"
Database

    ↓

Hazelcast Cache
```

---

## Excessive Distributed Locks

Problems:

* Blocking
* Reduced throughput

---

## Large Object Replication

Problems:

* Memory pressure
* Slow synchronization

---

## Chatty Distributed Operations

Problems:

* Network overhead
* Latency growth

---

# AI Coding Checklist

When generating Hazelcast code:

* Keep objects lightweight
* Choose correct distributed structures
* Avoid excessive locking
* Consider network cost
* Design for eventual consistency
* Do not use Hazelcast as a system of record

---

# Database Layer — Apache Solr Engineering Guide

**File:** `database/solr.md`
**Version:** 1.0

---

# Purpose

Apache Solr is a distributed search platform built on Apache Lucene.

It is designed for:

* Full-text search
* Indexing
* Filtering
* Faceted search
* Search analytics

Solr should be treated as a search engine, not a transactional database.

---

# Philosophy

Core principles:

* Search is not storage
* Index design drives performance
* Relevance tuning is iterative
* Schema design is critical

---

# Solr Architecture

Typical flow:

```text id="8r1jmv"
Application

      |
      ↓

Solr Index

      |
      ↓

Search Query

      |
      ↓

Results
```

---

# Schema Design

Schema defines:

* Searchable fields
* Data types
* Analysis rules

---

# Rules

Prefer:

* Explicit fields
* Clear naming
* Appropriate analyzers

Avoid:

* Excessive dynamic fields
* Uncontrolled schema growth

---

# Field Design

Consider:

* Search requirements
* Filtering needs
* Sorting requirements
* Faceting requirements

---

# Indexing Strategy

Prefer:

## Batch Indexing

Better for:

* Large datasets
* Initial imports
* Bulk updates

---

Avoid:

## Frequent Small Updates

Problems:

* Lower throughput
* More overhead

---

# Commit Strategy

Optimize commit frequency.

Trade-off:

Frequent commits:

* Faster visibility
* Higher overhead

Less frequent commits:

* Better performance
* Delayed availability

---

# Query Design

Optimize queries using:

* Filters
* Proper field targeting
* Query relevance tuning

---

# Example Query

```text id="n0z9ps"
q=customer:john

fq=status:ACTIVE
```

---

# Filtering

Use filters for:

* Exact matches
* Structured conditions

Example:

```text id="6x8m8p"
status:ACTIVE
```

---

# Performance Guidelines

Optimize:

## Schema

Good schema improves:

* Index speed
* Query speed
* Relevance

---

## Cache Usage

Tune:

* Filter cache
* Query cache
* Result cache

---

## SolrCloud

For distributed search:

Consider:

* Sharding
* Replication
* Cluster health

---

# Search Relevance

Relevance tuning is iterative.

Improve using:

* Field weights
* Analyzers
* Ranking strategies

---

# Security

Production requirements:

* Secure Solr endpoints
* Restrict access
* Protect admin interfaces

---

# Solr Anti-Patterns

Avoid:

## Uncontrolled Wildcard Queries

Example:

```text id="m0x0qt"
*customer*
```

Problems:

* Expensive searches
* High CPU usage

---

## Poor Schema Design

Problems:

* Slow indexing
* Poor relevance

---

## Frequent Reindexing Without Planning

Problems:

* Resource spikes
* Operational complexity

---

## Using Solr as Transactional Database

Solr is for:

```text id="jq4d2x"
Search
```

Not:

```text id="x6r4hk"
Primary business storage
```

---

# AI Coding Checklist

When generating Solr integration:

* Design explicit schemas
* Choose analyzers carefully
* Avoid wildcard-heavy queries
* Use batch indexing
* Optimize search relevance
* Treat Solr as search infrastructure, not storage

---

# Next Step — Database Layer

Upcoming guide:

```text id="q7c0na"
database/

└── snowflake.md
```

Topics:

* Data warehouse architecture
* Analytical workloads
* Data modeling
* Large-scale analytics patterns
