# 💳 Credit Card Fraud Detection
### Supervised Learning • Imbalanced Classification • Threshold Optimisation

> A practical machine-learning project for identifying fraudulent credit-card transactions while keeping false alarms under control.

---

## 🚦 What is this project about?

Credit-card fraud detection is a **highly imbalanced classification problem**: genuine transactions are much more common than fraudulent ones.

That creates a simple problem with a not-so-simple solution — a model can achieve very high accuracy while still missing the fraud cases that actually matter.

This project focuses on three things:

**1. Handle the imbalance** → compare SMOTE and random undersampling.  
**2. Build and compare models** → Logistic Regression, Random Forest and XGBoost.  
**3. Choose a useful decision threshold** → instead of automatically treating `0.50` as the cutoff.

---

## 🎯 Project goal

Build a supervised-learning fraud detector and select an operating threshold using **Precision, Recall, F1-score, PR-AUC and a simple business cost-benefit calculation**.

The notebook follows the exam workflow and keeps the final test set untouched during model preparation.

---

## 📂 Dataset

This project uses the **Kaggle Credit Card Fraud Detection** dataset.

🔗 Dataset: https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud

For the practical exam, the notebook first creates a **stratified sample of 50,000 rows** and then makes an **80/20 stratified train-test split**.

> `creditcard.csv` is not stored in this submission package because the dataset was provided separately for the practical exam.

Place it in the project folder before running the notebook.

---

## 🧠 What was done?

```text
creditcard.csv
     │
     ▼
50,000-row stratified sample
     │
     ▼
80/20 train-test split
     │
     ├──────────────► Test set kept untouched
     │
     ▼
Preprocessing + class balancing
     │
     ├── SMOTE
     └── Random Undersampling
     │
     ▼
Model comparison
     │
     ├── Logistic Regression
     ├── Random Forest
     └── XGBoost
     │
     ▼
Precision-Recall evaluation
     │
     ▼
Threshold tuning
     │
     ▼
Business cost-benefit check
     │
     ▼
Final saved pipeline
```

---

## 📊 Final test-set result

The tuned **XGBoost** model produced the best PR-AUC in this run.

| Metric | Result |
|---|---:|
| **PR-AUC** | **0.9133** |
| Selected threshold | **0.3930** |
| Precision | 0.8824 |
| Recall | 0.8824 |
| F1-score | 0.8824 |
| True Positives | 15 |
| False Positives | 2 |
| False Negatives | 2 |
| Net benefit | **₹64,950** |

The selected threshold comes from **F1 optimisation**. On this particular test set, the same final predictions were also produced at thresholds `0.2` and `0.3`, so the calculated net benefit remained the same.

---

## 💰 Why threshold tuning matters

A fraud system does not only ask:

> “Is this transaction fraud?”

It also asks:

> “How confident should we be before we flag it?”

A lower threshold usually catches more fraud but can increase false positives. A higher threshold can reduce false alarms but may allow more fraudulent transactions through.

For that reason, the notebook checks several thresholds and compares the resulting business outcome instead of blindly using `0.50`.

---

## 🧪 Models compared

### Logistic Regression
A simple baseline that is fast and easy to interpret.

### Random Forest
A tree-based ensemble that can model non-linear relationships and interactions.

### XGBoost ⭐
A boosted-tree model that performed best on PR-AUC in this run and was used for the final saved pipeline.

---

## 📁 Project files

```text
credit-fraud-detection-submission/
│
├── 📓 FraudDetection_SupervisedLearning.ipynb
├── 🤖 fraud_detection_model.pkl
├── 📝 summary_report.md
├── 📋 requirements.txt
```

### Notebook
`FraudDetection_SupervisedLearning.ipynb`

The complete practical workflow with executed outputs, including EDA, imbalance handling, model comparison, PR curves, threshold tuning and cost-benefit analysis.

### Saved model
`fraud_detection_model.pkl`

A scikit-learn `Pipeline` containing the preprocessing and tuned XGBoost model. The selected decision threshold is stored as `threshold_` on the pipeline.

### Report
`summary_report.md`

A short summary of the approach, results, recommendation and business impact.

### Requirements
`requirements.txt`

The Python packages needed to run the project.


A natural speaking guide for the required practical-exam walkthrough.

---

## ▶️ How to run

### 1. Install the dependencies

```bash
pip install -r requirements.txt
```

### 2. Add the dataset

Put `creditcard.csv` in the same folder as the notebook.

### 3. Open the notebook

```text
FraudDetection_SupervisedLearning.ipynb
```

### 4. Run from top to bottom

The notebook creates the stratified sample, prepares the training data, compares the models, tunes the threshold and evaluates the held-out test set.

### 5. Saved model

The notebook saves:

```text
fraud_detection_model.pkl
```

---

## 🔒 Important: avoiding test-set leakage

The final test set is **not resampled**.

SMOTE and random undersampling are applied only to the **training split**. The held-out test data is used for final evaluation so that the reported performance remains a fair check of the model.

---

## 🧾 Business interpretation

Using the project cost assumptions, the selected operating point gives a calculated **net benefit of ₹64,950 on the 10,000-row test set**.

The same calculation gives a simple linear estimate of about **₹324,750 per 50,000 transactions** under the same assumptions.

These values are scenario-based business estimates from the practical exam setup, not a claim about real-world production savings.

---

## 🎥 Practical exam video

The practical requires a **5–10 minute recording** showing the work on screen along with the student's face.

After recording, add the final link here:

**Video URL:** `PASTE_YOUR_VIDEO_LINK_HERE`

---

## 🌐 GitHub repository

Required repository name:

```text
credit-fraud-detection-supervised-learning
```

After creating the public repository, add the link here:

**GitHub URL:** `PASTE_YOUR_GITHUB_REPO_URL_HERE`

---

## 🛠️ Tools used

`Python` · `Pandas` · `NumPy` · `Matplotlib` · `scikit-learn` · `imbalanced-learn` · `XGBoost`

---

## 📌 Final takeaway

The main lesson from this project is that **accuracy alone is not enough for fraud detection**.

Because fraud is rare, the model needs to be judged with metrics such as **Precision, Recall and PR-AUC**, and the final probability threshold should be chosen with the actual business trade-off in mind.

> **Best model in this run:** XGBoost  
> **Selected threshold:** 0.3930  
> **Test PR-AUC:** 0.9133

---

### 👤 Academic practical project
Prepared as a supervised-learning practical based on the supplied **Set C – Credit Card Fraud Detection** brief.
