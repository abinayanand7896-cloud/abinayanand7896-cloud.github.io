---
layout: default
title: Random Forests
parent: ML Foundations
nav_order: 7
---

# Random Forests

## Plain English Intro

One doctor's opinion can be wrong. But if you ask 100 doctors and take the majority vote, you'll usually get a better answer.

A Random Forest is exactly that — 100 (or 500, or 1000) Decision Trees, each trained on a random subset of your data, each voting on the final answer. The majority vote wins. This "wisdom of the crowd" approach almost always beats a single tree.

---

## How It Works

1. Take your training data and randomly sample it N times (with replacement)
2. Train one Decision Tree on each sample, using a random subset of features at each split
3. To predict: run the input through all trees and take the majority vote (for classification) or average (for regression)

The randomness prevents all trees from being identical, which reduces overfitting dramatically.

---

## When to Use It

**Good for:**
- Most tabular (spreadsheet-style) data problems
- When a single Decision Tree overfits
- When you want a strong, reliable model with minimal tuning

**Not ideal for:**
- Very high-dimensional sparse data like text (use Naive Bayes or neural networks)
- When you need a simple, explainable model (the forest of 500 trees is hard to explain)

---

## Hands-On Code

Install:

```bash
pip install scikit-learn
```

```python
from sklearn.datasets import load_iris
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score

# Load dataset
data = load_iris()
X = data.data
y = data.target

# Split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Create a forest of 100 trees
model = RandomForestClassifier(n_estimators=100, random_state=42)
model.fit(X_train, y_train)

# Evaluate
predictions = model.predict(X_test)
print(f"Accuracy: {accuracy_score(y_test, predictions) * 100:.1f}%")

# See which features matter most
importances = model.feature_importances_
for name, score in zip(data.feature_names, importances):
    print(f"  {name}: {score:.3f}")
```

**Expected output:**
```
Accuracy: 100.0%
  sepal length (cm): 0.091
  sepal width (cm):  0.026
  petal length (cm): 0.441
  petal width (cm):  0.442
```

Petal length and width matter most — the forest figured this out automatically.

---

## Key Takeaways

- Random Forests combine many Decision Trees and take a majority vote
- The randomness (in data sampling and feature selection) prevents overfitting
- They give you feature importance scores — useful for understanding your data
- One of the most reliable and widely-used ML algorithms
- Very little tuning needed compared to other powerful models

---

[← Decision Trees](decision-tree){: .btn } [Next → Support Vector Machines](svm){: .btn .btn-primary }
