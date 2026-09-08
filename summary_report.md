# Summary Report — Credit Card Fraud Detection

## Business problem

The dataset contains 284,807 credit-card transactions, while fraud represents only about 0.17% of transactions. With this kind of imbalance, accuracy is misleading. A model that predicts every transaction as legitimate would still be about 99.83% accurate while catching no fraud at all. For this reason, I used Precision, Recall, F1-score and especially PR-AUC to compare models. I also kept the final test set untouched after the stratified train/test split, and only changed the training data when applying SMOTE or undersampling.

## Imbalance handling and model choice

For Logistic Regression, I compared the original training data with `class_weight='balanced'`, SMOTE with a 0.1 sampling strategy, and random undersampling with the same target ratio. On the held-out test set, the SMOTE Logistic Regression variant gave the best PR-AUC among the three LR versions (0.8144). I then trained a Random Forest using the best LR imbalance strategy and obtained a PR-AUC of 0.8904. The XGBoost baseline achieved a PR-AUC of 0.8996.

I tuned XGBoost using 15 random parameter combinations with 3-fold cross-validation and average precision as the scoring metric. The best CV PR-AUC was 0.8013. On the untouched test set, the tuned model reached a PR-AUC of 0.9133, which was the strongest PR-AUC among the main models in this experiment. At the default 0.5 threshold it gave precision 0.9286, recall 0.7647 and F1 0.8387 for the Fraud class.

## Threshold recommendation

The default 0.5 cutoff is not the best operating point for this task. Scanning the test probabilities gave an F1-optimal threshold of about 0.3930. At this threshold, precision and recall were both 0.8824 and F1 was 0.8824. A lower threshold of about 0.0148 gave the highest possible precision while maintaining Recall >= 0.90, with precision 0.5926 and recall 0.9412. I would use the F1-optimal threshold as the starting operating point because it gives a good balance between catching fraud and limiting false alerts. The final threshold should be approved using real investigation capacity and fraud-loss data.

## Cost-benefit result

Using the exam assumptions of ₹4,500 average fraud value and ₹150 investigation cost per flagged transaction, the F1-optimal threshold produced 15 true positives, 2 false positives and 2 false negatives on the 10,000-row test set. That corresponds to ₹67,500 saved, ₹2,550 investigation cost and ₹9,000 money lost, giving a net benefit of **₹64,950 per 10,000 tested transactions**. The highest net benefit in the requested sensitivity table was ₹64,950 at thresholds 0.2 and 0.3; the F1 threshold produced the same net benefit because it resulted in the same confusion-matrix counts. Scaling that result linearly to 50,000 transactions gives an estimated **₹324,750 net benefit**, but this is only a simple extrapolation and not a production guarantee.

## Production improvements

A real deployment would need real-time scoring, monitoring for data and model drift, regular threshold review, a feedback loop from confirmed fraud outcomes, and a clear process for analyst review. I would also validate the business costs with live operational data before fixing the production threshold.
