---
layout: default
title: K-Nearest Neighbours
parent: ML Basics
nav_order: 10
---

# K-Nearest Neighbours

## What is it?

K-Nearest Neighbours classifies a new data point by looking at the $k$ training points closest to it and taking a majority vote of their labels. There is no training phase — the algorithm simply memorises the training set and uses distance to classify at prediction time. It is one of the simplest and most intuitive machine learning algorithms.

## The Idea

KNN is often called a "lazy learner" because it does no work upfront. At prediction time it calculates the distance from the new point to every training point, picks the $k$ closest ones, and returns the most common class among them (or the average value for regression). The key insight is that similar things tend to have similar labels, and distance is a useful proxy for similarity.

The choice of $k$ is the critical hyperparameter. With $k=1$ the model is extremely flexible — it memorises every training point exactly — but it overfits badly to noise. As $k$ grows, each prediction averages over more neighbours, smoothing out noise but potentially missing real local patterns. The right value of $k$ is usually found by cross-validation, and a small odd number like 3 or 5 is a common starting point.

Feature scaling matters enormously for KNN. If one feature ranges from 0 to 1000 and another from 0 to 1, the first feature will dominate every distance calculation simply because its raw values are larger. Always standardise or normalise features before using KNN so that every dimension contributes fairly to the distance.

## Visual

<div style="display:flex;justify-content:center;margin:1.5rem 0;">
<svg width="320" height="280" viewBox="0 0 320 280" xmlns="http://www.w3.org/2000/svg" style="font-family:sans-serif;background:#f8f9fa;border-radius:8px;border:1px solid #dee2e6;">

  <!-- Axis lines -->
  <line x1="40" y1="240" x2="300" y2="240" stroke="#aaa" stroke-width="1.5"/>
  <line x1="40" y1="240" x2="40" y2="20" stroke="#aaa" stroke-width="1.5"/>
  <text x="160" y="268" text-anchor="middle" font-size="11" fill="#666">Feature 1</text>
  <text x="12" y="135" text-anchor="middle" font-size="11" fill="#666" transform="rotate(-90,12,135)">Feature 2</text>

  <!-- Class A dots (blue) -->
  <circle cx="80"  cy="80"  r="7" fill="#4e9af1" stroke="#2c74d8" stroke-width="1.5"/>
  <circle cx="100" cy="110" r="7" fill="#4e9af1" stroke="#2c74d8" stroke-width="1.5"/>
  <circle cx="70"  cy="140" r="7" fill="#4e9af1" stroke="#2c74d8" stroke-width="1.5"/>
  <circle cx="120" cy="70"  r="7" fill="#4e9af1" stroke="#2c74d8" stroke-width="1.5"/>
  <circle cx="60"  cy="170" r="7" fill="#4e9af1" stroke="#2c74d8" stroke-width="1.5"/>
  <circle cx="90"  cy="50"  r="7" fill="#4e9af1" stroke="#2c74d8" stroke-width="1.5"/>

  <!-- Class B dots (orange) -->
  <circle cx="220" cy="80"  r="7" fill="#f4a34a" stroke="#d6820a" stroke-width="1.5"/>
  <circle cx="240" cy="130" r="7" fill="#f4a34a" stroke="#d6820a" stroke-width="1.5"/>
  <circle cx="260" cy="90"  r="7" fill="#f4a34a" stroke="#d6820a" stroke-width="1.5"/>
  <circle cx="210" cy="160" r="7" fill="#f4a34a" stroke="#d6820a" stroke-width="1.5"/>
  <circle cx="270" cy="170" r="7" fill="#f4a34a" stroke="#d6820a" stroke-width="1.5"/>
  <circle cx="250" cy="55"  r="7" fill="#f4a34a" stroke="#d6820a" stroke-width="1.5"/>

  <!-- One orange neighbour closer to query (to make k=3 mix: 2 blue, 1 orange) -->
  <circle cx="165" cy="145" r="7" fill="#f4a34a" stroke="#d6820a" stroke-width="1.5"/>

  <!-- Two blue neighbours close to query -->
  <circle cx="145" cy="115" r="7" fill="#4e9af1" stroke="#2c74d8" stroke-width="1.5"/>
  <circle cx="150" cy="155" r="7" fill="#4e9af1" stroke="#2c74d8" stroke-width="1.5"/>

  <!-- k=3 radius circle (dashed) centred on query point -->
  <circle cx="160" cy="135" r="38" fill="none" stroke="#555" stroke-width="1.5" stroke-dasharray="5,4"/>

  <!-- Lines from query to 3 nearest neighbours -->
  <line x1="160" y1="135" x2="145" y2="115" stroke="#4e9af1" stroke-width="1.5" stroke-dasharray="4,3"/>
  <line x1="160" y1="135" x2="150" y2="155" stroke="#4e9af1" stroke-width="1.5" stroke-dasharray="4,3"/>
  <line x1="160" y1="135" x2="165" y2="145" stroke="#f4a34a" stroke-width="1.5" stroke-dasharray="4,3"/>

  <!-- Query point (green diamond) -->
  <polygon points="160,117 172,135 160,153 148,135" fill="#2ecc71" stroke="#1a9e53" stroke-width="2"/>

  <!-- Legend -->
  <circle cx="55" cy="256" r="6" fill="#4e9af1" stroke="#2c74d8" stroke-width="1.5"/>
  <text x="66" y="260" font-size="11" fill="#444">Class A</text>
  <circle cx="120" cy="256" r="6" fill="#f4a34a" stroke="#d6820a" stroke-width="1.5"/>
  <text x="131" y="260" font-size="11" fill="#444">Class B</text>
  <polygon points="186,250 192,256 186,262 180,256" fill="#2ecc71" stroke="#1a9e53" stroke-width="1.5"/>
  <text x="198" y="260" font-size="11" fill="#444">Query (k=3)</text>

  <!-- Prediction label -->
  <text x="160" y="14" text-anchor="middle" font-size="11" fill="#333" font-weight="bold">2 blue, 1 orange → predicts Class A</text>
