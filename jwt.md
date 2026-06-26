# JWT Engineering Guide

**File:** `security/jwt.md`
**Version:** 1.0

---

# Purpose

This document defines enterprise-level best practices for designing, issuing, validating, and securing JSON Web Tokens (JWTs).

Topics covered:

* JWT structure
* Token lifecycle
* Secure storage
* Signing strategies
* Refresh token design
* Validation rules
* Common security mistakes

---

# JWT Overview

JWT (JSON Web Token) is a compact token format commonly used for:

* Stateless authentication
* API authorization
* Service-to-service communication

JWT allows systems to verify identity without storing session state on the server.

---

# JWT Structure

A JWT consists of three parts:

```text
Header.Payload.Signature
```

Example:

```text
xxxxx.yyyyy.zzzzz
```

---

# JWT Components

## Header

Contains metadata:

Example:

```json
{
  "alg": "RS256",
  "typ": "JWT"
}
```

Defines:

* Signing algorithm
* Token type

---

## Payload

Contains claims.

Example:

```json
{
  "sub": "user123",
  "roles": [
    "USER"
  ],
  "iat": 1700000000,
  "exp": 1700003600
}
```

---

## Signature

Ensures:

* Token integrity
* Authenticity
* Protection against modification

---

# Token Design Principles

JWT payloads should remain minimal.

Include only required information.

Recommended claims:

| Claim   | Purpose          |
| ------- | ---------------- |
| `sub`   | User identity    |
| `exp`   | Expiration time  |
| `iat`   | Issued timestamp |
| `roles` | User authorities |
| `scope` | Permissions      |

---

# Never Store Sensitive Data

Do not include:

* Passwords
* Personal secrets
* API keys
* Financial information

JWT payloads are encoded, not encrypted.

Anyone holding the token can decode the payload.

---

# JWT Lifecycle

Standard flow:

```text
User Authentication

        ↓

Server generates JWT

        ↓

Client securely stores token

        ↓

Client sends token with requests

        ↓

Server validates JWT

        ↓

Authorization decision

        ↓

Access granted
```

---

# Token Storage Recommendations

## Web Applications

Preferred:

```text
HttpOnly Secure Cookies
```

Advantages:

* Prevent JavaScript access
* Reduce token theft risk
* Support secure browser handling

Avoid:

```text
localStorage
```

for sensitive authentication tokens.

---

## Mobile Applications

Use platform secure storage:

Examples:

* iOS Keychain
* Android Keystore

Avoid:

* Plain application storage
* Shared preferences without encryption

---

# JWT Security Rules

Always:

* Validate signature
* Validate expiration
* Use secure algorithms
* Use short-lived access tokens
* Rotate signing keys

Never:

* Trust decoded client-side claims
* Accept unsigned tokens
* Store sensitive information in payload

---

# Signing Algorithms

## Recommended

### RS256

Asymmetric cryptography:

```text
Private Key
     |
     ↓
Signs Token


Public Key
     |
     ↓
Validates Token
```

Benefits:

* Better for distributed systems
* Safer key sharing model

---

# Avoid

## None Algorithm

Example:

```json
{
  "alg":"none"
}
```

Reason:

* No signature validation

---

## Weak HMAC Secrets

Avoid:

```text
short predictable secrets
```

Use:

* Strong random secrets
* Proper secret management

---

# Refresh Token Strategy

Use two-token architecture:

```text
Access Token
        |
        ↓
Short-lived


Refresh Token
        |
        ↓
Long-lived
```

---

# Access Token

Characteristics:

* Short expiration
* Used for API requests
* Frequently rotated

Example:

```text
5 - 15 minutes lifetime
```

---

# Refresh Token

Characteristics:

* Longer lifetime
* Stored securely
* Used to obtain new access tokens

Requirements:

* Rotate on usage
* Revoke when compromised
* Track usage where appropriate

---

# Refresh Token Rotation Flow

```text
Client

   |
   ↓

Refresh Token

   |
   ↓

Server validates

   |
   ↓

Issue new Access Token

   |
   ↓

Issue new Refresh Token

   |
   ↓

Invalidate old Refresh Token
```

---

# Common JWT Mistakes

Avoid:

## Overloading JWT Payload

Bad:

```json
{
 "userData": "everything"
}
```

JWT should not become a user database.

---

## Long-Lived Access Tokens

Problem:

* Increased exposure window
* Harder revocation

---

## Trusting Client Decoded Claims

Incorrect:

```text
Decode token → trust data
```

Correct:

```text
Validate signature → validate claims → authorize
```

---

## No Refresh Strategy

Problem:

* Forces long-lived access tokens
* Reduces security

---

# JWT Validation Checklist

Every request must validate:

## Signature

Confirm:

* Correct signing key
* Correct algorithm

---

## Expiration

Check:

```text
exp
```

Reject expired tokens.

---

## Issuer

Recommended:

```text
iss
```

Validates token source.

---

## Audience

Recommended:

```text
aud
```

Ensures token is intended for the service.

---

## Token Integrity

Validate:

* Structure
* Claims
* Signature
* Expiration

---

# Operational Security

Protect:

* Signing keys
* Refresh tokens
* Authentication logs

Use:

* Secret managers
* Key rotation
* Access controls

---

# Logging Rules

Never log:

* JWT tokens
* Authorization headers
* Refresh tokens
* Sensitive claims

Allowed:

* Authentication event IDs
* User identifiers (where appropriate)
* Failure reasons

---

# Performance Considerations

Optimize:

* Token validation speed
* Key loading
* Permission lookup

Avoid:

* Database calls on every filter execution
* Expensive authorization checks

Use:

* Cached public keys
* Efficient claim validation

---

# Production JWT Checklist

Before release:

* Use strong signing algorithms
* Keep access tokens short-lived
* Implement refresh token rotation
* Protect signing keys
* Validate every request
* Monitor suspicious token activity
* Follow OWASP recommendations

---

# AI Coding Checklist

When generating JWT code:

* Keep payload minimal
* Use secure signing algorithms
* Validate signature and expiration
* Implement refresh token strategy when required
* Never expose tokens in logs
* Never trust client-side claims
* Protect secrets externally
* Avoid security shortcuts

---

# Next Step — Integration Layer

Next guides:

```text
integration/
 ├── kafka.md
 ├── rabbitmq.md
 ├── apache-camel.md
 ├── graphql.md
 └── grpc.md
```

Topics covered:

* Event-driven architecture
* Messaging patterns
* Reliability guarantees
* Retry strategies
* Dead-letter queues
* Schema design
* Enterprise integration patterns
