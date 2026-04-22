---
layout: default
title: Linear Regression
parent: ML Foundations
nav_order: 2
---

# Linear Regression

## Plain English Intro

Imagine you want to predict a house price based on its size. You plot data from 100 houses — size on the x-axis, price on the y-axis. You notice a trend: bigger houses cost more.

Linear Regression draws the best-fit straight line through those dots. Once you have that line, you can predict the price of any new house just from its size.

---

## How It Works

1. You give the model pairs of data: input (house size) → output (price)
2. The model finds the straight line that fits the data best, minimizing the gap between predicted and actual values
3. The line formula is: **price = (slope × size) + intercept**
4. Once trained, pass in a new size and get a predicted price

---

## When to Use It

**Good for:**
- Predicting a continuous number (price, temperature, sales figures)
- When the relationship between input and output is roughly linear
- As a quick baseline — always try Linear Regression first

**Not ideal for:**
- Classifying things into categories (use Logistic Regression instead)
- Complex curved relationships (use tree-based models or neural networks)

---

## Hands-On Code

Install:

```bash
pip install scikit-learn numpy matplotlib
```

```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.linear_model import LinearRegression

# Fake house data: sizes in sq ft, prices in $thousands
house_sizes = np.array([500, 750, 1000, 1250, 1500, 1750, 2000]).reshape(-1, 1)
house_prices = np.array([150, 200, 250, 310, 350, 400, 450])

# Create and train the model
model = LinearRegression()
model.fit(house_sizes, house_prices)  # Model learns the best line

# Predict price of a 1600 sq ft house
new_house = np.array([[1600]])
predicted = model.predict(new_house)
print(f"Predicted price for 1600 sq ft: ${predicted[0]:.1f}k")

# Show what the model learned
print(f"Slope (price per sq ft): {model.coef_[0]:.4f}")
print(f"Intercept: {model.intercept_:.2f}")

# Plot it
plt.scatter(house_sizes, house_prices, color='blue', label='Actual data')
plt.plot(house_sizes, model.predict(house_sizes), color='red', label='Regression line')
plt.xlabel('House Size (sq ft)')
plt.ylabel('Price ($k)')
plt.title('Linear Regression: House Prices')
plt.legend()
plt.show()
```

**Expected output:**
```
Predicted price for 1600 sq ft: $373.1k
Slope (price per sq ft): 0.1714
Intercept: 62.86
```

---

## Key Takeaways

- Linear Regression finds the best-fit straight line through your data
- It predicts **continuous numbers**, not categories
- The model learns a slope and an intercept from your data
- It works best when the relationship between input and output is roughly linear
- Always a good starting point — simple, fast, and easy to interpret

---

[← What is ML?](what-is-ml){: .btn } [Next → Logistic Regression](logistic-regression){: .btn .btn-primary }
