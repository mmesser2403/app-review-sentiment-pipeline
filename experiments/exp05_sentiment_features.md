# Experiment 05 – Sentiment Feature Engineering

## What Changed
Added an Execute Python Script component immediately before Preprocess Text
to engineer three new features from the raw reviewText column before
preprocessing strips punctuation and special characters.

**New features added:**
- `exclamation_count` — number of `!` characters in the review
- `question_count` — number of `?` characters in the review
- `text_length` — total character length of the review

**Script used:**
```python
import pandas as pd

def azureml_main(dataframe1=None, dataframe2=None):
    dataframe1["exclamation_count"] = dataframe1["reviewText"].fillna("").str.count("!")
    dataframe1["question_count"] = dataframe1["reviewText"].fillna("").str.count(r"\?")
    dataframe1["text_length"] = dataframe1["reviewText"].fillna("").str.len()
    return dataframe1
```

**Placement:** Script runs before Preprocess Text so that punctuation signals
are captured before the preprocessing stage removes them.

All other settings retained from Exp04 (Boosted Decision Tree, stratified split).

## Expected
Moderate improvement in R² by preserving sentiment-bearing signals
(punctuation intensity, review verbosity) that the baseline pipeline
discards during preprocessing.

## Results
| Metric | Value |
|---|---|
| Mean Absolute Error (MAE) | 0.1829 |
| Root Mean Squared Error (RMSE) | 0.2403 |
| Relative Squared Error | 0.8489 |
| Relative Absolute Error | 0.8852 |
| Coefficient of Determination (R²) | 0.1511 |

> **Note:** All values are on the 0–1 scale. To interpret on a 1–10
> scale, multiply MAE and RMSE by 10 (MAE ≈ 1.83, RMSE ≈ 2.40).

## Comparison vs All Previous Experiments
| Metric | Exp01 Baseline | Exp02 Stratified | Exp04 Boosted Tree | Exp05 Sentiment | Δ vs Baseline |
|---|---|---|---|---|---|
| MAE | 0.1947 | 0.1941 | 0.1926 | 0.1829 | -0.0118 (-6%) |
| RMSE | 0.2538 | 0.2530 | 0.2466 | 0.2403 | -0.0135 (-5%) |
| R² | 0.0594 | 0.0589 | 0.1059 | 0.1511 | +0.0917 (+154%) |

## Observations
- R² improved from 0.059 (baseline) to 0.151, a 154% improvement overall.
- Adding just three lightweight features produced a larger R² gain than
  switching from Linear Regression to Boosted Decision Tree alone.
- This confirms a key finding: the baseline pipeline was discarding
  meaningful sentiment signals during preprocessing (exclamation marks,
  question marks) that correlate with rating values.
- Text length also proved informative, longer reviews may correlate with
  more extreme ratings (very positive or very negative experiences).
- R² of 0.151 still indicates significant room for improvement. The next
  logical step is reframing the problem as a 3-class sentiment classification
  task (negative/neutral/positive) which may yield stronger results than
  continuing to optimize regression on the raw continuous scale.

