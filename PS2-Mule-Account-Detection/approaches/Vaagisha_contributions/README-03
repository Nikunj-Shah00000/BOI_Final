# v3_lightgbm_threshold_optimization

## Overview

This approach implements a LightGBM-based mule account detection system with threshold optimization and risk scoring.

The primary objective is to identify suspicious mule accounts while handling extreme class imbalance and minimizing missed fraud cases.

Unlike traditional classification systems that rely on a fixed threshold of 0.5, this approach evaluates multiple thresholds to determine the optimal balance between fraud detection performance and false-positive generation.

---

## Problem Statement

Detect suspicious mule accounts from banking data and assign a risk score to each account.

Target Variable:

* **F3924 = 0** → Normal Account
* **F3924 = 1** → Suspicious/Mule Account

---

## Dataset Summary

| Metric            | Value   |
| ----------------- | ------- |
| Records           | 9,082   |
| Original Features | 3,925   |
| Final Features    | 4,696   |
| Normal Accounts   | 9,001   |
| Mule Accounts     | 81      |
| Imbalance Ratio   | 111 : 1 |

The dataset is highly imbalanced, with suspicious accounts representing less than 1% of the total observations.

---

## Methodology

### Data Cleaning

The following preprocessing steps were performed:

* Removed index-like columns
* Removed fully empty columns
* Removed suspected leakage column (`F2230`)
* Removed constant-value features

---

### Feature Engineering

#### Date Feature Transformation

Date columns were converted into numerical age-based features.

Example:

* Original Date → 2024-01-15
* Engineered Feature → Days Since Activity

---

#### Encoded Duration Feature Extraction

Fields containing encoded durations such as:

G365D

were decomposed into:

* Duration Type → G
* Duration Value → 365

This enables the model to learn both categorical and numerical patterns.

---

#### Missing Value Indicators

Missing-value indicator columns were generated for partially missing features.

Examples:

* F100_missing
* F200_missing

These indicators allow the model to learn patterns associated with incomplete account information.

---

### Feature Reduction

To reduce noise and improve learning efficiency:

* Columns with more than 95% missing values were removed.
* Constant-value columns were removed.
* Remaining categorical columns were converted to category type.

---

## Handling Class Imbalance

The dataset exhibits severe imbalance.

To ensure sufficient emphasis on suspicious accounts:

scale_pos_weight = 111.12

was used during model training.

This increases the penalty associated with misclassifying mule accounts.

---

## Model Architecture

### LightGBM

LightGBM was selected because it:

* Handles high-dimensional tabular data efficiently
* Supports missing values natively
* Provides feature importance scores
* Trains quickly on large feature spaces
* Performs well on imbalanced classification problems

---

## Cross Validation Strategy

A 5-Fold Stratified Cross Validation approach was used.

Benefits:

* Preserves class distribution in every fold
* Produces reliable performance estimates
* Reduces overfitting risk

---

## Evaluation Metrics

Because of the extreme class imbalance, accuracy was not used as the primary metric.

The model was evaluated using:

* ROC-AUC
* PR-AUC
* Precision
* Recall
* F1-Score
* Confusion Matrix

Special emphasis was placed on Recall because missing a mule account can be significantly more costly than investigating additional accounts.

---

## Threshold Optimization

Instead of using the default threshold of 0.50, multiple thresholds were evaluated.

Objectives:

* Maximize F1-Score
* Increase Recall
* Minimize False Negatives

Thresholds from:

0.001 → 0.10

were tested to study the trade-off between fraud detection and false-positive generation.

Two operating modes were considered:

### Balanced Detection Mode

Optimized for F1-score and operational efficiency.

### Maximum Security Mode

Optimized for 100% mule account detection, even if additional false positives are generated.

---

## Risk Scoring System

The model outputs probabilities between 0 and 1.

These probabilities are converted into risk scores:

Risk Score = Probability × 100

Examples:

| Probability | Risk Score |
| ----------- | ---------- |
| 0.95        | 95         |
| 0.72        | 72         |
| 0.18        | 18         |

This allows investigators to prioritize accounts based on risk.

---

##

---

## Key Features of this Approach

* Extensive feature engineering
* Missing-value intelligence
* Class imbalance handling
* 5-Fold Stratified Cross Validation
* Threshold optimization
* Risk scoring framework
* Feature importance analysis

---

## Future Improvements

Potential enhancements include:

* CatBoost integration
* LightGBM + CatBoost ensemble
* Cost-sensitive learning
* SHAP explainability analysis
* Graph-based transaction network analysis
* Real-time fraud detection API
* Dashboard for fraud investigators

---

## Conclusion

This approach establishes a strong baseline mule account detection pipeline using LightGBM and threshold optimization. By focusing on Recall, PR-AUC, and F1-score rather than accuracy, the model is better suited for real-world fraud detection scenarios where identifying suspicious accounts is the primary objective.

The resulting system generates both classification decisions and interpretable risk scores, making it practical for financial institutions and fraud investigation teams.
