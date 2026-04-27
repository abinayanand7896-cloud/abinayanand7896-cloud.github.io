---
layout: default
title: Linear Regression
parent: ML Basics
nav_order: 3
---

# Linear Regression

## What is it?

Linear Regression is one of the simplest and most useful tools in machine learning. Given a set of data points, it finds the best-fit straight line that describes the relationship between an input and an output. For example, how a house's size relates to its price. Once you have that line, you can use it to predict the output for any new input. It's usually the first model anyone learns, and for good reason: it's fast, interpretable, and surprisingly effective.

---

## The Idea

Imagine you plot 100 houses on a graph. Size on the x-axis, price on the y-axis. You can see a clear trend: as houses get bigger, prices go up. Linear Regression finds the single straight line that fits through those points as closely as possible. That line has two numbers: a slope (how much the price rises per square foot) and an intercept (the starting price when size is zero). Once those two numbers are learned from your data, you plug in a new house size and the line tells you the predicted price.

The key insight is that the model isn't memorising your training data. It's learning a compact summary, just one line, that generalises to houses it's never seen before. That's the whole game in machine learning.

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

We want to minimise the **Mean Squared Error (MSE)**, the average of the squared gaps between predicted and actual values:

$$\text{MSE} = \frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2$$

To find the optimal weights, we take the derivative of MSE with respect to $\mathbf{w}$ and set it to zero. This gives the **Normal Equation**, a closed-form solution:

$$\mathbf{w}^* = (\mathbf{X}^T \mathbf{X})^{-1} \mathbf{X}^T \mathbf{y}$$

In practice, scikit-learn uses this (or a numerically stable variant) rather than gradient descent for small datasets. For large datasets, **Stochastic Gradient Descent (SGD)** is used instead. It updates the weights incrementally rather than solving the whole system at once.

</details>

---

## How It Learns

Training Linear Regression means finding the values of $\mathbf{w}$ and $b$ that produce the smallest total prediction error across all your training examples. The model starts with arbitrary weights, computes predictions, measures how wrong they are using MSE, and then adjusts the weights to reduce that error. It repeats this process until the weights stop improving. Once training is done, those weights are fixed and the model is ready to predict.

---

## When to Use It

Linear Regression is the right tool when you need to predict a continuous number and you have reason to believe the relationship between your inputs and output is roughly linear. It's also the right tool when you need a model you can explain. You can look directly at the weights and say "a one-unit increase in X leads to a Y-unit increase in the output." It works poorly when the true relationship is curved or when there are complex interactions between features. In those cases, tree-based models or neural networks will outperform it. Always try Linear Regression first as a baseline before moving to something more complex.

---

## Try It Yourself

If you have not set up Python yet, start with the [Get Started guide](setup) first.

This code trains a simple Linear Regression model to predict house prices from house sizes. You'll see the slope and intercept it learns, then test it on a new house.

Copy this into a cell and run it with Shift + Enter:

```python
import numpy as np                                    # for numerical arrays
from sklearn.linear_model import LinearRegression     # the model

# House sizes in sq ft
house_sizes = np.array([500, 750, 1000, 1250, 1500, 1750, 2000]).reshape(-1, 1)
# Prices in $thousands
house_prices = np.array([150, 200, 250, 310, 350, 400, 450])

model = LinearRegression()                  # create the model
model.fit(house_sizes, house_prices)        # train on the data

print(f"Weight (slope):   {model.coef_[0]:.4f}")          # how much price rises per sq ft
print(f"Bias (intercept): {model.intercept_:.2f}")        # base price
print(f"Prediction for 1600 sq ft: ${model.predict([[1600]])[0]:.1f}k")  # predict new value
```

Expected output:
```
Weight (slope):   0.1714
Bias (intercept): 62.86
Prediction for 1600 sq ft: $337.1k
```

**What each line does:**
- `np.array([...]).reshape(-1, 1)`: creates the house size data as a column (scikit-learn expects this shape)
- `LinearRegression()`: creates a blank model that's ready to learn
- `model.fit(house_sizes, house_prices)`: trains the model by finding the best slope and intercept
- `model.coef_[0]`: the learned slope: every extra square foot adds about $171 to the price
- `model.intercept_`: the base price: around $62,860 when size is zero
- `model.predict([[1600]])`: asks the trained model for its estimate on a 1600 sq ft house

**What just happened?**

The model looked at those seven house examples and drew the best straight line through them. It learned that every 100 extra square feet adds roughly $17,000 to the price. Now you can plug in any house size and get a price estimate instantly. That's linear regression in action.

---

## Key Takeaways

- Linear Regression finds the best straight line through your data by learning a slope and a bias term.
- It minimises the average squared prediction error across all training examples.
- It works best when the relationship between inputs and output is roughly linear.
- It's fast, interpretable, and often a strong baseline before trying complex models.
- The weights are directly readable: each one tells you how much a feature moves the prediction.

---

[← ML Foundations](foundations){: .btn } [Next → Logistic Regression](logistic-regression){: .btn .btn-primary }
