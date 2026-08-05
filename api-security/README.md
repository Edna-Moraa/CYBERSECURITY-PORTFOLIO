# API Security

Hands-on API security assessments focused on **authentication**, **authorization**, **input validation**, and **business-logic abuse**, mapped to the **OWASP API Security Top 10 (2023)**.

Each case study includes scope, methodology, severity-ranked findings, remediation, and (where available) evidence.

## Case studies

| Project | Focus | Key outcomes |
|--------|--------|--------------|
| **[Vulnerable Bank API](vulnerable-bank-owasp-api-top10/)** | Full OWASP API Top 10 (API1–API10) + CORS / upload issues | End-to-end assessment with evidence-linked findings and retest criteria |
| **[Zero-Health API](zero-health-api-assessment/)** | Health-data API authorization and injection | High-severity BOLA enabling cross-user lab-result access; input validation gaps |
| **[vAPI Assessment](vapi-api-assessment/)** | JWT auth, BOLA, credential stuffing exposure | Structured endpoint review on a Dockerized API lab with authz and token-handling weaknesses |

## Capabilities demonstrated

- OWASP API Security Top 10 mapping
- Object-, function-, and property-level authorization testing
- JWT / session control review
- Evidence-backed reporting with remediation and retest guidance
- Risk framing for technical and non-technical audiences

## Tools

Burp Suite (Proxy, Repeater) · Postman · Browser testing · Source/route review · Docker lab environments
