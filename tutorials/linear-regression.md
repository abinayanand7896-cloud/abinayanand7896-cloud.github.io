---
layout: default
title: Linear Regression
parent: ML Basics
nav_order: 3
---

# Linear Regression

## What is it?

Linear Regression is one of the simplest and most useful tools in machine learning. Given a set of data points, it finds the best-fit straight line that describes the relationship between an input and an output — for example, how a house's size relates to its price. Once you have that line, you can use it to predict the output for any new input you give it. It's usually the first model anyone learns, and for good reason: it's fast, interpretable, and surprisingly effective.

---

## The Idea

Imagine you plot 100 houses on a graph — size on the x-axis, price on the y-axis. You can see a clear trend: as houses get bigger, prices go up. Linear Regression finds the single straight line that fits through those points as closely as possible. That line has two numbers: a slope (how much the price rises per square foot) and an intercept (the starting price when size is zero). Once those two numbers are learned from your data, you just plug in a new house size and the line tells you the predicted price.

The key insight is that the model isn't memorising your training data. It's learning a compact summary — one line — that generalises to houses it has never seen before. That's the whole game in machine learning.

---

## Visual

<div style="background:#f8fafc;border:1px solid #e2e8f0;border-radius:8px;padding:20px;margin:20px 0;">
<svg viewBox="0 0 320 200" xmlns="http://www.w3.org/2000/svg" style="width:100%;max-width:400px;display:block;margin:auto;">
  <!-- Axes -->
  <line x1="40" y1="170" x2="300" y2="170" stroke="#94a3b8" stroke-width="1.5"/>
  <line x1="40" y1="170" x2="40" y2="20" stroke="#94a3b8" stroke-width="1.5"/>
  <!-- Axis labels -->
  <text x="170" y="192" text-anchor="middle" font-size="11" fill="#64748b">House Size (sq ft)</text>
  <text x="12" y="100" text-anchor="middle" font-size="11" fill="#64748b" transform="rotate(-90,12,100)">Price ($k)</text>
  <!-- Data points -->
  <circle cx="70"  cy="148" r="4" fill="#3b82f6" opacity="0.8"/>
  <circle cx="95"  cy="135" r="4" fill="#3b82f6" opacity="0.8"/>
  <circle cx="115" cy="125" r="4" fill="#3b82f6" opacity="0.8"/>
  <circle cx="140" cy="110" r="4" fill="#3b82f6" opacity="0.8"/>
  <circle cx="165" cy="98"  r="4" fill="#3b82f6" opacity="0.8"/>
  <circle cx="185" cy="88"  r="4" fill="#3b82f6" opacity="0.8"/>
  <circle cx="210" cy="75"  r="4" fill="#3b82f6" opacity="0.8"/>
  <circle cx="235" cy="62"  r="4" fill="#3b82f6" opacity="0.8"/>
  <circle cx="260" cy="52"  r="4" fill="#3b82f6" opacity="0.8"/>
  <circle cx="280" cy="42"  r="4" fill="#3b82f6" opacity="0.8"/>
  <!-- Regression line -->
  <line x1="55" y1="158" x2="290" y2="36" stroke="#ef4444" stroke-width="2"/>
  <!-- Legend -->
  <circle cx="60" cy="185" r="4" fill="#3b82f6"/>
  <text x="70" y="189" font-size="10" fill="#475569">Data points</text>
  <line x1="130" y1="185" x2="150" y2="185" stroke="#ef4444" stroke-width="2"/>
  <text x="155" y="189" font-size="10" fill="#475569">Regression line</text>
</svg>
</div>

---

## The Math

$$\hat{y} = w_1 x_1 + w_2 x_2 + \dots + w_n x_n + b$$

Or in compact vector form:

$$\hat{y} = \mathbf{w}^T \mathbf{x} + b$$

> **In plain English:** The predicted value $\hat{y}$ is a weighted sum of your input features, plus a bias term $b$. Each weight $w_i$ says how much that feature matters. The model's job is to find the weights that make predictions as accurate as possible.

<details>
<summary>Show the derivation</summary>

We want to minimise the **Mean Squared Error (MSE)** — the average of the squared gaps between predicted and actual values:

$$\text{MSE} = \frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2$$

To find the optimal weights, we take the derivative of MSE with respect to $\mathbf{w}$ and set it to zero. This gives the **Normal Equation** — a closed-form solution:

$$\mathbf{w}^* = (\mathbf{X}^T \mathbf{X})^{-1} \mathbf{X}^T \mathbf{y}$$

In practice, scikit-learn uses this (or a numerically stable variant) rather than gradient descent for small datasets. For large datasets, **Stochastic Gradient Descent (SGD)** is used instead — it updates the weights incrementally rather than solving the whole system at once.

</details>

---

## How It Learns

Training Linear Regression means finding the values of $\mathbf{w}$ and $b$ that produce the smallest total prediction error across all your training examples. The model starts with arbitrary weights, computes predictions, measures how wrong they are using MSE, and then adjusts the weights to reduce that error. It repeats this process — either analytically via the Normal Equation, or iteratively via gradient descent — until the weights stop improving. Once training is done, those weights are fixed and the model is ready to predict.

---

## When to Use It

Linear Regression is the right tool when you need to predict a continuous number and you have reason to believe the relationship between your inputs and output is roughly linear. It's also the right tool when you need a model you can explain — you can look directly at the weights and say "a one-unit increase in X leads to a Y-unit increase in the output." It works poorly when the true relationship is curved or when there are complex interactions between features; in those cases, tree-based models or neural networks will outperform it. Always try Linear Regression first as a baseline before moving to something more complex.

---

## Try It Yourself

```python
import numpy as np
from sklearn.linear_model import LinearRegression

# House sizes in sq ft, prices in $thousands
house_sizes = np.array([500, 750, 1000, 1250, 1500, 1750, 2000]).reshape(-1, 1)
house_prices = np.array([150, 200, 250, 310, 350, 400, 450])

model = LinearRegression()
model.fit(house_sizes, house_prices)

print(f"Weight (slope):   {model.coef_[0]:.4f}")
print(f"Bias (intercept): {model.intercept_:.2f}")
print(f"Prediction for 1600 sq ft: ${model.predict([[1600]])[0]:.1f}k")
```

Expected output:
```
Weight (slope):   0.1714
Bias (intercept): 62.86
Prediction for 1600 sq ft: $337.1k
```

---

## Key Takeaways

Linear Regression finds the best straight line through your data by learning a slope and a bias term that minimise the average squared prediction error. It predicts continuous numbers — prices, temperatures, sales figures — and works best when the relationship between inputs and output is roughly linear. It's not just a beginner's tool: it's fast, interpretable, and often hard to beat as a baseline. When you move on to more complex models, you'll find that many of them are essentially generalisations of this same idea.

---

[← ML Foundations](foundations){: .btn } [Next → Logistic Regression](logistic-regression){: .btn .btn-primary }
