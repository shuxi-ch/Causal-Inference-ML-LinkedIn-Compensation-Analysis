# LinkedIn Compensation Causal Analysis

### Overview

This project applies structural causal inference and machine‐learning estimators to \~50 000 LinkedIn job postings to quantify how benefits offerings and experience requirements causally affect posted compensation. Guided by a DAG for back–door adjustment, we estimate average and heterogeneous treatment effects using IPW, doubly‐robust learners, and Causal Forests—revealing that each extra benefit adds \$3 000 and each additional year of experience adds \$1 200 to salary.

---

### Directory Layout

```bash
.
├── README.md
├── .gitignore
├── data
│   └── raw
│       ├── benefits.csv                 
│       ├── companies.csv
│       ├── company_industries.csv                 
│       ├── employee_counts.csv
│       ├── industries.csv                 
│       ├── job_industries.csv
│       ├── job_skills.csv                 
│       ├── postings.csv
│       ├── salaries.csv
│       └── skills.csv                  
└── notebooks               
    ├── DataPreperation.ipynb                  
    └── Causal ML_LinkedIn Compensation Causal Analysis.ipynb                            
                            
```

---

### Problem Statement

HR leaders need rigorous, data‐driven guidance on which job benefits to offer and how experience requirements translate into pay premiums. Raw comparisons conflate confounding factors—industry, company size, location—making it impossible to isolate causal effects. This project estimates the true average and heterogeneous treatment effects of benefits count and experience years on posted compensation, informing benefit design and salary benchmarking.

---

### Data Sources

* **Kaggle “LinkedIn Job Postings”** (≈50 000 records): JSON/CSV with job titles, company info, listed benefits, required experience, location, salary ranges, and more.
* **Derived Features**:

  * **Benefits Count** (number of listed perks: health, remote, 401k, etc.)
  * **Years of Experience** (parsed from job description)
  * **Confounders**: industry sector, company size category, geographic region

---

### Methodology / Approach

1. **Data Preparation** (`notebooks/01_data_preparation.ipynb` / `src/data_preparation.py`)

   * Flattened nested JSON into tabular form; imputed missing salaries; log‐transformed skewed features; encoded categorical variables; extracted benefits and experience indicators.
2. **DAG Specification** (`02_causal_ml.ipynb`)

   * Mapped treatments (benefits count, experience years), outcome (salary), and confounders (industry, company size, location) in a Directed Acyclic Graph to justify back-door adjustment.
3. **Propensity-Score Modeling** (`src/causal_analysis.py`)

   * Fitted logistic regressions for each treatment on confounders; checked overlap and covariate balance pre‐ and post‐weighting.
4. **Causal Estimation**

   * **IPW**: Inverse‐probability weighting to compute ATEs.
   * **Doubly‐Robust Learner**: Combined propensity scores with outcome models via EconML’s DR‐Learner.
   * **Causal Forests**: Estimated heterogeneous treatment effects across industries and locations.
5. **Sensitivity Analysis**

   * Rosenbaum bounds and placebo‐treatment tests to assess robustness to unobserved confounding.

---

### Key Learnings & Challenges

* **Back‐door Adjustment**: DAG‐guided confounder selection was essential to avoid omitted‐variable bias.
* **Overlap & Balance**: Poor overlap in some sectors increased variance of IPW; trimming and DR‐Learner improved precision.
* **Method Complementarity**: IPW, DR‐Learner, and Causal Forests yielded consistent ATEs, balancing bias and variance trade‐offs.
* **Reproducibility**: Capturing package versions via `sessionInfo()` and `requirements.txt` enabled teammates to reproduce results exactly.

---

### Next Steps / Future Enhancements

* Extend causal questions to hiring speed and applicant volume metrics.
* Integrate synthetic controls for evolving markets.
