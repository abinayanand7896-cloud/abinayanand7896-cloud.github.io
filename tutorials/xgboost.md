---
layout: default
title: XGBoost
parent: Intermediate ML
nav_order: 2
---

# XGBoost

## Plain English Intro

XGBoost (eXtreme Gradient Boosting) is Gradient Boosting with a turbo engine. It does the same thing — trains trees sequentially to fix mistakes — but with a smarter algorithm that's dramatically faster and handles large datasets well.

It became famous for winning machine learning competitions repeatedly. If you have tabular data and want the best results, XGBoost is almost always worth trying.

---

## How It Works

XGBoost follows the same sequential boosting idea as Gradient Boosting, with several key improvements:

- **Regularization** — Built-in penalties that prevent overfitting
- **Parallelization** — Builds each tree faster using multiple CPU cores
- **Handling missing values** — Automatically figures out what to do with missing data
- **Pruning** — Removes unnecessary branches for smaller, better trees

---

## When to Use It

**Good for:**
- Tabular data — XGBoost is often the go-to choice
- Competitions (it has won hundreds of Kaggle competitions)
- Datasets with missing values (handles them automatically)

**Not ideal for:**
- Images, audio, or raw text (use CNNs or transformers)
- Tiny datasets where simpler models generalize better

---

## Hands-On Code

Install:

```bash
pip install xgboost scikit-learn
```

```python
import xgboost as xgb
from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score

# Load dataset
data = load_breast_cancer()
X = data.data
y = data.target

# Split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Train XGBoost classifier
model = xgb.XGBClassifier(
    n_estimators=100,      # Number of trees
    learning_rate=0.1,     # How much each tree contributes
    max_depth=3,           # Keep trees shallow to avoid overfitting
    random_state=42,
    eval_metric='logloss'
)
model.fit(X_train, y_train)

# Evaluate
predictions = model.predict(X_test)
print(f"Accuracy: {accuracy_score(y_test, predictions) * 100:.1f}%")

# Save and reload the model (XGBoost's efficient format)
model.save_model("xgboost_model.json")
print("Model saved to xgboost_model.json")
```

**Expected output:**
```
Accuracy: 97.4%
Model saved to xgboost_model.json
```

---

## Key Takeaways

- XGBoost is a fast, powerful implementation of Gradient Boosting
- Built-in regularization prevents overfitting without extra work
- Handles missing values automatically
- One of the most-used algorithms in real-world ML and competitions
- Very similar API to scikit-learn — easy to swap in from other models

---

[← Gradient Boosting](gradient-boosting){: .btn } [Next → What are Neural Networks?](neural-networks-intro){: .btn .btn-primary }
