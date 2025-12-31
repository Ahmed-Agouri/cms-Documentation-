# ADR: Security & Authorization Strategy

## Context

The Complaint Management System (CMS) operates in a **multi-tenant, microservices-based architecture**. This means the platform must securely isolate tenant data (e.g., NatWest vs HSBC), protect user identities, and ensure only authorized users can perform actions such as creating complaints, resolving them, or viewing audit logs.

Security applies across services, including:

- **Authentication Service**
- **Complaint Service**
- **Audit Service**
- **Tenant Service**
- **API Gateway**

Security controls must protect enterprise-scale users such as banking and telecom organizations and support features like JWT authentication, audit logging, and API-key-protected internal service-to-service communication.

---

## Problem Statement

The security model must:

- Authenticate users reliably
- Enforce **role-based access control (RBAC)**
- Prevent cross-tenant data access
- Protect internal APIs between services
- Secure tokens and data in transit
- Provide traceability via audit logs
- Be compatible with a microservices architecture

Failure to secure these areas risks **data leakage, regulatory breaches, and unauthorized access** — unacceptable in regulated sectors such as banking.

---

## Considered Options

1. **JWT-Based Authentication with RBAC (Stateless Model)**
2. **Session-Based Authentication**
3. **OAuth2 / OpenID Connect**

---

## Decision Outcome  

### **Chosen Strategy: JWT Authentication + RBAC + API-Key Protected Internal Calls**

The CMS implements a **stateless JWT authentication model** at the API Gateway and services, combined with **role-based authorization** inside the application services.

Internal service-to-service communication (e.g., Complaint Service → Audit Service) is protected using **internal API Keys sent via headers**, preventing external callers from invoking internal endpoints.

Audit logging ensures **accountability and traceability** for all sensitive actions.

---

## Key Security Controls Implemented

### 🔐 1. Authentication — JWT Tokens
- Users log in via the Authentication Service  
- A signed **JWT token** is issued on success  
- Token contains:
  - `UserId`
  - `TenantId`
  - `Role`
- Tokens are validated at the API Gateway and microservices  
- **No token = request rejected**

This aligns with **modern cloud security best-practice**.

---

### 🏛 2. Authorization — Role-Based Access Control (RBAC)

Roles include:

- **Consumer** — create/view complaints, confirm resolution
- **Support Person** — update complaints and resolutions
- **Helpdesk/Admin** — manage and monitor complaints
- **System/Tenant Admin** — view tenant audit logs

Permissions are enforced in controllers & services.

Examples:

- A user **cannot confirm a complaint resolution** unless they own the complaint  
- Audit viewing is **tenant-restricted**  
- Complaint data is always scoped to **TenantId**  

---

### 🔐 3. Tenant Isolation

Tenant isolation is enforced by:

- Requiring **X-Tenant-Id header**
- Verifying tenant existence via Tenant Service
- Filtering all entity queries by `TenantId`
- Preventing cross-tenant lookups

This directly supports the case-study requirement that **NatWest cannot see Barclays data**.

---

### 🔑 4. Service-to-Service Security — Internal API Keys

Internal HTTP calls use **secure internal keys**, added automatically via HttpClient middleware. External callers cannot access private system APIs.

This prevents lateral movement between services.

---

### 📜 5. Audit Logging

Every important action is logged, such as:

- Complaint created
- Complaint updated
- Resolution added
- Resolution confirmed

This supports:

- Accountability
- Forensic tracking
- Security compliance obligations

---

### 🔒 6. Transport Security

All system communication is expected to use **HTTPS/TLS** in production.

---

## Consequences

### ✅ Pros

- Stateless JWT model scales well across microservices
- Strong tenant-level data isolation
- RBAC prevents unauthorized actions
- API-key internal calls protect backend services
- Full audit history supports compliance
- Aligns with enterprise-grade security standards

---

### ⚠️ Cons

- Token revocation requires additional design effort
- Incorrect tenant handling could expose data if mis-implemented
- API keys must be securely managed
- More complex than session-based systems

---

## Alternatives Considered

### 1️⃣ Session-Based Authentication
**Not chosen because:**

- Harder to scale across multiple services  
- Requires central session store  
- Increases coupling  



## Conclusion

The CMS adopts a **JWT-based authentication and RBAC security model**, combined with **tenant isolation, API-key-secured microservice communication, HTTPS transport security, and full audit logging**.

This approach:

- Supports **enterprise-level security expectations**
- Aligns with **microservices architecture**
- Ensures **tenant data separation**
- Meets **regulatory and audit requirements**
- Scales with projected **10% annual user growth**