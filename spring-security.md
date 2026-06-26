# Part 9 — Spring Security Engineering Guide

**File:** `spring/spring-security.md`
**Version:** 1.0

---

# Purpose

Spring Security is the standard framework for securing Spring applications.

This guide defines consistent patterns for:

* Authentication
* Authorization
* JWT-based security
* Secure API design
* Production security practices

---

# Philosophy

Security principles:

* Security must be enabled by default
* Authentication and authorization must be centralized
* Never trust client input
* Keep security logic declarative
* Prefer stateless authentication for APIs
* Apply security before business logic execution

---

# Security Architecture

Typical request flow:

```text
Client
   |
   ↓
Security Filter Chain
   |
   ↓
Authentication
   |
   ↓
Authorization
   |
   ↓
Controller
   |
   ↓
Service
```

Security checks must happen before application logic executes.

---

# Authentication vs Authorization

## Authentication

Answers:

> Who is the user?

Examples:

* Login verification
* Identity validation
* Token verification

---

## Authorization

Answers:

> What is the user allowed to do?

Examples:

* Access permissions
* Roles
* Resource ownership

---

## Rule

Never mix authentication and authorization responsibilities.

---

# JWT-Based Authentication

## Overview

JWT is commonly used for stateless authentication in REST APIs.

A JWT contains:

* User identity
* Roles / authorities
* Expiration time
* Optional claims

---

# JWT Structure

```text
Header.Payload.Signature
```

Example:

```text
xxxxx.yyyyy.zzzzz
```

---

# JWT Best Practices

Always:

* Keep payload minimal
* Validate signature
* Check expiration
* Use strong keys
* Rotate keys periodically

Never:

* Store passwords
* Store sensitive information
* Trust client-modified claims

Recommended:

* Strong secret keys
* RSA / asymmetric keys for distributed systems

---

# JWT Authentication Flow

```text
User Login
     |
     ↓
Generate JWT
     |
     ↓
Client stores token
     |
     ↓
Client sends Authorization header
     |
     ↓
Server validates token
     |
     ↓
Access granted
```

---

# Spring Security Configuration

Example:

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {


    @Bean
    public SecurityFilterChain filterChain(
            HttpSecurity http) throws Exception {


        return http

            .csrf(csrf -> csrf.disable())

            .sessionManagement(session ->
                session.sessionCreationPolicy(
                    SessionCreationPolicy.STATELESS
                )
            )

            .authorizeHttpRequests(auth -> auth

                .requestMatchers("/auth/**")
                .permitAll()

                .anyRequest()
                .authenticated()
            )

            .build();
    }
}
```

---

# JWT Authentication Filter

Responsibilities:

* Extract token
* Validate token
* Create authentication object
* Set security context

Example:

```java
public class JwtAuthenticationFilter
        extends OncePerRequestFilter {


    @Override
    protected void doFilterInternal(
            HttpServletRequest request,
            HttpServletResponse response,
            FilterChain filterChain)

            throws ServletException, IOException {


        String token = extractToken(request);


        if(token != null && validate(token)) {

            Authentication authentication =
                    getAuthentication(token);


            SecurityContextHolder
                    .getContext()
                    .setAuthentication(authentication);
        }


        filterChain.doFilter(request,response);
    }
}
```

---

# Authorization Models

## Role-Based Access Control (RBAC)

Common roles:

```text
ADMIN
USER
MANAGER
```

Example:

```java
@PreAuthorize("hasRole('ADMIN')")
public void deleteUser(Long id) {

}
```

---

# Method Security

Enable method-level security:

```java
@PreAuthorize
@PostAuthorize
@Secured
```

Recommendation:

Prefer:

```java
@PreAuthorize
```

because it provides expressive authorization rules.

---

# Password Security

Never store plaintext passwords.

Use strong hashing:

Recommended:

* BCrypt
* Argon2

Example:

```java
@Bean
PasswordEncoder passwordEncoder(){

    return new BCryptPasswordEncoder();

}
```

---

# CORS Configuration

Define allowed origins explicitly.

Example:

```java
@Bean
public CorsConfigurationSource corsConfigurationSource(){


    CorsConfiguration config =
            new CorsConfiguration();


    config.setAllowedOrigins(
        List.of(
            "https://yourdomain.com"
        )
    );


    config.setAllowedMethods(
        List.of(
            "GET",
            "POST",
            "PUT",
            "DELETE"
        )
    );


    config.setAllowedHeaders(
        List.of("*")
    );


    UrlBasedCorsConfigurationSource source =
            new UrlBasedCorsConfigurationSource();


    source.registerCorsConfiguration(
        "/**",
        config
    );


    return source;
}
```

Production rules:

Avoid:

```text
Allow-Origin: *
```

---

# CSRF Protection

Rules:

For JWT stateless APIs:

```text
CSRF disabled
```

For session-based applications:

```text
CSRF enabled
```

Never disable security protections without understanding the architecture.

---

# Security Headers

Configure:

* HSTS
* X-Content-Type-Options
* X-Frame-Options
* Content-Security-Policy

Goals:

* Prevent browser attacks
* Reduce injection risks
* Improve application security posture

---

# Exception Handling

Standard responses:

| Status | Meaning      |
| ------ | ------------ |
| 401    | Unauthorized |
| 403    | Forbidden    |

Rules:

Never expose:

* Stack traces
* Internal security details
* Sensitive information

---

# OAuth2 Concepts

Use OAuth2 when integrating with:

* Google
* GitHub
* Enterprise identity providers

Common flows:

## Authorization Code Flow

Used for:

* User login
* Browser-based applications

---

## Client Credentials Flow

Used for:

* Service-to-service authentication

---

Spring Security supports:

* OAuth2 clients
* Resource servers
* Identity provider integrations

---

# Session Management

## JWT Applications

Use:

```text
Stateless sessions
```

Avoid:

* Server-side session storage

---

## Legacy Session Applications

Protect against:

* Session fixation
* Long-lived sessions
* Unauthorized reuse

---

# Logging & Monitoring

Log:

* Failed authentication attempts
* Authorization failures
* Token validation failures

Never log:

* Passwords
* JWT tokens
* Authorization headers
* Sensitive user information

---

# Common Security Anti-Patterns

Avoid:

❌ Hardcoded secrets

```java
String secret="password123";
```

---

❌ Disabling security globally

---

❌ Allowing all CORS origins in production

---

❌ Storing sensitive data inside JWT

---

❌ Weak password hashing

---

❌ Security logic inside controllers

---

# Performance Considerations

Optimize:

* JWT validation speed
* Filter chain execution
* Permission lookup

Avoid:

* Heavy database calls inside filters
* Repeated permission calculations

Use:

* Permission caching where appropriate
* Efficient token validation

---

# Production Security Checklist

Before deployment:

* Enable authentication by default
* Validate every token
* Use secure password hashing
* Protect secrets externally
* Configure CORS correctly
* Enable security headers
* Monitor authentication failures
* Review OWASP risks

---

# AI Coding Checklist

When generating Spring Security code:

* Always enforce authentication by default
* Prefer stateless authentication for APIs
* Centralize security configuration
* Validate JWT signature and expiration
* Use role-based or attribute-based access control
* Never expose sensitive data in logs
* Never store secrets in source code
* Follow OWASP secure design practices

---

# Next Step — Part 10

Next topics:

## Messaging & Integration Engineering

Planned guides:

* Kafka Engineering Guide
* Apache Camel Engineering Guide
* RabbitMQ Engineering Guide
* Event-driven architecture patterns
* Reliable messaging
* Retry strategies
* Dead-letter queues
* Distributed systems practices
