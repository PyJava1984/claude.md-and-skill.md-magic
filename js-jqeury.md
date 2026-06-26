# Frontend Layer — JavaScript Engineering Guide

**File:** `frontend/javascript.md`
**Version:** 1.0

---

# Purpose

This guide defines standards for writing clean, maintainable, and scalable JavaScript in enterprise applications.

JavaScript code should prioritize:

* Maintainability
* Predictability
* Performance
* Security

---

# Philosophy

Core principles:

* Prefer clarity over cleverness
* Avoid global state
* Write modular code
* Keep side effects explicit
* Use modern ES6+ features

---

# Language Standards

Use modern JavaScript features.

---

# Variables

Prefer:

```javascript
const customer = getCustomer();
```

Use:

```javascript
let count = 0;
```

Avoid:

```javascript
var customer = {};
```

Reason:

* Better scope handling
* Reduced side effects

---

# Modern Syntax

Prefer:

## Arrow Functions

Example:

```javascript
const formatName = (name) => {

    return name.trim();

};
```

---

## Template Literals

Example:

```javascript
const message = `Hello ${name}`;
```

---

## Destructuring

Example:

```javascript
const { id, email } = customer;
```

---

## Modules

Prefer ES Modules:

```javascript
export function calculateTotal() {

}
```

Import:

```javascript
import { calculateTotal } from "./utils.js";
```

---

# Code Structure

Organize by feature.

Recommended:

```text
feature/

 ├── api.js

 ├── service.js

 ├── component.js

 └── utils.js
```

Benefits:

* Easier navigation
* Better ownership
* Better testing

---

# Functions

Functions should:

* Have one responsibility
* Stay small
* Be easy to test

---

# Avoid

Large functions:

```text
fetchData()

validate()

transform()

save()

render()

sendEmail()

```

Split responsibilities.

---

# Control Flow

Prefer early returns.

Example:

```javascript
function processCustomer(customer) {

    if (!customer) {

        return;

    }


    saveCustomer(customer);

}
```

Avoid deeply nested conditions.

---

# Async Programming

Prefer:

```javascript
async/await
```

Avoid:

```javascript
Promise
    .then()
    .then()
    .then();
```

---

# Example

```javascript
async function fetchCustomer(id) {

    const response =
        await api.get(`/customers/${id}`);

    return response.data;

}
```

---

# Error Handling

Always handle failures.

Use:

* try/catch
* Meaningful logging
* User-safe error messages

Example:

```javascript
try {

    await saveCustomer();

}
catch(error) {

    console.error(
        "Customer save failed",
        error
    );

}
```

---

# Avoid Silent Failures

Bad:

```javascript
catch(error){

}
```

Problems:

* Hard debugging
* Hidden failures

---

# State Management

Rules:

* Avoid global mutable state
* Centralize shared state
* Separate UI state from business state

---

# Good Separation

```text
UI State

    |

Business Logic

    |

API Layer
```

---

# Performance

Optimize:

## DOM Operations

Avoid:

* Repeated DOM queries
* Excessive updates

---

## Expensive Operations

Use:

* Debouncing
* Throttling
* Memoization where appropriate

---

# Rendering Performance

Minimize:

* Reflows
* Repaints
* Unnecessary UI updates

---

# Security

Always:

* Sanitize user input
* Validate API responses
* Prevent XSS

---

# Avoid

Never use:

```javascript
eval()
```

Reason:

* Security risk
* Difficult maintenance

---

# JavaScript Anti-Patterns

Avoid:

## Global Variables

Problems:

* Hidden dependencies
* State conflicts

---

## Deep Callback Nesting

Problems:

* Hard to read
* Hard to debug

---

## Mixed Responsibilities

Avoid:

```text
UI Code

+

Business Logic

+

API Calls
```

in one file.

---

## Monolithic Files

Split by:

* Feature
* Responsibility
* Module

---

# AI Coding Checklist

When generating JavaScript:

* Use ES6+ syntax
* Keep functions focused
* Prefer async/await
* Avoid global mutable state
* Handle errors explicitly
* Keep modules separated
* Consider performance and security

---

# Frontend Layer — jQuery Engineering Guide

**File:** `frontend/jquery.md`
**Version:** 1.0

---

# Purpose

jQuery is a legacy JavaScript library still used in many enterprise applications.

This guide defines safe maintenance practices and modernization strategies.

---

# Philosophy

Core principles:

* Do not expand jQuery usage
* Prefer minimal changes
* Maintain existing behavior safely
* Plan migration toward modern frameworks

---

# Usage Rules

Avoid:

* New large jQuery features
* jQuery-heavy architecture
* Mixing business logic with DOM code

---

# Example

```javascript
$("#submitBtn")
    .on("click", function () {

        $("#status")
            .text("Loading...");

    });
```

---

# DOM Manipulation

Rules:

## Cache Selectors

Avoid:

```javascript
$("#customer")
$("#customer")
$("#customer")
```

Prefer:

```javascript
const customerElement =
    $("#customer");
```

---

## Batch Updates

Avoid:

* Multiple unnecessary DOM changes

Prefer:

* Single update operations

---

# Event Handling

Prefer:

## Event Delegation

Useful for dynamic elements.

Example:

```javascript
$(document)
.on(
    "click",
    ".button",
    function(){

    }
);
```

---

# Avoid Inline Events

Avoid:

```html
<button onclick="save()">
```

Prefer:

```javascript
$("#save")
.on(
    "click",
    save
);
```

---

# Performance

Avoid:

## Repeated DOM Traversal

Problems:

* Slow execution
* Unnecessary work

---

## Heavy Animations

Use carefully.

---

## Uncontrolled Events

Use:

* Debouncing
* Throttling

for:

* Scroll
* Resize
* Search input

---

# Migration Strategy

Gradually replace jQuery with:

```text
React

Angular

Vue

Vanilla JavaScript Modules
```

---

# Migration Approach

Recommended:

```text
Existing jQuery

        ↓

Extract reusable logic

        ↓

Create modules

        ↓

Move to framework components
```

---

# jQuery Anti-Patterns

Avoid:

## Expanding Legacy Patterns

Do not create new jQuery architecture.

---

## Business Logic in Handlers

Avoid:

```javascript
click handler

    ↓

Validation

    ↓

API Call

    ↓

Database Logic
```

---

# AI Coding Checklist

When generating jQuery code:

* Keep changes minimal
* Avoid new jQuery-heavy patterns
* Prefer migration-friendly solutions
* Separate UI logic from business logic
* Avoid unnecessary DOM operations

