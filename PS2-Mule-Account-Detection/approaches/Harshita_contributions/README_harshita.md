# Bank of India Hackathon 2026
## PS2 - AI/ML Mule Account Detection

### Results
- AUC-ROC : 0.939
- Recall  : 81% (threshold 0.3)
- Recall  : 100% (threshold 0.2)
- Mules caught : 16/16

### Model
- XGBoost Classifier
- SMOTE for class imbalance
- SHAP explainability

### Key Features Used
F3891, F2956, F670, F115, F3887, F2582

### How to Run
1. Open BOI_Mule_Detection.ipynb in Google Colab
2. Upload DataSet.csv when prompted
3. Run all cells
