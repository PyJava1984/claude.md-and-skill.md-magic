# Database Layer — SQL Engineering Guide

**File:** `database/sql.md`
**Version:** 1.0

---

# Purpose

This guide defines standards for writing safe, maintainable, and high-performance SQL across enterprise systems.

SQL should optimize for:

* Correctness
* Readability
* Maintainability
* Performance at scale

---

# Philosophy

Core principles:

* Data integrity is non-negotiable
* Readability comes before optimization
* Avoid premature optimization
* Assume data volume will grow
* Prefer explicit SQL when ORM abstraction becomes unclear

---

# SQL Style Rules

## Naming

Use meaningful names:

Good:

```text id="sf2v6x"
customer

order

invoice
```

Avoid:

```text id="k7yr3a"
cust

ord

inv
```

---

# Naming Convention

Use:

```text id="q0z9te"
snake_case
```

Examples:

```text id="f2h6t9"
customer_id

created_at

order_status
```

---

# Query Formatting

Preferred:

```sql id="o6sz2v"
SELECT
    c.id,
    c.name,
    c.email
FROM customer c
WHERE c.status = 'ACTIVE'
  AND c.created_at >
      CURRENT_DATE - INTERVAL '30 days';
```

Benefits:

* Easier review
* Easier debugging
* Better maintainability

---

# Query Design

Rules:

* Select only required columns
* Avoid `SELECT *`
* Use explicit joins
* Keep conditions readable

---

# Good Example

```sql id="plqf4r"
SELECT
    o.id,
    c.name
FROM orders o
JOIN customer c
    ON o.customer_id = c.id;
```

---

# Avoid

Implicit joins:

```sql id="ql91xk"
SELECT *
FROM orders, customer;
```

Problems:

* Harder to read
* Risk of accidental Cartesian joins

---

# Indexing

Indexes improve:

* Filtering
* Joins
* Sorting

---

# Index Guidelines

Index:

* Foreign keys
* Frequently filtered columns
* Common query conditions

Avoid:

* Excessive indexes
* Indexing every column

---

# Composite Indexes

Use carefully.

Example:

```sql id="o9s9vn"
CREATE INDEX idx_order_customer_date

ON orders (
    customer_id,
    created_at
);
```

Order matters.

---

# Transactions

Use transactions for:

* Multi-step updates
* Financial operations
* Critical consistency requirements

Example:

```text id="qz1gjm"
BEGIN

Update A

Update B

COMMIT
```

---

# Transaction Guidelines

Ensure:

* Correct isolation level
* Minimal transaction scope
* Proper rollback handling

Avoid:

* Long-running transactions

---

# Performance Optimization

Focus on:

## Execution Plans

Use:

```sql id="2y6iyb"
EXPLAIN ANALYZE
```

---

## Index Usage

Verify:

* Correct indexes
* Selectivity
* Query plan

---

## Join Efficiency

Optimize:

* Join conditions
* Filtering order
* Result size

---

# Avoid Full Table Scans

Problems:

* Slow queries
* High database load

Especially dangerous on:

* Large tables
* High traffic systems

---

# Security

Always:

* Use parameterized queries
* Prevent SQL injection
* Restrict database permissions

Avoid:

* Unsafe dynamic SQL

---

# Pagination

## LIMIT/OFFSET

Simple approach:

```sql id="9q3v4s"
SELECT *

FROM customer

LIMIT 20 OFFSET 40;
```

Good for:

* Small datasets

---

## Keyset Pagination

Preferred for large datasets.

Example:

```sql id="7d1x5w"
WHERE id > :last_seen_id

ORDER BY id

LIMIT 20;
```

Benefits:

* Better performance
* Stable results

---

# SQL Anti-Patterns

Avoid:

❌ `SELECT *`

❌ Cartesian joins

❌ Unlimited queries

❌ Functions on indexed columns inside WHERE

Example:

Avoid:

```sql id="m9z1j3"
WHERE LOWER(email) = 'test@example.com'
```

because it may prevent index usage.

---

# AI Coding Checklist

When generating SQL:

* Avoid `SELECT *`
* Use explicit joins
* Consider indexes
* Keep queries readable
* Prevent SQL injection
* Design for future data growth

---

# Database Layer — PostgreSQL Engineering Guide

**File:** `database/postgresql.md`
**Version:** 1.0

---

# Purpose

This guide defines best practices for designing scalable, performant, and reliable PostgreSQL databases.

---

# Philosophy

Core principles:

* Data consistency first
* Normalize by default
* Denormalize only with justification
* Optimize based on real workloads
* Treat schema as evolving architecture

---

# Schema Design

Rules:

* Use clear table names
* Use snake_case
* Define primary keys
* Use foreign keys
* Enforce integrity constraints

---

# Normalization

Default approach:

```text id="9h0y7x"
Third Normal Form (3NF)
```

Benefits:

* Reduced duplication
* Better consistency

---

# Denormalization

Use only when justified.

Examples:

* Reporting performance
* Read-heavy workloads
* Analytics optimization

Always document:

* Why it exists
* Trade-offs
* Maintenance impact

---

# Indexing Strategy

PostgreSQL automatically indexes:

* Primary keys

Manually index:

* Foreign keys
* Frequently searched fields
* Common filters

---

# Partial Indexes

Useful for filtered subsets.

Example:

```sql id="q6g8f2"
CREATE INDEX idx_customer_active

ON customer(status)

WHERE status = 'ACTIVE';
```

---

# Query Optimization

Use:

```sql id="a7x9p2"
EXPLAIN ANALYZE
```

Review:

* Query plan
* Index usage
* Scan types
* Join strategy

---

# Avoid Blind Optimization

Always optimize based on:

* Real queries
* Actual execution plans
* Production patterns

---

# Transactions

Ensure:

* Atomic operations
* Correct isolation levels
* Minimal transaction duration

Avoid:

* Long-running transactions

---

# Concurrency

Understand:

* Row locking
* Deadlocks
* Isolation levels

Default:

```text id="a8o1zr"
READ COMMITTED
```

---

# Partitioning

Use partitioning for:

* Very large tables
* Time-series data
* Logs
* Historical records

Examples:

```text id="d9b7zk"
orders_2026_01

orders_2026_02
```

---

# JSON Support

PostgreSQL supports:

```text id="u5m7sp"
JSONB
```

Use for:

* Flexible attributes
* Semi-structured data

Avoid:

Replacing relational modeling with JSON.

---

# Security

Apply:

* Role-based access control
* Least privilege
* Encryption
* Parameterized queries

---

# Performance Areas

Focus on:

* Index strategy
* Vacuum process
* Query planning
* Connection pooling

---

# PostgreSQL Anti-Patterns

Avoid:

## Unindexed Foreign Keys

Causes:

* Slow joins
* Locking problems

---

## Massive Single Tables

Consider:

* Partitioning
* Archiving

---

## Excessive Triggers

Problems:

* Hidden behavior
* Hard debugging

---

## Unoptimized Nested Queries

Always review:

* Execution plan
* Index availability

---

# AI Coding Checklist

When generating PostgreSQL solutions:

* Include indexing strategy
* Review query plans
* Use transactions correctly
* Consider concurrency
* Avoid unnecessary joins
* Avoid over-normalization
* Avoid unnecessary denormalization

---

# Next Step — Database Layer

Upcoming guides:

```text id="5y9r0k"
database/

├── redis.md
├── hazelcast.md
├── solr.md
└── snowflake.md
```

Topics:

* Caching architecture
* Distributed data patterns
* Search systems
* Analytics warehouse design
