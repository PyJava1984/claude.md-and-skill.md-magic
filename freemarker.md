# Frontend Layer — FreeMarker (FTL) Engineering Guide

**File:** `frontend/freemarker.md`
**Version:** 1.0

---

# Purpose

FreeMarker (FTL — FreeMarker Template Language) is a server-side templating engine commonly used in JVM-based applications for generating HTML, emails, documents, and dynamic content.

This guide defines standards for writing clean, secure, and maintainable FreeMarker templates.

---

# Philosophy

Core principles:

* Templates are for presentation, not business logic
* Keep templates simple and readable
* Separate data preparation from rendering
* Avoid duplicating backend logic
* Security must be enforced at output boundaries

---

# Template Architecture

Typical flow:

```text id="m4q8yx"
Controller

     |

Model Data

     |

FreeMarker Template

     |

Generated Output
```

The backend prepares data.

The template renders data.

---

# Project Structure

Recommended:

```text id="k9v3pz"
templates/

├── layout/

├── fragments/

├── pages/

├── components/

└── emails/
```

---

# Template Responsibilities

Templates should handle:

* Display formatting
* Conditional rendering
* Iteration
* Layout composition

Avoid:

* Business rules
* Database access
* Complex calculations

---

# Variables

Use clear names.

Example:

```ftl id="c7x2mw"
<h1>
${customer.name}
</h1>
```

---

# Avoid

Unclear variables:

```ftl id="a9z5kw"
${x}
```

Prefer:

```ftl id="p3n7qs"
${customerName}
```

---

# Null Handling

Always consider missing values.

Example:

```ftl id="d5m8vz"
${customer.email!"Not Available"}
```

Benefits:

* Prevent template failures
* Improve user experience

---

# Conditionals

Use simple conditions.

Example:

```ftl id="w2q6ky"
<#if customer.active>

    Active

<#else>

    Inactive

</#if>
```

---

# Avoid Complex Logic

Bad:

```ftl id="r8m4kp"
<#if
    customer.orders?size > 10
    && customer.status == "ACTIVE"
>
```

Move complex decisions into backend code.

---

# Loops

Use loops for rendering collections.

Example:

```ftl id="e6v9tz"
<#list customers as customer>

    <p>
        ${customer.name}
    </p>

</#list>
```

---

# Reusable Components

Use macros and includes.

Example:

```ftl id="n5k7xr"
<#include "header.ftl">
```

Benefits:

* Reuse
* Consistency
* Easier maintenance

---

# Layout Design

Recommended:

```text id="y4p8mz"
layout/

    base.ftl


fragments/

    header.ftl

    footer.ftl


pages/

    customer.ftl
```

---

# Data Preparation

Prefer:

```text id="j6m2qw"
Controller

   ↓

Service

   ↓

Model

   ↓

Template
```

---

# Avoid

Templates directly calling:

* Services
* Repositories
* External APIs

---

# Security

FreeMarker output should always consider security.

---

# XSS Protection

Use escaping.

Example:

```ftl id="u3p9vk"
${userInput}
```

should be escaped by default configuration.

---

# Avoid

Unsafe rendering:

```ftl id="b7m5qa"
${unsafeHtml?no_esc}
```

unless content is trusted and sanitized.

---

# Sensitive Data

Never expose:

* Passwords
* Tokens
* Internal identifiers
* Sensitive metadata

---

# Performance

Optimize:

* Template reuse
* Fragment usage
* Data preparation

Avoid:

* Large nested loops
* Expensive calculations
* Excessive formatting logic

---

# Internationalization

Support localization.

Example:

```ftl id="x2m8vp"
${message.customer.welcome}
```

Benefits:

* Multi-language support
* Consistent messaging

---

# Error Handling

Handle missing data gracefully.

Avoid:

* Broken pages
* Template exceptions in production

---

# FreeMarker Anti-Patterns

Avoid:

## Business Logic in Templates

Bad:

```text id="z8q1mw"
Template

    ↓

Complex Rules

    ↓

Calculations
```

---

## Huge Templates

Problems:

* Difficult maintenance
* Poor reuse

---

## Direct Database Access

Templates should never access persistence layers.

---

## Unsafe HTML Rendering

Problems:

* XSS vulnerabilities

---

# AI Coding Checklist

When generating FreeMarker code:

* Keep templates presentation-focused
* Avoid business logic
* Use reusable fragments
* Handle null values safely
* Escape user content
* Avoid exposing sensitive data
* Keep templates small and maintainable

