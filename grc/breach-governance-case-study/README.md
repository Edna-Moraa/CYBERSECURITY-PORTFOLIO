# Breach Governance Case Study — Change Healthcare vs eCitizen

**Prepared by:** Edna Maburi  
**Type:** Comparative incident / governance analysis (public-source case study)  
**Focus:** Control failures, regulatory context, and lessons for governance and resilience

> Analysis of publicly reported incidents for portfolio demonstration. Not affiliated with the organizations discussed.

---

## Executive summary

This case study compares two major incidents:

1. **Change Healthcare ransomware (USA, 2024)** — confidentiality and integrity failure at national healthcare-payments scale  
2. **eCitizen disruption (Kenya, 2023)** — availability failure against centralized government digital services  

Both events show how weak controls, limited monitoring, and over-centralized digital dependency create systemic risk. The impact profiles differ: Change Healthcare involved data theft and encryption; eCitizen primarily disrupted service availability without confirmed data theft.

| Dimension | Change Healthcare (2024) | eCitizen (2023) |
|-----------|--------------------------|-----------------|
| Primary impact | Confidentiality + integrity (exfiltration + ransomware) | Availability (DDoS / service disruption) |
| Sector | Healthcare payments / clearinghouse | Government digital services |
| Initial access theme | Stolen credentials on remote access lacking MFA | External volumetric / availability attack |
| Data confirmed stolen | Yes (large-scale PHI exposure reported) | No (availability impact emphasized) |
| Governance lesson | Legacy access gaps, weak segmentation, delayed detection | Concentration risk, resilience and redundancy gaps |

---

## Incident A — Change Healthcare (USA)

### What happened (governance-relevant facts)

- Attackers gained initial access via a remote-access pathway protected inadequately relative to policy expectations (MFA gap on a legacy/acquired system pathway reported publicly).
- Extended dwell time enabled large-scale data staging before ransomware deployment.
- Operational disruption cascaded across healthcare providers dependent on payment/claims infrastructure.
- Regulatory exposure spanned HIPAA/HITECH obligations and broader disclosure expectations for a public-company ecosystem.

### Control and governance failures (analysis)

1. **Identity and remote access governance** — MFA and privileged remote-access standards not consistently enforced across inherited systems  
2. **Segmentation / least privilege** — credential abuse enabled broader access than role requirements should allow  
3. **Detection and response latency** — prolonged undetected activity indicates monitoring, hunting, or alerting gaps  
4. **Backup and recovery governance** — ransomware impact severity rises when backups are reachable or recovery unproven  
5. **Third-party / concentration risk** — healthcare ecosystem dependency converted one compromise into national-scale disruption  

### Governance implications

- Board-level metrics should include MFA coverage on external pathways, mean time to detect, and recovery test results — not only policy existence.
- Acquisition integration must include security control inheritance, not only product integration.
- Critical third parties require resilience and concentration-risk oversight equal to internal systems.

---

## Incident B — eCitizen (Kenya)

### What happened (governance-relevant facts)

- Centralized government services platform faced disruptive attack activity affecting citizen-facing availability.
- Public reporting emphasized service interruption across many digital government services.
- Unlike Change Healthcare, the dominant reported harm was availability, not confirmed mass data exfiltration.

### Control and governance failures (analysis)

1. **Concentration risk** — many citizen services depending on a shared platform increases blast radius of availability events  
2. **Resilience architecture** — DDoS readiness, failover, and surge capacity become first-class governance controls for digital public infrastructure  
3. **Crisis communications and continuity** — citizen trust depends on transparent status handling and alternative service channels  
4. **Security investment prioritization** — availability protections (WAF/DDoS, rate controls, redundancy) must be funded as core national-service controls  

### Governance implications

- Availability is a security and governance outcome, not only an IT operations KPI.
- National digital platforms need explicit risk appetite statements for downtime and cascading citizen impact.
- Tabletop exercises should include sustained availability attacks, not only data-breach scenarios.

---

## Comparative lessons for GRC practitioners

| Lesson | Practical control / governance action |
|--------|----------------------------------------|
| Authn gaps become enterprise events | Enforce MFA on all external remote-access paths; track coverage as a board metric |
| Detection delay multiplies loss | Invest in monitoring, anomaly detection, and tested escalation paths |
| Centralization multiplies blast radius | Architect redundancy and segmentation; map dependency concentration |
| Policy without assurance fails | Test control inheritance after M&A / platform changes |
| Impact type differs; root themes rhyme | Weak identity, weak monitoring, weak resilience planning |

---

## Recommendations (portable)

1. Maintain an authoritative inventory of internet-facing and remote-access systems with MFA enforcement evidence  
2. Define and report MTTD/MTTR and backup restore-test success to leadership  
3. Treat critical third parties and centralized platforms as concentration risks in the enterprise risk register  
4. Run joint incident exercises covering ransomware *and* prolonged availability attacks  
5. Ensure exception management for legacy systems has time-bounded remediation owners  

---

## What this demonstrates

- Structured comparative incident analysis across jurisdictions and impact types
- Ability to separate technical failure modes from governance root causes
- Practical recommendations usable by GRC, security leadership, and risk committees
- Clear writing for mixed technical and non-technical audiences
