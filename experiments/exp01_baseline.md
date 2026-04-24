# Experiment 01 – Baseline Evaluation

## What Changed
This is the unmodified baseline pipeline

## Expected
Poor performance. Linear regression on a 0–1 scale with no stratification
is a weak setup for this problem. We expected low R² and high relative error.

## Results
| Metric | Value |
|---|---|
| Mean Absolute Error (MAE) | 0.1947 |
| Root Mean Squared Error (RMSE) | 0.2538 |
| Relative Squared Error | 0.9406 |
| Relative Absolute Error | 0.9385 |
| Coefficient of Determination (R²) | 0.0594 |

> **Note:** All values are on the 0–1 scale. To interpret in the context of a
> 1–10 rating scale, multiply MAE and RMSE by 10 (MAE ≈ 1.95, RMSE ≈ 2.54).

## Observations
- The R² of **0.059** is the most telling result, the model explains only ~6%
  of the variance in ratings. This is barely better than predicting the mean
  rating every time.
- Relative Squared Error of 0.94 confirms the model's predictions are almost
  as erratic as a naive baseline.
- Output is on the 0–1 scale, not the required 1–10 scale.
- Stratification is disabled, meaning rating distributions may be uneven
  across train/test splits.
- No confidence scores are generated.
- This run establishes the comparison baseline for all future experiments.
  All subsequent experiments will reference these numbers to quantify improvement.