</svg>
</div>

## The Math

$$d(\mathbf{x}, \mathbf{x'}) = \sqrt{\sum_{j=1}^{p}(x_j - x'_j)^2}$$

> **In plain English:** The distance between two points is the square root of the sum of squared differences in each feature — ordinary Euclidean distance. The $k$ points with the smallest distance to the query point are the neighbours that vote.

<details>
<summary>Show the derivation</summary>

Euclidean distance is the $\ell_2$ norm $\|\mathbf{x} - \mathbf{x'}\|_2$. It is the most common choice, but other metrics are widely used too. Manhattan distance uses the $\ell_1$ norm — the sum of absolute differences rather than squared differences — and is more robust to outliers in individual features. Minkowski distance generalises both as the $\ell_p$ norm for any $p \geq 1$.

For text and other sparse high-dimensional data, cosine similarity is often preferred because it measures the angle between vectors rather than their absolute separation, which makes it invariant to the magnitude of feature values.

The choice of metric can significantly affect which points are considered "nearest," and there is no universally correct answer — the best metric depends on the structure of your data. One important caveat applies to all metrics: in very high dimensions, the distances between points tend to become nearly equal. This is known as the **curse of dimensionality**, and it is one of the main reasons KNN struggles when $p$ is large. As the number of features grows, more and more data is needed to keep the neighbourhood dense enough to be meaningful.

</details>

## How It Learns

KNN has no training phase in the traditional sense. The algorithm simply stores the entire training set in memory, and all computation is deferred to prediction time. When a prediction is requested, the algorithm computes the distance from the query point to every stored point, sorts them by distance, selects the $k$ nearest, and returns the majority class among those neighbours.

The computational cost of this brute-force approach is $O(n \cdot p)$ per prediction, where $n$ is the number of training points and $p$ is the number of features. For large datasets this becomes expensive, because every new query must scan the entire training set. In practice, data structures like KD-trees and ball trees organise the training points spatially, which can reduce the search cost to $O(p \log n)$ for low-dimensional data. Scikit-learn selects the appropriate algorithm automatically based on the size and dimensionality of the dataset.

## When to Use It

KNN works well when the decision boundary is irregular and the dataset is small enough that prediction time is acceptable. It makes no assumptions about the underlying data distribution, which gives it a natural flexibility that parametric models lack. It also adapts seamlessly to multi-class problems — there is nothing special to change, because the majority vote mechanism handles any number of classes.

The main limitations are prediction-time cost rather than training-time cost, sensitivity to feature scale and to irrelevant features that pollute the distance calculation, and degraded performance as the number of dimensions grows. KNN is often used as a baseline to test whether a more complex model is actually adding value. It is also a natural fit for problems where the training data changes frequently, since adding new training examples requires nothing more than appending them to the stored set — there is no model to retrain.

## Try It Yourself

```python
from sklearn.datasets import load_iris
from sklearn.neighbors import KNeighborsClassifier
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.metrics import accuracy_score

# Load the Iris dataset
data = load_iris()
X, y = data.data, data.target

# Split into train and test sets
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)

# Always scale features before KNN
scaler = StandardScaler()
X_train = scaler.fit_transform(X_train)
X_test = scaler.transform(X_test)

# Train with k=5
model = KNeighborsClassifier(n_neighbors=5)
model.fit(X_train, y_train)

predictions = model.predict(X_test)
print(f"Accuracy: {accuracy_score(y_test, predictions) * 100:.1f}%")
```

Expected output:

```
Accuracy: 100.0%
```

## Key Takeaways

KNN is one of the most intuitive algorithms in machine learning — to classify a new point, just find its neighbours and take a vote. Its simplicity is also its limitation: it stores all training data, prediction time scales with the dataset size, and it struggles in high dimensions. Always standardise features before using KNN, and use cross-validation to choose $k$. For small, well-scaled datasets with non-linear boundaries, it can be surprisingly competitive.

---

[← Support Vector Machines](svm){: .btn } [Next → Naive Bayes](naive-bayes){: .btn .btn-primary }
