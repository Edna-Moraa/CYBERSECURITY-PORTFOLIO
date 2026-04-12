## Vulnerable Bank API — OWASP API Top 10 (2023) Assessment

### Executive summary

This assessment evaluates a banking-style API (local deployment) against the **OWASP API Security Top 10 (2023)**. Coverage includes **API1–API10** plus **CORS** and **path traversal** in upload handling.

**Cross-cutting theme:** weak enforcement at trust boundaries—**object ownership**, **function-level authorization**, **token integrity and session lifecycle**, and **input/schema validation**.

### Scope & environment

- **Perspective tested:** authenticated standard user
- **Environment:** containerized local deployment
- **Tooling:** Postman (OpenAPI-driven cases); Burp Suite (proxy, repeater) for replay and header/path manipulation

### Capabilities demonstrated

- OWASP API Top 10 (2023) mapping with risk-ordered reporting  
- Authorization testing: object-, function-, and property-level  
- JWT/session controls: validation, abuse cases, retest criteria  
- Evidence-backed findings with remediation aligned to common secure-design patterns  

---

## Findings 

### API1 — Broken Object Level Authorization (BOLA)

**Assessment focus**

- Whether the API enforces ownership checks when requesting account/transaction objects.

**Result**

- As an authenticated user, I changed an account identifier in the request path to another user’s identifier and the API returned that user’s data.

**Risk / impact**

- Cross-account data exposure (privacy breach, financial exposure, identity theft risk).

**Recommended remediation**

- Enforce **object-level authorization** on every request:
  - derive caller identity from the access token
  - verify requested objects belong to the caller (deny by default)
  - avoid relying on client-supplied identifiers for authorization decisions

**Validation / retest**

- Verify that requesting another user’s object returns **403** (or equivalent) with no sensitive data.

**Evidence**

- [BOLA screenshot](evidence/api1-bola/BOLA.png)

---

### API2 — Broken Authentication

**Assessment focus**

- Whether logout invalidates active sessions/tokens and whether JWT integrity is enforced.

**Result**

- A previously issued token remained usable after logout.
- A tampered token payload (privilege escalation attempt) was accepted, enabling access to admin-only functionality.

**Risk / impact**

- Stolen tokens remain useful beyond logout.
- Token tampering acceptance can lead to privilege escalation and full account compromise.

**Recommended remediation**

- Validate JWTs robustly:
  - enforce signature verification (reject `alg=none` / algorithm confusion)
  - validate expiration and relevant claims (issuer/audience if used)
  - use strong secret/key management and rotation
- Implement session invalidation patterns:
  - short-lived access tokens + refresh tokens
  - server-side refresh token revocation; optionally denylist access tokens if immediate logout invalidation is required

**Validation / retest**

- Confirm tokens fail after logout (per design).
- Confirm tampered tokens are rejected due to signature validation.

**Evidence**

- [Weak token](evidence/api2-broken-auth/Weak%20token.png)
- [Reuses jwt after logout](evidence/api2-broken-auth/Reuses%20jwt.png)
- [Modified JWT accepted](evidence/api2-broken-auth/modified%20jwt.png)
- [Admin panel access](evidence/api2-broken-auth/ADMIN%20PANEL.png)

---

### API3 — Broken Object Property Level Authorization (BOPLA)

**Assessment focus**

- Whether sensitive/internal properties can be set by the client during registration or update.

**Result**

- The API accepted sensitive fields that should be server-controlled (e.g., privilege flags / financial fields).

**Risk / impact**

- Privilege escalation and unauthorized manipulation of sensitive fields.

**Recommended remediation**

- Enforce strict **input schema validation** and **allowlists**:
  - reject unexpected properties (fail closed)
  - assign sensitive fields server-side only
- Separate privileged actions into admin-only routes and enforce RBAC/ABAC consistently.

**Validation / retest**

- Attempt to submit sensitive fields and verify they are rejected or ignored and cannot change privilege/financial state.

**Evidence**

- [BOPLA 1](evidence/api3-bopla/BOPLA1.png)
- [BOPLA 2](evidence/api3-bopla/BOPLA2.png)

---

### API4 — Unrestricted Resource Consumption (rate limiting)

**Assessment focus**

- Whether endpoints enforce throttling/quotas to prevent abuse.

**Result (what happened)**

- The server processed repeated requests without throttling (no rate-limit behavior observed, e.g., HTTP 429).

**Risk / impact**

- Brute-force enablement and denial-of-service style abuse.

