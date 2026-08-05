# Artifact: Sample Security RACI

**Context:** Illustrative mid-size organization establishing clear ownership for security activities.  
**Legend:** R = Responsible · A = Accountable · C = Consulted · I = Informed

| Activity | Board | CEO | CISO | SecOps | IT Ops | Business Units |
|----------|-------|-----|------|--------|--------|----------------|
| Approve cyber risk appetite | A | R | C | I | I | I |
| Own information security strategy | I | A | R | C | C | C |
| Operate security monitoring / SOC triage | I | I | A | R | C | I |
| Patch / harden production systems | I | I | A | C | R | I |
| Approve exceptions to security standards | I | C | A | C | R | C |
| Report material cyber incidents | A | R | R | C | C | I |
| Maintain access reviews (Joiner/Mover/Leaver) | I | I | A | C | R | R |
| Vendor security due diligence | I | I | A | C | C | R |
| Security awareness program | I | A | R | C | I | R |
| Audit / assurance response | I | A | R | C | C | C |

## Design notes

- **One Accountable owner per activity** — avoids diffusion of responsibility
- CISO accountable for control design/assurance; IT Ops often responsible for implementation
- Board accountable for risk appetite and oversight of material incidents
- Business units remain responsible where process ownership sits with them (access attestations, vendor context)
