---
layout: default
title: K-Means Clustering
parent: ML Foundations
nav_order: 9
---

# K-Means Clustering

## Plain English Intro

Imagine you work at a store and want to group your customers into segments — bargain hunters, premium shoppers, occasional buyers — without knowing these categories in advance. You just have their purchase history.

K-Means finds these natural groups automatically. You tell it how many groups (K) you want, and it figures out which customers belong together based on their similarities.

---

## How It Works

1. Pick K random starting points (called centroids)
2. Assign each data point to the nearest centroid
3. Move each centroid to the average position of its assigned points
4. Repeat steps 2–3 until the centroids stop moving

The result is K groups, each with a centroid at its center.

**Note:** K-Means is *unsupervised* — you don't provide labels. It discovers structure in the data on its own.

---

## When to Use It

**Good for:**
- Customer segmentation
- Grouping similar documents or images
- Compressing data by representing groups with their centroid

**Not ideal for:**
- When you don't know how many groups exist (try different K values)
- Clusters of very different sizes or non-circular shapes
- Data with many outliers (centroids get pulled toward them)

---

## Hands-On Code

Install:

```bash
pip install scikit-learn matplotlib numpy
```

```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.cluster import KMeans
from sklearn.datasets import make_blobs

# Create fake customer data: 3 natural groups
X, true_labels = make_blobs(n_samples=300, centers=3, random_state=42)

# Run K-Means with K=3
model = KMeans(n_clusters=3, random_state=42, n_init=10)
model.fit(X)

# Each point now has a cluster label (0, 1, or 2)
labels = model.labels_
centroids = model.cluster_centers_

print(f"Cluster sizes: {np.bincount(labels)}")
print(f"Centroid positions:\n{centroids.round(2)}")

# Visualize the clusters
plt.scatter(X[:, 0], X[:, 1], c=labels, cmap='viridis', alpha=0.6)
plt.scatter(centroids[:, 0], centroids[:, 1], c='red', marker='X', s=200, label='Centroids')
plt.title('K-Means Clustering: 3 Customer Groups')
plt.legend()
plt.show()
```

**Expected output:**
```
Cluster sizes: [100 100 100]
Centroid positions:
[[-5.49  2.7 ]
 [ 0.99  4.26]
 [-2.69 -2.34]]
```

---

## Key Takeaways

- K-Means is an unsupervised algorithm — no labels needed
- You must specify K (the number of clusters) in advance
- It works by iteratively assigning points to the nearest centroid and updating centroids
- Works best with roughly equal-sized, roughly circular clusters
- Try several values of K and pick the one that makes the most sense for your problem

---

[← Support Vector Machines](svm){: .btn } [Next → PCA](pca){: .btn .btn-primary }
