# Coffee Spring Campaign Review & Statistical Audit

> **Read this in Arabic:** [اللغة العربية](README_AR.md)

Statistical audit and quantitative evaluation of the "Coffee Spring" marketing campaign (conducted in March–April 2026) to evaluate campaign impact on branch revenue (5 treatment branches vs. 7 control branches) using Difference-in-Differences (DiD) and causal inference statistical methodology.

---

## Table of Contents
- [1. Executive Summary & Key Findings](#1-executive-summary--key-findings)
- [2. Project Directory Structure](#2-project-directory-structure)
- [3. Detailed Directory & File Description](#3-detailed-directory--file-description)
- [4. Methodology & Tech Stack](#4-methodology--tech-stack)
- [5. Requirements & How to Run](#5-requirements--how-to-run)
- [6. Key Deliverables](#6-key-deliverables)

---

## 1. Executive Summary & Key Findings

* **Original Report Claim:** The Marketing Department's original report claimed a **33% sales growth** across the 5 participating treatment branches, recommending full roll-out to the remaining 7 branches along with a proportional budget increase.
* **Methodological Flaws Identified:**
  1. **Baseline Bias:** The original baseline included all 12 months (including the 2 campaign months), artificially depressing the baseline and distorting the growth calculation.
  2. **No Control Group:** The original analysis completely ignored the performance of the 7 non-participating branches.
* **Corrected DiD Audit Results:**
  * After adjusting the baseline to the 10 pre/post non-campaign months, treatment branches actually grew by **42.76%**.
  * However, the control group (7 non-participating branches) grew by **42.21%** over the same period due to shared macro factors (general price reduction in March/April and seasonal demand).
  * Using a **Difference-in-Differences (DiD)** framework, the net incremental effect attributable to the campaign is only **0.55 percentage points** (~0.4% relative gain).
  * This effect is statistically insignificant and lies entirely within the natural measure noise ($\text{SE} = 1.55\%$, distance from baseline is $0.24 \text{ SE}$).
* **Final Recommendation:** **Do NOT roll out the campaign** to the remaining 7 branches based on current data. Instead, design a proper Randomized Controlled Trial (RCT) with duration calibrated to the commercial break-even point.

---

## 2. Project Directory Structure

```plaintext
coffee-spring-campaign-review/
├── README.md               # Main project documentation (English)
├── README.ar.md            # Full project documentation (Arabic)
├── data/
│   └── raw/
│       └── campaign-raw.csv # Raw dataset (144 rows × 8 columns) - [READ-ONLY]
├── source/                  # Original audited report (campaign-report.html)
├── notebooks/
│   └── coffee_spring_campaign_analysis.ipynb # Primary analysis & statistical audit notebook
├── reports/                 # Generated outputs
│   ├── html/               # Interactive HTML reports (Executive Summary, DiD, Integrity, etc.)
│   ├── figures/            # Charts & visual assets (ratio_trend_monthly_chart, branch_revenue_growth_chart)
│   └── final/              # Final delivered outputs (مذكرة_نهائية.pdf)
└── memo/                   # Handcrafted drafts & working documents
    ├── notes.md            # Analyst notes & working process
    └── مذكرة_نهائية.docx     # Executive Memo (.docx)
```

---

## 3. Detailed Directory & File Description

| Directory / File | Description & Purpose |
| :--- | :--- |
| `data/raw/` | Contains the primary raw dataset (`campaign-raw.csv`). Kept strictly read-only to preserve data integrity. |
| `source/` | Houses the original un-audited report (`campaign-report.html`) submitted prior to review. |
| `notebooks/` | Contains the core Jupyter notebook (`coffee_spring_campaign_analysis.ipynb`) executing data cleaning, DiD modeling, and metric calculations. |
| `reports/html/` | Contains automated interactive HTML reports generated per analytical section (Executive Summary, Integrity, DiD, AOV, Orders, Detection, Branches, Audit). |
| `reports/figures/` | Contains extracted high-resolution charts (e.g., `ratio_trend_monthly_chart.png` and `branch_revenue_growth_chart.png`) formatted for memo presentation. |
| `reports/final/` | Contains official delivered outputs in PDF format (`مذكرة_نهائية.pdf`). |
| `memo/` | Contains analyst working drafts (`notes.md`) and the final executive memo in Word format (`مذكرة_نهائية.docx`). |

---

## 4. Methodology & Tech Stack

* **Language & Core Libraries:** `Python 3.x`, `Pandas`, `NumPy`, `Jupyter Notebook`.
* **Statistical Methods:**
  * **Difference-in-Differences (DiD):** Isolating net treatment effect from common trends.
  * **Baseline Correction:** Removing treatment periods from reference averages.
  * **Noise Modeling & Standard Error:** Calculating 2-month average standard error ($1.55\%$) to distinguish signal from noise.
  * **Automated Audit Engine:** Programmatic validation verifying exact matches across 85 metric points.
* **Reporting Output:** Responsive HTML5/CSS3 templates with native RTL (Arabic) rendering support.

---

## 5. Requirements & How to Run

1. **Environment Setup:** Ensure Python 3.8+ is installed along with `pandas`, `numpy`, and `jupyter`.
2. **Data Check:** Verify that `data/raw/campaign-raw.csv` exists and is untouched.
3. **Run Analysis Notebook:**
   ```bash
   jupyter notebook notebooks/coffee_spring_campaign_analysis.ipynb
   ```
4. **Audit Verification:** Running sections 1–10 re-generates all HTML sub-reports and verifies consistency. Section 10 verifies that all 85 reported numbers match the memo exactly:
   `Audit Passed: 85/85 numbers match the memo`

---

## 6. Key Deliverables

1. **Executive Decision Memo:**
   * `memo/مذكرة_نهائية.docx` (Word document)
   * `reports/final/مذكرة_نهائية.pdf` (Delivered PDF version)
2. **Key Visual Charts:**
   * `reports/figures/ratio_trend_monthly_chart.png`
   * `reports/figures/branch_revenue_growth_chart.png`
3. **Interactive Sub-Reports:**
   * `reports/html/analysis_report_0_executive_summary.html` through `analysis_report_10_audit.html`