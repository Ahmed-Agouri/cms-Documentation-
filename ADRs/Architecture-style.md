# CMS Architecture Style — Architecture Decision Record (ADR)

## Context

The Complaint Management System (CMS) is intended to support very large enterprise clients such as UK and European banking and telecom organisations. These clients typically support **millions of consumers**, with the case study requiring the solution to scale with at least **10% annual user growth**. The backend must also support multi‑tenant isolation, secure authentication, independent service deployment, and future expansion such as chatbot integration.

To meet these requirements, an architectural style was required that supports **scalability, resilience, independent deployment, and clear separation of concerns** across services such as authentication, complaint handling, tenant management, and auditing.

## Problem Statement

The architecture needed to address the following key concerns:

- Support multi‑tenant data isolation and security
- Allow services to **scale independently** (e.g., complaints service scaling faster than tenant service)
- Reduce system downtime when deploying or updating components
- Support enterprise‑grade security, resilience, and 24/7 availability expectations
- Allow new services (e.g., chatbot, reporting) to be added without re‑architecting the platform
- Align with industry practice for large‑scale systems

Therefore, an architectural style was required that goes beyond a single‑system deployment model while still supporting clear API‑based integration between components.

## Considered Options

1. **Microservices Architecture**
2. **Service‑Oriented Architecture (SOA)**
3. **Monolithic Modular Architecture**
4. **Layered / N‑Tier Architecture (Single Deployable)**

## Chosen Architecture: Microservices Architecture

After reviewing the available options, **Microservices Architecture** was selected for CMS.

CMS is implemented using **four independently hosted backend services**:

- Complaint Service  
- Authentication Service  
- Tenant Service  
- Audit Service  

These are fronted by an **API Gateway (Ocelot)** and each service has **its own database and API**, communicating via HTTP clients. This means services can be developed, deployed, scaled, and maintained independently — which strongly aligns with microservices architecture.

The system also follows the **Client–Server model**, where frontend clients communicate only through the API Gateway rather than directly accessing backend services.

## Consequences of the Decision

### Positive Effects

- **Independent Scalability**  
  Each service can scale based on demand (e.g., Complaint Service can scale faster than Tenant Service).

- **Supports Growth Requirements**  
  The architecture supports projected **10%+ year‑on‑year user growth** without needing to redesign the system.

- **Service Isolation & Fault Tolerance**  
  Failure in one service (e.g., Audit Service) does not bring down the entire CMS.

- **Independent Deployment & Updates**  
  Features or fixes can be released without taking the whole system offline.

- **Technology Flexibility**  
  Each service *could* adopt different technology stacks in future if required.

- **Clear Bounded Contexts**  
  Tenant data, audit logs, authentication, and complaints are logically separated.

### Negative Effects

- **Increased Operational Complexity**  
  Monitoring, logging, and deployment require greater DevOps maturity.

- **Distributed Data Challenges**  
  Ensuring data consistency across services is more complex than in a monolith.

- **More Infrastructure Required**  
  Multiple runtimes, APIs, and databases introduce overhead.

- **More Complex Testing**  
  Integration and contract testing become essential.

## Pros of Microservices Architecture

- Scales services independently  
- Reduces impact of service‑level failures  
- Enables zero‑downtime deployments  
- Supports long‑term system evolution  
- Well aligned to enterprise architectures  
- Teams can work in parallel across services  
- Secure separation between service responsibilities  

## Cons of Microservices Architecture

- Requires strong DevOps and monitoring capability  
- More complex to design, test, and operate  
- Requires careful API design and versioning  
- Distributed transactions are harder to manage  

## Confirmations

This architecture style was reviewed against:

- **Enterprise usage scenarios**
- **Security & multi‑tenant isolation requirements**
- **Expected user growth (10% per year minimum)**
- **Need for independent system evolution**
- **Alignment with modern cloud‑ready system design**

Microservices was determined to best meet these requirements.

---

## Pros and Cons of Other Considered Options

### 1. Service‑Oriented Architecture (SOA)

**Pros**
- Supports loosely‑coupled services
- Encourages reuse of shared services

**Cons**
- Typically relies on central orchestration layers  
- Can introduce latency and governance overhead

### 2. Monolithic Modular Architecture

**Pros**
- Simple to develop and deploy  
- Easier debugging and testing  

**Cons**
- Does not scale well for large user bases  
- Any change requires redeploying the whole system  
- Hard to isolate tenant‑specific security boundaries  

### 3. Layered / N‑Tier Monolith

**Pros**
- Clear separation of UI, Business, Data layers  
- Easier to maintain in early phases  

**Cons**
- Single deployment unit — poor for scaling  
- Does not support independent service evolution  

---

## Conclusion

**Microservices Architecture** was chosen because it provides the scalability, service isolation, and long‑term flexibility required for an enterprise‑grade Complaint Management System.

When combined with the **Client–Server model**, the system ensures:

- Secure access via a single gateway  
- Strong tenant data isolation  
- Independent service scaling and deployment  
- Ability to meet future growth and feature expansion  