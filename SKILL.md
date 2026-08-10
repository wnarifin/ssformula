---
name: sample-size-calculator
description: >
  Calculates the required sample size for a wide range of research designs,
  including estimation and hypothesis-testing scenarios for means, proportions,
  correlations, reliability coefficients (ICC, Cronbach's alpha, Kappa),
  diagnostic accuracy (sensitivity/specificity, AUROC), logistic regression,
  McNemar's test, repeated measures, animal studies, and structural equation
  modelling (SEM/RMSEA). Activate this skill whenever the user asks about
  sample size determination, power analysis, or dropout adjustment for any of
  these designs.
version: 1.0.0
tools:
  - calc_ss1mean
  - calc_ss1prop
  - calc_ss2mean
  - calc_ss2mean_ratio1
  - calc_hx_ss2mean_paired
  - calc_sd_ss2mean_paired
  - calc_ss2mean_rm
  - calc_ss2prop
  - calc_ss2prop_ratio1
  - calc_hx_ssalpha
  - calc_est_ssalpha
  - calc_ssanimal
  - calc_hx_ssauroc
  - calc_est_ssauroc
  - calc_hx_sscorr
  - calc_est_sscorr
  - calc_hx_ssicc
  - calc_est_ssicc
  - calc_hx_sskappa
  - calc_est_sskappa
  - calc_sslogistic
  - calc_ssmcnemar
  - calc_ssrmsea1
  - calc_sssnsp
  - ncp
  - pncchisq
tags:
  - statistics
  - biostatistics
  - research-methods
  - sample-size
  - power-analysis
  - clinical-research
---

# Skill: Sample Size Calculator

## Overview

This skill enables the agent to determine the required sample size for a broad
range of quantitative research designs encountered in health sciences,
biomedical research, and the social sciences. It is backed by the
`ssformula_annotated.js` library, which consolidates peer-reviewed formulas
for estimation and hypothesis-testing scenarios.

The agent's primary objective is to:

1. Clarify the researcher's study objective.
2. Clarify the outcome and predictor/associated variables.
3. Clarify the measurement scale of all variables involved.
4. Suggest suitable statistical analysis options and let the researcher choose.
5. Select and invoke the correct formula function(s) from `ssformula_annotated.js`.
6. Return the computed sample size together with the dropout-adjusted figure in a
   structured format.

---

## Prerequisites & Context

- **Required Inputs:** The user must supply (or agree to default values for)
  all parameters relevant to their chosen design (see the *Function Reference*
  section below).
- **Required Tools:**

| Tool function | Purpose |
|---|---|
| `calc_ss1mean` | Sample size for estimating a single mean |
| `calc_ss1prop` | Sample size for estimating a single proportion |
| `calc_ss2mean` | Sample size for comparing two independent means (unequal or equal ratio) |
| `calc_ss2mean_ratio1` | As above, assuming a 1:1 group ratio (simplified formula) |
| `calc_hx_ss2mean_paired` | Sample size for a paired t-test (dependent means) |
| `calc_sd_ss2mean_paired` | Helper — derives SD of differences from pre/post SDs and correlation |
| `calc_ss2mean_rm` | Sample size for a repeated-measures design |
| `calc_ss2prop` | Sample size for comparing two independent proportions (unequal or equal ratio) |
| `calc_ss2prop_ratio1` | As above, assuming a 1:1 group ratio (simplified formula) |
| `calc_hx_ssalpha` | Sample size for testing Cronbach's alpha (hypothesis testing) |
| `calc_est_ssalpha` | Sample size for estimating Cronbach's alpha (with precision) |
| `calc_ssanimal` | Sample size for animal studies via the resource-equation (ANOVA) approach |
| `calc_hx_ssauroc` | Sample size for testing AUROC against a null value |
| `calc_est_ssauroc` | Sample size for estimating AUROC with a given precision |
| `calc_hx_sscorr` | Sample size for testing a Pearson correlation coefficient |
| `calc_est_sscorr` | Sample size for estimating a Pearson correlation coefficient |
| `calc_hx_ssicc` | Sample size for testing the ICC (reliability hypothesis testing) |
| `calc_est_ssicc` | Sample size for estimating the ICC with a given precision |
| `calc_hx_sskappa` | Sample size for testing Cohen's Kappa |
| `calc_est_sskappa` | Sample size for estimating Cohen's Kappa with a given precision |
| `calc_sslogistic` | Sample size for logistic regression (EPV rule of thumb) |
| `calc_ssmcnemar` | Sample size for McNemar's test (paired proportions) |
| `calc_ssrmsea1` | Sample size for SEM based on RMSEA |
| `calc_sssnsp` | Sample size for estimating sensitivity and specificity |
| `ncp` | Internal helper — computes the non-centrality parameter for SEM |
| `pncchisq` | Internal helper — computes the non-central chi-square CDF (used by `ncp`) |

---

## Function Reference

Each subsection lists the required inputs, the function call signature, and the
outputs returned. All inputs are **numeric** unless stated otherwise. Parameters
for `ci` (confidence level) must be supplied as a **proportion** (e.g., `0.95`
for 95 %). Parameters for `drop` are supplied as a **percentage** (e.g., `10`
for 10 %).

---

### 1. One-Mean Estimation — `calc_ss1mean`

Estimates the sample size required to estimate a single population mean with
a specified precision (margin of error).

**Formula reference:** Naing (2003); Arifin (2013).

| Parameter | Type | Description |
|---|---|---|
| `sd` | number | Expected standard deviation |
| `precision` | number | Desired margin of error (same units as SD) |
| `ci` | number | Confidence level as a proportion (e.g., `0.95`) |
| `drop` | number | Expected dropout rate (%) |

**Call:** `calc_ss1mean(sd, precision, ci, drop)`

**Returns:** `{ n, n_drop }`
- `n` — minimum sample size
- `n_drop` — sample size adjusted for dropout

---

### 2. One-Proportion Estimation — `calc_ss1prop`

Estimates the sample size required to estimate a single population proportion
with a specified precision.

**Formula reference:** Naing (2003); Arifin (2013).

| Parameter | Type | Description |
|---|---|---|
| `p` | number | Expected proportion (0–1) |
| `precision` | number | Desired margin of error (proportion scale) |
| `ci` | number | Confidence level as a proportion |
| `drop` | number | Expected dropout rate (%) |

**Call:** `calc_ss1prop(p, precision, ci, drop)`

**Returns:** `{ n, n_drop }`

---

### 3. Two Independent Means — `calc_ss2mean`

Calculates sample size for a hypothesis test comparing two independent group
means, allowing an unequal group-size ratio.

**Formula reference:** Machin et al. (2009).

| Parameter | Type | Description |
|---|---|---|
| `sd` | number | Pooled (common) standard deviation |
| `diff` | number | Expected difference in means |
| `m` | number | Ratio n0/n1 (1 for equal groups) |
| `alpha` | number | Significance level (e.g., `0.05`) |
| `power` | number | Desired power (e.g., `0.80`) |
| `drop` | number | Expected dropout rate (%) |

**Call:** `calc_ss2mean(sd, diff, m, alpha, power, drop)`

**Returns:** `{ n1, n1_drop, n0, n0_drop, n, n_drop }`
- `n1` / `n0` — sizes for Groups 1 and 0
- `n` — total sample size

> **Note:** For a guaranteed 1:1 ratio, prefer `calc_ss2mean_ratio1` which
> uses the Lemeshow/Naing simplified formula.

---

### 4. Two Independent Means, 1:1 Ratio — `calc_ss2mean_ratio1`

As above but assumes equal group sizes and uses a simplified formula.

**Formula reference:** Lemeshow et al. (1990); Naing (2011).

| Parameter | Type | Description |
|---|---|---|
| `sd` | number | Pooled standard deviation |
| `diff` | number | Expected difference in means |
| `alpha` | number | Significance level |
| `power` | number | Desired power |
| `drop` | number | Expected dropout rate (%) |

**Call:** `calc_ss2mean_ratio1(sd, diff, alpha, power, drop)`

**Returns:** `{ n, n_drop }` (n is the per-group size)

---

### 5a. Paired Means — SD of Differences Helper — `calc_sd_ss2mean_paired`

Use this helper **first** when the user knows only the pre- and
post-intervention SDs and the pre–post correlation, not the SD of differences.

**Formula reference:** Arifin (2014).

| Parameter | Type | Description |
|---|---|---|
| `sd_pre` | number | Standard deviation at baseline |
| `sd_post` | number | Standard deviation at follow-up |
| `r_pre_post` | number | Pearson correlation between pre and post (−1 to 1) |

**Call:** `calc_sd_ss2mean_paired(sd_pre, sd_post, r_pre_post)`

**Returns:** `{ sd_d }` — SD of the differences (3 decimal places)

---

### 5b. Paired Means — Hypothesis Testing — `calc_hx_ss2mean_paired`

Calculates sample size for a dependent (paired) t-test.

**Formula reference:** Naing (2011); Arifin (2014).

| Parameter | Type | Description |
|---|---|---|
| `sd` | number | Standard deviation of the differences |
| `diff` | number | Expected mean difference |
| `alpha` | number | Significance level |
| `power` | number | Desired power |
| `drop` | number | Expected dropout rate (%) |

**Call:** `calc_hx_ss2mean_paired(sd, diff, alpha, power, drop)`

**Returns:** `{ n, n_drop }` (n is the number of pairs)

---

### 6. Repeated Measures — `calc_ss2mean_rm`

Calculates sample size for a repeated-measures design, with or without a
baseline measurement.

**Formula reference:** Machin et al. (2009).

| Parameter | Type | Description |
|---|---|---|
| `sd` | number | Standard deviation |
| `diff` | number | Expected mean difference |
| `r` | number | Total number of time points (including baseline if applicable) |
| `base` | number | `1` if a baseline measurement is included; `0` otherwise |
| `rho` | number | Correlation between repeated measurements (0–1) |
| `alpha` | number | Significance level |
| `power` | number | Desired power |
| `drop` | number | Expected dropout rate (%) |

**Call:** `calc_ss2mean_rm(sd, diff, r, base, rho, alpha, power, drop)`

**Returns:** `{ n, n_drop }`

---

### 7. Two Independent Proportions — `calc_ss2prop`

Calculates sample size for testing the difference between two independent
proportions, allowing unequal group sizes.

**Formula reference:** Machin et al. (2009).

| Parameter | Type | Description |
|---|---|---|
| `p0` | number | Proportion in Group 0 (control/reference) |
| `p1` | number | Proportion in Group 1 (treatment/comparator) |
| `m` | number | Ratio n0/n1 (1 for equal groups) |
| `alpha` | number | Significance level |
| `power` | number | Desired power |
| `drop` | number | Expected dropout rate (%) |

**Call:** `calc_ss2prop(p0, p1, m, alpha, power, drop)`

**Returns:** `{ n1, n1_drop, n0, n0_drop, n, n_drop }`

---

### 8. Two Independent Proportions, 1:1 Ratio — `calc_ss2prop_ratio1`

As above but assumes equal group sizes.

**Formula reference:** Lemeshow et al. (1990).

| Parameter | Type | Description |
|---|---|---|
| `p0` | number | Proportion in Group 0 |
| `p1` | number | Proportion in Group 1 |
| `alpha` | number | Significance level |
| `power` | number | Desired power |
| `drop` | number | Expected dropout rate (%) |

**Call:** `calc_ss2prop_ratio1(p0, p1, alpha, power, drop)`

**Returns:** `{ n, n_drop }` (n is the per-group size)

---

### 9. Cronbach's Alpha — Hypothesis Testing — `calc_hx_ssalpha`

Calculates sample size to test whether Cronbach's alpha differs from a null
(baseline) value.

**Formula reference:** Bonett (2002).

| Parameter | Type | Description |
|---|---|---|
| `cronbach0_hx` | number | Null hypothesis alpha (H0) |
| `cronbach1_hx` | number | Alternative hypothesis alpha (H1) |
| `alpha_hx` | number | Significance level |
| `power_hx` | number | Desired power |
| `item_hx` | number | Number of items in the scale |
| `drop_hx` | number | Expected dropout rate (%) |

**Call:** `calc_hx_ssalpha(cronbach0_hx, cronbach1_hx, alpha_hx, power_hx, item_hx, drop_hx)`

**Returns:** `{ n_hx, n_drop_hx }`

---

### 10. Cronbach's Alpha — Estimation — `calc_est_ssalpha`

Calculates sample size to estimate Cronbach's alpha with a desired precision.

**Formula reference:** Bonett (2002).

| Parameter | Type | Description |
|---|---|---|
| `cronbach_est` | number | Expected Cronbach's alpha |
| `precision_est` | number | Desired margin of error (half-width of CI) |
| `ci_est` | number | Confidence level as a proportion |
| `item_est` | number | Number of items in the scale |
| `drop_est` | number | Expected dropout rate (%) |

**Call:** `calc_est_ssalpha(cronbach_est, precision_est, ci_est, item_est, drop_est)`

**Returns:** `{ n_est, n_drop_est }`

---

### 11. Animal Studies — `calc_ssanimal`

Estimates the sample size per group for animal studies using the
resource-equation (ANOVA) approach.

**Formula reference:** Arifin & Zahiruddin (2017).

| Parameter | Type | Description |
|---|---|---|
| `k` | number | Number of groups (use 1 if no between-group comparison) |
| `r` | number | Number of repeated measurements (use 1 if no repeated measures) |
| `sacrifice` | number | `1` if animal sacrifice is required at each time point; `0` otherwise |

**Call:** `calc_ssanimal(k, r, sacrifice)`

**Returns:** `{ n_min, n_max, design, sacrifice_req, N_min, N_max }`
- `n_min` / `n_max` — minimum and maximum per-group sample sizes
- `design` — text description of the inferred ANOVA design
- `sacrifice_req` — `"required"` or `"not required"`
- `N_min` / `N_max` — total number of animals required

---

### 12. AUROC — Hypothesis Testing — `calc_hx_ssauroc`

Calculates sample size to test whether the AUROC differs from a null value.

**Formula reference:** Zhou, Obuchowski & McClish (2011), Equations 6.6 & 6.8.

> **Dependency:** Requires `Decimal` (Decimal.js) for high-precision arithmetic.
> Pass the `Decimal` constructor as the final argument.

| Parameter | Type | Description |
|---|---|---|
| `A0` | number | Null hypothesis AUROC (H0) |
| `A` | number | Alternative hypothesis AUROC (H1) |
| `p` | number | Disease prevalence (0–1) |
| `alpha` | number | Significance level |
| `power` | number | Desired power |
| `drop` | number | Expected dropout rate (%) |
| `Decimal` | object | Decimal.js constructor |

**Call:** `calc_hx_ssauroc(A0, A, p, alpha, power, drop, Decimal)`

**Returns:** `{ n, n_drop }`

---

### 13. AUROC — Estimation — `calc_est_ssauroc`

Calculates sample size to estimate the AUROC with a desired precision.

**Formula reference:** Zhou, Obuchowski & McClish (2011), Equations 6.2 & 6.6.

> **Dependency:** Requires `Decimal` (Decimal.js). Pass the constructor as the final argument.

| Parameter | Type | Description |
|---|---|---|
| `A` | number | Expected AUROC |
| `p` | number | Disease prevalence (0–1) |
| `precision` | number | Desired margin of error |
| `ci` | number | Confidence level as a proportion |
| `drop` | number | Expected dropout rate (%) |
| `Decimal` | object | Decimal.js constructor |

**Call:** `calc_est_ssauroc(A, p, precision, ci, drop, Decimal)`

**Returns:** `{ n, n_drop }`

---

### 14. Correlation — Hypothesis Testing — `calc_hx_sscorr`

Calculates sample size to test whether a Pearson correlation coefficient is
non-zero (or differs from a specified null).

**Formula reference:** Machin et al. (2009).

| Parameter | Type | Description |
|---|---|---|
| `corr` | number | Expected correlation coefficient r (non-zero) |
| `alpha` | number | Significance level |
| `power` | number | Desired power |
| `drop` | number | Expected dropout rate (%) |

**Call:** `calc_hx_sscorr(corr, alpha, power, drop)`

**Returns:** `{ n, n_drop }`

---

### 15. Correlation — Estimation — `calc_est_sscorr`

Calculates sample size to estimate a Pearson correlation coefficient with a
specified precision.

**Formula reference:** Moinester & Gottfried (2014), Equation 8.

| Parameter | Type | Description |
|---|---|---|
| `corr` | number | Expected correlation coefficient r |
| `precision` | number | Desired margin of error (half-width of CI) |
| `ci` | number | Confidence level as a proportion |
| `drop` | number | Expected dropout rate (%) |

**Call:** `calc_est_sscorr(corr, precision, ci, drop)`

**Returns:** `{ n, n_drop }`

---

### 16. ICC — Hypothesis Testing — `calc_hx_ssicc`

Calculates the number of subjects required to test whether the ICC differs
from a null value.

**Formula reference:** Walter, Eliasziw & Donner (1998).

| Parameter | Type | Description |
|---|---|---|
| `icc0` | number | Null hypothesis ICC (H0) |
| `icc1` | number | Alternative hypothesis ICC (H1) |
| `alpha` | number | Significance level |
| `power` | number | Desired power |
| `rater` | number | Number of raters or replicates per subject (>=2) |
| `drop` | number | Expected dropout rate (%) |

**Call:** `calc_hx_ssicc(icc0, icc1, alpha, power, rater, drop)`

**Returns:** `{ n, n_drop }` (n is the number of subjects)

---

### 17. ICC — Estimation — `calc_est_ssicc`

Calculates the number of subjects required to estimate the ICC with a desired
precision.

**Formula reference:** Bonett (2002).

| Parameter | Type | Description |
|---|---|---|
| `icc` | number | Expected ICC |
| `precision` | number | Desired margin of error (half-width of CI) |
| `ci` | number | Confidence level as a proportion |
| `rater` | number | Number of raters or replicates per subject (>=2) |
| `drop` | number | Expected dropout rate (%) |

**Call:** `calc_est_ssicc(icc, precision, ci, rater, drop)`

**Returns:** `{ n, n_drop }`

---

### 18. Cohen's Kappa — Hypothesis Testing — `calc_hx_sskappa`

Calculates sample size to test whether Cohen's Kappa differs from a null value.

**Formula references:** Donner & Eliasziw (1992); Shoukri, Asyali & Donner (2004).

| Parameter | Type | Description |
|---|---|---|
| `k0` | number | Null hypothesis Kappa (H0) |
| `k1` | number | Alternative hypothesis Kappa (H1) |
| `p` | number | Prevalence of the trait (0–1) |
| `alpha` | number | Significance level |
| `power` | number | Desired power |
| `drop` | number | Expected dropout rate (%) |

**Call:** `calc_hx_sskappa(k0, k1, p, alpha, power, drop)`

**Returns:** `{ n, n_drop }`

---

### 19. Cohen's Kappa — Estimation — `calc_est_sskappa`

Calculates sample size to estimate Cohen's Kappa with a desired precision.

**Formula references:** Donner & Eliasziw (1992); Shoukri, Asyali & Donner (2004).

| Parameter | Type | Description |
|---|---|---|
| `k` | number | Expected Kappa value |
| `precision` | number | Desired margin of error (half-width of CI) |
| `p` | number | Prevalence of the trait (0–1) |
| `ci` | number | Confidence level as a proportion |
| `drop` | number | Expected dropout rate (%) |

**Call:** `calc_est_sskappa(k, precision, p, ci, drop)`

**Returns:** `{ n, n_drop }`

---

### 20. Logistic Regression — `calc_sslogistic`

Estimates sample size for logistic regression using the Events Per Variable
(EPV) rule of thumb.

**Formula references:** Hosmer, Lemeshow & Sturdivant (2013); Peduzzi et al.
(1996); Vittinghoff & McCulloch (2007).

| Parameter | Type | Description |
|---|---|---|
| `k` | number | Total number of independent (predictor) variables |
| `epp` | number | Desired events per variable (EPV; commonly 10–20) |
| `p` | number | Proportion of subjects with the outcome event (0–1) |
| `drop` | number | Expected dropout rate (%) |

**Call:** `calc_sslogistic(k, epp, p, drop)`

**Returns:** `{ n1, n, n_drop }`
- `n1` — minimum number of outcome events required
- `n` — total sample size

---

### 21. McNemar's Test — `calc_ssmcnemar`

Calculates sample size (number of pairs) for McNemar's test of paired
proportions.

**Formula reference:** Machin et al. (2009).

| Parameter | Type | Description |
|---|---|---|
| `p0` | number | Proportion in Group 0 |
| `p1` | number | Proportion in Group 1 |
| `alpha` | number | Significance level |
| `power` | number | Desired power |
| `drop` | number | Expected dropout rate (%) |

**Call:** `calc_ssmcnemar(p0, p1, alpha, power, drop)`

**Returns:** `{ n, n_drop }` (n is the number of pairs)

---

### 22. SEM / RMSEA — `ncp` + `calc_ssrmsea1`

Calculates sample size for Structural Equation Modelling (SEM) based on the
Root Mean Square Error of Approximation (RMSEA).

**Formula reference:** Kim (2005).

**Step 1 — Compute the non-centrality parameter (NCP):**

| Parameter | Type | Description |
|---|---|---|
| `alpha` | number | Significance level |
| `power` | number | Desired power |
| `df` | number | Degrees of freedom of the SEM model |

**Call:** `delta = ncp(alpha, power, df)`

**Step 2 — Compute sample size:**

| Parameter | Type | Description |
|---|---|---|
| `rmsea` | number | Desired (acceptable) RMSEA value (e.g., `0.05`) |
| `alpha` | number | Significance level |
| `power` | number | Desired power |
| `df` | number | Degrees of freedom |
| `drop` | number | Expected dropout rate (%) |
| `delta` | number | NCP from Step 1 |

**Call:** `calc_ssrmsea1(rmsea, alpha, power, df, drop, delta)`

**Returns:** `{ n, n_drop }`

---

### 23. Sensitivity & Specificity — `calc_sssnsp`

Calculates sample size for estimating sensitivity and/or specificity of a
diagnostic test with a specified precision.

**Formula reference:** Buderer (1996).

| Parameter | Type | Description |
|---|---|---|
| `sn` | number | Expected sensitivity (0–1) |
| `sp` | number | Expected specificity (0–1) |
| `p` | number | Disease prevalence (0–1) |
| `precision` | number | Desired margin of error (proportion scale) |
| `ci` | number | Confidence level as a proportion |
| `drop` | number | Expected dropout rate (%) |

**Call:** `calc_sssnsp(sn, sp, p, precision, ci, drop)`

**Returns:** `{ n1, n2, n, n_drop }`
- `n1` — sample size driven by sensitivity requirement
- `n2` — sample size driven by specificity requirement
- `n` — overall required sample size (maximum of n1 and n2)

---

## Workflow Steps

Follow these steps sequentially for every sample size request. Steps 1–4 are a
consultation phase; do not proceed to calculation until they are complete.

### Step 1 — Clarify the Study Objective

1. Ask the researcher to describe the purpose of their study in plain language.
2. Establish whether the goal is **estimation** (e.g., describing a parameter
   with a confidence interval) or **hypothesis testing** (e.g., comparing
   groups or testing an association).
3. Note any contextual constraints (e.g., limited budget, fixed number of
   sites, animal ethics restrictions).

---

### Step 2 — Clarify the Outcome and Predictor/Associated Variables

1. Ask the researcher to name the **primary outcome variable** (dependent
   variable) and any **predictor or associated variables** (independent
   variables, grouping factors, covariates).
2. Determine the number of groups or conditions being compared, if applicable.
3. Confirm whether the study is cross-sectional, longitudinal, or involves
   repeated measurements.

---

### Step 3 — Clarify the Scale of Variables

1. For each variable identified in Step 2, determine its **measurement scale**:
   - **Continuous** (e.g., blood pressure in mmHg, weight in kg)
   - **Binary / dichotomous** (e.g., presence or absence of disease)
   - **Ordinal** (e.g., Likert scale items)
   - **Nominal / categorical** (e.g., blood group)
2. The scale of the outcome variable is the primary driver of formula
   selection; note it explicitly before proceeding.

---

### Step 4 — Suggest Statistical Analyses and Select a Formula

1. Based on the objective (Step 1), variables (Step 2), and scales (Step 3),
   present the researcher with **two to four suitable statistical analysis
   options**, briefly explaining the rationale for each.
2. Let the researcher **choose** the analysis they wish to proceed with. Do
   not select on their behalf.
3. Once the analysis is chosen, map it to the appropriate function using the
   **Design to Function Quick-Lookup** table below.

**Design to Function Quick-Lookup**

| Chosen analysis / design | Function(s) |
|---|---|
| Estimating a single mean | `calc_ss1mean` |
| Estimating a single proportion | `calc_ss1prop` |
| Comparing two independent means (equal groups) | `calc_ss2mean_ratio1` |
| Comparing two independent means (unequal groups) | `calc_ss2mean` |
| Paired t-test / before-after comparison | `calc_sd_ss2mean_paired` then `calc_hx_ss2mean_paired` |
| Repeated measures / longitudinal design | `calc_ss2mean_rm` |
| Comparing two independent proportions (equal groups) | `calc_ss2prop_ratio1` |
| Comparing two independent proportions (unequal groups) | `calc_ss2prop` |
| Testing Cronbach's alpha | `calc_hx_ssalpha` |
| Estimating Cronbach's alpha | `calc_est_ssalpha` |
| Animal study (ANOVA / resource equation) | `calc_ssanimal` |
| Testing AUROC | `calc_hx_ssauroc` |
| Estimating AUROC | `calc_est_ssauroc` |
| Testing Pearson correlation | `calc_hx_sscorr` |
| Estimating Pearson correlation | `calc_est_sscorr` |
| Testing ICC | `calc_hx_ssicc` |
| Estimating ICC | `calc_est_ssicc` |
| Testing Cohen's Kappa | `calc_hx_sskappa` |
| Estimating Cohen's Kappa | `calc_est_sskappa` |
| Logistic regression | `calc_sslogistic` |
| McNemar's test (paired proportions) | `calc_ssmcnemar` |
| SEM / RMSEA | `ncp` then `calc_ssrmsea1` |
| Diagnostic test (sensitivity/specificity) | `calc_sssnsp` |

---

### Step 5 — Collect Input Parameters

1. List every parameter required by the chosen function (refer to the
   *Function Reference* section).
2. Check which parameters the researcher has **already provided**.
3. Ask for **only the missing parameters** in a single, numbered prompt. Do
   not ask for parameters one by one across multiple turns.
4. When the researcher is unsure of a parameter value, **guide them** with
   practical suggestions — for example, pointing to published pilot studies,
   clinical guidelines, or rule-of-thumb values.
5. Apply the following **default values** when the researcher does not specify
   a preference and the context does not contradict them:

| Parameter | Default | Rationale |
|---|---|---|
| `alpha` / significance level | `0.05` | Standard two-tailed test |
| `power` | `0.80` | Widely accepted minimum |
| `ci` | `0.95` | Standard 95% confidence interval |
| `m` (group ratio) | `1` | Equal group sizes |
| `drop` | `0` | No dropout adjustment unless specified |
| `base` (repeated measures) | `0` | No baseline measurement unless specified |

6. Confirm the final parameter set with the researcher before proceeding if
   any defaults have been applied to critical values (e.g., `sd`, `diff`, `p`).

---

### Step 6 — Execute the Calculation

1. Invoke the appropriate function(s) from `ssformula_annotated.js` with the
   collected parameter values.
2. For **paired means**: invoke `calc_sd_ss2mean_paired` first if the
   researcher has not provided `sd_d`, then pass its output to
   `calc_hx_ss2mean_paired`.
3. For **SEM/RMSEA**: invoke `ncp(alpha, power, df)` first, then pass the
   returned `delta` to `calc_ssrmsea1`.
4. For **AUROC** functions: ensure a `Decimal` (Decimal.js) constructor is
   available in scope and pass it as the final argument.
5. Record all intermediate outputs for inclusion in the response.

---

### Step 7 — Validate the Results

1. Verify that the returned sample size is a **positive integer** (the
   functions use `Math.ceil`, so this should always hold).
2. If `n_drop` equals `n` (i.e., `drop` was `0`), note this explicitly.
3. For animal studies, check that the `design` field describes the
   researcher's intended ANOVA structure and flag any mismatch.
4. For logistic regression, remind the researcher that EPV >= 10 is the
   conservative guideline; EPV of 5–9 may suffice in some contexts
   (Vittinghoff & McCulloch, 2007).
5. For SEM, remind the researcher that `df` must be computed from their model
   specification (observed variables minus estimated parameters).

---

### Step 8 — Report the Result

Present the result in the structured format defined in the *Output Format*
section below.

---

## Constraints & Guardrails

- **MUST DO:**
  - Always report **both** `n` (unadjusted) and `n_drop` (dropout-adjusted)
    in the output.
  - State the **formula reference** and the **function used** for
    reproducibility.
  - Use **British English** spelling and grammar throughout (e.g., *analyse*,
    *colour*, *recognised*, *modelling*).
  - Round all sample sizes **upwards** to the nearest whole number — never
    truncate or round down.

- **DO NOT:**
  - Invent parameter values not provided or agreed to by the user.
  - Use a function for a design it was not intended for (e.g., do not use
    `calc_ss2mean_ratio1` when the user has specified an unequal group ratio).
  - Report sample sizes as fractions or decimals.
  - Apply dropout adjustment when `drop = 0`; in this case `n_drop` equals
    `n` — report it without comment.
  - Use `calc_ssrmsea1` without first computing `delta` via `ncp`.

- **Edge Cases:**

| Situation | Handling |
|---|---|
| User supplies `ci` as a percentage (e.g., 95) | Convert to proportion: `ci = value / 100` |
| User supplies `alpha` as a percentage (e.g., 5) | Convert to proportion: `alpha = value / 100` |
| User does not know the SD of differences for a paired test | Use `calc_sd_ss2mean_paired` first |
| `p0 == p1` in `calc_ss2prop` or `calc_ssmcnemar` | Reject input — the difference between proportions must be non-zero |
| `corr == 0` in `calc_hx_sscorr` | Reject input — a non-zero expected correlation must be specified |
| `icc0 == icc1` or `k0 == k1` or `cronbach0 == cronbach1` | Reject input — H0 and H1 values must differ |
| Animal study with `k == 1` and `r == 1` | Inform the user that this design is inappropriate for the resource-equation method; request valid inputs |
| `ncp` or `pncchisq` returns `NaN` | Report a computation error and request the user to check their `df`, `alpha`, and `power` values |

---

## Output Format

Present the result in plain Markdown using the following structure. Adapt
section headings as appropriate for the specific design.

---

### Output Structure Template

```
#### Sample Size Calculation — [Design Name]

**Parameters used:**

| Parameter | Value |
|---|---|
| [Parameter name] | [Value with units] |
| ... | ... |

**Function invoked:** `[function_name]([args])`

**Results:**

| Outcome | Value |
|---|---|
| Required sample size (n) | [value] |
| Sample size with [X]% dropout adjustment (n_drop) | [value] |
| [Additional outputs, e.g. n1, n0 for two-group designs] | [values] |

**Interpretation:**

A minimum of **[n]** [subjects/pairs/animals] is required. Accounting for an
expected dropout rate of **[drop]%**, the recommended sample size to recruit
is **[n_drop]**.

[For two-group designs, specify the per-group allocation.]
[For animal studies, include the ANOVA design description and sacrifice requirement.]

**Formula Reference:**

[Author(s) (Year). Title. Journal, Vol(Issue), pages.]
```

---

### Worked Example

**User request:** "I want to compare the mean blood pressure between a
treatment group and a control group. The pooled SD is 15 mmHg, and I expect a
difference of 5 mmHg. I will use equal group sizes, a significance level of
0.05, and 80% power. I expect 10% dropout."

**Step 1 — Design:** Two independent means, 1:1 ratio -> `calc_ss2mean_ratio1`

**Step 2 — Parameters:**

| Parameter | Value |
|---|---|
| `sd` | 15 |
| `diff` | 5 |
| `alpha` | 0.05 |
| `power` | 0.80 |
| `drop` | 10 |

**Step 3 — Calculation:**

```
calc_ss2mean_ratio1(15, 5, 0.05, 0.80, 10)
-> { n: 142, n_drop: 158 }
```

**Step 4 — Output:**

#### Sample Size Calculation — Two Independent Means (1:1 Ratio)

**Parameters used:**

| Parameter | Value |
|---|---|
| Pooled SD | 15 mmHg |
| Expected difference | 5 mmHg |
| Significance level (alpha) | 0.05 (two-tailed) |
| Power (1 - beta) | 0.80 (80%) |
| Group ratio | 1:1 |
| Dropout rate | 10% |

**Function invoked:** `calc_ss2mean_ratio1(15, 5, 0.05, 0.80, 10)`

**Results:**

| Outcome | Value |
|---|---|
| Required sample size per group (n) | 142 |
| Total sample size (2 x n) | 284 |
| Per-group size with 10% dropout (n_drop) | 158 |
| Total size with dropout (2 x n_drop) | 316 |

**Interpretation:**

A minimum of **142 participants per group** (284 total) is required. Accounting
for an expected dropout rate of **10%**, the recommended number to recruit is
**158 per group** (316 total).

**Formula Reference:**

Lemeshow, S., Hosmer Jr, D. W., Klar, J., & Lwanga, S. K. (1990). *Adequacy
of sample size in health studies.* England: John Wiley & Sons Ltd.

Naing, N. N. (2011). *A practical guide on determination of sample size in
health sciences research.* Kelantan: Pustaka Aman Press.

---

## References

The following reference papers are available in the `references/` directory and
may be consulted for methodological background:

- `2013 - Arifin - Introduction to sample size calculation.pdf` — foundational
  overview of sample size methods for means and proportions.
- `2018 Arifin - A Web-based Sample Size Calculator for Reliability Studies.pdf`
  — covers ICC, Cronbach's alpha, and Kappa methods.
- `2025 Arifin - A Web-Based Sample Size Calculator for Structural Equation Modelling.pdf`
  — covers RMSEA-based SEM sample size methods.
- `ssc_tutorial.pdf` — practical tutorial accompanying the online calculator at
  https://wnarifin.github.io/ssc_web.html
