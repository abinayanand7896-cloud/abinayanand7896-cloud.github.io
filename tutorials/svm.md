---
layout: default
title: Support Vector Machines
parent: ML Foundations
nav_order: 8
---

# Support Vector Machines (SVM)

## Plain English Intro

Imagine two neighborhoods on a map — one with red houses, one with blue houses. You want to draw a boundary line between them. SVM finds the widest possible "street" between the two neighborhoods, placing the boundary right in the middle of that street.

This widest street is called the **maximum margin**. The data points closest to the street boundary are called **support vectors** — they define the street's edges.

---

## How It Works

1. Plot all training examples in space
2. Find the line (or curve in higher dimensions) that separates the classes with the largest possible gap
3. The examples closest to the boundary (support vectors) determine the final line
4. To classify a new point: which side of the boundary does it fall on?

SVMs can handle non-linear boundaries using a trick called the **kernel** — it maps data into a higher dimension where a linear boundary works.

---

## When to Use It

**Good for:**
- High-dimensional data (text classification, image classification)
- Small to medium datasets where the classes are clearly separable
- When you need a strong classifier with good generalization

**Not ideal for:**
- Very large datasets (training is slow)
- When you need probability scores (SVM natively gives class labels, not probabilities)

---

## Hands-On Code

Install:

```bash
pip install scikit-learn
```

```python
from sklearn.datasets import load_breast_cancer
from sklearn.svm import SVC
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.metrics import accuracy_score

# Load dataset
data = load_breast_cancer()
X = data.data
y = data.target

# Split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Scale features — SVMs are sensitive to feature scale
scaler = StandardScaler()
X_train = scaler.fit_transform(X_train)
X_test  = scaler.transform(X_test)

# Train SVM with RBF kernel (handles non-linear boundaries)
model = SVC(kernel='rbf', random_state=42)
model.fit(X_train, y_train)

# Evaluate
predictions = model.predict(X_test)
print(f"Accuracy: {accuracy_score(y_test, predictions) * 100:.1f}%")
print(f"Number of support vectors: {model.n_support_}")
```

**Expected output:**
```
Accuracy: 97.4%
Number of support vectors: [37 60]
```

---

## Key Takeaways

- SVM finds the widest possible boundary between classes
- The closest training examples to the boundary are called support vectors
- Always scale your features before using SVM (use StandardScaler)
- The kernel trick lets SVM handle non-linear problems
- Strong performer on small, high-dimensional datasets

---

[← Random Forests](random-forest){: .btn } [Next → K-Means Clustering](kmeans){: .btn .btn-primary }
