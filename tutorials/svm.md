---
layout: default
title: Support Vector Machines
parent: ML Basics
nav_order: 9
---

# Support Vector Machines

Imagine drawing a line on a piece of paper to separate blue dots from red dots. You could draw many different lines that all technically separate them. But which line is best? The answer is the one that sits as far as possible from both groups, so even if a new dot lands slightly off-centre, it still gets sorted correctly. That is exactly the idea behind a Support Vector Machine.

---

## What is a Support Vector Machine?

A Support Vector Machine (often called an SVM) is a model that finds the boundary between two categories by placing it as far away as possible from both groups. The gap between the boundary and the closest points on either side is called the **margin** (which means the safety zone). A wider margin means the model is less likely to make a mistake when it sees a new example.

The data points closest to the boundary, which are the ones that actually define where the boundary goes, are called **support vectors** (which means the examples "supporting" or holding up the boundary line). Once those points are found, everything else in the training data is irrelevant.

---

## A simple way to think about it

Imagine you are drawing a safety corridor down the middle of a road to separate oncoming traffic. You want the corridor to be as wide as possible, so cars on both sides have the most room for error.

The SVM does the same thing with data. It searches for the widest possible corridor that separates two categories. The edges of the corridor are defined by the closest examples from each category, the support vectors. The actual decision boundary runs down the middle of that corridor.

When the data cannot be separated by a straight line (for example, one category is surrounded by the other), SVM uses a mathematical trick to solve the problem. It mentally re-imagines the data in a higher number of dimensions, where a flat boundary does separate the categories. This trick is called a **kernel** (which means a function that measures similarity between examples in a transformed space, without actually doing the full transformation). The most common kernel is called the RBF kernel (Radial Basis Function), and it works well on a wide range of problems.

---

## How it works, step by step

1. Find all the data points that are closest to the boundary between the two categories
2. Draw the widest possible corridor between the two categories, using those closest points as guides
3. Place the decision boundary down the middle of that corridor
4. For non-linear problems, use a kernel to transform the data so a flat boundary can separate it
5. For a new example, check which side of the boundary it falls on and return that category

---

## See it visually

