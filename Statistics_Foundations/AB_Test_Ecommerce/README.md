# A/B Test — E-Commerce Landing Page Conversion

## Context

An e-commerce website tested whether a **new landing page** (`new_page`) converts visitors
to customers at a higher rate than the existing **old landing page** (`old_page`).
Users were randomly assigned to the control group (old page) or the treatment group (new page).

## Dataset

| File | Description |
|---|---|
| `data/ab_test_raw.csv` | Raw experiment data (~294,478 rows) |

**Column reference:**

| Column | Type | Description |
|---|---|---|
| `id` | int | Unique user identifier |
| `time` | str | Timestamp of visit |
| `con_treat` | str | Group: `control` or `treatment` |
| `page` | str | Page shown: `old_page` or `new_page` |
| `converted` | int | Outcome: `1` = converted, `0` = did not convert |

## Hypotheses

- **H₀:** Conversion rate (treatment) = Conversion rate (control)
- **H₁:** Conversion rate (treatment) > Conversion rate (control)

Significance level: **α = 0.05** (one-tailed test)

## Experiment Design

- **Assignment:** Random — each user was independently assigned to exactly one group
- **Sample sizes:** ~147,200 per group (balanced)
- **Metric:** Binary conversion rate

## Analysis

The full analysis is in [`notebooks/01_ab_test_ecommerce.ipynb`](notebooks/01_ab_test_ecommerce.ipynb).

Steps performed:
1. Load and inspect the dataset
2. Clean mismatched group/page assignments
3. Compute conversion rates per group
4. Check sample balance across groups
5. Run a **two-proportion z-test** (one-tailed, `statsmodels`)
6. Compute **95% confidence intervals** for each group and the difference
7. Interpret results in business terms

## Results

| Group | n | Conversions | Rate |
|---|---|---|---|
| Control | 145,274 | 17,489 | 12.04% |
| Treatment | 145,311 | 17,264 | 11.88% |

- **Z-statistic:** −1.31 | **P-value:** 0.905
- **Difference (treat − control):** −0.158 percentage points
- **95% CI for difference:** [−0.394 pp, +0.078 pp]

## Conclusion

We **fail to reject H₀**. The new landing page does not produce a statistically significant
improvement in conversion rate. The test is well-powered (~147K users per arm); the
null result is reliable. **Recommendation: do not launch the new page.**

## Statistical Assumptions

- Independence of observations (random assignment, no cross-contamination)
- Large-sample normal approximation (both n > 100,000)

## How to Run

1. Place the Kaggle CSV as:

   ```text
   Statistics_Foundations/AB_Test_Ecommerce/data/ab_test_raw.csv
   ```

2. Install dependencies:

   ```bash
   pip install -r Statistics_Foundations/AB_Test_Ecommerce/requirements.txt
   ```

3. Open and run the notebook:

   ```bash
   jupyter notebook Statistics_Foundations/AB_Test_Ecommerce/notebooks/01_ab_test_ecommerce.ipynb
   ```

4. Review the final section for test statistics, confidence intervals, and recommendation.

## Tools & Libraries

- Python, Jupyter Notebook
- pandas, NumPy, matplotlib, seaborn
- statsmodels (`proportions_ztest`, `proportion_confint`) for hypothesis testing

## File Structure

```
Statistics_Foundations/
└── AB_Test_Ecommerce/
    ├── data/
    │   └── ab_test_raw.csv          # Raw experiment data
    ├── notebooks/
    │   └── 01_ab_test_ecommerce.ipynb  # Full analysis notebook
    ├── reports/
    │   └── ab_test_summary.md       # Written summary of results
    ├── requirements.txt             # Python dependencies
    └── README.md                    # This file
```
