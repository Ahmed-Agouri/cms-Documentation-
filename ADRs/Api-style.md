# ADR: API Style Decision — RESTful APIs for the CMS

## Context
The Complaint Management System (CMS) requires a reliable, secure, and scalable way for client applications (web, mobile, internal services) to communicate with backend services. The API layer must support clear resource‑based operations, authentication enforcement, tenant isolation, and integration with an API Gateway. It must also remain maintainable and easy to test during Proof‑of‑Concept (PoC) development.

The choice of API style directly affects performance, developer experience, security, and long‑term extensibility of the platform.

## Problem Statement
The CMS requires an API communication style that:
- Supports **resource‑driven CRUD workflows**
- Is **easy to secure using JWT authentication**
- Works well with **microservices and an API gateway**
- Supports **JSON‑based data exchange**
- Is **simple for frontend clients to consume**
- Allows **clear versioning and documentation**
- Is suitable for both **current PoC scope and future scaling**

## Considered Options
1. **RESTful APIs (Representational State Transfer)**
2. **GraphQL**
3. **gRPC**
4. **SOAP / XML‑based Services**

---

## Decision Outcome — Chosen Option: RESTful APIs

The CMS adopts a **RESTful API style** across backend services.

REST provides a simple, well‑understood, resource‑oriented approach that maps naturally to CMS entities such as complaints, tenants, audits, and users. JSON responses integrate cleanly with the React frontend, and REST supports standard web practices such as HTTP methods, status codes, routing, and middleware.

This approach also aligns strongly with ASP.NET Core Web API, API Gateway routing, and unit/API testing tools (e.g., Postman).

---

## Consequences

### ✅ Benefits of Choosing REST
- **Simple and intuitive** — easy for frontend and backend developers to consume
- **Resource‑oriented** — clean mapping to CMS domain objects
- **Secure and mature** — integrates naturally with JWT authentication and middleware
- **Browser‑friendly** — works seamlessly with HTTP/JSON
- **Great tooling support** — Postman, Swagger/OpenAPI, monitoring tools
- **Easy to cache and version**
- **Well‑suited for microservices exposed via an API Gateway**
- **Works cleanly with async .NET APIs and EF Core data models**

---

### ⚠️ Trade‑offs / Limitations
- Over/under‑fetching may occur in some scenarios
- Not optimised for very high‑chatty or streaming workloads
- Strongly coupled to endpoint design and versioning strategy

These limitations were assessed and found **acceptable for the CMS functional and non‑functional requirements**.

---

## Alternatives Considered

### ❌ GraphQL
**Pros**
- Flexible queries (clients request only what they need)
- Strong schema‑driven development
- Useful for complex UI dashboards

**Cons**
- **More complicated security enforcement per‑field**
- **Caching and logging more difficult**
- **Adds unnecessary overhead for CRUD‑centric workflows**

GraphQL was rejected because the CMS mostly performs structured CRUD operations where REST is simpler and safer.

---

### ❌ gRPC
**Pros**
- Very high performance
- Strong for internal microservice‑to‑microservice calls
- Contract‑first APIs

**Cons**
- Not browser‑native (requires proxy / transport translation)
- Steeper learning curve
- Harder debugging compared to JSON REST
- Over‑engineered for external web‑facing APIs

gRPC may be considered **later for internal service communication**, but not for client‑facing APIs.

---

### ❌ SOAP / XML Web Services
**Pros**
- Strong enterprise‑level standards
- Built‑in contract validation

**Cons**
- Heavyweight & verbose XML
- Slower development
- Poor fit for modern web/mobile apps
- Limited flexibility vs REST

SOAP was rejected as unnecessarily complex.

---

## Alignment with the Proof‑of‑Concept
For the PoC implementation:
- REST APIs were implemented in **ASP.NET Core Web API**
- API routes are exposed clearly (e.g., `/api/complaints`, `/api/authentication/login`)
- JSON is used for request/response bodies
- Postman was used for API testing
- JWT authentication protects secure endpoints
- API Gateway routing supports REST paths

This ensured the CMS PoC remained **testable, lightweight, secure, and aligned with Microservices best practices**.

---

## Decision Summary
**REST was selected as the API style for the CMS because it provides the best balance of simplicity, security, developer productivity, and scalability — both for the PoC and future production expansion.**

REST allows the CMS to maintain a **clear, resource‑driven API design**, supports **JWT‑based authentication and tenant isolation**, and integrates effectively with the chosen **.NET technology stack and API Gateway architecture**.
