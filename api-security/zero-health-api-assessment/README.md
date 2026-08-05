# Zero-Health API — Security Assessment

**Prepared by:** Edna Maburi  
**Target:** Zero-Health API (deliberately vulnerable lab application)  
**Type:** Focused API penetration test  
**Audience:** Security / engineering / risk stakeholders

> Lab assessment for portfolio demonstration. Not a production client engagement.

---

## Executive summary

A focused security assessment of the Zero-Health API identified **high-severity authorization and input-validation weaknesses**. The most material finding was **Broken Object Level Authorization (BOLA)**, which allowed an authenticated user to access another user’s **lab results** by modifying object identifiers.

A secondary **injection / unsafe input handling** issue showed insufficient server-side validation of JSON request bodies.

| Finding | OWASP mapping | Severity |
|---------|---------------|----------|
| Broken Object Level Authorization (BOLA) | API1:2023 | High |
| Injection / unsafe input handling | API8 / injection class | High |
| Excessive data exposure indicators | API3-related | Medium |
| Insufficient authorization controls (cross-cutting) | API1 / API5 | High |

**Business impact:** unauthorized access to sensitive health records, horizontal privilege escalation, and elevated privacy / regulatory exposure if similar flaws existed in a production health API.

---

## Scope and constraints

**In scope**
- Zero-Health API endpoints reachable as an authenticated standard user
- Authorization testing at object level
- Input validation / injection probing
- Reconnaissance (passive + active)

**Out of scope**
- Denial-of-service / destructive testing
- Social engineering
- Production systems

**Tools**
- Burp Suite (Proxy, Repeater)
- Postman
- Browser-based testing
- Public source/documentation review

---

## Methodology

1. **Passive reconnaissance** — locate API documentation and public repository context  
2. **Active reconnaissance** — intercept authenticated traffic; note auth model and identifier patterns  
3. **Authorization testing** — BOLA via object-ID manipulation  
4. **Input validation testing** — malformed / unexpected JSON payloads  
5. **Reporting** — severity, impact, remediation, and validation criteria  

**Key reconnaissance observations**
- Bearer token authentication in use
- Object identifiers predictable / numeric
- Authorization appeared to stop at authentication in tested flows

---

## Findings

### 1. Broken Object Level Authorization (BOLA) — High

**OWASP:** API1:2023 — Broken Object Level Authorization

**What was tested**  
Whether the API verifies that the authenticated caller is authorized to access a specific object (lab results).

**Result**  
After authenticating as a standard user and capturing a legitimate request to `GET /api/lab-results/`, the object identifier was modified and replayed in Burp Repeater. The API returned **HTTP 200** with **another user’s lab results**.

**Impact**
- Unauthorized access to sensitive health data
- Horizontal privilege escalation
- Potential privacy / regulatory consequences in a real healthcare context

**Remediation**
- Enforce **server-side object ownership checks** on every object request
- Derive caller identity from the validated access token
- Deny by default when ownership/authorization cannot be proven
- Do not rely on client-side controls for authorization

**Retest**  
Requesting another user’s object ID must return **403** (or equivalent) with no sensitive payload.

---

### 2. Injection / unsafe input handling — High

**What was tested**  
Whether user-controlled JSON inputs are validated and safely handled.

**Result**  
Unexpected and malformed inputs were accepted and processed. Backend behavior changed without clear rejection, indicating weak validation/sanitization.

**Impact**
- Unauthorized data manipulation risk
- Potential logic bypass pathways
- Expanded attack surface for chained exploits

**Remediation**
- Strict schema validation (allowlist properties and types)
- Parameterized queries / safe ORM usage
- Reject unexpected fields and malformed payloads (fail closed)
- Centralize input validation in API middleware

**Retest**  
Malformed and out-of-schema payloads are rejected consistently; no behavioral anomaly or data mutation occurs.

---

## Recommendations (priority)

1. Implement object ownership checks on all object-access endpoints  
2. Enforce authorization server-side (never UI-only)  
3. Validate and sanitize all inputs with strict schemas  
4. Add monitoring/alerting for cross-object access anomalies  
5. Retest against OWASP API Security Top 10 on a fixed cadence  

---

## What this demonstrates

- Focused API pentest methodology (recon → authz → input → report)
- Clear severity ranking and business-impact framing for sensitive data APIs
- Practical use of Burp Suite / Postman for authorization abuse cases
- Remediation language engineers can implement and retest
