# Experiment 09 – Upsampling with ReviewerRating Feature Restored

## What Changed
Restored `reviewerRating` as a feature column (removed the drop line from
Script 1) while retaining all other changes from Exp08:
- Upsampling on training split only
- Stratification on `sentiment_class`
- All sentiment feature engineering from Exp05 retained

## Expected
Higher accuracy than Exp08 by restoring the numeric rating signal
alongside balanced class representation from upsampling.

## Results
| Metric | Value |
|---|---|
| Overall Accuracy | 0.9269 |
| Micro Precision | 0.9269 |
| Macro Precision | 0.8856 |
| Micro Recall | 0.9269 |
| Macro Recall | 0.9011 |

## Comparison vs All Experiments
| Experiment | Accuracy | Macro Precision | Macro Recall |
|---|---|---|---|
| Exp06 Multiclass Classification | 0.7771 | 0.2590 | 0.3333 |
| Exp07 Tuned Decision Forest | 0.7771 | 0.2590 | 0.3333 |
| Exp08 Upsampling (no reviewerRating) | 0.4837 | 0.4042 | 0.4432 |
| **Exp09 Upsampling + reviewerRating** | **0.9269** | **0.8856** | **0.9011** |

## Observations
- Overall accuracy improved dramatically to 92.7% with upsampling and
  `reviewerRating` restored as a feature.
- Macro Precision (0.886) and Macro Recall (0.901) are both significantly
  improved over Exp06, indicating the model is now performing well across
  all three sentiment classes, not just the majority "positive" class.
- The combination of upsampling and `reviewerRating` as a feature produced
  the strongest results across all metrics in the project.
- This experiment represents the best performing pipeline configuration
  and will be retained as the final registered model.
- It should be noted that in a pure text-only deployment scenario, reviewerRating would not be available at inference time. Future iterations could explore removing this feature to test text-only prediction performance
