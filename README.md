# Predicting COVID-19 Vaccine Uptake and Positivity Rates Using Behavioral and Belief Indicators 
**Team Members:**  <br>
- ssamadda@andrew.cmu.edu  <br>
- abharga3@andrew.cmu.edu  <br>
- aakulshr@andrew.cmu.edu  <br>
- shrenyam@andrew.cmu.edu  <br>

---

## Introduction

The COVID-19 pandemic revealed critical gaps in public health preparedness, especially the need for data-driven insights to guide interventions. Across U.S. regions, significant variation in vaccine uptake and COVID-19 positivity rates highlighted the importance of understanding the behaviors and beliefs influencing these outcomes.
<br>
This project addresses two core predictive tasks:<br>
- **Can vaccine uptake be accurately predicted using self-reported behaviors and beliefs?**<br>
- **Can COVID-19 positivity rates be reliably forecasted using the same indicators?**<br>

By applying predictive modeling and analysis, we aim to generate actionable insights for targeted public health initiatives, resource optimization, and enhanced community resilience.<br>

---

## Dataset Overview

- **Source:** COVID-19 Trends and Impact Survey (CTIS), Carnegie Mellon University Delphi Group in collaboration with Facebook<br>
- **Timeframe:** January 7, 2021 – February 12, 2021<br>
- **Instances:** 25,627 county-day records<br>

**Key Features:**
- *Behavioral Indicators:* Mask-wearing, public event attendance, work outside home, public transit use, etc.<br>
- *Belief Indicators:* Trust in vaccine recommendations from WHO, friends/family, government, politicians.<br>
- *COVID Activity Indicators:* Symptom reporting, testing rates, vaccination rates.<br>

**Targets:**
- `smoothed_wcovid_vaccinated`: % vaccinated <br>
- `smoothed_wtested_positive_14d`: COVID-19 test positivity rate (14-day window)<br>

Each record aggregates survey responses for a U.S. county on a given day.<br>

---

## Data Analysis

### Data Cleaning

- **Dropping Columns:** Removed columns with >50% missing values.<br>
- **Time-Series Imputation:** Grouped by county, sorted by time, applied interpolation, forward-fill, and backward-fill.<br>
- **Outlier Capping:** Values capped at 1st and 99th percentiles.<br>
- **Feature Scaling:** Standardized all numeric features (zero mean, unit variance).<br>
- **Multicollinearity Check:** No feature pairs with absolute Pearson correlation >0.90.<br>

### Exploratory Data Analysis (EDA)

- **Correlation Heatmap:** Examined feature relationships; some predictors highly correlated (r > 0.8).<br>
- **Feature Engineering:**  <br>
  - *Behavioral Compliance Index*: Combines masking, vaccination, and trust in health authorities.<br>
  - *Social Exposure Score*: Estimates daily exposure risk.<br>
  - *Mistrust Gap*: Difference in trust between health experts and politicians.<br>
  - *Peer Influence Index*: Quantifies susceptibility to peer norms.<br>
  - *CLI Risk Behavior Flag*: Flags counties with high COVID-like illness and low protective behaviors.
<br>
---

## Dataset Splitting Strategy

- **Chronological Split:**  <br>
  - *Training set*: Data up to Feb 5, 2021  <br>
  - *Testing set*: Data from Feb 6, 2021 onward  <br>
- **Rationale:** Preserves temporal dependencies and prevents data leakage, simulating real-world forecasting.<br>
- **Limitation:** Potential for distribution shifts due to policy, seasonal, or behavioral changes.<br>

---

## Modeling Approaches

- **Baseline:** Linear Regression with Lasso regularization (feature selection, interpretable).<br>
- **Advanced Models:**<br>
  - Random Forest Regressor (captures non-linearities, feature importance)<br>
  - Gradient Boosting Regressor (sequential error correction, strong for temporal data)<br>
  - Neural Network (PyTorch, 4 layers, ReLU, early stopping, Adam optimizer)<br>

---

## Evaluation Metrics

- **Root Mean Squared Error (RMSE):** Penalizes large errors, critical for public health.<br>
- **R-squared (R²):** Measures variance explained by the model.<br>
- **No k-fold cross-validation:** Not suitable for time series; used chronological split instead.<br>

---

## Results

### Vaccine Uptake Prediction

- **Best Model:** Ridge Regression <br>
  - *Test R²*: ~0.94<br>
  - *Interpretability and regularization preferred for public health use*<br>
- **Neural Network:** Negative R², indicating poor performance<br>
- **Random Forest & Gradient Boosting:** Overfit training data, less reliable for generalization<br>

### COVID-19 Positivity Prediction

- **Best Model:** Optimized Gradient Boosting Regressor<br>
  - *Test R²*: 0.5546<br>
  - *RMSE*: 0.5735<br>
  - *Captured complex, nonlinear patterns better than linear models*<br>

---

## Key Predictors

| Task                  | Most Important Predictors                                                                                  |
|-----------------------|-----------------------------------------------------------------------------------------------------------|
| Vaccine Uptake        | Behavioral compliance index, working outside home, shopping, trust in WHO and politicians                 |
| COVID-19 Positivity   | smoothed_wcli (clinic visits), smoothed_wothers_masked (masking in environment), large events, compliance  |

- **Peer influence and mistrust**: Minimal impact on vaccine uptake and positivity once analyzed through Lasso.

---

## Relationship Between Vaccine Uptake and COVID Positivity

- **Pearson Correlation (r):** -0.0668 (not significant) <br>
- **R²:** 0.0045 (only 0.45% variance explained)<br>
- **Interpretation:** Weak, non-significant relationship; regional variability dominates. <br>

---

## Policy Recommendations

- **Peer-Driven Behavioral Campaigns:**  
  Launch "Masking Matters" challenges leveraging social media and peer influence (masking has -0.40 correlation with positivity). <br>
- **Urban-Rural Tailoring:**  
  Urban: Focus on vaccination drives.  <br>
  Rural: Deploy mobile clinics/telehealth.<br>
- **CLI Hotspot Early Warning:**  
  Use CLI risk flag to deploy rapid response teams in high-risk counties.<br>
- **Trust-Building Initiatives:**  
  Host "Community Health Days" with local health networks to boost compliance and trust.<br>
- **Longitudinal Monitoring:**  
  Fund studies to track uptake, positivity, and variants for ongoing model refinement.
<br>
---

## Assumptions and Limitations

- **Self-Reported Data:** Subject to bias (social desirability, recall).<br>
- **Timeframe:** Early 2021; findings may not generalize to later pandemic phases.<br>
- **Outcome Focus:** Positivity rates, not severe outcomes (e.g., hospitalizations).<br>
- **Distribution Shifts:** Policy and seasonal changes may affect future model performance.<br>

---

## Tools Used

- Google Colab<br>
- Gemini<br>
- Stack Overflow<br>
- JIRA<br>

**Dataset:**  
Carnegie Mellon University Delphi Group. (2021). COVID-19 Trends and Impact Survey (CTIS)<br>

---

## Contact

For questions or collaboration, please reach out to any team member listed above.

---
