# vAPI — Security Assessment

**Prepared by:** Edna Maburi  
**Target:** vAPI (deliberately vulnerable API lab)  
**Environment:** Local Docker Compose (`localhost:8000`)  
**Type:** Focused API penetration test (endpoints 1–5)  
**Audience:** Security / engineering stakeholders

> Lab assessment for portfolio demonstration. Not a production client engagement.

---

## Executive summary

This assessment evaluated authentication, authorization, and input-handling controls on vAPI endpoints 1–5. The application uses **JWT-based authentication**, but testing identified weaknesses in **object-level authorization**, **token validation rigor**, and **credential-stuffing exposure** on login flows.

| Finding | Area | Severity |
|---------|------|----------|
| Broken Object Level Authorization (BOLA) | Authorization | High |
| Weak / incomplete JWT validation on some protected flows | Authentication | Medium–High |
| Credential-stuffing exposure / lack of login throttling controls | Authentication | High |
| Input validation gaps (injection class testing) | Input handling | Medium–High |

**Business impact:** unauthorized access to user resources, account takeover risk via stuffing/replay patterns, and integrity risk where authorization checks are incomplete.

---

## Scope and environment

**In scope**
- vAPI application endpoints 1–5
- Authentication endpoint (`/api/login` and related login routes)
- Authorization controls on protected user/resource endpoints
- Local Docker lab only

**Environment deployed**
- vAPI web application — port 8000
- phpMyAdmin — port 8001
- MySQL — port 3306

**Out of scope**
- Production systems
- Destructive denial-of-service testing beyond control observations

---

## Methodology

Aligned to professional API penetration testing practice and OWASP API Security Top 10 thinking:

1. **Reconnaissance** — enumerate routes (Laravel Artisan routing), map methods and patterns  
2. **Authentication testing** — valid/invalid credentials, missing token, manipulated JWT  
3. **Authorization testing (BOLA)** — change user/resource IDs on authenticated requests  
4. **Injection testing** — JSON body manipulation and query-style probes  
5. **Impact validation** — confidentiality / integrity / access-control impact  

**Tools / techniques**
- Docker Compose lab deployment
- Route enumeration via application CLI
- Authenticated request replay and ID manipulation
- Credential-stuffing wordlist exposure review (lab resource directory)

---

## Findings

### 1. Broken Object Level Authorization (BOLA) — High

**What was tested**  
After authenticating as User A, object identifiers in routes such as `/apiX/user/{id}` were modified to reference another user.

**Result**  
Authorization did not consistently enforce ownership across tested object-access patterns. Missing authentication was correctly rejected in some cases (`authHeaderNotFound`), but object-level authorization remained the primary weakness once authenticated.

**Impact**
- Unauthorized access to other users’ data
- Privacy breach potential
- Pathway to unauthorized update/delete attempts on foreign objects

**Remediation**
- Server-side ownership checks on every object operation
- Centralized authorization middleware
- Deny-by-default for cross-object access

**Retest**  
Authenticated User A cannot read/update/delete User B resources; responses return 403 with no sensitive data.

---

### 2. Authentication / JWT validation weaknesses — Medium–High

**What was tested**  
Login behavior, access without tokens, and JWT manipulation/validation edge cases on protected endpoints.

**Result**
- Valid login issued JWT as expected
- Missing token correctly denied on protected routes in baseline checks
- Insufficient validation observed during certain endpoint interactions, creating token-abuse / replay risk if deployed without hardening

**Remediation**
- Enforce signature verification, expiry, and required claims on every protected route
- Short-lived access tokens + refresh-token rotation/revocation
- Consistent auth middleware (no endpoint exceptions)

---

### 3. Credential stuffing exposure / weak anti-automation controls — High

**What was tested**  
Login endpoint resilience and lab resources related to credential stuffing scenarios.

**Result**
- Login responses did not clearly differentiate user-enumeration signals in a safe way under rapid attempts
- Multiple rapid requests were possible without observed blocking/throttling
- Lab packaging included credential-stuffing material, underscoring the intended weakness class for this target

**Impact**
- Elevated account-takeover risk where users reuse passwords
- Fraud / abuse potential on compromised accounts

**Remediation**
- Rate limiting, lockouts, and bot protections on authentication endpoints
- MFA for sensitive accounts
- Monitoring for stuffing patterns (velocity, IP/ASN anomalies)
- Generic authentication error messages without account-oracle behavior

---

## Recommendations (priority)

1. Enforce object-level authorization on all resource endpoints  
2. Validate JWT signature and expiration rigorously on every request  
3. Add rate limiting and anti-automation controls on login  
4. Use parameterized queries and strict input schemas  
5. Log and alert on abnormal cross-object access attempts  

---

## What this demonstrates

- End-to-end lab deployment and attack-surface mapping
- Practical BOLA and authentication testing against a JWT API
- Clear separation of authn vs authz failures
- Remediation priorities suitable for engineering backlogs
