# A/B Test Summary Report — E-Commerce Landing Page

## Experiment Overview

An e-commerce site ran an A/B test to evaluate whether a redesigned landing page
(`new_page`) increases the user conversion rate compared to the original page (`old_page`).

- **Test period:** recorded in `ab_test_raw.csv`
- **Assignment:** random — each user was assigned to exactly one group
- **Metric:** binary conversion (1 = converted, 0 = did not convert)

---

## Hypotheses

| | Statement |
|---|---|
| **H₀** | Conversion rate (treatment) = Conversion rate (control) |
| **H₁** | Conversion rate (treatment) > Conversion rate (control) |

- **Test type:** one-tailed (right-sided) two-proportion z-test
- **Significance level:** α = 0.05

---

## Data Cleaning

The raw file contains 294,478 rows. After removing 3,893 mismatched rows (users whose
group assignment did not match the page they were shown), the cleaned dataset has
**290,585 rows**.

## Sample Sizes

| Group | Users | Conversions | Conversion Rate |
|---|---|---|---|
| Control (old_page) | 145,274 | 17,489 | **12.04%** |
| Treatment (new_page) | 145,311 | 17,264 | **11.88%** |

Both groups are approximately equal in size, confirming proper random assignment.

---

## Statistical Test

**Method:** Two-proportion z-test (`statsmodels.stats.proportion.proportions_ztest`)

| Statistic | Value |
|---|---|
| Z-statistic | −1.3116 |
| P-value (one-tailed) | 0.9052 |

Since **p-value (0.905) >> α (0.05)**, we **fail to reject H₀**.

---

## Confidence Interval

95% confidence interval for the difference in conversion rates (treatment − control):

| Estimate | Value |
|---|---|
| Point estimate | −0.158 percentage points |
| 95% CI | [−0.394 pp, +0.078 pp] |

The interval **contains zero**, meaning the data are consistent with no true difference.

### Per-group 95% CIs

| Group | Conversion Rate | 95% CI |
|---|---|---|
| Control | 12.04% | [11.87%, 12.21%] |
| Treatment | 11.88% | [11.71%, 12.05%] |

---

## Assumptions

1. **Independence:** Users are independently assigned; no cross-contamination between groups.
2. **Large-sample approximation:** Both groups have n > 100,000 and more than 5 successes/failures, satisfying the normal approximation for the z-test.
3. **Single exposure:** Each user ID appears at most once per group.

---

## Conclusion

> **The new landing page does not produce a statistically significant increase in conversion rate.**
>
> The observed difference is −0.16 percentage points, with a 95% confidence interval of
> [−0.394 pp, +0.078 pp] — well within the margin of noise.  
> With approximately 145,000 users per arm the test is highly powered; the true underlying
> effect is negligible.

**Business recommendation:** Do not launch the new landing page. Invest in further UX
research or a more differentiated variant before running another experiment.
