---
layout: default
title: K-Means Clustering
parent: ML Basics
nav_order: 12
---

# K-Means Clustering

## What is it?

K-Means Clustering is an unsupervised algorithm that partitions data into $k$ groups by repeatedly assigning each point to its nearest cluster centre (centroid) and moving the centroid to the mean of its assigned points. It iterates until the assignments stop changing. Unlike classification, there are no labels. The algorithm discovers structure in the data on its own.

---

## The Idea

K-Means works through a beautifully simple two-step loop. First, every data point is assigned to the centroid it's closest to. Whichever cluster centre sits nearest in space claims that point. Second, each centroid is relocated to the mean position of all the points that were just assigned to it. The centroid literally moves to the "centre of gravity" of its current members. These two steps repeat until no point changes its cluster assignment.

Because each step nudges the configuration toward a lower total within-cluster sum of squared distances, the algorithm is guaranteed to stop. But it may settle at a local minimum rather than the global best solution. Where the centroids start out matters enormously. Random initialisation can land them in unfortunate positions and produce lopsided or overlapping clusters. K-Means++ was introduced to address this: it spreads the initial centroids out by choosing each successive starting point with a probability proportional to its squared distance from the nearest already-chosen centroid. This makes convergence faster and the final result more reliable.

The other key challenge is choosing $k$ before you begin. The elbow method offers a practical heuristic: run K-Means for several values of $k$, record the total inertia (within-cluster sum of squares) for each, and plot the results. The curve typically drops steeply at first and then flattens out. The "elbow", the value of $k$ where the improvement starts to level off, is a good candidate for the right number of clusters.

---

## Visual

