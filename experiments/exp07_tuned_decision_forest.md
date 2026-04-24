# Experiment 07 – Tuned Multiclass Decision Forest

## What Changed
Adjusted hyperparameters of the Multiclass Decision Forest component
to explore whether increased model complexity would improve accuracy.

| Setting | Exp06 Value | Exp07 Value |
|---|---|---|
| Number of decision trees | 100 | 200 |
| Minimum samples per leaf | 1 | 5 |

All other pipeline components remain identical to Exp06.

## Expected
Modest accuracy improvement due to increased number of trees providing
more robust ensemble predictions, and higher minimum samples per leaf
reducing overfitting on minority classes.

## Results
| Metric | Value |
|---|---|
| Overall Accuracy | 0.7771 |
| Micro Precision | 0.7771 |
| Macro Precision | 0.2590 |
| Micro Recall | 0.7771 |
| Macro Recall | 0.3333 |

## Comparison vs Exp06
| Metric | Exp06 | Exp07 | Delta |
|---|---|---|---|
| Overall Accuracy | 0.7771 | 0.7771 | 0.0000 |
| Macro Precision | 0.2590 | 0.2590 | 0.0000 |
| Macro Recall | 0.3333 | 0.3333 | 0.0000 |

## Observations
- Hyperparameter tuning produced **zero measurable improvement** results
  are identical to Exp06 to 6 decimal places.
- This is a meaningful negative result. It confirms that the bottleneck
  is not model complexity or tree count, but rather the underlying
  class distribution and label boundaries.
- The persistent low Macro Precision (0.259) and Macro Recall (0.333)
  despite tuning further confirms that class imbalance is the primary
  issue — the model continues to over-predict "positive" regardless of
  tree settings.
- Hyperparameter tuning is not the right lever to pull here.
