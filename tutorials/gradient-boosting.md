---
layout: default
title: Gradient Boosting
parent: ML Basics
nav_order: 7
---

# Gradient Boosting

## What is it?

Gradient Boosting builds models sequentially. Each new model is trained specifically to correct the errors made by the previous ones. The final prediction is the sum of all models' contributions, each scaled by a learning rate. It consistently produces some of the most accurate predictions on tabular data.

## The Idea

The clearest way to understand Gradient Boosting is to contrast it with Random Forest. In a Random Forest, hundreds of trees are trained independently on random subsets of the data, then their predictions are averaged. Gradient Boosting takes a completely different approach: trees are built one after another, and every new tree is built to fix the mistakes of the model so far.

Tree 1 makes its best guess at the target. Tree 2 is not trained on the original labels. It's trained on the residuals, the gap between Tree 1's predictions and reality. Tree 3 is then trained on Tree 2's residuals, and so on. Each individual tree is deliberately shallow, usually with just 3 to 8 leaves, making it a "weak learner." But when you stack dozens or hundreds of them additively, they compose into a remarkably powerful model.

The word "gradient" gives the technique its name and its generality. For mean squared error loss, the negative gradient of the loss with respect to the predictions turns out to be exactly the residuals. So fitting each new tree to the residuals is gradient descent. For other loss functions, the "pseudo-residuals" look different but the principle is identical: each tree takes a small step in the direction that most reduces the current loss. This is gradient descent, not in a parameter space, but in function space.

## Visual

```mermaid
graph LR
  D["Training Data"] --> M1["Tree 1\n(weak learner)"]
  M1 --> R1["Residuals 1"]
  R1 --> M2["Tree 2\n(fits errors)"]
  M2 --> R2["Residuals 2"]
  R2 --> M3["Tree 3\n(fits errors)"]
  M3 --> Sum["F(x) = T1 + η·T2 + η·T3"]
```

## The Math

$$F_m(\mathbf{x}) = F_{m-1}(\mathbf{x}) + \eta \cdot h_m(\mathbf{x})$$

> **In plain English:** The model at step $m$ is the previous model plus a small step. $\eta$ is the learning rate (typically 0.01–0.1) and $h_m$ is a new small tree trained to correct the remaining errors.

<details><summary>Show the derivation</summary>

For MSE loss $\mathcal{L} = \frac{1}{2}(y - F(\mathbf{x}))^2$, the negative gradient is $y - F(\mathbf{x})$, which are the residuals. For other losses (log-loss, MAE) the "pseudo-residuals" differ but the principle is identical: fit each tree to the negative gradient of the loss. This is gradient descent in function space rather than parameter space.

</details>

## How It Learns

The algorithm fits Tree 1 to the training data and records the residuals, the difference between its predictions and the true labels. Tree 2 is then fitted to those residuals, and its predictions are added to the running total, scaled by the learning rate $\eta$: $F_1 = F_0 + \eta h_1$. The process repeats: compute the new residuals, fit the next tree, update the model. It runs for $M$ iterations, with $M$ being one of the key hyperparameters. A smaller $\eta$ paired with more trees leads to better generalisation but slower training.

## When to Use It

Gradient Boosting typically beats Random Forest on structured data when tuned. The trade-offs are real: training is sequential and therefore slower than Random Forest's parallel tree building, the algorithm is more sensitive to outliers (which inflate residuals and can mislead subsequent trees), and it requires careful tuning of at least three hyperparameters: learning rate, tree depth, and number of estimators. It's also the conceptual foundation for XGBoost and LightGBM, which add regularisation, approximate split-finding, and hardware optimisations to make the same idea faster and more robust at scale.

## Try It Yourself

If you have not set up Python yet, start with the [Get Started guide](setup) first.

This code trains a Gradient Boosting classifier on a breast cancer dataset. It builds 100 sequential trees, each correcting the last one's mistakes.

Copy this into a cell and run it with Shift + Enter:

```python
from sklearn.datasets import load_breast_cancer              # medical dataset
from sklearn.ensemble import GradientBoostingClassifier      # the model
from sklearn.model_selection import train_test_split         # split train/test
from sklearn.metrics import accuracy_score                   # measure accuracy

# Load dataset (569 samples, 30 features, 2 classes: malignant / benign)
data = load_breast_cancer()
X, y = data.data, data.target

# Hold out 20% for testing
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Train Gradient Boosting: 100 shallow trees, small learning rate
model = GradientBoostingClassifier(
    n_estimators=100,    # number of trees to build sequentially
    learning_rate=0.1,   # how much each tree contributes
    max_depth=3,         # keep trees shallow (weak learners)
    random_state=42
)
model.fit(X_train, y_train)   # train one tree at a time

# Evaluate on the held-out test set
predictions = model.predict(X_test)
print(f"Accuracy: {accuracy_score(y_test, predictions) * 100:.1f}%")
```

Expected output:

```
Accuracy: 96.5%
```

**What each line does:**
- `n_estimators=100`: builds 100 trees, each correcting what came before
- `learning_rate=0.1`: each tree's contribution is scaled down by 10% to prevent overshooting
- `max_depth=3`: keeps trees shallow so they're weak learners, not memorisers
- `model.fit(...)`: trains the trees one by one, each on the previous tree's residuals
- `model.predict(X_test)`: sums up all 100 trees' contributions to make the final prediction

**What just happened?**

100 shallow, individually weak trees worked together to get 96.5% accuracy on cancer diagnosis. No single tree would do that well alone. Each one just corrected what the last one got wrong. That's boosting.

## Key Takeaways

- Gradient Boosting builds an additive model one shallow tree at a time, each correcting the previous tree's mistakes.
- The learning rate controls each tree's contribution. Lower rates paired with more trees generalise better.
- It consistently outperforms Random Forest on tabular data when properly tuned.
- It's sequential, so it can't be parallelised the way Random Forest can.
- Understanding Gradient Boosting unlocks XGBoost and LightGBM, which are built on the same idea.

---

[← Random Forests](random-forest){: .btn } [Next → XGBoost](xgboost){: .btn .btn-primary }
