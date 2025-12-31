# ADR: Deployment Platform Selection

## Context

The Complaint Management System (CMS) is designed using a microservices-based architecture with a React frontend and ASP.NET Core backend services. A suitable deployment platform is required to host the application in a scalable, secure, and globally accessible manner when moving beyond the Proof of Concept (PoC) phase.

## Problem Statement

The deployment platform must:
- Reliably host both frontend and backend services
- Support containerised .NET Core APIs
- Provide strong global performance
- Offer secure transport and environment isolation
- Scale to match projected user demand, including 10% annual growth from large telecom and banking clients

Although the PoC was not deployed to the cloud, a hosting decision was made to demonstrate forward‑planning and architectural suitability.

## Considered Options

1. **Vercel**
2. **Render**
3. **Self‑Hosted / On‑Premise Cloud**
4. **Azure App Service**

## Chosen Deployment Platforms

### **Frontend – Vercel**
Vercel was chosen for frontend deployment due to its seamless React support, automatic builds, integrated CI/CD, zero‑configuration deployment pipeline, and global CDN performance. This ensures fast delivery of UI assets with minimal operational overhead.

### **Backend – Render**
Render was selected for backend deployment as it provides native support for Docker and ASP.NET Core APIs, background workers, managed TLS, and persistent service hosting without complex infrastructure management. It aligns well with independently deployable microservices.

> **Note:** The PoC implementation was not deployed externally, but these platforms were selected as the intended future hosting environments.

## Consequences of the Decision

- Supports independent deployment of each microservice
- Enables global content delivery and low latency for users
- Reduces DevOps overhead compared to self‑managed hosting
- Aligns with scalable, multi‑tenant enterprise requirements
- Provides HTTPS, environment separation, and logging out‑of‑the‑box

## Pros

- **Vercel**
  - Fast global CDN distribution
  - Automatic builds from Git
  - Excellent React compatibility
  - Zero‑config deployment

- **Render**
  - Native .NET and Docker hosting
  - Scales horizontally as demand grows
  - Managed SSL and networking
  - Simple deployment lifecycle and monitoring

## Cons

- Dependent on third‑party platforms
- Vendor pricing increases at enterprise scale
- Advanced networking and security controls are limited compared to Azure
- Multi‑region deployments require upgrade plans

## Confirmations

These deployment platforms were selected because they provide:
- A lightweight DevOps footprint
- Scalable hosting aligned to microservices
- Secure and reliable cloud operation
- Industry‑standard CI/CD workflows

They also align with the case study requirements for:
- High‑availability services
- International service expansion
- Growth of the user base over time

## Alternative Option Summary

### Azure App Service
**Pros**
- Deep enterprise security and networking
- Native identity, logging, and scaling features
- Strong fit for banking/telecom environments

**Cons**
- More complex to manage
- Higher entry‑level cost

### Self‑Hosted / On‑Premise
**Pros**
- Full infrastructure control
- Custom security governance

**Cons**
- Very high operational burden
- Requires DevOps resources and hardware

---

## Conclusion

Vercel and Render were chosen as preferred deployment platforms because they provide modern, scalable, cloud‑native hosting that aligns with a microservices architecture while minimising operational effort. Although the PoC was not deployed externally, these decisions demonstrate how the CMS can evolve into a production‑ready SaaS solution capable of supporting large‑scale banking and telecom clients.