# Frontend Layer — React Engineering Guide

**File:** `frontend/react.md`
**Version:** 1.0

---

# Purpose

React is a component-based UI library designed for building scalable and maintainable frontend applications.

React applications should prioritize:

* Reusable components
* Clear data flow
* Predictable state management
* Separation of UI and business logic

---

# Philosophy

Core principles:

* Composition over inheritance
* Functional components preferred
* Hooks for state and lifecycle management
* Keep components pure where possible
* Build small reusable UI units

---

# React Architecture

Typical flow:

```text id="h6v2qa"
Component

     |

Hooks

     |

Service/API Layer

     |

Backend
```

Components should focus on rendering and user interaction.

---

# Component Design

Prefer functional components.

Example:

```jsx id="3m8y4q"
function CustomerCard({ customer }) {

    return (

        <div>
            {customer.name}
        </div>

    );

}
```

---

# Component Rules

Components should:

* Be small
* Have a single responsibility
* Accept data through props
* Avoid hidden side effects

---

# Avoid

Large components containing:

```text id="n8v4kx"
UI

+

Business Logic

+

API Calls

+

State Management
```

Split responsibilities.

---

# Composition

React prefers composition instead of inheritance.

Example:

```jsx id="u4v9pk"
<Card>

    <CustomerInfo />

</Card>
```

Benefits:

* Reusability
* Flexibility
* Cleaner architecture

---

# Hooks

Hooks manage:

* State
* Lifecycle behavior
* Shared logic

---

# Hook Rules

Always:

* Call hooks at the top level
* Keep hook usage predictable
* Follow React hook rules

---

# Avoid

Conditional hooks:

```jsx id="w8y1gz"
if(user){

    useState();

}
```

Problems:

* Breaks hook execution order

---

# Custom Hooks

Extract reusable logic.

Example:

```javascript id="4h9w2s"
function useCustomer(id){

    // reusable logic

}
```

Benefits:

* Cleaner components
* Better testing
* Better reuse

---

# State Management

Choose based on application complexity.

---

# Local State

Use:

```javascript id="1s5v3m"
useState()
```

For:

* Component-level state
* Small UI interactions

Examples:

* Form values
* Toggle states

---

# Context API

Use for:

* Shared application data
* Theme
* Authentication state

Avoid using it for all application state.

---

# External State Libraries

Examples:

```text id="z7x4nm"
Redux

Zustand
```

Use when:

* State becomes complex
* Many components depend on shared data
* Predictability is required

---

# Performance Optimization

Optimize carefully.

---

# Re-render Management

Avoid unnecessary renders.

Use:

* Component splitting
* Memoization when needed
* Proper state placement

---

# Memoization

Tools:

```javascript id="d3h8qz"
React.memo()

useMemo()

useCallback()
```

Use only when performance requires it.

Avoid premature optimization.

---

# Component Splitting

Prefer:

```text id="2k8m4c"
CustomerPage

 ├── CustomerHeader

 ├── CustomerDetails

 └── CustomerActions
```

over:

```text id="f7x1cz"
CustomerPage.jsx

(1000 lines)
```

---

# Side Effects

Use:

```javascript id="q5r9wp"
useEffect()
```

for:

* API calls
* External subscriptions
* Browser integrations

---

# useEffect Rules

Always:

* Keep dependencies correct
* Avoid unnecessary effects
* Separate unrelated concerns

---

# Example

```javascript id="v2x9jh"
useEffect(() => {

    loadCustomer();

}, [customerId]);
```

---

# Avoid Infinite Loops

Problem example:

```javascript id="n6j3pp"
useEffect(() => {

    setValue(value + 1);

});
```

No dependency control.

---

# API Integration

Separate API logic.

Recommended:

```text id="r4m7sk"
components/

services/

api/

hooks/
```

---

# Security

React provides protection against many common issues, but secure development is required.

---

# Input Handling

Always:

* Validate user input
* Sanitize dynamic content
* Validate API responses

---

# Dangerous HTML

Avoid:

```jsx id="q3h6mv"
dangerouslySetInnerHTML
```

unless absolutely required.

If used:

* Sanitize content first
* Restrict trusted sources

---

# React Anti-Patterns

Avoid:

## Large Monolithic Components

Problems:

* Difficult maintenance
* Poor reuse

---

## Overusing Context API

Problems:

* Hard-to-track updates
* Unnecessary renders

---

## Uncontrolled Side Effects

Problems:

* Unexpected behavior
* Difficult debugging

---

## Mixing UI and Business Logic

Avoid:

```text id="m4k8tw"
Component

    ↓

Business Rules

    ↓

Database Logic
```

---

# Testing Recommendations

Test:

* Component rendering
* User interactions
* Custom hooks
* State behavior

Prefer:

* Unit tests
* Component tests
* Integration tests

---

# AI Coding Checklist

When generating React code:

* Use functional components
* Use hooks correctly
* Keep components small
* Separate UI and business logic
* Avoid unnecessary re-renders
* Manage state intentionally
* Handle side effects carefully
* Follow security practices
