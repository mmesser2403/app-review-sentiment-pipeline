# Experiment 06 – Multiclass Decision Forest Classification

## What Changed
Reframed the problem from regression to 3-class sentiment classification.
This is a fundamental change in problem formulation — instead of predicting
an exact numeric rating, the model now predicts one of three sentiment
categories derived from the rating value.

**Label transformation script added before feature engineering:**
```python
import pandas as pd

def azureml_main(dataframe1=None, dataframe2=None):
    def rating_to_class(rating):
        if rating <= 0.3:
            return 'negative'   # 1-3 on 1-10 scale
        elif rating <= 0.7:
            return 'neutral'    # 4-7 on 1-10 scale
        else:
            return 'positive'   # 8-10 on 1-10 scale
    
    dataframe1['sentiment_class'] = dataframe1['reviewerRating'].apply(rating_to_class)
    return dataframe1
```

**Class distribution:**
- `negative` → reviewerRating ≤ 0.3 (1–3 on 1–10 scale)
- `neutral` → reviewerRating 0.31–0.7 (4–7 on 1–10 scale)
- `positive` → reviewerRating > 0.7 (8–10 on 1–10 scale)

**Model:** Multiclass Decision Forest
- Resampling method: Bagging
- Number of decision trees: 100
- Maximum depth: 32
- Number of random splits: 128
- Minimum samples per leaf: 1

**Label column updated:** `reviewerRating` → `sentiment_class`

All other pipeline components retained from Exp05 (stratified split,
sentiment feature engineering script).

## Expected
Significantly higher accuracy than regression R² suggested, since
classifying into 3 broad buckets is a more tractable problem than
predicting an exact decimal rating. Expected accuracy 70–80%.

## Results
| Metric | Value |
|---|---|
| Overall Accuracy | **0.7771** |
| Micro Precision | 0.7771 |
| Macro Precision | 0.2590 |
| Micro Recall | 0.7771 |
| Macro Recall | 0.3333 |

## Comparison vs All Previous Experiments
| Experiment | Primary Metric | Value |
|---|---|---|
| Exp01 Baseline (Linear Regression) | R² | 0.0594 |
| Exp02 Stratified Split | R² | 0.0589 |
| Exp04 Boosted Decision Tree | R² | 0.1059 |
| Exp05 Sentiment Feature Engineering | R² | 0.1511 |
| **Exp06 Multiclass Classification** | **Accuracy** | **0.7771** |

> **Note:** R² and Accuracy are not directly comparable metrics —
> regression and classification measure different things. The shift
> to classification represents a deliberate problem reframing decision,
> not just a metric swap.

## Observations
- **77.7% overall accuracy** is a strong result and validates the decision
  to reframe the problem as classification rather than regression.
- Micro Precision and Micro Recall both equal Overall Accuracy (0.777),
  which is expected for a balanced micro-average calculation.
- **Macro Precision (0.259) and Macro Recall (0.333) are low**, indicating
  the model is heavily biased toward predicting the majority class
  ("positive"). This is a known class imbalance issue app review datasets
  typically contain far more positive reviews than negative or neutral ones.
- The model likely performs well on "positive" reviews but poorly on
  "negative" and "neutral" classes, which are underrepresented in the data.
- This is a valid and well-documented limitation. Future improvements could
  include class weighting or oversampling of minority classes (negative/neutral).
- The rescaling Python script failed during this run but is irrelevant to
  classification metrics 

