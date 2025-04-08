CPD Project - Final Report

Group Members

Alina Chen

Yulan Guo

Qinyan Li

Haohan Shi

Code and Documentation

Main Notebooks

cpd_models.ipynb: Contains our final backdoor estimator, the bootstrapped confidence intervals, our DoubleML model using random forest, and our preliminary model with final finding as the outcome.

cpd_synthetic_data.ipynb: Includes the code we used to generate our synthetic data and compares the expected and observed causal effect with a naive estimate at sample sizes of 1,000, 10,000, and 100,000.

Estimation Implementation

Backdoor Estimation (Cell 27 in cpd_models.ipynb): Isolates the causal effect of rank on penalization rates while controlling for confounding effects from race, gender, age, and allegation category.

Bootstrap Confidence Intervals (Cell 30 in cpd_models.ipynb): Verifies the statistical significance of our estimates using 95% confidence intervals.

DoubleML Model (Cell 28 in cpd_models.ipynb): Implements a DoubleML estimator from the econML library, using LassoCV and RandomForest models. Since handling multiclass treatments was challenging, we grouped ranks 2-5 into a "low" rank and 6-10 into a "high" rank.

Model Assumptions

We assume:

Consistency - The treatment effect does not change based on unobserved variables.

Conditional Exchangeability - Given our confounders, rank is independent of potential outcomes.

No Unmeasured Confounding - All necessary confounders are included in the model.

Our counterfactual function is defined as:


where 

Since we built separate models for each rank, the four model parameters represent the coefficients of the four confounders in our logistic regression model:

Rank 2 (‘police officer’): [-0.170, 0.295, 0.539, -0.151]

Rank 8 (highest ranks combined): [-2.48, 0.156, -0.214, -0.224]

Key Findings

Negative correlation between rank and penalization rate: Higher-ranked officers are less likely to be penalized.

No overlapping confidence intervals: Indicates statistically significant differences in penalization likelihood across ranks.

Model flexibility: Different confounder effects based on rank suggest our approach captures non-linear relationships.

Changes Since Last Update

Refined DAG: Replaced appointment date with age to better account for tenure effects and officer experience.

Additional Confounders: Converted allegation category into a binary variable (1 for violent complaints, 0 for nonviolent).

Added Bootstrap Confidence Intervals: Strengthened statistical validity of our results.

Experimented with Complex Models: Implemented DoubleML to reduce categorical variable assumptions and experimented with different machine learning models.

Conclusion

This project provides a data-driven analysis of how rank influences penalization outcomes in sustained complaints. By leveraging causal inference techniques such as backdoor estimation and DoubleML, we ensure robust statistical insights while minimizing bias from confounding variables. Our findings suggest systemic disparities in disciplinary actions based on rank, highlighting the importance of accounting for structural factors in police accountability research.

For further details, refer to the respective Jupyter notebooks and accompanying documentation.

