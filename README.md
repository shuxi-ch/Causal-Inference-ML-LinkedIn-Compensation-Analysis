# LinkedIn Job Postings: Estimating the Causal Effects of Benefits and Experience on Job Outcomes

### Overview

This project conducts two quasi-experimental studies on a dataset of \~50,000 LinkedIn job postings to estimate how **remote work offerings** and **required experience levels** causally affect **applicant engagement** and **posted compensation**, respectively. Using **directed acyclic graphs (DAGs)** to guide identification strategies, each study applies a tailored causal inference pipeline, including **Propensity Score Matching (PSM)**, **meta-learners (X-learner, R-learner)**, and **causal mediation analysis**.

We estimate **ATEs** and **CATEs**, and visualize results using **SHAP values**, **uplift curves**, and **partial dependence plots**, offering a nuanced view of treatment heterogeneity and causal mechanisms.

---

### **Key Policy Takeaways**

#### **1. Remote Work Policies Expand Talent Reach**

* Remote-friendly postings **causally increase applicant engagement**, especially in mid-level tech and marketing roles.
* Effects are **heterogeneous**—greater impact in jobs from smaller firms and non-central locations.

> **Implication**: Remote flexibility is a low-cost, high-return lever for firms aiming to **broaden their talent pool** without raising salaries.

#### **2. Experience Requirements Signal and Distort Compensation**

* Each year of required experience **increases salary**, but **nonlinearly**—with diminishing returns beyond mid-level.
* Mediation analysis reveals that **title inflation** (e.g., "Senior") accounts for a substantial portion of this effect.

> **Implication**: Firms may use job titles and experience thresholds to **manipulate wage expectations** or signal quality, potentially exacerbating **wage compression**.

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

### Problem Statement

HR leaders and job seekers alike face a central question: which job attributes actually *cause* higher applicant engagement and compensation? Raw comparisons can be misleading due to confounding factors like industry, company size, and geographic location. This project applies modern causal inference techniques to a rich dataset of \~50,000 LinkedIn job postings to estimate the **true average and heterogeneous treatment effects** of:

* **Remote Work Availability** on **application engagement**, and
* **Experience Requirements** on **posted salary**

Our findings help:

* **HR professionals** design competitive compensation packages and attract talent.
* **Job seekers** understand what attributes matter most to pay and visibility.

---

### Data Sources

* **Kaggle Dataset**: [LinkedIn Job Postings (\~50K records after our processing)](https://www.kaggle.com/datasets/arshkon/linkedin-job-postings)
* Structured across 11 CSV files, including job descriptions, company details, perks, salary, experience level, and metadata (\~68 features).

---

### Methodology

This project was implemented across two quasi-experimental studies using Python, `econml`, `causalml`, and `DoWhy`. Each study follows a full pipeline from data cleaning and feature extraction to robust causal inference and interpretability.

###### 1. Data Preparation and Feature Engineering

* Merged and flattened data across multiple files.
* Parsed job descriptions using NLP (spaCy) to extract **years of experience**.
* Created binary and count indicators for **benefits** (e.g., health, dental, remote, 401k).
* Identified **confounders**: industry, location, seniority, company size.
* Imputed missing salaries using group medians by job type and location.
* Log-transformed salary to correct skewness.

###### 2. Causal Graph Specification (DAG)

* Defined causal structure via **Directed Acyclic Graphs (DAGs)** for each study.
* Treatments:

  * Study 1: Remote Work (binary)
  * Study 2: Experience Years (continuous)
* Outcomes:

  * Study 1: Application count
  * Study 2: Posted salary
* Confounders: industry, location, company size, job level, and sector.
* DAGs guided the choice of adjustment variables for valid identification.

###### 3. Propensity Score Modeling and Matching

* Trained logistic regression models for binary treatments.
* Applied **Propensity Score Matching (PSM)** to form balanced samples.
* Checked covariate overlap using histograms and **standardized mean difference (SMD)** plots.

###### 4. Causal Effect Estimation

**Study 1: Remote Work → Applicant Engagement**

* Applied **X-Learner** to estimate Conditional Average Treatment Effects (CATEs).
* Benchmarked with **Inverse Probability Weighting (IPW)**, **Doubly Robust Learner (DR-Learner)**, and **Causal Forests**.

**Study 2: Required Experience → Posted Salary**

* Used **R-Learner** for flexible CATE estimation.
* Performed **Causal Mediation Analysis** to decompose:

  * **Direct Effects** of experience
  * **Indirect Effects** through **job title inflation** (e.g., adding "Senior").

###### 5. Sensitivity and Robustness Checks

* **Rosenbaum Bounds**: tested robustness against hidden bias from unobserved confounders.
* **Placebo Tests**: tested for false positives by reassigning treatments randomly.
* **Threshold Sensitivity**: varied experience cutoffs to test model stability.

###### 6. Interpretability and Visualization

* Used **SHAP values** to explain individual prediction contributions.
* Visualized causal findings via:

  * **Uplift Curves** (showing net lift per segment)
  * **Partial Dependence Plots (PDPs)**
  * **CATE distributions** across industries and job types

---

### Key Learnings & Challenges

* **Back‐door Adjustment**: DAG‐guided confounder selection was essential to avoid omitted‐variable bias.
* **Overlap & Balance**: Poor overlap in some sectors increased variance of IPW; trimming and DR‐Learner improved precision.
* **Method Complementarity**: IPW, DR‐Learner, and Causal Forests yielded consistent ATEs, balancing bias and variance trade‐offs.