<div style="display:flex; flex-direction:column; align-items:center; margin: 1.5rem 0;">
<svg width="320" height="260" viewBox="0 0 320 260" xmlns="http://www.w3.org/2000/svg" style="background:#f8f9fa; border-radius:8px; border:1px solid #dee2e6;">

  <!-- Cluster 1: blue dots, upper-left -->
  <circle cx="55" cy="55" r="5" fill="#4C8BE2" opacity="0.85"/>
  <circle cx="70" cy="40" r="5" fill="#4C8BE2" opacity="0.85"/>
  <circle cx="48" cy="72" r="5" fill="#4C8BE2" opacity="0.85"/>
  <circle cx="80" cy="60" r="5" fill="#4C8BE2" opacity="0.85"/>
  <circle cx="62" cy="85" r="5" fill="#4C8BE2" opacity="0.85"/>
  <circle cx="40" cy="48" r="5" fill="#4C8BE2" opacity="0.85"/>
  <circle cx="90" cy="45" r="5" fill="#4C8BE2" opacity="0.85"/>
  <circle cx="75" cy="78" r="5" fill="#4C8BE2" opacity="0.85"/>
  <circle cx="50" cy="92" r="5" fill="#4C8BE2" opacity="0.85"/>
  <circle cx="35" cy="65" r="5" fill="#4C8BE2" opacity="0.85"/>
  <!-- Cluster 1 centroid: blue star -->
  <polygon points="63,52 66,62 76,62 68,68 71,78 63,72 55,78 58,68 50,62 60,62"
           fill="#1a5fba" stroke="#fff" stroke-width="1.5"/>

  <!-- Cluster 2: orange dots, lower-centre -->
  <circle cx="148" cy="185" r="5" fill="#E8812A" opacity="0.85"/>
  <circle cx="165" cy="200" r="5" fill="#E8812A" opacity="0.85"/>
  <circle cx="135" cy="205" r="5" fill="#E8812A" opacity="0.85"/>
  <circle cx="175" cy="185" r="5" fill="#E8812A" opacity="0.85"/>
  <circle cx="155" cy="218" r="5" fill="#E8812A" opacity="0.85"/>
  <circle cx="130" cy="192" r="5" fill="#E8812A" opacity="0.85"/>
  <circle cx="170" cy="210" r="5" fill="#E8812A" opacity="0.85"/>
  <circle cx="145" cy="225" r="5" fill="#E8812A" opacity="0.85"/>
  <circle cx="185" cy="198" r="5" fill="#E8812A" opacity="0.85"/>
  <circle cx="160" cy="175" r="5" fill="#E8812A" opacity="0.85"/>
  <!-- Cluster 2 centroid: orange star -->
  <polygon points="158,190 161,200 171,200 163,206 166,216 158,210 150,216 153,206 145,200 155,200"
           fill="#c45e00" stroke="#fff" stroke-width="1.5"/>

  <!-- Cluster 3: green dots, upper-right -->
  <circle cx="255" cy="50" r="5" fill="#3BAA5C" opacity="0.85"/>
  <circle cx="272" cy="68" r="5" fill="#3BAA5C" opacity="0.85"/>
  <circle cx="240" cy="70" r="5" fill="#3BAA5C" opacity="0.85"/>
  <circle cx="285" cy="48" r="5" fill="#3BAA5C" opacity="0.85"/>
  <circle cx="260" cy="88" r="5" fill="#3BAA5C" opacity="0.85"/>
  <circle cx="248" cy="42" r="5" fill="#3BAA5C" opacity="0.85"/>
  <circle cx="278" cy="82" r="5" fill="#3BAA5C" opacity="0.85"/>
  <circle cx="265" cy="58" r="5" fill="#3BAA5C" opacity="0.85"/>
  <circle cx="290" cy="72" r="5" fill="#3BAA5C" opacity="0.85"/>
  <circle cx="242" cy="90" r="5" fill="#3BAA5C" opacity="0.85"/>
  <!-- Cluster 3 centroid: green star -->
  <polygon points="265,58 268,68 278,68 270,74 273,84 265,78 257,84 260,74 252,68 262,68"
           fill="#1e7a3c" stroke="#fff" stroke-width="1.5"/>

  <!-- Legend -->
  <circle cx="20" cy="240" r="5" fill="#4C8BE2"/>
  <text x="30" y="244" font-size="11" fill="#444" font-family="sans-serif">Cluster 1</text>
  <circle cx="95" cy="240" r="5" fill="#E8812A"/>
  <text x="105" y="244" font-size="11" fill="#444" font-family="sans-serif">Cluster 2</text>
  <circle cx="170" cy="240" r="5" fill="#3BAA5C"/>
  <text x="180" y="244" font-size="11" fill="#444" font-family="sans-serif">Cluster 3</text>
  <polygon points="248,237 250,243 256,243 251,247 253,253 248,249 243,253 245,247 240,243 246,243"
           fill="#555" stroke="#fff" stroke-width="1"/>
  <text x="260" y="248" font-size="11" fill="#444" font-family="sans-serif">Centroid</text>

</svg>
<p style="font-size:0.85rem; color:#666; margin-top:0.5rem; text-align:center;">K=3: each colour is a cluster, stars are centroids</p>
</div>

---

## The Math

$$\arg\min_S \sum_{i=1}^{k} \sum_{\mathbf{x} \in S_i} \|\mathbf{x} - \boldsymbol{\mu}_i\|^2$$

> **In plain English:** Find the assignment of points to clusters $S_1, \dots, S_k$ that minimises the total squared distance from each point to its cluster's centre. The centre $\boldsymbol{\mu}_i$ is the mean of all points in cluster $i$.

<details><summary>Show the derivation</summary>

This objective is NP-hard to optimise globally. Finding the true best partition over all possible assignments is computationally intractable for large datasets. Lloyd's algorithm sidesteps this by alternating between two exact sub-steps, each of which decreases (or leaves unchanged) the total objective.

In the assignment step, each point is sent to its nearest centroid. For a fixed set of centroids, this is the provably optimal assignment.

In the update step, each centroid is moved to the mean of its assigned points. For a fixed assignment, the mean is the unique point that minimises the sum of squared distances to a set of points, so this step is also optimal given the current assignment.

Because each step is individually optimal and the objective is bounded below by zero, the alternation is guaranteed to converge to a local minimum. Different initialisations can converge to different local minima, which is why K-Means++ and multiple random restarts (controlled by `n_init` in sklearn) are standard practice. The run with the lowest final inertia is returned as the result.

</details>

---

## How It Learns

