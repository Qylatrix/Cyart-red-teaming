# Theory 4 — Risk Management Advanced Concepts

**Intern:** Pretam Saha | **Week:** 2 | **Reviewer:** CyArt

---

## 1. Quantitative vs Qualitative Risk Assessment

| Type | Method | Output | When to Use |
|------|--------|--------|-------------|
| **Quantitative** | Uses numbers, formulas, financial values | Dollar amounts (ALE) | Board-level reporting, budget decisions |
| **Qualitative** | Uses labels like High/Medium/Low | Risk matrix scores | Quick assessments, limited data |

---

## 2. ALE Calculation (Quantitative)

### Formula
```
ALE = SLE × ARO

SLE = Single Loss Expectancy   (cost of ONE incident)
ARO = Annual Rate of Occurrence (how often it happens per year)
ALE = Annualized Loss Expectancy (expected yearly cost)
```

### Ransomware Scenario Calculation

```
Given:
  SLE = $10,000  (cost of one ransomware attack)
  ARO = 0.2      (happens once every 5 years = 0.2 times/year)

Calculation:
  ALE = $10,000 × 0.2
  ALE = $2,000 per year
```

**Interpretation:** The organization should be willing to spend up to $2,000/year on ransomware prevention controls.

### Google Sheets Formula
```
=B2*B3   where B2=SLE value, B3=ARO value
```

---

## 3. Business Impact Analysis (BIA)

BIA identifies which business functions are critical and how long they can be disrupted.

| Term | Definition | Ransomware Example |
|------|------------|-------------------|
| **RTO** | Recovery Time Objective — max downtime allowed | 4 hours for payment systems |
| **RPO** | Recovery Point Objective — max data loss allowed | Last backup from 1 hour ago |
| **MTD** | Maximum Tolerable Downtime | 24 hours before business collapses |

---

## 4. Risk Matrix (5×5)

Likelihood (rows) vs Impact (columns):

| | Very Low (1) | Low (2) | Medium (3) | High (4) | Very High (5) |
|--|--|--|--|--|--|
| **Very High (5)** | 5-Medium | 10-High | 15-High | 20-Critical | 25-Critical |
| **High (4)** | 4-Low | 8-Medium | 12-High | 16-Critical | 20-Critical |
| **Medium (3)** | 3-Low | 6-Medium | 9-Medium | 12-High | 15-High |
| **Low (2)** | 2-Very Low | 4-Low | 6-Medium | 8-Medium | 10-High |
| **Very Low (1)** | 1-Very Low | 2-Very Low | 3-Low | 4-Low | 5-Medium |

### Ransomware Scenario Score
- **Likelihood:** Medium (3) — happens occasionally
- **Impact:** High (4) — significant financial and operational damage
- **Risk Score:** 3 × 4 = **12 → HIGH RISK**

**Action Required:** Immediate controls needed — offline backups, patch management, user training.

---

## Key Objectives Achieved

- [x] Understood quantitative vs qualitative risk assessment
- [x] Calculated ALE for ransomware scenario (SLE=$10,000, ARO=0.2, ALE=$2,000)
- [x] Created 5×5 risk matrix and scored ransomware scenario
- [x] Studied Business Impact Analysis concepts (RTO, RPO, MTD)
- [x] Used FAIR Institute methodology for risk quantification
