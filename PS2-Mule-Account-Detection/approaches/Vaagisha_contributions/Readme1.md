
## Approach: LightGBM-Based Mule Account Risk Scoring

This approach builds a supervised machine learning pipeline to classify bank accounts as normal or suspicious mule accounts using the target column **F3924**. Since the dataset is highly imbalanced, with very few mule accounts compared to normal accounts, the model focuses on fraud-sensitive metrics such as **PR-AUC, recall, and F1-score** instead of accuracy.

First, the dataset is loaded and the target distribution is analyzed. Basic cleaning is performed by removing index-like columns, fully empty columns, and a suspected leakage column. The target variable **F3924** is separated from the input features.

Next, feature engineering is applied to improve model learning. Date-based columns are converted into numerical age features, and coded duration-like fields are split into separate numerical and categorical components. Columns with more than 95% missing values are removed, while missing-value indicators are created for moderately missing columns because missing information itself may be useful in fraud detection. Constant columns are also removed, and categorical columns are converted into category type for LightGBM.

To handle class imbalance, `scale_pos_weight` is calculated as the ratio of normal accounts to mule accounts. This gives higher importance to the minority mule class during training, helping the model detect rare suspicious accounts.

The model used is **LightGBM**, which is well-suited for high-dimensional tabular data with missing values and categorical features. A **5-fold Stratified Cross Validation** strategy is used so that each fold maintains a similar ratio of normal and mule accounts. The model is trained with early stopping to avoid overfitting.

After training, the model generates out-of-fold probability predictions for each account. These probabilities represent the likelihood of an account being suspicious. Instead of using the default threshold of 0.5, multiple thresholds are tested, and the threshold with the best F1-score is selected.

Finally, the model outputs a risk score from 0 to 100 by multiplying the predicted probability by 100. The final output file contains the actual label, predicted risk probability, risk score, and predicted label. Feature importance is also saved to identify which features contributed most to mule account detection.
