# Approach: Ensemble-Based Mule Account Detection and Risk Scoring

## Objective

The objective of this approach is to identify suspicious mule accounts using an ensemble of machine learning models and generate a risk score that can assist financial institutions in prioritizing investigations.

The target variable is **F3924**, where:

* 0 → Normal Account
* 1 → Suspicious/Mule Account

---

# Dataset Overview

Initial Dataset:

* Records: 9,082
* Features: 3,925

Target Distribution:

* Normal Accounts: 9,001
* Mule Accounts: 81

The dataset is highly imbalanced with approximately a **111:1 ratio**, making traditional accuracy an unreliable evaluation metric.

---

# Data Preprocessing

## Basic Cleaning

The following preprocessing steps were applied:

* Removed index-like columns (e.g., `Unnamed: 0`)
* Removed suspected leakage or identifier columns (`F2230`)
* Removed fully empty columns
* Removed constant-value features

---

## Feature Engineering

### Date Transformation

Date columns were converted into numerical features representing account age or activity age.

Example:

* Original Date → 2024-01-15
* Engineered Feature → Days Since Activity

---

### Encoded Duration Features

Fields such as:

G365D

were split into:

* Type = G
* Duration = 365

This enables the model to learn both categorical and numerical patterns.

---

### Missing Value Intelligence

Instead of simply filling missing values, missingness indicators were generated.

Example:

* F100_missing
* F200_missing

This allows the model to learn whether the absence of information itself is indicative of suspicious behavior.

---

## Feature Reduction

To reduce noise:

* Features with more than 95% missing values were removed.
* Constant columns were removed.
* Remaining categorical features were encoded using LightGBM and CatBoost compatible formats.

Final Dataset:

* Records: 9,082
* Features: 4,696

---

# Class Imbalance Handling

Because mule accounts represent less than 1% of the dataset, class imbalance was addressed using:

scale_pos_weight = 111.12

This increases the importance of suspicious accounts during model training.

---

# Model Architecture

## Model 1: LightGBM

LightGBM was selected because:

* Handles high-dimensional tabular data efficiently
* Supports missing values natively
* Works effectively on imbalanced datasets
* Provides feature importance for interpretability

---

## Model 2: CatBoost

CatBoost was added because:

* Handles categorical features exceptionally well
* Learns complex feature interactions
* Often captures patterns missed by gradient boosting models

---

## Ensemble Strategy

Both models generate probability predictions.

A weighted ensemble is created:

Final Probability =
(w × LightGBM Probability)
+
((1 - w) × CatBoost Probability)

Multiple ensemble weights were tested, and the weight producing the highest PR-AUC was selected.

---

# Cross Validation

To ensure robust evaluation, 5-Fold Stratified Cross Validation was used.

Benefits:

* Preserves class distribution in every fold
* Reduces overfitting risk
* Produces more reliable performance estimates

---

# Threshold Optimization

Instead of using the default classification threshold of 0.5, thresholds from 0.01 to 0.99 were evaluated.

The threshold producing the highest F1-score was selected as the final operating point.

This allows better balancing between:

* Fraud detection rate (Recall)
* False alarm rate (Precision)

---

# Risk Scoring System

The ensemble outputs a probability value between 0 and 1.

This probability is transformed into a Risk Score:

Risk Score = Probability × 100

Examples:

* 0.95 → Risk Score 95
* 0.72 → Risk Score 72
* 0.08 → Risk Score 8

This enables investigators to prioritize high-risk accounts.

---

# Outputs Generated

The pipeline generates:

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

Used for model interpretability and fraud pattern analysis.

---

# Expected Benefits

This ensemble approach aims to:

* Improve mule account detection accuracy
* Reduce missed suspicious accounts
* Generate explainable risk scores
* Support investigator decision-making
* Provide a scalable fraud detection framework suitable for real-world banking systems
