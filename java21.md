# Java 21 Engineering Guide

**Version:** 1.0

---

# Purpose

Java 21 is the current **Long-Term Support (LTS)** release and should be the preferred baseline for new enterprise applications unless project constraints require an earlier Java version.

Java 21 introduces significant improvements in concurrency, language expressiveness, performance, and developer productivity while maintaining Java’s core strengths:

* Readability
* Stability
* Type safety
* Performance
* Backward compatibility

This guide defines engineering standards and best practices for building modern enterprise Java 21 applications.

---

# Engineering Philosophy

Use modern Java features to improve:

* Maintainability
* Clarity
* Developer productivity
* Application reliability

Do not adopt new syntax or APIs simply because they exist.

> **Guiding Principle:** Modern Java should make software easier to understand, test, and evolve.

---

# Java 21 Language Features

Adopt modern Java 21 features where they provide clear engineering benefits.

Recommended features:

* Virtual Threads (Project Loom)
* Structured Concurrency (Preview)
* Scoped Values (Preview)
* Record Patterns
* Pattern Matching for `switch`
* Sequenced Collections
* String Templates (where supported and finalized)
* Foreign Function & Memory API

## Production Usage Rule

Before adopting any advanced feature:

Verify whether it is:

* Final
* Preview
* Incubator

Preview and incubator features require explicit project approval before production usage.

---

# Virtual Threads

Virtual Threads are one of the most significant additions in Java 21.

They enable highly scalable concurrency for applications with many blocking operations.

---

## Recommended Use Cases

Virtual threads are ideal for:

* REST APIs with blocking I/O
* Database operations
* HTTP clients
* Messaging consumers
* File operations
* Network communication

Example:

```java id="p2kq7s"
try (var executor =
        Executors.newVirtualThreadPerTaskExecutor()) {

    executor.submit(() ->
        customerService.process(customerId)
    );
}
```

---

## Benefits

Virtual threads provide:

* High concurrency with simpler code
* Reduced thread management overhead
* Improved scalability for I/O workloads
* Simpler alternatives to reactive programming in many scenarios

---

## Avoid Using Virtual Threads For

Virtual threads provide limited benefit for:

* CPU-heavy algorithms
* Intensive mathematical processing
* Tight computational loops
* Workloads dominated by synchronization

---

## Virtual Thread Guidelines

Avoid pinning virtual threads by:

* Holding synchronized locks during long blocking operations
* Performing blocking calls inside synchronized sections
* Excessive shared mutable state

Prefer:

* Immutable objects
* Message passing
* Stateless services

---

# Structured Concurrency

Structured concurrency provides a clearer way to manage related concurrent tasks.

It treats multiple concurrent operations as a single logical operation.

---

## Benefits

Structured concurrency improves:

* Task ownership
* Error propagation
* Cancellation handling
* Workflow visibility
* Debugging

Example scenarios:

* Calling multiple downstream services
* Parallel data retrieval
* Aggregating multiple independent results

---

## Guidelines

Use structured concurrency when:

* Tasks have a common lifecycle
* Failure handling needs coordination
* Cancellation behavior matters

Avoid adoption until:

* Project support is confirmed
* Preview feature usage is approved

---

# Scoped Values

Scoped values provide an alternative approach to immutable contextual data propagation.

They are intended to replace some ThreadLocal use cases.

---

## Recommended Uses

Use scoped values for:

* Request IDs
* Correlation IDs
* Security context
* Tenant identifiers
* Trace metadata

---

## Avoid

Do not use scoped values as:

* Mutable global state
* Application-wide storage
* Hidden dependency containers

Prefer explicit dependency passing whenever possible.

---

# Record Patterns

Record patterns simplify extracting values from records.

Example:

```java id="t0czm6"
if (obj instanceof Customer(Long id, String name)) {
    process(id, name);
}
```

---

## Guidelines

Use record patterns when they:

* Improve readability
* Reduce repetitive accessors
* Make data processing clearer

Avoid complex nested deconstruction that hides intent.

---

# Pattern Matching for switch

Pattern matching for switch improves branching over different object types.

Example:

```java id="n8z0d6"
String result = switch (payment) {
    case CardPayment card ->
        processCard(card);

    case CashPayment cash ->
        processCash(cash);
};
```

---

## Guidelines

Ensure:

* Exhaustive handling
* Clear default behavior
* Minimal nesting
* Domain logic remains understandable

Avoid large switch statements that indicate missing domain abstractions.

---

# Sequenced Collections

Java 21 introduces sequenced collection interfaces.

Use them when:

* Insertion order matters
* First/last element access is common
* Working directly with Java 21 collection APIs

Prefer programming against interfaces:

```java id="c9q3pp"
List<Order>
Map<String, Customer>
Set<Role>
```

---

# Performance

Java 21 includes improvements across:

* Garbage collectors
* JVM optimizations
* JIT compilation
* Thread scheduling

Performance guidance:

* Measure before optimizing
* Profile real workloads
* Validate assumptions
* Focus on business bottlenecks

Common optimization areas:

* Database access
* Network latency
* Memory allocation
* Serialization
* Concurrency bottlenecks

---

# Migration Guidance

## Migration From Java 17

Recommended steps:

### 1. Upgrade Tooling

Update:

* Build tools
* CI pipelines
* Static analysis tools
* Dependency versions

---

### 2. Enable Java 21 Compilation

Update:

* Compiler configuration
* Runtime environments
* Container images

---

### 3. Adopt Virtual Threads Incrementally

Start with:

* New services
* I/O-heavy workloads
* Background processing

Avoid large-scale rewrites.

---

### 4. Review ThreadLocal Usage

Replace suitable ThreadLocal patterns with scoped values where appropriate.

Validate:

* Lifecycle behavior
* Security implications
* Framework compatibility

---

### 5. Introduce Pattern Matching

Modernize carefully:

* `instanceof`
* Complex branching logic
* Type-based processing

Only refactor when readability improves.

---

### 6. Validate Dependencies

Confirm compatibility with:

* Frameworks
* Libraries
* Monitoring agents
* Build plugins

---

# Testing

When adopting Java 21 features, include tests for:

* Virtual thread behavior
* Concurrent workflows
* Cancellation scenarios
* Error propagation
* Record pattern logic
* Pattern matching branches

Concurrency tests should verify:

* Correctness
* Thread safety
* Failure handling

---

# Security

Java 21 applications should continue following secure development practices:

* Keep dependencies updated
* Validate external inputs
* Protect sensitive data
* Secure serialization boundaries
* Externalize secrets
* Follow least privilege principles

Modern concurrency features must not introduce hidden security risks.

---

# AI Coding Checklist

Before generating Java 21 code, an AI coding assistant should verify:

* Prefer virtual threads for suitable blocking I/O workloads
* Do not convert all applications to reactive programming unnecessarily
* Use records for immutable data models
* Use sealed types where domain hierarchies are closed
* Apply record patterns only when readability improves
* Use switch pattern matching appropriately
* Confirm preview feature usage before recommending production adoption
* Include concurrency and regression tests
* Preserve existing architecture conventions
* Explain assumptions and migration impacts

---

# Summary

Java 21 provides a powerful foundation for modern enterprise development. Virtual threads, improved pattern matching, records, and concurrency enhancements allow teams to build scalable systems with simpler and more maintainable code.

The goal is not to use every new feature, but to apply the right feature at the right time while preserving clarity, reliability, and long-term maintainability.
