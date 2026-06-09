# 05 — Statistical Analysis Guide

> **Document status:** Canonical reference | **Version:** 1.0.0 | **Last updated:** 2026

---

## 1. Analysis Philosophy

This protocol follows **pre-registered, reproducible analysis** principles:

1. All primary hypotheses and analysis plans are pre-registered (OSF) before data collection
2. Analysis scripts (`pip_analysis.R` and `pip_analysis.py`) are version-controlled and frozen before unblinding
3. Primary analyses are confirmatory; exploratory analyses are clearly labelled
4. Effect sizes and confidence intervals are reported alongside p-values
5. Raw data and analysis code are published with any resulting paper

---

## 2. Primary Analysis: H1 — Fidelity Effect on Presence

### 2.1 Model Specification

**Linear Mixed-Effects Model (LMM):**

```
IPQ_GP ~ FidelityLevel + TaskComplexity + FidelityLevel:TaskComplexity
       + VR_Experience + (1 + FidelityLevel | ParticipantID)
```

- **Fixed effects:** Fidelity Level (3-level factor), Task Complexity (2-level), their interaction, VR Experience (covariate)
- **Random effects:** Random intercept + random slope for Fidelity Level per participant
- **Software:** `lme4` in R; `statsmodels` MixedLM in Python
- **Degrees of freedom:** Kenward-Roger approximation

### 2.2 Planned Contrasts

| Contrast | Hypothesis | Direction |
|---|---|---|
| HF vs. LF | H1 primary | HF > LF |
| HF vs. MF | H1 secondary | HF > MF |
| MF vs. LF | Threshold region | MF > LF |

All contrasts are Bonferroni-corrected (adjusted α = .05/3 = .017).

### 2.3 Effect Size

Report Cohen's d (pairwise) and partial η² (overall model) with 95% CIs.

---

## 3. Threshold Analysis: H2 — Change-Point Detection

### 3.1 Frame Rate Change-Point

To test whether there is a discontinuous jump in presence scores at a specific frame rate:

```r
# R code
library(changepoint)
fps_presence <- aggregate(ipq_gp ~ frame_rate_mean_binned, data = df, mean)
cpt_result <- cpt.mean(fps_presence$ipq_gp, method = "PELT", penalty = "BIC")
plot(cpt_result)
```

```python
# Python code
import ruptures as rpt
import numpy as np

signal = fps_presence_array  # mean IPQ-GP binned by fps
algo = rpt.Pelt(model="rbf").fit(signal)
breakpoints = algo.predict(pen=10)
```

### 3.2 Interpretation

- If a single change-point is detected between 45–90 fps bins → H2 supported
- Report change-point location, confidence interval, and effect magnitude
- If no change-point detected → report null result; presence change is gradual

---

## 4. Moderation Analysis: H3 — VR Experience Moderation

### 4.1 Model

Add VR Experience × Fidelity Level interaction to the LMM:

```
IPQ_GP ~ FidelityLevel * VR_Experience + TaskComplexity
       + (1 | ParticipantID)
```

### 4.2 Follow-Up

If the interaction is significant, run simple slopes analysis:
- Plot IPQ-GP ~ FidelityLevel separately for Novice, Intermediate, Experienced
- Report simple slopes with standard errors

---

## 5. Safety Analysis: H4 — Simulator Sickness

### 5.1 Descriptive Analysis

For each fidelity level, report:
- Mean SSQ Total Score ± SD
- Proportion of participants with SSQ-TS > 10 (warning level)
- Proportion of participants with SSQ-TS > 15 (stop criterion)

### 5.2 Trend Analysis

```r
# Test monotonic increase in SSQ-TS across fidelity levels
library(coxme)
jonckheere.test(ssq_ts ~ fidelity_level, data = df,
                alternative = "increasing")
```

### 5.3 Safety Reporting

Regardless of statistical outcome, report the full SSQ trajectory for all participants who triggered the stop criterion (as case studies).

---

## 6. Audio Analysis: H5 — HRTF Effect on Spatial Presence

### 6.1 Model

Focus on IPQ Spatial Presence subscale (IPQ-SP):

```
IPQ_SP ~ AudioMode + FidelityLevel + AudioMode:FidelityLevel
       + (1 | ParticipantID)
```

