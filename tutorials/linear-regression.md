---
layout: default
title: Linear Regression
parent: ML Basics
nav_order: 3
---

# Linear Regression

Every time a real estate website shows you an estimated house price, it is using something very close to what you are about to learn. The bigger the house, the higher the price. Linear Regression finds that relationship in data and uses it to make predictions.

---

## What is Linear Regression?

Linear Regression is a way for a computer to learn the relationship between two things (like house size and house price)and then use that relationship to predict new values it has never seen before.

Here is the key idea: if you plot your data on a graph (size on one axis, price on the other), you will see a pattern. Bigger houses cost more. Linear Regression draws the single best straight line through all those points. Once you have that line, you can predict the price of any house just by finding where its size lands on the line.

**New word, model:** In machine learning, a "model" is just the thing that makes predictions. Here, the model is the straight line it learned.

---

## A simple way to think about it

Imagine you are tutoring 10 students. You notice that students who study more hours tend to score higher on tests. You write down each student's study hours and their test score.

Now a new student asks: "If I study for 6 hours, what score might I get?"

You look at your notes, spot the pattern, and estimate. That is exactly what Linear Regression does, but with maths, so it is precise and consistent every time.

The line it draws has two numbers:
- **Slope:** how much the prediction goes up for each extra unit of input (for every extra hour of study, score goes up by X points)
- **Intercept:** the starting point (a student who studied 0 hours would still get Y points, on average)

Once those two numbers are locked in, you can answer any "what if" question instantly.

---

## How it works, step by step

1. You collect data (pairs of inputs and outputs (size and price, study hours and scores, age and salary))
2. The algorithm draws a line through your data and measures how far off it is from every point
3. It adjusts the line to reduce those errors
4. It keeps adjusting, thousands of times, until the line fits as well as possible
5. Training is done. The line is fixed and ready to use for new predictions

---

## See it visually

<div style="background:#f8fafc;border:1px solid #e2e8f0;border-radius:8px;padding:20px;margin:20px 0;">
<svg viewBox="0 0 320 200" xmlns="http://www.w3.org/2000/svg" style="width:100%;max-width:400px;display:block;margin:auto;">
  <line x1="40" y1="170" x2="300" y2="170" stroke="#94a3b8" stroke-width="1.5"/>
  <line x1="40" y1="170" x2="40" y2="20" stroke="#94a3b8" stroke-width="1.5"/>
  <text x="170" y="192" text-anchor="middle" font-size="11" fill="#64748b">House Size (sq ft)</text>
  <text x="12" y="100" text-anchor="middle" font-size="11" fill="#64748b" transform="rotate(-90,12,100)">Price ($k)</text>
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
  <line x1="55" y1="158" x2="290" y2="36" stroke="#ef4444" stroke-width="2"/>
  <circle cx="60" cy="185" r="4" fill="#3b82f6"/>
  <text x="70" y="189" font-size="10" fill="#475569">Data points</text>
  <line x1="130" y1="185" x2="150" y2="185" stroke="#ef4444" stroke-width="2"/>
  <text x="155" y="189" font-size="10" fill="#475569">Best-fit line</text>
</svg>
</div>

The blue dots are your actual data. The red line is what Linear Regression learned. You can use that line to predict the price of any house size, even sizes that were not in your original data.

---

## The maths (do not panic)

Here is the formula the line uses to make a prediction:

$$\hat{y} = w \cdot x + b$$

> **In plain English:** The predicted value ($\hat{y}$) equals your input ($x$) multiplied by a weight ($w$, which is the slope), plus a bias ($b$, which is the intercept). The algorithm's job is to find the best values of $w$ and $b$ from your data.

<details>
<summary>Show more detail</summary>

To find the best line, the algorithm minimises something called **Mean Squared Error (MSE)**. MSE measures the average of the squared gaps between the line's predictions and the real values:

$$\text{MSE} = \frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2$$

Think of it as: take the gap between each prediction and the real answer, square it (so positive and negative gaps both count), then average them all. A perfect line would have an MSE of zero.

The algorithm adjusts $w$ and $b$ to make MSE as small as possible.

</details>

---

## Run the code yourself

You are going to train a model that predicts house prices based on size. The model will study seven examples and then predict the price of an 1600 sq ft house it has never seen.

**Step 1:** Open [Google Colab](https://colab.research.google.com) and create a new notebook. (Or use Jupyter if you followed the [Get Started guide](setup).)

**Step 2:** Copy this code into a cell:

```python
# Import the tools we need
import numpy as np                                 # numpy helps us work with lists of numbers
from sklearn.linear_model import LinearRegression  # this is the model we will train

# Our training data
# Each house has a size (in sq ft) and a price (in $thousands)
house_sizes  = np.array([500, 750, 1000, 1250, 1500, 1750, 2000]).reshape(-1, 1)
house_prices = np.array([150, 200,  250,  310,  350,  400,  450])

# Create a blank Linear Regression model (it knows nothing yet)
model = LinearRegression()

# Train the model: show it all 7 houses so it can learn the pattern
model.fit(house_sizes, house_prices)

# See what it learned
print(f"Slope  (how much price rises per sq ft): {model.coef_[0]:.4f}")
print(f"Intercept (base price when size is zero): {model.intercept_:.2f}")

# Now ask the model to predict a house it has never seen: 1600 sq ft
predicted_price = model.predict([[1600]])[0]
print(f"Predicted price for 1600 sq ft: ${predicted_price:.1f}k")
```

**Step 3:** Press **Shift + Enter** to run it.

You should see:
```
Slope  (how much price rises per sq ft): 0.1714
Intercept (base price when size is zero): 62.86
Predicted price for 1600 sq ft: $337.1k
```

**What each line does:**
- `import numpy as np`: loads a tool called numpy that makes working with lists of numbers fast and easy
- `import LinearRegression`: loads the pre-built Linear Regression model from a library called scikit-learn
- `house_sizes = ...`: creates our list of house sizes. The `.reshape(-1, 1)` just puts them into the shape scikit-learn expects
- `model = LinearRegression()`: creates a blank model that knows nothing yet
- `model.fit(...)`: this is the training step. The model studies all 7 examples and finds the best slope and intercept
- `model.coef_[0]`: the slope the model learned (about 0.17 means every extra sq ft adds $170)
- `model.intercept_`: the intercept it learned (around $62,860 at size zero)
- `model.predict([[1600]])`: use the learned line to predict the price of a 1600 sq ft house

**What just happened?**

The model studied 7 houses and spotted the pattern: roughly $171 more for every extra square foot. You never told it that rule. It worked it out on its own. Now it can estimate the price of any house size, including ones it has never seen. That is exactly what machine learning means: the computer learns the rule from the data instead of you writing it by hand.

---

## Quick recap

- Linear Regression draws the best straight line through your data to find a relationship between input and output
- The line has two numbers: a slope (how fast the output rises) and an intercept (the starting point)
- Once trained, you can give it any new input and it will predict an output
- It works best when the relationship is roughly a straight line (bigger input = proportionally bigger output)
- It is usually the first thing you try in any prediction problem because it is fast, simple, and easy to understand

---

[← ML Foundations](foundations){: .btn } [Next → Logistic Regression](logistic-regression){: .btn .btn-primary }
