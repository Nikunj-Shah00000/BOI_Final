# v2_lightgbm_catboost_ensemble

## Overview

This version implements an ensemble-based mule account detection system by combining **LightGBM** and **CatBoost** models. The goal is to improve suspicious account detection performance while minimizing false positives.

Unlike the baseline LightGBM-only approach, this version leverages the strengths of two different gradient boosting algorithms and combines their predictions using a weighted ensemble strategy.

---

# Problem Statement

Detect suspicious mule accounts from banking data and assign a risk score to each account.

Target Variable:

* **F3924 = 0** → Normal Account
* **F3924 = 1** → Suspicious/Mule Account

---

# Dataset Summary

| Metric            | Value   |
| ----------------- | ------- |
| Records           | 9,082   |
| Original Features | 3,925   |
| Final Features    | 4,696   |
| Normal Accounts   | 9,001   |
| Mule Accounts     | 81      |
| Imbalance Ratio   | 111 : 1 |

The dataset is highly imbalanced, making accuracy an unsuitable evaluation metric.

---

# Methodology

## Data Cleaning

The following preprocessing steps were applied:

* Removed index columns
* Removed fully empty columns
* Removed suspected leakage column (`F2230`)
* Removed constant-value features

---

## Feature Engineering

### Date Features

Date columns were transformed into numerical age-based features.

### Encoded Duration Features

Fields such as:

G365D

were split into:

* Type = G
* Duration = 365

### Missing Value Indicators

Missing-value flags were created for partially missing features.

Example:

* F100_missing
* F200_missing

These indicators help capture patterns associated with incomplete account information.

---

# Class Imbalance Handling

Since suspicious accounts are extremely rare, class imbalance was handled using:

scale_pos_weight = 111.12

This forces the models to pay greater attention to the minority class.

---

# Models Used

## LightGBM

Advantages:

* Handles large feature spaces efficiently
* Supports missing values natively
* Provides feature importance
* Fast training and inference

---

## CatBoost

Advantages:

* Strong handling of categorical features
* Robust to overfitting
* Learns complex interactions automatically

---

# Ensemble Strategy

Predictions from both models were combined using a weighted ensemble:

Final Probability =
(0.95 × LightGBM Probability)
+
(0.05 × CatBoost Probability)

The optimal weight was selected automatically by maximizing PR-AUC.

---

# Evaluation Strategy

A 5-Fold Stratified Cross Validation strategy was used.

Evaluation Metrics:

* ROC-AUC
* PR-AUC
* Precision
* Recall
* F1-Score
* Confusion Matrix

Since the dataset is highly imbalanced, PR-AUC, Recall, and F1-score were prioritized over accuracy.

---

# Results

## Model Comparison

| Metric                 | LightGBM Only | Ensemble   |
| ---------------------- | ------------- | ---------- |
| ROC-AUC                | 0.9992        | 0.9923     |
| PR-AUC                 | 0.9351        | **0.9448** |
| F1-Score               | 0.8989        | **0.9581** |
| Mule Accounts Detected | 80 / 81       | 80 / 81    |
| False Positives        | 17            | **6**      |

---

## Best Threshold

0.06

---

## Classification Performance

| Metric    | Value  |
| --------- | ------ |
| Precision | 0.93   |
| Recall    | 0.99   |
| F1-Score  | 0.96   |
| ROC-AUC   | 0.9923 |
| PR-AUC    | 0.9448 |

---

## Confusion Matrix

| Actual / Predicted | Normal | Mule |
| ------------------ | ------ | ---- |
| Normal             | 8995   | 6    |
| Mule               | 1      | 80   |

---

# Key Achievements

* Detected **80 out of 81** mule accounts.
* Reduced false positives from **17 to 6** compared to the baseline model.
* Improved F1-score from **89.89% to 95.81%**.
* Improved PR-AUC from **0.9351 to 0.9448**.
* Generated risk scores for every account.

---

# Generated Outputs

## optimized_mule_account_risk_scores.csv

Contains:

* Actual Label
* LightGBM Probability
* CatBoost Probability
* Ensemble Probability
* Risk Score (0–100)
* Predicted Label

---

## optimized_lgb_feature_importance.csv

Contains:

* Feature Names
* Feature Importance Scores

---

# Conclusion

The LightGBM + CatBoost Ensemble achieved the best overall performance among all tested approaches. The ensemble maintained near-perfect mule account detection while significantly reducing false alarms, making it the strongest candidate for deployment and final submission.

This version is currently the best-performing model in the project.
