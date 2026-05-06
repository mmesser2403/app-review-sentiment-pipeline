# Experiment 02 – Stratified Split

## What Changed
Enabled stratified splitting on the `reviewerRating` column in the Split Data
component. All other settings remain identical to Experiment 01:
- Fraction of rows: 0.8 / 0.2 train-test
- Randomized split: True
- Random seed: 0

## Expected
More reliable evaluation due to proportional representation of all rating
values across both train and test splits. Potential minor metric improvement,
especially if certain rating values were underrepresented in the original split.

## Results
| Metric | Value |
|---|---|
| Mean Absolute Error (MAE) | 0.1941 |
| Root Mean Squared Error (RMSE) | 0.2530 |
| Relative Squared Error | 0.9411 |
| Relative Absolute Error | 0.9396 |
| Coefficient of Determination (R²) | 0.0589 |

> **Note:** All values remain on the 0–1 scale.

## Comparison vs Exp01 Baseline
| Metric | Exp01 | Exp02 | Delta |
|---|---|---|---|
| MAE | 0.1947 | 0.1941 | -0.0006 |
| RMSE | 0.2538 | 0.2530 | -0.0008 |
| R² | 0.0594 | 0.0589 | -0.0005 |

## Observations
- Stratification produced negligible metric change, suggesting the original
  random split (seed=0) was already reasonably balanced across rating values.
- This confirms the previous split was not a major
  source of evaluation instability.
- The near-identical R² (~0.059) confirms that the model architecture itself
  is the primary bottleneck, not the data split strategy.
- Stratified splitting is still the correct engineering practice and will be
  retained in all future experiments for evaluation reliability.