<div style="display:flex; flex-direction:column; align-items:center; margin: 2rem 0;">
  <svg width="340" height="280" viewBox="0 0 340 280" xmlns="http://www.w3.org/2000/svg" style="font-family: sans-serif;">

    <!-- Margin band background -->
    <polygon points="60,240 280,80 280,112 60,272" fill="#f0f4ff" opacity="0.8"/>
    <polygon points="60,208 280,48 280,80 60,240" fill="#fff0f0" opacity="0.8"/>

    <!-- Dashed margin lines -->
    <line x1="60" y1="240" x2="280" y2="80" stroke="#4a90d9" stroke-width="1.5" stroke-dasharray="6,4"/>
    <line x1="60" y1="208" x2="280" y2="48" stroke="#d94a4a" stroke-width="1.5" stroke-dasharray="6,4"/>

    <!-- Decision boundary (solid black) -->
    <line x1="60" y1="224" x2="280" y2="64" stroke="#222" stroke-width="2.2"/>

    <!-- Margin width arrow and label -->
    <line x1="200" y1="102" x2="200" y2="138" stroke="#555" stroke-width="1.2" marker-start="url(#arr)" marker-end="url(#arr)"/>
    <text x="206" y="124" font-size="11" fill="#555">margin</text>

    <defs>
      <marker id="arr" markerWidth="6" markerHeight="6" refX="3" refY="3" orient="auto">
        <path d="M0,0 L0,6 L6,3 z" fill="#555"/>
      </marker>
    </defs>

    <!-- Blue class points (circles) — well separated -->
    <circle cx="80"  cy="180" r="7" fill="#4a90d9" opacity="0.85"/>
    <circle cx="100" cy="155" r="7" fill="#4a90d9" opacity="0.85"/>
    <circle cx="65"  cy="145" r="7" fill="#4a90d9" opacity="0.85"/>
    <circle cx="120" cy="130" r="7" fill="#4a90d9" opacity="0.85"/>
    <circle cx="90"  cy="118" r="7" fill="#4a90d9" opacity="0.85"/>
    <circle cx="55"  cy="110" r="7" fill="#4a90d9" opacity="0.85"/>
    <circle cx="140" cy="100" r="7" fill="#4a90d9" opacity="0.85"/>
    <circle cx="110" cy="88"  r="7" fill="#4a90d9" opacity="0.85"/>

    <!-- Support vector blue (highlighted with ring) -->
    <circle cx="130" cy="162" r="7" fill="#4a90d9"/>
    <circle cx="130" cy="162" r="12" fill="none" stroke="#4a90d9" stroke-width="2" opacity="0.6"/>

    <!-- Red class points (squares) — well separated -->
    <rect x="175" y="122" width="14" height="14" rx="2" fill="#d94a4a" opacity="0.85"/>
    <rect x="210" y="100" width="14" height="14" rx="2" fill="#d94a4a" opacity="0.85"/>
    <rect x="245" y="78"  width="14" height="14" rx="2" fill="#d94a4a" opacity="0.85"/>
    <rect x="190" y="148" width="14" height="14" rx="2" fill="#d94a4a" opacity="0.85"/>
    <rect x="230" y="130" width="14" height="14" rx="2" fill="#d94a4a" opacity="0.85"/>
    <rect x="260" y="108" width="14" height="14" rx="2" fill="#d94a4a" opacity="0.85"/>
    <rect x="215" y="158" width="14" height="14" rx="2" fill="#d94a4a" opacity="0.85"/>

    <!-- Support vector red (highlighted with ring) -->
    <rect x="172" y="96" width="14" height="14" rx="2" fill="#d94a4a"/>
    <rect x="169" y="93" width="20" height="20" rx="4" fill="none" stroke="#d94a4a" stroke-width="2" opacity="0.6"/>

    <!-- Labels -->
    <text x="58"  y="265" font-size="12" fill="#4a90d9" font-weight="bold">Class A</text>
    <text x="240" y="72"  font-size="12" fill="#d94a4a" font-weight="bold">Class B</text>
    <text x="148" y="242" font-size="11" fill="#444">Decision boundary</text>
    <text x="60"  y="30"  font-size="11" fill="#777" font-style="italic">Ringed points = support vectors</text>

  </svg>
  <p style="font-size:0.85rem; color:#666; margin-top:0.5rem;">
    The solid line is the decision boundary. Dashed lines are the margin edges. Ringed points are the support vectors, the only training examples that define the boundary.
  </p>
</div>

The solid black line is the decision boundary. The dashed lines on either side mark the edges of the margin (the safety corridor). The ringed points are the support vectors: the only examples that matter once training is done. All the other training points are irrelevant to the final boundary.

---

## The maths (do not panic)

Here is the formula that makes this work. We will break down every part.

$$\min_{\mathbf{w}, b} \frac{1}{2}\|\mathbf{w}\|^2 \quad \text{subject to } y_i(\mathbf{w}^T\mathbf{x}_i + b) \geq 1$$

> **In plain English:** The model finds the weights ($\mathbf{w}$) that define the boundary line. Making $\|\mathbf{w}\|^2$ small is mathematically equivalent to making the margin wide. The second part of the formula says that every example must end up on the correct side of the margin, not just on the correct side of the boundary.

<details>
<summary>Show more detail</summary>

The distance from the boundary to the nearest point on either side turns out to be $1/\|\mathbf{w}\|$. So the total margin width is $2/\|\mathbf{w}\|$. Making the margin as wide as possible is therefore the same as making $\|\mathbf{w}\|^2$ as small as possible. Minimising $\|\mathbf{w}\|^2$ is a standard shape of maths problem (called a convex quadratic programme) that can be solved reliably.

