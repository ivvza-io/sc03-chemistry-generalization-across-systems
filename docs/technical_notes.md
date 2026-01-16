# Technical Notes — SC03  
**Chemistry-Only Modeling: Generalization Across Alloy Systems**  
**Audit-Level Methodology, Diagnostics, and Design Decisions**

---

## 1. Scope and Role of This Document

These technical notes document the analytical rationale, methodological decisions, and diagnostic evidence supporting **SC03 — Chemistry-Only Modeling: Generalization Across Alloy Systems**.

They complement:
- the **public README**, which focuses on framework-level conclusions and decision implications,
- and the **notebooks**, which contain full analytical execution and figures.

This document exists to explain **why and how** the chemistry-only modeling framework was evaluated for generalization, not to introduce new modeling techniques.

---

## 2. Analytical Objective

The analytical objective of SC03 is to evaluate whether the **chemistry-only modeling framework** established in SC02 remains valid when applied to **different alloy systems**, even if the optimal functional representation changes.

Specifically, this study aims to determine:
- whether chemistry continues to define a meaningful UTS envelope across systems,
- and which aspects of the chemistry–UTS relationship are **system-dependent**.

### Explicit Non-Objectives
This analysis does **not** aim to:
- derive universal chemistry–UTS equations,
- transfer fixed model coefficients across alloys,
- or optimize accuracy for any single system.

---

## 3. Data Contracts and Grain Definition

### 3.1 Analytical Unit

- **Grain:** one row per heat  
- **Justification:**
  - Chemistry, tensile results, and production records are naturally indexed at heat level.
  - Internal standards are defined at the **alloy and temper level**, while evaluation uses heat-level data.
  - Maintains consistency with SC02 and prevents leakage.

---

### 3.2 Dataset Construction

- **Source:** SQL semantic layer defined in SC01.
- **Systems analyzed:** AA1050-O and AA1100-O (treated jointly as a 1xxx-O family).
- **Inputs:**
  - Chemical composition (Fe, Si).
  - Tensile test results (UTS).
- **Aggregation:**
  - Multiple tensile tests per heat aggregated to heat-level statistics.
- **Exclusions:**
  - None related to chemistry completeness; all included heats have complete chemistry records.

This construction intentionally mirrors SC02 to isolate **system effects** from analytical artifacts.

> *AA1050-O and AA1100-O were grouped to reflect real industrial handling.*

---

### 3.3 Domain of Validity

Supported chemistry domain:
- **Fe:** ~0.07–0.49 %
- **Si:** ~0.01–0.16 %

Supported mechanical response:
- **UTS:** ~55–100 MPa

Standards or conclusions are valid only within this observed domain.

---

## 4. Validation and Leakage Control

### 4.1 Validation Strategy

- **Method:** Group-aware cross-validation (GroupKFold).
- **Grouping variable:** heat identifier.
- **Rationale:**
  - Prevents leakage from repeated measurements within heats.
  - Preserves comparability with SC02 results.

All performance metrics are computed from **out-of-fold predictions**.

---

### 4.2 Leakage Risks Considered

Potential leakage vectors:
- Multiple tensile tests per heat.
- Shared processing context within heats.

Mitigations:
- Heat-level aggregation.
- Group-aware validation.

Residual risks:
- Temporal correlation across heats is not explicitly modeled.

This is accepted as representative of real industrial data conditions.

---

## 5. Modeling Choices and Rationale

### 5.1 Fixed Analytical Pipeline

To test generalization at the **framework level**, the following elements were intentionally held constant relative to SC02:

- Data grain
- Validation strategy
- Feature selection discipline
- Evaluation metrics

This ensures observed differences reflect **metallurgical behavior**, not methodological changes.

---

### 5.2 Model Classes Evaluated

- **Ridge regression**
  - Linear baseline.
- **Polynomial regression (degree 2)**
  - Controlled non-linearity.
- **Random Forest**
  - High-flexibility reference model.

Tree-based models are evaluated diagnostically but are **not intended** as candidates for design tools and internal standards.

---

## 6. Evaluation Metrics and Interpretation

### 6.1 Metrics Used

- **Median MAE (MPa):** central error tendency.
- **P95 absolute error (MPa):** tail risk indicator.

As in SC02, tail metrics are prioritized to assess robustness rather than average fit.

---

### 6.2 Metric Interpretation Across Systems

- Comparable MAE values do not imply equivalent robustness.
- Differences in tail behavior reveal system-dependent sensitivity to chemistry.
- Metric interpretation focuses on **relative behavior**, not absolute thresholds.

---

## 7. Diagnostics and Sensitivity Checks

- Fold-level MAE and P95 values assessed for stability.
- No fold-driven instabilities observed.
- Feature importance rankings are stable across folds.
- Dominant chemistry drivers align with metallurgical expectations for 1xxx alloys.

No additional sensitivity checks were introduced to preserve methodological parity with SC02.

---

## 8. Known Limitations and Failure Modes

- Chemistry captures dominant trends but not all strengthening mechanisms.
- Small composition changes may induce non-linear effects in 1xxx systems.
- Historical chemistry domains constrain applicability.
- Drift in upstream processes may invalidate **model outputs** over time, requiring revalidation.

These limitations reinforce the need for system-aware standards rather than universal formulas.

---

## 9. Decision Implications

This analysis supports the following conclusions:

- Chemistry-only modeling remains a valid **modeling framework** across alloy systems.
- The **functional form** of chemistry–UTS relationships is system-dependent.
- Decision and analysis tools must be adapted per alloy family, even when methodology is reused.

These findings guide:
- variable screening decisions (SC04),
- and the design of uncertainty-aware decision tools (SC05).

---

## 10. Design Decisions Log

Key non-obvious decisions recorded for traceability:

- Reused the SC02 analytical pipeline without modification to isolate system effects.
- Treated AA1050-O and AA1100-O jointly to reflect practical grouping in production.
- Evaluated Random Forest models only as a robustness reference, not as standard candidates.
- Avoided introducing process variables to preserve comparability.

---

## 11. Traceability

- **Notebook:** `sc03_chemistry_generalization_across_systems.ipynb`
- **Data semantics:** SQL semantic layer defined in SC01
- **Commit:** `< pendiente>`

---

### Closing Note

These technical notes document how chemistry-only modeling generalizes as a **methodological framework**, while remaining sensitive to system-specific metallurgical behavior. This distinction underpins disciplined internal standard definition across alloy families.