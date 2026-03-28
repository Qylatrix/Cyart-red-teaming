# Practical 6 — Risk Assessment Practice

**Tool:** Google Sheets  
**Intern:** Pretam Saha | **Week:** 2

---

## ALE Calculation — Ransomware Scenario

```
Formula:  ALE = SLE × ARO

Given:
  SLE (Single Loss Expectancy)    = $10,000
  ARO (Annual Rate of Occurrence) = 0.2

Calculation:
  ALE = $10,000 × 0.2
  ALE = $2,000 per year
```

**Google Sheets Formula:**
| Cell | Label | Value | Formula |
|------|-------|-------|---------|
| A1 | SLE | $10,000 | - |
| A2 | ARO | 0.2 | - |
| A3 | ALE | $2,000 | =A1*A2 |

**Interpretation:** The organization faces an expected annual loss of $2,000 from ransomware. Any control costing less than $2,000/year is cost-justified.

---

## 5×5 Risk Matrix

| Likelihood \ Impact | Very Low (1) | Low (2) | Medium (3) | High (4) | Very High (5) |
|--------------------|-------------|---------|------------|----------|--------------|
| **Very High (5)** | 5 | 10 | 15 | 20 | **25 CRITICAL** |
| **High (4)** | 4 | 8 | 12 | **16 CRITICAL** | 20 |
| **Medium (3)** | 3 | 6 | 9 | **12 HIGH** ← Ransomware | 15 |
| **Low (2)** | 2 | 4 | 6 | 8 | 10 |
| **Very Low (1)** | 1 | 2 | 3 | 4 | 5 |

**Ransomware Scenario:**
- Likelihood = Medium (3)
- Impact = High (4)
- **Risk Score = 12 → HIGH RISK**
- **Action Required:** Implement controls within 30 days
