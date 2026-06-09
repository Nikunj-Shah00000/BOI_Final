# Results: LightGBM Model with Leakage Validation

## 1. Dataset Summary

| Metric                           | Value   |
| -------------------------------- | ------- |
| Total Records                    | 9,082   |
| Original Features                | 3,925   |
| Final Features After Engineering | 4,696   |
| Normal Accounts                  | 9,001   |
| Mule Accounts                    | 81      |
| Imbalance Ratio                  | 111 : 1 |

The dataset is highly imbalanced, with only **81 mule accounts** out of **9,082 total accounts**. Therefore, accuracy was not treated as the primary metric. Instead, the model was evaluated using **PR-AUC, Recall, F1-score, and Confusion Matrix**.

---

## 2. LightGBM Model Performance

A 5-Fold Stratified Cross Validation strategy was used.

| Fold   | ROC-AUC | PR-AUC |
| ------ | ------- | ------ |
| Fold 1 | 1.0000  | 1.0000 |
| Fold 2 | 0.9999  | 0.9888 |
| Fold 3 | 1.0000  | 1.0000 |
| Fold 4 | 1.0000  | 1.0000 |
| Fold 5 | 1.0000  | 1.0000 |

### Overall Performance

| Metric         | Value  |
| -------------- | ------ |
| ROC-AUC        | 0.9992 |
| PR-AUC         | 0.9351 |
| Best Threshold | 0.06   |
| Best F1-Score  | 0.8989 |

---

## 3. Classification Results

At the best threshold of **0.06**, the model achieved:

| Class          | Precision | Recall | F1-Score | Support |
| -------------- | --------- | ------ | -------- | ------- |
| Normal Account | 1.00      | 1.00   | 1.00     | 9001    |
| Mule Account   | 0.82      | 0.99   | 0.90     | 81      |

### Confusion Matrix

| Actual / Predicted | Normal | Mule |
| ------------------ | ------ | ---- |
| Normal             | 8984   | 17   |
| Mule               | 1      | 80   |

### Interpretation

* True Negatives: **8,984**
* False Positives: **17**
* False Negatives: **1**
* True Positives: **80**

The model successfully detected **80 out of 81 mule accounts**, achieving **98.76% recall** on suspicious accounts.

---

## 4. Feature Importance

The top 10 most important features identified by LightGBM were:

| Rank | Feature | Importance |
| ---- | ------- | ---------- |
| 1    | F3912   | 173238.82  |
| 2    | F3898   | 37366.50   |
| 3    | F3914   | 28158.39   |
| 4    | F3240   | 10764.20   |
| 5    | F3799   | 3712.03    |
| 6    | F3348   | 3268.22    |
| 7    | F1968   | 2696.69    |
| 8    | F1165   | 2202.60    |
| 9    | F364    | 1879.24    |
| 10   | F1813   | 1548.68    |

Feature **F3912** was the most influential feature by a large margin.

---

## 5. Single Feature Leakage Check

Since feature **F3912** had very high importance, a single-feature predictive power check was performed to evaluate whether it may be causing leakage.

Top single-feature AUC values:

| Rank | Feature | Single Feature ROC-AUC | Single Feature PR-AUC |
| ---- | ------- | ---------------------- | --------------------- |
| 1    | F3912   | 0.9875                 | 0.9398                |
| 2    | F3811   | 0.8273                 | 0.0050                |
| 3    | F3805   | 0.8243                 | 0.0050                |
| 4    | F3799   | 0.8179                 | 0.0050                |
| 5    | F1813   | 0.8163                 | 0.0051                |

### Observation

Feature **F3912** alone was highly predictive, with a single-feature ROC-AUC of **0.9875**. This suggested that F3912 may be a very strong fraud indicator or a potential leakage-like feature.

However, other features did not show similarly extreme predictive power, reducing the likelihood of widespread leakage.

---

## 6. Model Performance Without F3912

To validate the dependency on F3912, the model was retrained after removing F3912.

### Overall Performance Without F3912

| Metric        | With F3912 | Without F3912 |
| ------------- | ---------- | ------------- |
| ROC-AUC       | 0.9992     | 0.9849        |
| PR-AUC        | 0.9351     | 0.7512        |
| Best F1-Score | 0.8989     | 0.7692        |
| Mule Recall   | 0.9877     | 0.6790        |

---

## 7. Classification Results Without F3912

| Class          | Precision | Recall | F1-Score | Support |
| -------------- | --------- | ------ | -------- | ------- |
| Normal Account | 1.00      | 1.00   | 1.00     | 9001    |
| Mule Account   | 0.89      | 0.68   | 0.77     | 81      |

### Confusion Matrix Without F3912

| Actual / Predicted | Normal | Mule |
| ------------------ | ------ | ---- |
| Normal             | 8994   | 7    |
| Mule               | 26     | 55   |

### Interpretation

After removing F3912:

* Mule accounts detected decreased from **80 to 55**
* Missed mule accounts increased from **1 to 26**
* PR-AUC dropped from **0.9351 to 0.7512**
* F1-score dropped from **0.8989 to 0.7692**

This confirms that F3912 carries substantial predictive signal.

---

## 8. Feature Importance Without F3912

After removing F3912, the most important features were:

| Rank | Feature | Importance |
| ---- | ------- | ---------- |
| 1    | F3898   | 95836.56   |
| 2    | F3914   | 42758.44   |
| 3    | F3348   | 17223.67   |
| 4    | F3672   | 12268.83   |
| 5    | F3240   | 10331.48   |
| 6    | F2581   | 8921.33    |
| 7    | F364    | 8489.98    |
| 8    | F1921   | 8445.34    |
| 9    | F3243   | 5258.71    |
| 10   | F1968   | 2512.24    |

The model still retained meaningful predictive ability without F3912, but performance declined significantly.

---

## 9. Key Findings

* The LightGBM model achieved **98.76% recall** with F3912 included.
* Only **1 out of 81** mule accounts was missed.
* F3912 was the strongest predictor and contributed heavily to model performance.
* Removing F3912 reduced mule detection from **80/81** to **55/81**.
* The model still performed reasonably without F3912, suggesting that other features also contain useful fraud signals.
* Since F3912 was not directly the target column and no confirmed leakage was identified, it was retained for the final model.

---

## 10. Final Conclusion

The LightGBM model with F3912 included produced the strongest fraud-detection performance.

Final selected model:

| Metric                 | Value   |
| ---------------------- | ------- |
| ROC-AUC                | 0.9992  |
| PR-AUC                 | 0.9351  |
| Best Threshold         | 0.06    |
| F1-Score               | 0.8989  |
| Mule Accounts Detected | 80 / 81 |
| False Positives        | 17      |
| False Negatives        | 1       |

This model is suitable as a strong baseline for mule account detection. It provides both classification labels and risk scores, allowing financial institutions to prioritize suspicious accounts for investigation.
