# Experiment 04 – Boosted Decision Tree Regression

## What Changed
Replaced Linear Regression with Boosted Decision Tree Regression.
All other pipeline components remain identical to Experiment 02.

**Model settings:**
- Maximum leaves per tree: 20
- Minimum samples per leaf: 10
- Learning rate: 0.1
- Number of trees: 100
- Stratified split: True (retained from Exp02)

## Expected
Significant improvement in R² due to the model's ability to capture
non-linear relationships between text features and ratings. Boosted
Decision Trees handle complex feature interactions better than
Ordinary Least Squares Linear Regression.

## Results
| Metric | Value |
|---|---|
| Mean Absolute Error (MAE) | 0.1926 |
| Root Mean Squared Error (RMSE) | 0.2466 |
| Relative Squared Error | 0.8941 |
| Relative Absolute Error | 0.9323 |
| Coefficient of Determination (R²) | 0.1059 |

> **Note:** All values are on the 0–1 scale. To interpret on a 1–10
> scale, multiply MAE and RMSE by 10 (MAE ≈ 1.93, RMSE ≈ 2.47).

## Comparison vs All Previous Experiments
| Metric | Exp01 Baseline | Exp02 Stratified | Exp04 Boosted Tree | Δ vs Baseline |
|---|---|---|---|---|
| MAE | 0.1947 | 0.1941 | 0.1926 | -0.0021 |
| RMSE | 0.2538 | 0.2530 | 0.2466 | -0.0072 |
| R² | 0.0594 | 0.0589 | 0.1059 | +0.0465 (+79%) |

## Observations
- R² nearly doubled from 0.059 → 0.106, confirming that Boosted Decision
  Tree is a meaningfully better model for this problem than Linear Regression.
- Despite the improvement, R² of 0.106 indicates the model still only explains
  ~11% of rating variance. This suggests the bottleneck is now the **feature
  representation** (bag-of-words from Preprocess Text) rather than the model.
- MAE and RMSE improvements are modest, suggesting the model still struggles
  with the full 0–1 continuous scale.
- This points toward a problem reframing approach — grouping ratings into
  sentiment classes (negative/neutral/positive) may yield stronger results
  than continuing to optimize regression on the raw scale.

