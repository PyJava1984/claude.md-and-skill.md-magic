# Frontend Layer — Angular Engineering Guide

**File:** `frontend/angular.md`
**Version:** 1.0

---

# Purpose

Angular is a full-featured frontend framework designed for building scalable enterprise applications.

Angular provides:

* Component architecture
* TypeScript-based development
* Dependency injection
* Reactive programming
* Structured application design

---

# Philosophy

Core principles:

* Component-driven architecture
* Strong typing with TypeScript
* Reactive programming with RxJS
* Modular design
* Lazy loading by default
* Separation of concerns

---

# Angular Architecture

Typical flow:

```text id="2j7k0s"
Component

    |

Service

    |

API Layer

    |

Backend
```

Components handle presentation.

Services handle business operations.

---

# Project Structure

Recommended:

```text id="e5q2mg"
app/

├── core/

├── shared/

├── features/

├── services/

└── models/
```

---

# Folder Responsibilities

## core/

Contains:

* Application-wide services
* Guards
* Interceptors
* Global configuration

Examples:

```text id="2k1f1d"
auth.service.ts

http.interceptor.ts
```

---

## shared/

Contains reusable:

* Components
* Pipes
* Directives
* Utilities

Used across features.

---

## features/

Contains business modules.

Example:

```text id="x9q3kr"
features/

 └── customer/

      ├── components/

      ├── services/

      └── models/
```

---

# Components

Components represent UI behavior.

---

# Component Rules

Components should:

* Stay small
* Handle presentation
* Delegate logic
* Avoid direct API calls

---

# Avoid

Large components containing:

```text id="h6a1ku"
UI

+

Business Logic

+

API Calls

+

Data Transformation
```

---

# Good Component Flow

```text id="9i2l6x"
Component

    ↓

Service

    ↓

Backend API
```

---

# Services

Services contain reusable application logic.

Responsibilities:

* Business logic
* API communication
* Shared state
* Data transformation

---

# Service Benefits

Services are:

* Injectable
* Reusable
* Testable

Example:

```typescript id="9m4gsy"
@Injectable({
  providedIn: 'root'
})
export class CustomerService {

}
```

---

# Dependency Injection

Prefer Angular dependency injection.

Avoid:

* Creating service instances manually
* Tight coupling

---

# RxJS

Angular uses RxJS for reactive programming.

---

# Observable Rules

Use:

* Observables correctly
* Operators
* Proper subscription handling

---

# Avoid Nested Subscriptions

Avoid:

```typescript id="w5r3u9"
serviceA()
.subscribe(a => {

    serviceB()
    .subscribe(b => {

    });

});
```

Problems:

* Hard to read
* Hard error handling

---

# Prefer Operators

Use:

## map

Transform data.

---

## switchMap

Switch async operations.

Example:

```typescript id="b8q2kk"
customerId$

.pipe(
    switchMap(id =>
        customerService.get(id)
    )
);
```

---

## mergeMap

Run parallel async operations.

---

# Forms

Angular supports:

* Template-driven forms
* Reactive forms

---

# Enterprise Recommendation

Prefer:

```text id="8t5z7k"
Reactive Forms
```

Benefits:

* Better validation
* Better testing
* Better scalability

---

# Avoid

Large enterprise forms using:

```text id="7k8zq2"
Template-driven forms
```

because complexity grows quickly.

---

# Performance Optimization

Optimize Angular applications using:

---

# Lazy Loading

Load features only when needed.

Example:

```text id="4p9w3k"
Application Start

      ↓

Load Core

      ↓

Load Feature Module When Required
```

Benefits:

* Faster startup
* Smaller initial bundle

---

# Change Detection

Prefer:

```typescript id="j9m2qa"
ChangeDetectionStrategy.OnPush
```

Benefits:

* Reduced unnecessary rendering
* Better performance

---

# Avoid

Unnecessary:

* Watchers
* Re-rendering
* Heavy template calculations

---

# State Management

Choose based on complexity.

---

# Simple Applications

Use:

* Services
* RxJS BehaviorSubject

---

# Complex Applications

Use:

```text id="5c2k7x"
NgRx
```

for:

* Large shared state
* Predictable state transitions
* Complex workflows

---

# State Management Rules

Avoid:

* Scattered state
* Duplicate sources of truth
* Global mutable variables

---

# Security

Angular provides built-in protections, but applications must still follow secure practices.

---

# Input Handling

Always:

* Validate user input
* Sanitize dynamic content

---

# XSS Protection

Avoid unsafe DOM manipulation.

---

# Avoid

Direct HTML injection:

```typescript id="n2j7wv"
element.innerHTML = value;
```

unless properly sanitized.

---

# API Security

Use:

* HTTP interceptors
* Token handling
* Proper error management

---

# Angular Anti-Patterns

Avoid:

## Fat Components

Move logic into services.

---

## Logic in Templates

Avoid:

```html id="r5f8nd"
{{ calculateSomething() }}
```

for expensive operations.

---

## Manual Subscription Management

Prefer:

* async pipe
* automatic cleanup patterns

---

# AI Coding Checklist

When generating Angular code:

* Use strict TypeScript
* Keep components thin
* Move logic into services
* Use RxJS operators properly
* Prefer reactive forms
* Avoid logic-heavy templates
* Use lazy loading
* Apply security best practices
