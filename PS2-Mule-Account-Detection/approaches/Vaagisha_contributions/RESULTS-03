# Results

## Dataset Statistics

| Metric                           | Value   |
| -------------------------------- | ------- |
| Total Records                    | 9,082   |
| Original Features                | 3,925   |
| Final Features After Engineering | 4,696   |
| Normal Accounts                  | 9,001   |
| Mule Accounts                    | 81      |
| Class Imbalance Ratio            | 111 : 1 |

The dataset was highly imbalanced, with mule accounts representing less than 1% of all observations.

To address this imbalance:

```python
scale_pos_weight = 111.12
```

was applied during training.

---

# Cross Validation Performance

A 5-Fold Stratified Cross Validation strategy was used to ensure reliable evaluation.

| Fold   | ROC-AUC | PR-AUC |
| ------ | ------- | ------ |
| Fold 1 | 1.0000  | 1.0000 |
| Fold 2 | 0.9999  | 0.9888 |
| Fold 3 | 1.0000  | 1.0000 |
| Fold 4 | 1.0000  | 1.0000 |
| Fold 5 | 1.0000  | 1.0000 |

---

# Overall Model Performance

| Metric         | Value  |
| -------------- | ------ |
| ROC-AUC        | 0.9992 |
| PR-AUC         | 0.9351 |
| Best Threshold | 0.06   |
| Best F1-Score  | 0.8989 |

The model achieved near-perfect ranking performance and strong fraud detection capability despite the severe class imbalance.

---

# Classification Performance

| Metric    | Value  |
| --------- | ------ |
| Precision | 0.8247 |
| Recall    | 0.9877 |
| F1-Score  | 0.8989 |

The model successfully detected:

* **80 out of 81 mule accounts**
* Missed only **1 mule account**

---

# Confusion Matrix

| Actual / Predicted | Normal | Mule |
| ------------------ | ------ | ---- |
| Normal             | 8984   | 17   |
| Mule               | 1      | 80   |

### Interpretation

* True Positives = 80
* True Negatives = 8984
* False Positives = 17
* False Negatives = 1

Only 17 legitimate accounts were incorrectly flagged while detecting almost every suspicious account.

---

# Threshold Optimization Analysis

To understand the trade-off between fraud detection and false alerts, multiple thresholds were tested.

| Threshold | Mule Detected | Mule Missed | False Positives | Precision | Recall | F1    |
| --------- | ------------- | ----------- | --------------- | --------- | ------ | ----- |
| 0.10      | 64            | 17          | 5               | 0.928     | 0.790  | 0.853 |
| 0.09      | 64            | 17          | 6               | 0.914     | 0.790  | 0.848 |
| 0.08      | 64            | 17          | 9               | 0.877     | 0.790  | 0.831 |
| 0.07      | 64            | 17          | 10              | 0.865     | 0.790  | 0.826 |
| 0.06      | 80            | 1           | 17              | 0.825     | 0.988  | 0.899 |
| 0.05      | 80            | 1           | 28              | 0.741     | 0.988  | 0.847 |
| 0.04      | 80            | 1           | 47              | 0.630     | 0.988  | 0.769 |
| 0.03      | 80            | 1           | 72              | 0.526     | 0.988  | 0.687 |
| 0.02      | 80            | 1           | 126             | 0.388     | 0.988  | 0.557 |
| 0.01      | 81            | 0           | 281             | 0.224     | 1.000  | 0.366 |

---

# Balanced Detection Mode

### Selected Threshold: 0.06

Results:

* Mule Accounts Detected: 80 / 81
* Recall: 98.76%
* Precision: 82.47%
* F1-Score: 89.89%
* False Positives: 17

This threshold produced the highest F1-score and provided the best balance between fraud detection and investigation workload.

---

# Maximum Security Mode

### Threshold: 0.01

Results:

* Mule Accounts Detected: 81 / 81
* Recall: 100%
* False Negatives: 0
* False Positives: 281

This mode guarantees that no mule account is missed but significantly increases the number of legitimate accounts flagged for review.

---

# Feature Importance Analysis

Top features contributing to mule account detection:

| Rank | Feature |
| ---- | ------- |
| 1    | F3912   |
| 2    | F3898   |
| 3    | F3914   |
| 4    | F3240   |
| 5    | F3799   |
| 6    | F3348   |
| 7    | F1968   |
| 8    | F1165   |
| 9    | F364    |
| 10   | F1813   |

Feature **F3912** emerged as the most influential predictor by a large margin.

---

# Key Findings

* The model achieved **98.76% recall** while maintaining a low false-positive rate.
* Only **1 suspicious account** was missed during validation.
* The threshold optimization study demonstrated that **100% mule detection** is possible at lower thresholds.
* The model generates a continuous **Risk Score (0–100)**, enabling investigators to prioritize accounts based on risk.

---

# Conclusion

This LightGBM-based approach provides a strong baseline for mule account detection. The system combines extensive feature engineering, imbalance handling, threshold optimization, and risk scoring to achieve high fraud-detection performance.

Two operating modes are supported:

1. **Balanced Detection Mode (Threshold = 0.06)**
   Recommended for deployment due to its strong balance of precision and recall.

2. **Maximum Security Mode (Threshold = 0.01)**
   Recommended when missing even a single mule account is unacceptable.

The approach successfully demonstrates how machine learning can be used to detect suspicious banking accounts while providing interpretable risk scores for investigation teams.
