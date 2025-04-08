# CPD Project Final Report
**CS 396 Causal Inference**

## 1. Group Members
Alina Chen, Yulan Guo, Qinyan Li, Haohan Shi

## 2. Code and Documentation
- `cpd_models.ipynb`: Contains our final backdoor estimator, bootstrapped confidence intervals, DoubleML model using random forest, and preliminary model with final finding as the outcome.
- `cpd_synthetic_data.ipynb`: Includes code to generate synthetic data and compares expected vs. observed causal effects at sample sizes of 1,000, 10,000, and 100,000.

## 3. Estimation Implementation
- Estimation code: Cell 27 in `cpd_models.ipynb`
- Results: Cell 29
- Bootstrap estimate code: Cell 30

We used backdoor estimation to isolate the causal effect of **rank** on **penalty_code** for sustained complaints, controlling for:
- Race
- Gender
- Age
- Allegation category

Each rank was modeled separately using logistic regression. Model parameters showed confounders had varying effects by rank, indicating flexibility in modeling nonlinear relationships.

## 4. Changes Since the Update

### 4.1 Additional Confounders
- Replaced appointment date with age.
- Added binary variable for allegation category (1 = violent, 0 = nonviolent).
- Focused only on sustained complaints.

### 4.2 Bootstrap Confidence Intervals
- 95% CIs for each rank’s penalization probability.
- No overlap in intervals → statistically significant differences across ranks.

### 4.3 Complex Models
- Used DoubleML from `econML`, tried `LassoCV` and `RandomForest`.
- Grouped ranks into "low" (2–5) and "high" (6–10).
- Found an average treatment effect (ATE) of -0.0938 (promotion → 9% decrease in penalization).

### 4.4 IPW (Inverse Probability Weighting)
- Estimated propensity scores with logistic regression.
- Weighted samples to adjust for imbalanced confounders.
- Trained separate models by rank.

#### IPW Results
| Rank                  | E[Y^a]            |
|-----------------------|------------------|
| Police Officer        | 0.8786           |
| Field Training Officer| 0.7351           |
| Detective             | 0.8327           |
| Sergeant              | 0.8174           |
| Lieutenant            | 0.5783           |

## 5. Interpreting Results

### Before
| Rank                   | E[Y^a] |
|------------------------|--------|
| Police Officer         | 0.844  |
| Field Training Officer | 0.763  |
| Investigator           | 0.785  |
| Detective              | 0.800  |
| Sergeant               | 0.772  |
| Lieutenant             | 0.663  |
| Commander              | 0.484  |

### After (Final Estimation)
| Rank                            | E[Y^a] |
|----------------------------------|--------|
| Police Officer                  | 0.819  |
| Field Training Officer          | 0.636  |
| Investigator/Detective          | 0.753  |
| Sergeant                        | 0.728  |
| Lieutenant                      | 0.662  |
| Captain/Commander/Deputy Chief  | 0.479  |

- Strong trend: higher rank → lower probability of punishment.
- Police officers are 1.7x more likely to be penalized than high-ranking officers.
- Notable exception: Field Training Officers (likely due to their unique role).

### Bootstrap CI Summary
- Rank 2 (Police): CI = [0.818, 0.822]
- Highest rank group: CI = [0.432, 0.530]
- Despite wide CI in top ranks, difference is still statistically significant.

### Before vs. After
- Results didn’t change much.
- Bootstrap and confounder additions helped refine estimates.
- Suggests race and gender remain dominant covariates in punishment prediction.

## 6. Synthetic Data
- Generated synthetic binary data for race, gender.
- Modeled "low" vs "high" rank and outcome with descending probability.
- True causal effect = weight(high) - weight(low).

| Sample Size | Expected | Observed | Observed % Error | Naive | Naive % Error |
|-------------|----------|----------|------------------|-------|----------------|
| 1,000       | -0.144   | -0.179   | 0.243            | -0.101| 0.299          |
| 10,000      | -0.168   | -0.166   | 0.0119           | -0.0958| 0.430         |
| 100,000     | -0.151   | -0.149   | 0.0132           | -0.0771| 0.541         |

Backdoor estimator consistently outperformed naive model, especially at larger sample sizes.

## 7. Reflections

### What was interesting?
- Applying causal inference to real data provided depth and challenge.
- Implemented class concepts (backdoor, IPW, bootstrap) in practice.
- Explored DoubleML and DoWhy for model flexibility.

### What was difficult?
- Parsing large and messy datasets.
- Identifying a viable causal question with enough data to support it.
- Handling multiple confounders and model tuning.

### Unaddressed Challenges
- Lacked variables like:
  - Officer disciplinary history
  - Complainant identity/info
  - Rank-specific duties
- Limited sample size and generalizability.

### What’s left to do?
- Expand DoubleML to include more categorical confounders.
- Explore additional models (hierarchical, neural nets).
- Consider alternate outcomes (e.g., final finding).
- Combine with qualitative research (e.g., interviews).


This project provides a data-driven analysis of how rank influences penalization outcomes in sustained complaints. By leveraging causal inference techniques such as backdoor estimation and DoubleML, we ensure robust statistical insights while minimizing bias from confounding variables. Our findings suggest systemic disparities in disciplinary actions based on rank, highlighting the importance of accounting for structural factors in police accountability research.

For further details, refer to the respective Jupyter notebooks and accompanying documentation.