The solution is found using a technique called Lagrange multipliers. Each training example gets an associated number $\alpha_i \geq 0$. After solving, most of these numbers turn out to be zero. Only the support vectors (the examples on the margin edges) end up with $\alpha_i > 0$. That is why only those points define the final boundary.

Real-world data is rarely perfectly separable. The soft-margin version of SVM allows some examples to cross the margin, controlled by the setting $C$:

$$\min_{\mathbf{w}, b, \boldsymbol{\xi}} \frac{1}{2}\|\mathbf{w}\|^2 + C\sum_{i}\xi_i \quad \text{subject to } y_i(\mathbf{w}^T\mathbf{x}_i + b) \geq 1 - \xi_i,\; \xi_i \geq 0$$

The $\xi_i$ values (called slack variables) measure how much each example is allowed to violate the margin. The setting $C$ controls the trade-off: a large $C$ punishes violations heavily and creates a narrow margin. A small $C$ tolerates more violations in exchange for a wider, more robust margin.

</details>

---

## Run the code yourself

This code trains an SVM on a breast cancer dataset. Notice that we scale (which means adjust) the features before training. This step is essential with SVMs and must not be skipped.

**Step 1:** Open [Google Colab](https://colab.research.google.com) and create a new notebook. (Or use Jupyter if you followed the [Get Started guide](setup).)

**Step 2:** Copy this code into a cell:

```python
from sklearn.datasets import load_breast_cancer       # medical dataset: 569 tumours, 2 classes
from sklearn.svm import SVC                           # the Support Vector Classifier
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler      # rescales each feature to a similar range
from sklearn.metrics import accuracy_score

# Load the breast cancer dataset
data = load_breast_cancer()
X, y = data.data, data.target

# Split into 80% training and 20% testing
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)

# Scale the features: SVMs are very sensitive to features that are on very different scales
# For example, one feature might range from 0 to 1 and another from 0 to 1000
scaler = StandardScaler()
X_train = scaler.fit_transform(X_train)   # learn the scaling from training data and apply it
X_test  = scaler.transform(X_test)        # apply the same scaling to test data (never learn from test data)

# Train the SVM using the RBF kernel (good for non-linear problems)
model = SVC(kernel='rbf', C=1.0, random_state=42)
model.fit(X_train, y_train)

# Check accuracy on examples the model never saw during training
predictions = model.predict(X_test)
print(f"Accuracy: {accuracy_score(y_test, predictions) * 100:.1f}%")
print(f"Number of support vectors per class: {model.n_support_}")
```

**Step 3:** Press **Shift + Enter** to run it.

You should see:
```
Accuracy: 97.4%
Number of support vectors per class: [37 60]
```

**What each line does:**
- `StandardScaler()`: creates a tool that will rescale each feature to have a mean of 0 and a spread of 1
- `scaler.fit_transform(X_train)`: learns the right scaling from the training data and applies it
- `scaler.transform(X_test)`: applies the same scaling to test data (you must never learn the scaling from test data, only from training data)
- `SVC(kernel='rbf', C=1.0)`: creates an SVM with the RBF kernel and a moderate setting for how strictly to enforce the margin
- `model.n_support_`: shows how many support vectors the final model uses from each category

**What just happened?**

The model found the widest possible boundary between malignant and benign tumours in a high-dimensional space. It only needed 97 support vectors from the entire training set of 455 examples. Once those 97 critical points were found, all the other training examples became irrelevant. That is what makes SVMs elegant: a small number of well-chosen examples define the entire decision boundary.

---

## Quick recap

- An SVM finds the decision boundary that creates the widest possible gap between two categories
- The support vectors are the only examples that matter once training is done. Everything else is ignored.
- Kernels (like the RBF kernel) allow SVMs to handle problems where a straight-line boundary is not enough
- Always rescale your features before training an SVM. Without scaling, the model will perform poorly.
- SVMs work especially well on problems with many features and relatively few examples

---

[← XGBoost](xgboost){: .btn } [Next → K-Nearest Neighbours](knn){: .btn .btn-primary }
