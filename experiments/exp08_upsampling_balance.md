# Experiment 08 – Upsampling for Class Balance

## What Changed
Added an upsampling script on the training split to address class imbalance
identified in Exp06 and Exp07. The upsampling script resamples the minority
classes (negative and neutral) to match the size of the majority class
(positive) using random oversampling with replacement.

**Upsampling script (applied to training split only):**
```python
import pandas as pd
from sklearn.utils import resample

def azureml_main(dataframe1=None, dataframe2=None):
    positive = dataframe1[dataframe1['sentiment_class'] == 'positive']
    neutral = dataframe1[dataframe1['sentiment_class'] == 'neutral']
    negative = dataframe1[dataframe1['sentiment_class'] == 'negative']
    
    target_size = len(positive)
    
    neutral_upsampled = resample(neutral,
                                  replace=True,
                                  n_samples=target_size,
                                  random_state=42)
    negative_upsampled = resample(negative,
                                   replace=True,
                                   n_samples=target_size,
                                   random_state=42)
    
    balanced = pd.concat([positive, neutral_upsampled, negative_upsampled])
    balanced = balanced.sample(frac=1, random_state=42).reset_index(drop=True)
    return balanced
```

**Pipeline wiring:**
- Upsampling applied to training split only (left output of Split Data)
- Test split remains untouched (right output of Split Data)
- Stratification column updated from `reviewerRating` → `sentiment_class`
- `reviewerRating` column dropped before Split Data to prevent leakage

## Expected
Improved Macro Precision and Macro Recall compared to Exp06, since the
model will now see balanced class representation during training and
should perform better on minority classes (negative and neutral).

## Results
| Metric | Value |
|---|---|
| Overall Accuracy | 0.4837 |
| Micro Precision | 0.4837 |
| Macro Precision | 0.4042 |
| Micro Recall | 0.4837 |
| Macro Recall | 0.4432 |

## Comparison vs Exp06
| Metric | Exp06 | Exp08 | Delta |
|---|---|---|---|
| Overall Accuracy | 0.7771 | 0.4837 | -0.2934 |
| Macro Precision | 0.2590 | 0.4042 | +0.1452 |
| Macro Recall | 0.3333 | 0.4432 | +0.1099 |

## Observations
- Overall accuracy dropped significantly from 77.7% to 48.3% after dropping
  `reviewerRating` and applying upsampling.
- However Macro Precision and Macro Recall both improved meaningfully,
  indicating the model is now performing more evenly across all three classes
  rather than defaulting to predicting "positive" for everything.
- The drop in overall accuracy is largely attributable to the removal of
  `reviewerRating` as a feature, which was a correlated signal the model
  was relying on in previous experiments.
- Text features alone at 48.3% accuracy represent the true baseline for
  text-only sentiment classification on this dataset.

