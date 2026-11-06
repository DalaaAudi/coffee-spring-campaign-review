# Coffee Spring Campaign Review & Statistical Audit

> 🌐 **Read this in Arabic:** [العربية](README_AR.md)

> [!NOTE]
> **Reviewer note:** the reviewer note submitted with this project points to rows 4 and 9 of Table 5-1 (the earlier draft ordering). In the final memo the same two findings are **rows 1 and 2**: no control group, and the 30–35% projection for the seven branches.

A statistical audit of the marketing department's report on the "Coffee Spring" campaign (March–April 2026). The questions: did the campaign raise revenue in the 5 participating branches, and does the data support rolling it out to the other 7 and raising the marketing budget? The audit compares the 5 branches that received the campaign with the 7 that did not, using a Difference-in-Differences (DiD) design. It is an observational (non-randomized) comparison; see [Limitations](#7-limitations).

---

## Table of Contents
- [1. Executive Summary & Key Findings](#1-executive-summary--key-findings)
- [2. Start Here: Reviewer Guide](#2-start-here-reviewer-guide)
- [3. Project Directory Structure](#3-project-directory-structure)
- [4. Directory & File Description](#4-directory--file-description)
- [5. Data](#5-data)
- [6. Methodology](#6-methodology)
- [7. Limitations](#7-limitations)
- [8. Requirements & How to Run](#8-requirements--how-to-run)
- [9. Key Deliverables](#9-key-deliverables)

---

## 1. Executive Summary & Key Findings

**Original claim.** Revenue in the 5 participating branches rose 33% (against a 12-month average baseline). The report attributed this to the campaign and recommended rolling it out to the other 7 branches, expecting a 30–35% effect, and raising the marketing budget.

**Recommendation.** Do not roll out the campaign or raise the budget on the basis of this report. Test it in a randomized trial on comparable branches, with the duration set by the commercial break-even point. This is not a claim that the campaign has no effect: with two campaign months, only an effect above roughly 3% (about 4% at 80% power) is detectable.

**Why.**
1. **The baseline included the campaign months.** This raised the baseline and understated growth: 33.26% with the report's method, 42.76% on the corrected 10-month baseline.
2. **No control group.** The 7 branches that did not receive the campaign grew 42.21% in the same months. Applying the report's own method to them gives +32.86%, essentially the same as the +33.26% of the 5 branches.
3. **Evidence misread.** All 12 branches moved together, so the 5 branches rising together is not evidence of a campaign effect. The claims about "bigger" and "more" orders were not tested against the normal month-to-month variation.
4. **A confounder was not mentioned.** A general price cut applied to all 12 branches in March and April.

The report's arithmetic is correct. The flaws are in the design of the comparison and in its interpretation.

### Key results

| Measure | 5 campaign branches | 7 control branches | Difference |
| :--- | ---: | ---: | :--- |
| Revenue growth, report's method (12-month baseline) | +33.26% | +32.86% | +0.40 pp |
| Revenue growth, corrected baseline (10 months) | +42.76% | +42.21% | **+0.55 pp** (about 0.4% relative) |
| Average order value growth | +3.07% | +0.75% | +2.32 pp (0.95 SE, within noise) |
| Orders growth | +38.51% | +41.15% | −2.64 pp (−0.64 SE, within noise) |

pp = percentage points. SE = standard error of a two-month average; for revenue, the campaign months sit 0.24 SE from the baseline (SE = 1.55%). Percentage-point differences are not additive, because revenue = orders × average order value.

---

## 2. Start Here: Reviewer Guide

1. **`memo/مذكرة_نهائية.docx`, section 1:** the decision, the recommendation, and what is not known.
2. **Table 5-1 in the memo:** 11 defects of the original report, ranked by their weight in the roll-out decision. **Rows 1 and 2 are the decisive ones** (no control group; the 30–35% projection). Rows 10–11 are calculation errors that do not change the conclusion.
3. **Notebook, section 3:** the report's own method applied to both groups (+33.26% vs +32.86%).
4. **Notebook, section 10:** automated audit that every number in the memo matches the computed value (`audit passed: 85/85`).
5. **`reports/html/analysis_report_0_executive_summary.html`:** one-page visual summary.

---

## 3. Project Directory Structure

```plaintext
coffee-spring-campaign-review/
├── README.md                      # Project documentation (English)
├── README_AR.md                   # Project documentation (Arabic)
├── data/
│   └── raw/
│       └── campaign-raw.csv       # Raw dataset, 144 rows x 8 columns (never modified)
├── source/
│   └── أثر حملة «ربيع القهوة» على المبيعات — تقرير داخلي.html   # Original report under review
├── notebooks/
│   └── coffee_spring_campaign_analysis.ipynb   # Analysis, report generation, audit
├── reports/                       # Everything the code or the export produces
│   ├── html/                      # HTML reports generated by the notebook
│   │   └── analysis_report_*.html # one per notebook section (see section 4)
│   ├── figures/                   # Memo figures (PNG)
│   │   ├── ratio_trend_monthly.png
│   │   └── branch_revenue_growth.png
│   └── final/                     # Delivered PDF of the memo
│       └── مذكرة_نهائية.pdf
└── memo/                          # Written by hand
    ├── notes.md                   # Analyst working notes
    └── مذكرة_نهائية.docx            # Executive memo (Word)
```

---

## 4. Directory & File Description

| Directory / File | Description |
| :--- | :--- |
| `data/raw/` | The raw dataset `campaign-raw.csv`. Treated as read-only: no code writes to it. |
| `source/` | The original report under review, unmodified: `أثر حملة «ربيع القهوة» على المبيعات — تقرير داخلي.html`. |
| `notebooks/` | The analysis notebook: loads the data, runs integrity checks, DiD, the replication of the report's method, noise and detection analysis, per-branch analysis, generates the HTML reports, and audits the memo's numbers. |
| `reports/html/` | HTML reports generated by the notebook (safe to delete; re-created on every run). One per section: `0_executive_summary`, `1_integrity`, `2_did`, `3_report_method`, `4_revenue_ratio`, `5_aov`, `6_orders`, `7_detection`, `8_branches`, `10_audit`. `9_sensitivity` appears only when `RUN_SENSITIVITY = True`. |
| `reports/figures/` | The two memo figures: `ratio_trend_monthly.png` and `branch_revenue_growth.png`. They are exported from the data behind reports 4 and 8 with annotations added, and are **not** regenerated by the notebook. |
| `reports/final/` | The delivered PDF of the memo (`مذكرة_نهائية.pdf`). |
| `memo/` | Hand-written material: `notes.md` and the memo source `مذكرة_نهائية.docx`. |

**Rule of thumb:** what you write goes in `memo/`; what the code produces goes in `reports/`; what comes from outside goes in `data/` and `source/`.

---

## 5. Data

`campaign-raw.csv`: 144 rows = 12 branches × 12 months (September 2025 – August 2026). The campaign ran in March and April 2026.

| Column | Meaning |
| :--- | :--- |
| `الشهر` | Month (`YYYY-MM`) |
| `الفرع` | Branch name |
| `ضمن فروع الحملة` | Branch received the campaign (yes: 5 branches, no: 7 branches) |
| `الحملة فعالة هذا الشهر` | Campaign active for this branch in this month (yes only for the 5 branches in March–April 2026) |
| `خفض سعر عام` | General price cut in this month (yes for all 12 branches in March–April 2026) |
| `الإيراد` | Revenue (SAR) |
| `عدد الطلبات` | Number of orders |
| `متوسط قيمة الطلب` | Average order value (revenue ÷ orders) |

Integrity checks (all pass): no missing values, no duplicate branch–month rows, 12 months for every branch, and revenue equals orders × average order value within a maximum rounding gap of 20.4 SAR.

---

## 6. Methodology

* **Difference-in-Differences.** Growth of the 5 campaign branches minus growth of the 7 control branches over the same months, on monthly group totals. Baseline: the 10 months outside March–April 2026.
* **Replication of the report's method.** The same calculation with the report's 12-month baseline, applied to both groups.
* **Parallel trends.** Before the campaign (September–February) the ratio of the 5 branches' revenue to the 7 branches' stayed between 1.59 and 1.63. This is checked before the campaign only; it cannot be checked during it.
* **Noise and detection.** The monthly revenue ratio has a standard deviation of 2.2% over the 10 baseline months, so the standard error of a two-month average is about 1.55%. An effect needs to exceed about 2 SE (about 3%) to be distinguishable from noise, and about 2.8 SE (about 4%) to be detected with 80% power. A more conservative standard error that accounts for the uncertainty of the baseline mean (about 1.7%) gives about 3.4% and 4.8%; the conclusions do not change.
* **Decomposition.** Revenue = orders × average order value. Orders and average order value moved in opposite directions, so the net revenue effect is close to zero.
* **Experiment design.** The duration of a future trial follows from the break-even point: (campaign cost ÷ gross margin) ÷ revenue of the covered branches during the campaign. Table 6-1 in the memo gives months needed per effect size.
* **Automated audit.** A registry of the memo's numbers (`MEMO`) is compared to the computed values; the run stops if any number differs.
* **Significance tests.** No formal significance tests are used in the analysis. Optional supplementary tests (Welch, permutation, Mann–Whitney) are in section 9 of the notebook, disabled by default.
* **Output.** HTML reports with native right-to-left Arabic rendering and SVG charts.

---

## 7. Limitations

* **Not randomized.** The 5 branches were chosen for their capacity to absorb demand, so they may differ from the other 7 in ways the data does not show.
* **Two campaign months.** Only an effect above about 3% is detectable. Absence of evidence is not evidence of absence.
* **Price cut and season are confounded.** The general price cut and spring seasonality coincide with the campaign in all 12 branches. The control group absorbs what is common to both groups, but the estimate is for the campaign together with these conditions, and the data cannot say whether the campaign's effect depends on the price cut.
* **Parallel trends** are verified before the campaign only.
* **The 7 control branches never received the campaign**, so there is no data on how they would respond to it.
* **Long-term effects** (such as loyalty) are outside the window: two campaign months and four months after.
* **Figures** in `reports/figures/` were exported outside the notebook (see above).

---

## 8. Requirements & How to Run

**Requirements:** Python 3.9 or later; `pandas`, `numpy`, and Jupyter. `scipy` is needed only for the optional section 9.

```bash
pip install pandas numpy jupyterlab
# optional, only for RUN_SENSITIVITY = True:
pip install scipy
```

**Run (from the repository root or from `notebooks/`):**

```bash
jupyter lab notebooks/coffee_spring_campaign_analysis.ipynb
# then: Kernel > Restart & Run All
```

Or without the interface:

```bash
jupyter nbconvert --to notebook --execute --inplace notebooks/coffee_spring_campaign_analysis.ipynb
```

The notebook finds the project root by locating `data/raw/`, reads `data/raw/campaign-raw.csv`, and writes the HTML reports to `reports/html/`. A successful run ends with:

```
audit passed: 85/85 numbers match the memo
```

**Optional settings** (first cells of the notebook):

| Setting | Effect |
| :--- | :--- |
| `RUN_SENSITIVITY = True` | Adds section 9 (supplementary tests); requires `scipy`. |
| `COST`, `MARGIN` (section 7) | Provided by marketing and finance; the break-even point is computed only when both are set. |
| Environment variable `CAMPAIGN_CSV` | Reads the data from a different path. |

Verified with Python 3.12, pandas 3.0, and NumPy 2.4.

---

## 9. Key Deliverables

1. **Executive memo**
   * `memo/مذكرة_نهائية.docx` (Word, editable)
   * `reports/final/مذكرة_نهائية.pdf` (delivered PDF)
2. **Memo figures**
   * `reports/figures/ratio_trend_monthly.png`
   * `reports/figures/branch_revenue_growth.png`
3. **HTML reports generated by the notebook**
   * `reports/html/analysis_report_0_executive_summary.html` and the reports for sections 1–8 and 10.
