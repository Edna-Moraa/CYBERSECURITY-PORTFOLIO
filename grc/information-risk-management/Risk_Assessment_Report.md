# Risk Assessment Report — Information Security Risk Management

**Prepared by:** Edna Maburi  
**Purpose:** Qualitative and quantitative risk prioritization, annual loss exposure, and treatment planning against an illustrative enterprise risk register.

*Figures and organizational context below are synthetic and used to demonstrate assessment methodology.*

---

## 1. Executive summary

This assessment evaluates seven registered risks spanning cyber attack, insider threat, social engineering, technical failure, regulatory exposure, and vendor dependency. Analysis combined **qualitative scoring** (likelihood × impact → priority tier) with **quantitative annual loss exposure (ALE)** to inform investment and treatment decisions.

**Key conclusions**

- **Highest qualitative priority:** ransomware against the customer database (Critical tier).
- **Shared maximum ALE:** three risks tie at **$1,000,000** annualized expected loss—demonstrating that qualitative rank alone does not replace financial exposure when budgeting controls.
- **Treatment gap:** one High-severity data-breach scenario relied on encryption as the primary control; a **defense-in-depth** enhancement was recommended (see Section 6).

Supporting detail: [Risk_Register.xlsx](Risk_Register.xlsx).

---

## 2. Definitions

| Term | Definition |
|------|------------|
| **SLE** (Single Loss Expectancy) | Estimated financial loss from a single occurrence of the risk event. |
| **ARO** (Annual Rate of Occurrence) | Estimated frequency of the risk event per year (e.g. 0.2 = once every five years on average). |
| **ALE** (Annualized Loss Expectancy) | Expected monetary loss per year: **ALE = SLE × ARO**. |

**Qualitative risk score** = Likelihood (1–5) × Impact (1–5), mapped to management priority bands (Critical / High / Medium / Low) per the agreed risk matrix.

---

## 3. Qualitative assessment and prioritization

### 3.1 Risk register summary

| Risk ID | Description | Asset | Score | Priority |
|---------|-------------|-------|-------|----------|
| R001 | Ransomware attack | Customer database | 12 | Critical |
| R002 | Insider threat | Financial records | 8 | High |
| R003 | Phishing attack | User credentials | 8 | High |
| R004 | System failure | Email system | 9 | High |
| R005 | Data breach | Customer data | 10 | High |
| R006 | Compliance violation | All systems | 6 | Medium |
| R007 | Third-party failure | Cloud services | 6 | Medium |

### 3.2 Interpretation

- **Likelihood** captures how often the threat may materialize given current exposure; **impact** captures consequence to the organization if it does. The product balances **frequency** and **severity** so high-impact, lower-frequency risks (e.g. large-scale data breach) still surface appropriately.
- **R001 (Critical)** warrants immediate executive visibility: customer-facing data and operational continuity are at stake, with controls (backup, antivirus) reducing but not eliminating exploit paths via unpatched systems.
- **R005 (High)** is driven primarily by **impact** (sensitive customer data, regulatory and reputational exposure) even where likelihood is moderate.
- **R003 (High)** reflects **high likelihood** of phishing and its role as a precursor to credential abuse and downstream incidents.

---

## 4. Quantitative assessment (ALE)

### 4.1 Results

| Risk ID | SLE | ARO | ALE |
|---------|-----|-----|-----|
| R001 | $5,000,000 | 0.2 | **$1,000,000** |
| R002 | $2,000,000 | 0.5 | **$1,000,000** |
| R003 | $500,000 | 1.0 | $500,000 |
| R004 | $1,000,000 | 0.25 | $250,000 |
| R005 | $10,000,000 | 0.1 | **$1,000,000** |
| R006 | $500,000 | 0.5 | $250,000 |
| R007 | $1,000,000 | 0.2 | $200,000 |

**Combined illustrative ALE (all risks):** **$4.2M** per year (sum of row ALEs). This supports portfolio-level security budgeting and cost-benefit discussion for control investments.

### 4.2 Insight

R001, R002, and R005 share the **top ALE band** despite different qualitative stories: ransomware is Critical on the matrix, while insider and data breach compete for the same annual dollar exposure. Security investment should align with **both** priority tier and **ALE**, not either alone.

---

## 5. Treatment strategy evaluation

Current strategies were reviewed against **existing controls** and **risk severity**.

- **Reduce** is appropriate where technical or procedural controls directly address the stated vulnerability (e.g. access controls for insider risk; redundancy for single points of failure).
- **Transfer** (R007) aligns with vendor SLAs and contracts where operational failure is partly outside direct control.
- **Gap:** several High-priority risks depend on **single-layer** or baseline controls; sufficiency of those controls relative to impact and ALE must be challenged, not assumed from strategy labels alone.

---

## 6. Recommendation — R005 (data breach)

**Current posture:** Reduce, with **encryption** as a primary control.

**Assessment:** Encryption addresses data at rest; modern breach paths often include **compromised credentials, application flaws, and insider misuse**—vectors encryption alone does not remove.

**Recommended enhancement:** Strengthen **Reduce** with **defense in depth**, for example:

- Data loss prevention (DLP)  
- User and entity behavior analytics (UEBA)  
- Privileged access management (PAM)  
- Security awareness (aligned to phishing and credential abuse)

This better aligns residual risk with regulatory expectations and with the **SLE/ALE** profile of the scenario.

*(Detailed rationale per risk is documented in the Treatment Rationale column of the register.)*

---

## 7. Artifacts

| Artifact | Location |
|-------------|----------|
| Full risk register (formulas, ARO notes, treatment rationale) | [Risk_Register.xlsx](Risk_Register.xlsx) |
| This assessment narrative | `Risk_Assessment_Report.md` |
