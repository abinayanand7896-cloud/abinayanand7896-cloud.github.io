---
layout: default
title: Support Vector Machines
parent: ML Basics
nav_order: 9
---

# Support Vector Machines

## What is it?

A Support Vector Machine finds the hyperplane that separates two classes with the largest possible margin. The data points closest to the boundary — the support vectors — define that margin, and the algorithm's entire job is to maximise it. A wider margin means the model is less likely to be fooled by small variations in new data.

---

## The Idea

Among all the lines (or hyperplanes in higher dimensions) that could separate two classes, many will work — but most will hug one class too tightly, leaving little room for error. SVM takes a different approach: it searches for the boundary that sits as far as possible from both classes at once. That gap between the boundary and the nearest point from each class is called the margin, and the closest points themselves are the support vectors. Everything else in the training set is irrelevant to the final boundary — only those boundary-defining points matter.

When data is not linearly separable in its original space, SVM uses the kernel trick. A kernel function computes dot products between data points as though they had been mapped into a higher-dimensional space, without ever explicitly constructing that space. In that higher dimension, the classes may become linearly separable, so a simple flat boundary there corresponds to a curved, non-linear boundary back in the original space. The RBF (Gaussian) kernel is the most popular choice and handles a wide range of non-linear problems well.

Real-world data is rarely perfectly separable, so SVM also supports a soft margin controlled by the hyperparameter C. A large C tells the model to penalise every misclassification heavily, producing a narrow margin that tries hard to classify all training points correctly. A small C tolerates more violations in exchange for a wider, more robust margin. Tuning C — typically via cross-validation — is one of the key practical decisions when using SVM.

---

## Visual

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
    The solid line is the decision boundary. Dashed lines are the margin edges. Ringed points are the support vectors — the only training examples that define the boundary.
  </p>
</div>

---

## The Math

$$\min_{\mathbf{w}, b} \frac{1}{2}\|\mathbf{w}\|^2 \quad \text{subject to } y_i(\mathbf{w}^T\mathbf{x}_i + b) \geq 1$$

> **In plain English:** Find the weights $\mathbf{w}$ that define the boundary. Making $\|\mathbf{w}\|^2$ small is equivalent to making the margin wide. The constraint ensures every point is on the correct side of the margin — points with $y_i = +1$ must satisfy $\mathbf{w}^T\mathbf{x}_i + b \geq 1$ and points with $y_i = -1$ must satisfy $\mathbf{w}^T\mathbf{x}_i + b \leq -1$.

<details>
<summary>Show the derivation</summary>

The distance from the decision boundary to the nearest point on either side turns out to be $1/\|\mathbf{w}\|$, so the total margin width is $2/\|\mathbf{w}\|$. Maximising this is equivalent to minimising $\|\mathbf{w}\|^2$, which is a convex objective and easier to work with than $\|\mathbf{w}\|$ directly.

The resulting constrained quadratic programme is solved via Lagrange multipliers. Each training point $i$ gets a multiplier $\alpha_i \geq 0$, and the Karush–Kuhn–Tucker conditions show that $\alpha_i = 0$ for every point that lies strictly outside the margin. Only the support vectors — the points exactly on the margin — have $\alpha_i > 0$. This is why the model is called a Support Vector Machine: all the geometric information is concentrated in those few boundary-defining points.

For the soft-margin version, slack variables $\xi_i \geq 0$ allow individual points to violate the margin constraint. The objective becomes

$$\min_{\mathbf{w}, b, \boldsymbol{\xi}} \frac{1}{2}\|\mathbf{w}\|^2 + C\sum_{i}\xi_i \quad \text{subject to } y_i(\mathbf{w}^T\mathbf{x}_i + b) \geq 1 - \xi_i,\; \xi_i \geq 0$$

The hyperparameter $C$ trades margin width against total violation: large $C$ penalises mistakes heavily (narrow margin, risk of overfitting); small $C$ accepts more violations to widen the margin (risk of underfitting). Cross-validation is typically used to find a good value.

</details>

---

## How It Learns

Training an SVM means solving the quadratic programme described above to find the optimal weights $\mathbf{w}$ and bias $b$. The solver identifies the support vectors — the boundary-defining points — and positions the decision boundary exactly halfway between the closest points from each class. Once training is done, the support vectors are all the model needs; the rest of the training data can be discarded.

For non-linear problems the kernel trick is central. Rather than explicitly mapping every point into a high-dimensional feature space (which may be infinite-dimensional for the RBF kernel), the optimisation only ever needs dot products between pairs of points. A kernel function $k(\mathbf{x}_i, \mathbf{x}_j)$ computes that dot product in the implicit high-dimensional space directly from the original coordinates. This keeps the computation tractable while still allowing highly non-linear decision boundaries in the original space.

---

## When to Use It

SVMs shine in high-dimensional settings — text classification with tens of thousands of word features is a classic example where they have historically been very competitive. They also handle the regime where features outnumber samples well, which is common in genomics and certain signal-processing tasks. With the RBF kernel, they can capture complex non-linear boundaries without requiring you to engineer polynomial features by hand.

The main practical downsides are computational. Training scales roughly as $O(n^2)$ to $O(n^3)$ in the number of samples, so SVMs become slow and memory-hungry on datasets with more than a few tens of thousands of points. Feature scaling is not optional — SVMs are highly sensitive to the relative magnitudes of features, so you should always standardise inputs with `StandardScaler` before fitting. The C and kernel hyperparameters also need careful tuning, typically through cross-validation, which adds to the training cost. For large datasets or problems where you want calibrated probabilities, tree ensembles or logistic regression are usually more practical starting points.

---

## Try It Yourself

```python
from sklearn.datasets import load_breast_cancer
from sklearn.svm import SVC
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.metrics import accuracy_score

# Load dataset
data = load_breast_cancer()
X, y = data.data, data.target

# Split into train and test sets
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)

# Scale features — SVMs are very sensitive to feature magnitudes
scaler = StandardScaler()
X_train = scaler.fit_transform(X_train)
X_test  = scaler.transform(X_test)

# Train SVM with RBF kernel
model = SVC(kernel='rbf', C=1.0, random_state=42)
model.fit(X_train, y_train)

# Evaluate
predictions = model.predict(X_test)
print(f"Accuracy: {accuracy_score(y_test, predictions) * 100:.1f}%")
print(f"Number of support vectors per class: {model.n_support_}")
```

Expected output:

```
Accuracy: 97.4%
Number of support vectors per class: [37 60]
```

---

## Key Takeaways

SVMs find the decision boundary that maximises the margin between classes — a wider margin means more room for error on new data. The support vectors, the points closest to the boundary, are the only data points that matter for the final model; everything else in the training set is irrelevant once training is complete. Kernels extend this idea to non-linear problems by implicitly mapping data into higher dimensions where a linear boundary is sufficient. For high-dimensional and small-sample problems, SVMs remain a competitive option even in the age of deep learning. Always standardise your features before fitting — it is one of the most important practical steps when using this algorithm.

---

[← XGBoost](xgboost){: .btn } [Next → K-Nearest Neighbours](knn){: .btn .btn-primary }
