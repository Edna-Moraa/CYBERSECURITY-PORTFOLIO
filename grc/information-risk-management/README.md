## Information Risk Management — Assessment & Treatment Planning

### Summary

Structured **information security risk assessment** against a consolidated register spanning cyber, operational, insider, vendor, and regulatory domains. **Objectives:** prioritize risk using qualitative scoring and **annualized loss exposure (ALE)**, validate **treatment choices** against current controls, and flag positions where residual risk is still misaligned with appetite.

*Dataset and figures are illustrative; entity references are synthetic—suitable for public portfolio display.*

### Scope

- **Assets in scope**: customer data, financial records, credentials, email infrastructure, cloud-dependent services, enterprise-wide compliance posture.
- **Risk domains**: ransomware, insider threat, phishing, system resilience, data breach, regulatory compliance, third-party dependency.
- **Outputs:** risk register ([Risk_Register.xlsx](Risk_Register.xlsx)) and written assessment ([Risk_Assessment_Report.md](Risk_Assessment_Report.md)).

### Approach

1. **Qualitative assessment** — Risk score = likelihood × impact; mapping to priority tiers (Critical / High / Medium / Low) using an agreed risk matrix and required management response timelines.
2. **Quantitative assessment** — Single Loss Expectancy (SLE), Annual Rate of Occurrence (ARO), and **Annualized Loss Expectancy (ALE = SLE × ARO)** to compare annual financial exposure across risks.
3. **Treatment analysis** — Validated alignment between **current controls** and stated strategies (reduce, transfer, accept, avoid); documented rationale per risk and proposed **enhanced reduction** where baseline controls were insufficient for severity.

### Findings (high level)

- **Priority concentration** at Critical / High included ransomware against customer data, high-impact breach scenarios, and high-likelihood phishing—candidates for executive visibility and accelerated remediation.
- **ALE** showed multiple risks at the same maximum annual-loss band—**likelihood-only prioritization understates** sustained dollar exposure (e.g. insider and repeat-loss cases).
- **Recommendation:** for severe data-breach exposure, **defense in depth** beyond encryption alone—DLP, UEBA, PAM, and awareness—aligned to the full attack lifecycle.

### Artifacts

| Artifact | File |
|----------|------|
| Risk register | [Risk_Register.xlsx](Risk_Register.xlsx) — scores, priority, SLE, ARO, ALE, treatment rationale, alternatives |
| Assessment report | [Risk_Assessment_Report.md](Risk_Assessment_Report.md) — definitions, qualitative and quantitative analysis, prioritization, recommendations |

### What this demonstrates

- Risk register design and consistent scoring  
- When to weight **qualitative priority** vs **ALE** for investment decisions  
- Treatment adequacy review and **defensible alternatives** for leadership  
