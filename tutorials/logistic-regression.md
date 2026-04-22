---
layout: default
title: Logistic Regression
parent: ML Foundations
nav_order: 3
---

# Logistic Regression

## Plain English Intro

Despite its name, Logistic Regression is used for **classification**, not regression. Think of an email spam filter: given an email, it needs to decide — spam or not spam? Logistic Regression looks at the features of the email and outputs a probability (0 to 100%) that it's spam. Above 50% → spam. Below 50% → not spam.

---

## How It Works

1. You give the model labeled examples: email features → spam/not spam
2. It learns which features push the probability up or down
3. It outputs a probability between 0 and 1
4. You set a threshold (usually 0.5) to make the final decision

Unlike Linear Regression's straight line, Logistic Regression uses an S-shaped curve (the sigmoid function) that keeps predictions between 0 and 1.

---

## When to Use It

**Good for:**
- Binary classification (yes/no, spam/not spam, pass/fail)
- When you want a probability score, not just a class label
- As a fast baseline for classification problems

**Not ideal for:**
- Predicting continuous numbers (use Linear Regression)
- Complex, non-linear decision boundaries (use tree models or neural networks)

---

## Hands-On Code

Install:

```bash
pip install scikit-learn
```

```python
from sklearn.datasets import load_breast_cancer
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score

# Load dataset: tumor measurements, labeled as malignant(0) or benign(1)
data = load_breast_cancer()
X = data.data    # 30 features per tumor measurement
y = data.target  # 0 = malignant, 1 = benign

# Split: 80% for training, 20% for testing
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Create and train the model
model = LogisticRegression(max_iter=10000)
model.fit(X_train, y_train)

# Predict on the test set
predictions = model.predict(X_test)

# Check how accurate it is
accuracy = accuracy_score(y_test, predictions)
print(f"Accuracy: {accuracy * 100:.1f}%")

# Get the probability for the first test case
prob = model.predict_proba(X_test[:1])
print(f"Probability malignant: {prob[0][0]*100:.1f}%")
print(f"Probability benign:    {prob[0][1]*100:.1f}%")
```

**Expected output:**
```
Accuracy: 97.4%
Probability malignant: 1.0%
Probability benign:    99.0%
```

---

## Key Takeaways

- Logistic Regression classifies data into categories despite the word "regression" in its name
- It outputs a probability (0–1), not a direct class label
- You pick a threshold (usually 0.5) to convert the probability into a prediction
- Great fast baseline for binary classification problems
- The name is historical — it's built on regression math, but it's used for classification

---

[← Linear Regression](linear-regression){: .btn } [Next → K-Nearest Neighbors](knn){: .btn .btn-primary }
