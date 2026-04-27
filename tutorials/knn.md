---
layout: default
title: K-Nearest Neighbours
parent: ML Basics
nav_order: 10
---

# K-Nearest Neighbours

Have you ever asked a friend "should I watch this show?" and they said "well, your three closest friends all love it, so you probably will too"? That is exactly how this algorithm works.

---

## What is K-Nearest Neighbours?

K-Nearest Neighbours (KNN) is a way for a computer to make decisions by looking at similar examples it has already seen. When it needs to classify something new, it finds the $k$ most similar items in its memory and takes a vote.

**New word: classify** means sorting something into a group or category. For example, deciding if an email is spam or not spam is a classification task.

**New word: k** is just a number you choose. If $k = 3$, the algorithm looks at the 3 most similar examples. If $k = 5$, it looks at 5.

---

## A simple way to think about it

Imagine you move to a new town and want to know if a neighbourhood is friendly. You do not know anyone yet.

So you walk around and find the 5 houses closest to the one you are considering. You knock on each door and ask: "Is this a friendly street?" Four say yes, one says no. You conclude: probably friendly.

That is KNN. You did not memorise a rule. You just looked at nearby examples and took a vote.

The only two things you need to decide are:
- **How many neighbours to ask (k):** Ask too few and one unusual person can sway you. Ask too many and you include people who live far away and are not really relevant.
- **How to measure "nearby":** In real life that means distance in metres. In machine learning it usually means measuring how different two sets of numbers are.

---

## How it works, step by step

1. The algorithm stores every training example it has seen, including its label (like "spam" or "not spam").
2. A new, unlabelled example arrives.
3. The algorithm measures the distance from that new example to every stored example.
4. It picks the $k$ closest examples (the nearest neighbours).
5. It takes a vote among those $k$ neighbours and predicts the most common answer.

---

## See it visually

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

The green diamond is the new unknown point. The dashed circle shows the 3 nearest neighbours. Two are blue and one is orange, so the algorithm votes: Class A wins. The new point gets labelled blue.

---

## The maths (do not panic)

Here is how the algorithm measures "distance" between two points:

$$d(\mathbf{x}, \mathbf{x'}) = \sqrt{\sum_{j=1}^{p}(x_j - x'_j)^2}$$

> **In plain English:** To find how far apart two examples are, subtract each feature value, square the differences so negatives become positive, add them all up, then take the square root. This is just the ruler distance you learned in school, extended to as many features as you have.

<details>
<summary>Show more detail</summary>

This formula is called Euclidean distance. It is the same formula you use to find the straight-line distance between two points on a map.

There are other ways to measure distance too. Manhattan distance adds up the differences without squaring them first. Cosine similarity measures the angle between two examples rather than their separation. Different tasks call for different measures, but Euclidean is the most common starting point.

One important warning: if one feature uses very large numbers (like salary in thousands) and another uses very small numbers (like age), the large-number feature will dominate the distance calculation unfairly. Always rescale your features before using KNN so every feature contributes equally.

</details>

---

## Run the code yourself

This code teaches KNN to identify which species of flower an iris belongs to, based on measurements of its petals and sepals. The model never writes a rule. It simply memorises all the training flowers and checks which ones are most similar when a new flower arrives.

**Step 1:** Open [Google Colab](https://colab.research.google.com) and create a new notebook.

**Step 2:** Copy this code into a cell:

```python
# Import the tools we need
from sklearn.datasets import load_iris                  # a built-in dataset of 150 flowers
from sklearn.neighbors import KNeighborsClassifier      # the KNN model
from sklearn.model_selection import train_test_split    # splits data into train and test
from sklearn.preprocessing import StandardScaler        # rescales features to be fair
from sklearn.metrics import accuracy_score              # measures how often we are right

# Load the Iris dataset (150 flowers, 4 measurements each, 3 species)
data = load_iris()
X, y = data.data, data.target

# Split data: 80% for training, 20% for testing
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)

# Rescale features so no single measurement dominates the distance calculation
scaler = StandardScaler()
X_train = scaler.fit_transform(X_train)   # learn the scale from training data
X_test = scaler.transform(X_test)         # apply the same scale to test data

# Create a KNN model that votes among 5 nearest neighbours
model = KNeighborsClassifier(n_neighbors=5)
model.fit(X_train, y_train)               # "training" just stores all the data

# Ask the model to predict species for the test flowers
predictions = model.predict(X_test)
print(f"Accuracy: {accuracy_score(y_test, predictions) * 100:.1f}%")
```

**Step 3:** Press **Shift + Enter** to run it.

You should see:
```
Accuracy: 100.0%
```

**What each line does:**
- `load_iris()`: loads a famous dataset of 150 iris flowers with 4 petal/sepal measurements each
- `train_test_split(...)`: shuffles the data and sets aside 20% to test the model later
- `StandardScaler()`: rescales each feature so distances are fair across all measurements
- `KNeighborsClassifier(n_neighbors=5)`: creates a KNN model that uses 5 neighbours for each vote
- `model.fit(X_train, y_train)`: stores all training flowers in memory (no actual learning happens yet)
- `model.predict(X_test)`: for each test flower, finds the 5 nearest training flowers and takes a vote
- `accuracy_score(...)`: compares predictions to the true species labels and reports the percentage correct

**What just happened?**

The model memorised 120 training flowers. When it saw each of the 30 test flowers, it found the 5 most similar training flowers and predicted the most common species among them. It got every single one right. Try changing `n_neighbors` to 1 or to 20 and see how the accuracy changes. That is the key trade-off: too few neighbours and one unusual example can throw you off; too many and you start including flowers that are not really similar at all.

---

## Quick recap

- KNN makes predictions by finding the $k$ most similar training examples and taking a majority vote among them.
- There is no training phase. The algorithm simply stores all examples and does its work when a new one arrives.
- Always rescale your features first, otherwise features with large numbers will dominate the distance calculation.
- Choosing $k$ is the main decision: small $k$ reacts to individual examples, large $k$ looks at broader patterns.
- It works surprisingly well on small datasets where the pattern in the data is clear and consistent.

---

[← Support Vector Machines](svm){: .btn } [Next → Naive Bayes](naive-bayes){: .btn .btn-primary }
