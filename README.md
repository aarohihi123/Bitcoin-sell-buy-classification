👊 Project Overview

This notebook performs Bitcoin price prediction and signal generation using technical indicators and machine learning with XGBoost. The approach is focused on binary classification: predicting whether a price condition (like being overbought) is met.

1. Load and Explore Data
Imported a historical Bitcoin price dataset.
Parsed datetime and set the index for time-series operations.
Performed basic data cleaning and inspection.

3. Exploratory Data Analysis
Plotted Bitcoin price over time.
Overlaid Bollinger Bands and RSI to visually inspect signals.
Observed price behavior near upper/lower bands and volatility zones.

4. Target Variable Construction
Defined a binary classification target using multiple approaches:

Approach 1: Price increase over next 5 days
Target = (Close.shift(-5) > Close)
Approach 2: Overbought condition
Target = (RSI > 70) or (Price > Upper Bollinger Band)
Approach 3: Price drop threshold
Target = (pct_change_5days < -5%)
➡️ Final implementation used Approach 2 to detect overbought signals.

6. Modeling with XGBoost
Used an XGBoost Classifier to predict the binary target.
Applied TimeSeriesSplit (5-fold) cross-validation to respect time order.
Tuned model with:
early_stopping_rounds
learning_rate = 0.01
eval_metric = 'logloss'

7. Model Evaluation
For each fold, printed:
Classification Report (precision, recall, F1-score)
Accuracy Score
Plotted:
ROC Curve with AUC
Confusion Matrix

8. Conclusion
| Fold    | Accuracy  | Precision (1) | Recall (1) | F1-Score (1) | Support (1) |
| ------- | --------- | ------------- | ---------- | ------------ | ----------- |
| 1       | 0.971     | 1.00          | 0.91       | 0.95         | 141         |
| 2       | 0.980     | 1.00          | 0.93       | 0.96         | 133         |
| 3       | 0.978     | 1.00          | 0.88       | 0.94         | 82          |
| 4       | 0.975     | 1.00          | 0.89       | 0.94         | 104         |
| 5       | 0.984     | 1.00          | 0.93       | 0.96         | 98          |
| **Avg** | **0.978** | **1.00**      | **0.91**   | **0.95**     | —           |

👊The XGBoost classifier achieved very high accuracy (~97.8%) across all time-based validation folds.
👊Class 1 (Overbought Signal) performance was strong:
👊Precision = 1.00: The model makes almost no false overbought predictions.
👊Recall = ~0.91: It successfully catches the majority of real overbought signals.
👊F1-Score = ~0.95: Balanced performance between sensitivity and precision.
👊 Model performs robustly across time, indicating good generalization and no overfitting to specific trends.
👊Evaluation used TimeSeriesSplit to preserve chronological order and avoid data leakage.

The ROC curve confirms that the classifier is not only accurate but also discriminative, meaning it confidently distinguishes between "overbought" and "not overbought" states — essential for a real-world trading signal system.