**Recommended remediation**

- Implement rate limiting/throttling per user and IP plus abuse detection (progressive delays, lockouts, anomaly detection).

**Evidence**

- [API4 rate limits](evidence/api4-rate-limits/API4.png)

---

### API5 — Broken Function Level Authorization (BFLA)

**Assessment focus**

- Whether non-admin users can invoke admin-only functions by directly calling restricted endpoints.

**Result** 

- Admin functionality was accessible without proper privilege checks.

**Risk / impact**

- Unauthorized access to administrative capabilities and data.

**Recommended remediation**

- Enforce authorization checks on every endpoint server-side (RBAC/ABAC), not only in the UI.

**Evidence**

- [API5 BFLA 1](evidence/api5-bfla/API5%20BFLA%201.png)
- [API5 BFLA 2](evidence/api5-bfla/API5%20BFLA2.png)

---

### API6 — Unrestricted Access to Sensitive Business Flows

**Assessment focus**

- Whether sensitive actions enforce workflow rules, frequency limits, or idempotency.

**Result**

- Sensitive actions could be replayed repeatedly without workflow/frequency controls.

**Risk / impact**

- Business logic abuse (fraud, integrity issues, resource consumption).

**Recommended remediation**

- Add workflow validation, idempotency controls, and action limits (per account/session/time window).

**Evidence**

- [API6 business flow](evidence/api6-business-flow/API6%20Unrestricted%20access%20to%20sensitive%20business%20flows.png)

---

### API7 — Server-Side Request Forgery (SSRF)

**Assessment focus**

- Whether user-supplied URLs are validated and constrained.

**Result (what happened)**

- The server attempted to process internal/loopback URLs supplied via a URL-based parameter.

**Risk / impact**

- Internal service access, metadata exposure, network pivoting.

**Recommended remediation**

- Strict URL allowlisting, block private IP ranges, restrict outbound egress, enforce timeouts, and add DNS protections.

**Evidence**

- [API7 SSRF](evidence/api7-ssrf/API7%20Server%20side%20Request%20Forgery.png)

---

### API8 — Security Misconfiguration 

**Assessment focus**

- Whether file upload handling sanitizes filenames/paths.

**Result** 

- Upload handling accepted traversal sequences in a filename and processed the request successfully.

**Risk / impact**

- Potential file write outside intended directory, overwrite risk, exploitation chaining.

**Recommended remediation**

- Strip traversal characters, generate server-side filenames, validate type/MIME, store outside web root, harden filesystem permissions.

**Evidence**

- [Path traversal](evidence/api8-security-misconfiguration/path%20traversal.png)

---

### API9 — Improper Inventory Management 

**Assessment focus**

- Whether older API versions remain accessible and weaker than current versions.

**Result** 

- A legacy version of a reset flow exposed weaker behavior compared to a newer version.

**Risk / impact**

- Attackers bypass newer security controls by targeting legacy endpoints.

**Recommended remediation**

- Retire deprecated versions, maintain an accurate API inventory, and monitor for deprecated route access.

**Evidence**

- [API9 v1](evidence/api9-inventory-management/API9%20improper%20inventory%20management.png)
- [API9 v3](evidence/api9-inventory-management/api9%20v3.png)

---

### API10 — Unsafe Consumption of APIs

**Assessment focus**

- Whether the application safely processes untrusted external/user-controlled input.

**Result** 

- The application attempted to process untrusted input without sufficient validation.

**Risk / impact**

- SSRF, data manipulation, and downstream trust issues.

**Recommended remediation**

- Validate and sanitize inputs/responses, enforce schemas, and apply trust boundaries consistently.

**Evidence**

- [API10 unsafe consumption](evidence/api10-unsafe-consumption/API10%20unsafe%20consumption%20of%20api.png)

---

## Additional finding: CORS misconfiguration

**Assessment focus**

- Whether CORS policies are overly permissive for authenticated endpoints.

**Result** 

- CORS behavior appeared permissive (wildcard and/or origin reflection patterns).

**Risk / impact**

- Enables malicious websites to interact with authenticated APIs (depending on credentials/cookie behavior), increasing risk of data exposure or unauthorized actions.

**Recommended remediation**

- Use an explicit allowlist of trusted origins, avoid reflecting arbitrary origins, and handle credentials settings safely.

**Evidence**

- [CORS misconfiguration](evidence/cors/CORS%20miscon.png)
- [Cross-origin attack](evidence/cors/cross%20origin%20attack.png)