K-Means alternates between two steps until convergence. In the assignment step, every training point is assigned to the centroid it's closest to, measured by Euclidean distance. In the update step, each centroid is moved to the mean position of all the points assigned to it. The centroid is literally redefined as the average of its current members.

Because each step strictly decreases (or leaves unchanged) the total within-cluster sum of squares, and because there are only finitely many possible ways to assign $n$ points to $k$ clusters, the algorithm is guaranteed to converge in a finite number of iterations. In practice it's usually fast, often converging in tens of iterations even on large datasets.

Sklearn runs K-Means multiple times with different initialisations, controlled by the `n_init` parameter, and returns the solution with the lowest inertia. This guards against unlucky starting positions leading to a poor local minimum.

---

## When to Use It

K-Means is the go-to first attempt for clustering tabular data. It's fast, scales well to large datasets, and produces interpretable results. The centroids describe the "typical" member of each cluster, making them easy to explain to stakeholders.

The main limitations are that you must choose $k$ before you begin, the algorithm assumes roughly spherical clusters of similar size, and it uses Euclidean distance so feature scaling matters. If your features have very different ranges, standardise them first, otherwise the clustering will be dominated by whichever feature has the largest absolute values.

For data with elongated, curved, or irregularly shaped clusters, DBSCAN or Gaussian Mixture Models will usually fit better.

---

## Try It Yourself

If you have not set up Python yet, start with the [Get Started guide](setup) first.

This code clusters the iris flowers into 3 groups without using the species labels. Then it checks how well those groups match the true species.

Copy this into a cell and run it with Shift + Enter:

```python
from sklearn.cluster import KMeans                      # the clustering model
from sklearn.datasets import load_iris                  # classic flower dataset
from sklearn.preprocessing import StandardScaler        # scale features
from sklearn.metrics import adjusted_rand_score         # measure cluster quality

# Load iris: ignore the labels during clustering (that's the whole point of unsupervised)
iris = load_iris()
X = iris.data
y_true = iris.target   # we'll use this later only to evaluate quality

# Scale features: distance-based algorithms are sensitive to feature ranges
X_scaled = StandardScaler().fit_transform(X)

# Fit K-Means: find 3 clusters, use K-Means++ initialisation for better starts
km = KMeans(n_clusters=3, init='k-means++', n_init=10, random_state=42)
km.fit(X_scaled)   # alternates assign/update until convergence

print(f"Inertia: {km.inertia_:.2f}")   # total within-cluster sum of squares
print(f"Adjusted Rand Score: {adjusted_rand_score(y_true, km.labels_):.3f}")  # 1.0 = perfect, 0 = random
```

Expected output:
```
Inertia: 139.82
Adjusted Rand Score: 0.730
```

**What each line does:**
- `StandardScaler().fit_transform(X)`: rescales all features to the same range before clustering
- `KMeans(n_clusters=3, init='k-means++')`: creates a K-Means model with smarter starting positions
- `n_init=10`: runs the whole algorithm 10 times with different random starts, keeps the best result
- `km.fit(X_scaled)`: runs the assign/update loop until clusters stop changing
- `km.inertia_`: total squared distance from each point to its centroid (lower is better)
- `adjusted_rand_score(y_true, km.labels_)`: compares the found clusters to the true species labels

**What just happened?**

The algorithm never saw the species labels, but it still found clusters that match them reasonably well. An Adjusted Rand Score of 0.73 means the clustering is much better than random (which would score near 0). It's not perfect because two iris species overlap heavily in feature space. But it found real structure without any supervision.

---

## Key Takeaways

- K-Means partitions data into $k$ clusters by alternating between assigning points to the nearest centroid and moving centroids to the mean of their assigned points.
- It's fast, interpretable, and guaranteed to converge, but it may find a local minimum rather than the global best.
- Use K-Means++ initialisation and run with multiple random starts for more reliable results.
- Always scale features before clustering. Euclidean distance is sensitive to unscaled magnitudes.
- Best suited to roughly spherical clusters of similar size. For irregular shapes, try DBSCAN or Gaussian Mixture Models.

---

[← Naive Bayes](naive-bayes){: .btn } [Next → PCA](pca){: .btn .btn-primary }
