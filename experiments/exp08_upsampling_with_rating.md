# Experiment 08 – Upsampling with Class Balancing and Full Feature Set
 
## What Changed
Added an upsampling script on the training split to address class imbalance identified in Exp06 and Exp07. The upsampling script resamples the minority classes (negative and neutral) to match the size of the majority class (positive) using random oversampling with replacement. Stratification was updated from `reviewerRating` to `sentiment_class` and all sentiment feature engineering from Exp05 was retained.
 
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
- `reviewerRating` retained as a feature column alongside all sentiment features
 
## Expected
Improved Macro Precision and Macro Recall compared to Exp06 and Exp07, since the model will now see balanced class representation during training and should perform better on minority classes (negative and neutral), while retaining the numeric rating signal from `reviewerRating`.
 
## Results
| Metric | Value |
|---|---|
| Overall Accuracy | 0.9269 |
| Micro Precision | 0.9269 |
| Macro Precision | 0.8856 |
| Micro Recall | 0.9269 |
| Macro Recall | 0.9011 |
 
## Comparison vs Previous Experiments
| Experiment | Accuracy | Macro Precision | Macro Recall |
|---|---|---|---|
| Exp06 Multiclass Classification | 0.7771 | 0.2590 | 0.3333 |
| Exp07 Tuned Decision Forest | 0.7771 | 0.2590 | 0.3333 |
| **Exp08 Upsampling + Full Feature Set** | **0.9269** | **0.8856** | **0.9011** |
 
## Observations
- Overall accuracy improved dramatically to 92.7% with upsampling applied to the training split only.
- Macro Precision of 0.886 and Macro Recall of 0.901 are both significantly improved over Exp06 and Exp07, indicating the model is now performing well across all three sentiment classes and not just defaulting to predicting positive for everything.
- The combination of upsampling and the full feature set produced the strongest results across all metrics in the project.
- This experiment represents the best performing pipeline configuration and will be retained as the final registered model.
- It should be noted that in a pure text-only deployment scenario, `reviewerRating` would not be available at inference time. Future iterations could explore removing this feature to test text-only prediction performance.
 
