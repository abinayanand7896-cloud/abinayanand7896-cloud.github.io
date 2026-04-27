---
layout: default
title: Logistic Regression
parent: ML Basics
nav_order: 4
---

# Logistic Regression

## What is it?

Logistic Regression predicts the probability that an input belongs to a category — it outputs a number between 0 and 1, not a raw label. Despite the name, it is a classification algorithm, not a regression one. It is used everywhere: spam detection, disease diagnosis, credit risk scoring.

---

## The Idea

If you tried to use Linear Regression for classification, you would run into an immediate problem: a straight line can output any number, including negatives and values far above 1. That makes no sense for a probability, which must live between 0 and 1.

The fix is the sigmoid function. Feed it any number — positive, negative, enormous — and it squashes the result into the (0, 1) range. Near zero input gives you something close to 0.5. Very large positive input approaches 1. Very large negative input approaches 0. The curve looks like a stretched S.

Logistic Regression combines both ideas. First it computes a weighted sum of the input features, exactly the way Linear Regression does. Then it passes that sum through the sigmoid, turning an unbounded number into a probability. If that probability is above 0.5, the model predicts class 1; below 0.5, it predicts class 0. The threshold of 0.5 can be adjusted if your problem calls for it.

---

## Visual

<div style="background:#f8fafc;border:1px solid #e2e8f0;border-radius:8px;padding:20px;margin:20px 0;">
<svg viewBox="0 0 320 160" xmlns="http://www.w3.org/2000/svg" style="width:100%;max-width:400px;display:block;margin:auto;">
  <line x1="20" y1="130" x2="300" y2="130" stroke="#94a3b8" stroke-width="1.5"/>
  <line x1="160" y1="10" x2="160" y2="145" stroke="#94a3b8" stroke-width="1.5" stroke-dasharray="4"/>
  <text x="150" y="155" font-size="10" fill="#64748b">0</text>
  <text x="10" y="134" font-size="10" fill="#64748b">0</text>
  <text x="10" y="24" font-size="10" fill="#64748b">1</text>
  <text x="130" y="155" font-size="10" fill="#64748b">z →</text>
  <line x1="20" y1="125" x2="60" y2="123" stroke="#3b82f6" stroke-width="2" fill="none"/>
  <path d="M 60 123 Q 100 120 120 110 Q 140 95 160 80 Q 180 65 200 50 Q 220 38 260 28 L 300 25"
        stroke="#3b82f6" stroke-width="2.5" fill="none"/>
  <line x1="20" y1="20" x2="60" y2="20" stroke="#94a3b8" stroke-width="1" stroke-dasharray="3"/>
  <line x1="260" y1="20" x2="300" y2="20" stroke="#94a3b8" stroke-width="1" stroke-dasharray="3"/>
  <circle cx="160" cy="80" r="3" fill="#ef4444"/>
  <text x="165" y="78" font-size="9" fill="#ef4444">σ(0) = 0.5</text>
  <text x="200" y="155" font-size="10" fill="#3b82f6">sigmoid curve</text>
</svg>
</div>

---

## The Math

$$\hat{y} = \sigma(\mathbf{w}^T \mathbf{x} + b), \quad \text{where} \quad \sigma(z) = \frac{1}{1 + e^{-z}}$$

> **In plain English:** The model computes a linear combination of inputs (just like Linear Regression), then passes it through the sigmoid function which squashes the result to a probability between 0 and 1.

<details><summary>Show the derivation</summary>

To train the model, we need a loss function that penalises wrong predictions. Mean squared error — the workhorse of Linear Regression — does not behave well with probabilities. Instead, Logistic Regression uses **binary cross-entropy**:

$$\mathcal{L} = -\frac{1}{n}\sum_{i=1}^{n}\left[y_i \log(\hat{y}_i) + (1-y_i)\log(1-\hat{y}_i)\right]$$

The intuition is elegant. When the true label is 1, only the first term survives: $$-\log(\hat{y}_i)$$. If the model predicts a probability close to 1, the loss is near zero. If the model is confidently wrong — say, $$\hat{y}_i = 0.01$$ — the loss becomes very large, because $$-\log(0.01) \approx 4.6$$. The same logic applies in the other direction for true label 0. Confident mistakes are punished harshly; uncertain predictions are punished mildly.

Unlike Linear Regression, which has a closed-form solution (the Normal Equation), Logistic Regression has no algebraic shortcut. We minimise the cross-entropy loss iteratively using gradient descent, nudging the weights step by step in the direction that reduces the loss.

</details>

---

## How It Learns

At the start of training the weights are small random numbers, so the model's probability estimates are essentially guesses. On each pass through the training data, gradient descent computes how much each weight contributed to the error and nudges every weight slightly in the direction that would reduce the loss. Repeat this thousands of times and the weights settle into values that make the predicted probabilities as accurate as possible on the training set.

This is different from Linear Regression, which can solve for the optimal weights in a single algebraic step using the Normal Equation. The sigmoid's non-linearity breaks that shortcut, so iterative optimisation is the only path. In practice this is rarely a problem — modern optimisers converge quickly even on large datasets.

---

## When to Use It

Logistic Regression shines on binary classification problems where you want an interpretable probability output, not just a hard label. The weight attached to each feature directly tells you how much that feature pushes the prediction toward one class — positive weights increase the probability of class 1, negative weights decrease it. That interpretability is genuinely valuable in medicine, finance, and any domain where you need to explain a decision.

It works best when the true decision boundary between classes is roughly linear in the feature space. When the boundary is highly curved or the features interact in complex ways, a tree-based model or a neural network will usually do better. For problems with more than two classes, swapping the sigmoid for a softmax function extends the same idea to multi-class settings.

---

## Try It Yourself

```python
from sklearn.datasets import load_iris
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score

# Load iris and treat it as binary: setosa (0) vs. not setosa (1 or 2)
data = load_iris()
X = data.data
y = (data.target != 0).astype(int)  # 0 = setosa, 1 = not setosa

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

model = LogisticRegression(max_iter=200)
model.fit(X_train, y_train)

predictions = model.predict(X_test)
print(f"Accuracy: {accuracy_score(y_test, predictions) * 100:.1f}%")

# Show predicted probabilities for the first five test samples
probs = model.predict_proba(X_test[:5])
for i, (p0, p1) in enumerate(probs):
    print(f"Sample {i+1}: P(setosa)={p0:.2f}  P(not setosa)={p1:.2f}")
```

Expected output:
```
Accuracy: 100.0%
Sample 1: P(setosa)=0.00  P(not setosa)=1.00
Sample 2: P(setosa)=0.00  P(not setosa)=1.00
Sample 3: P(setosa)=0.98  P(not setosa)=0.02
Sample 4: P(setosa)=0.00  P(not setosa)=1.00
Sample 5: P(setosa)=0.00  P(not setosa)=1.00
```

---

## Key Takeaways

Logistic Regression classifies by predicting a probability, with the sigmoid function doing the work of squashing any linear combination of inputs into the 0–1 range. It is trained by minimising cross-entropy loss through gradient descent — there is no algebraic shortcut the way there is for Linear Regression. Each weight in the model is directly interpretable, telling you how strongly a given feature pulls the prediction toward one class. Despite its simplicity, it remains one of the most widely deployed classification algorithms in industry because of that combination of speed, reliability, and explainability. It is also the conceptual building block of neural networks: stack logistic units in layers, and you have a deep learning model.

---

[← Linear Regression](linear-regression){: .btn } [Next → Decision Trees](decision-tree){: .btn .btn-primary }