`AudioMode` is derived from the fidelity level (None / Stereo / HRTF).

### 6.2 Planned Contrast

- HRTF vs. Stereo, controlling for visual fidelity level
- Expected: HRTF > Stereo, independent of fidelity level

---

## 7. Exploratory Analyses (Not Pre-Registered)

The following are clearly labelled as exploratory:

### 7.1 Head Movement Entropy as Presence Proxy

```python
from scipy.stats import entropy
import numpy as np

def angular_velocity_entropy(rotation_log_df):
    """Compute Shannon entropy of angular velocity distribution."""
    angular_vel = np.diff(rotation_log_df[['rx','ry','rz']].values, axis=0)
    speed = np.linalg.norm(angular_vel, axis=1)
    hist, _ = np.histogram(speed, bins=50, density=True)
    hist = hist[hist > 0]  # remove zero bins
    return entropy(hist)
```

Correlate head movement entropy with IPQ-GP across participants and conditions.

### 7.2 Reaction Time as Cognitive Load Indicator

- Compare RT distributions across task complexity levels (Simple vs. Complex)
- Test whether high fidelity reduces RT in the Complex task (attentional engagement hypothesis)

### 7.3 Individual Threshold Estimation

For each participant, fit a sigmoid (logistic) function to their presence scores across fidelity levels:

```python
from scipy.optimize import curve_fit
import numpy as np

def sigmoid(x, x0, k):
    return 1 / (1 + np.exp(-k * (x - x0)))

# x = immersion score (composite), y = normalised IPQ-GP
popt, _ = curve_fit(sigmoid, immersion_composite, ipq_gp_normalised)
individual_threshold = popt[0]  # inflection point
```

This yields a **per-participant immersion threshold** on the composite immersion scale.

---

## 8. Reporting Standards

All results should be reported following APA 7th edition and the JARS-Quant guidelines:

| Element | Required |
|---|---|
| Sample size with justification | Yes |
| Effect sizes with 95% CIs | Yes |
| Exact p-values (not just p < .05) | Yes |
| Model assumptions checked | Yes |
| Missing data handling described | Yes |
| Analysis code availability statement | Yes |
| Pre-registration link | Yes |

---

## 9. Figures

### 9.1 Required Figures

1. **Figure 1:** IPQ-GP mean ± SE by Fidelity Level (bar chart with individual data points)
2. **Figure 2:** IPQ-GP by Fidelity Level, split by VR Experience (moderation plot)
3. **Figure 3:** SSQ Total Score by Fidelity Level (safety figure)
4. **Figure 4:** Change-point analysis for frame rate × presence relationship
5. **Figure 5:** Individual threshold distribution (histogram of sigmoid inflection points)

### 9.2 Colour Scheme (Accessibility)

All figures use a colour-blind-safe palette:
- LF: `#0077BB` (blue)
- MF: `#EE7733` (orange)
- HF: `#009988` (teal)

---

## 10. Software Environment

### R Environment

```r
# Required packages
packages <- c("lme4", "lmerTest", "emmeans", "effectsize",
              "ggplot2", "changepoint", "NSM3", "tidyverse")
install.packages(packages)

# Session info
sessionInfo()  # Record and report in supplementary
```

### Python Environment

```bash
# requirements.txt
pandas>=2.0
numpy>=1.26
scipy>=1.13
statsmodels>=0.14
ruptures>=1.1
matplotlib>=3.9
seaborn>=0.13
pingouin>=0.5
```

---

## 11. Pre-Registration Template

Register the following on OSF before data collection:

```
1. Research questions and hypotheses (H1-H5)
2. Sample size and power analysis
3. Inclusion/exclusion criteria
4. Primary analysis model specification
5. Planned contrasts and corrections
6. Stopping rules
7. Definition of outliers
8. Software and version
9. Analysis script (frozen version)
```

OSF pre-registration link: *(to be added before data collection)*

---

## 12. References

- Cummings, J. J., & Bailenson, J. N. (2016). How immersive is enough? *Media Psychology*, 19(2).
- Killick, R., & Eckley, I. (2014). changepoint: An R package for changepoint analysis. *Journal of Statistical Software*, 58(3).
- Truong, C., et al. (2020). Selective review of offline change point detection methods. *Signal Processing*, 167.
