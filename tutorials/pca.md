---
layout: default
title: PCA
parent: ML Basics
nav_order: 13
---

# Principal Component Analysis (PCA)

## What is it?

Principal Component Analysis (PCA) is a technique that reduces the number of features in a dataset by finding the directions along which the data varies most. Those directions, called the principal components, are ordered from most to least variance, so you can keep just the first few and discard the rest without losing too much information. It's the most widely used dimensionality reduction technique in machine learning.

---

## The Idea

Imagine a cloud of data points in 3D space, but they mostly lie close to a flat surface embedded in that space. PCA finds that surface, and any other lower-dimensional structure, by identifying the axes along which the data spreads out the most. The first principal component is the direction of maximum variance. The second is the direction of maximum remaining variance that's perpendicular to the first. And so on. By projecting all data points onto just the first two or three principal components, you can reduce 100 features to 2 while keeping most of the meaningful variation.

This is useful for two reasons. First, high-dimensional data is hard to visualise and slower to train on. PCA compresses it without supervision. No labels needed. Second, dimensions with very low variance often capture noise rather than signal, so discarding them can actually improve downstream model performance.

The components are linear combinations of the original features. Each one is a weighted average of all your input variables, with the weights chosen to maximise variance. These weights are the eigenvectors of the data's covariance matrix, and the amount of variance each component explains is given by the corresponding eigenvalue.

---

## Visual

<div style="display:flex; justify-content:center; margin: 2rem 0;">
<svg width="340" height="260" viewBox="0 0 340 260" xmlns="http://www.w3.org/2000/svg" style="font-family: sans-serif;">

  <!-- Background -->
  <rect width="340" height="260" fill="#f9f9fb" rx="10"/>

  <!-- Title -->
  <text x="170" y="22" text-anchor="middle" font-size="12" fill="#555" font-weight="bold">PCA: Finding the Axes of Most Variance</text>

  <!-- Data points — elongated ellipse cluster, tilted ~35 degrees -->
  <!-- Generated along a tilted ellipse: major axis ~130px, minor axis ~35px, center (170,135) -->
  <circle cx="107" cy="170" r="3.5" fill="#aaa" opacity="0.7"/>
  <circle cx="116" cy="162" r="3.5" fill="#aaa" opacity="0.7"/>
  <circle cx="122" cy="157" r="3.5" fill="#aaa" opacity="0.7"/>
  <circle cx="128" cy="150" r="3.5" fill="#aaa" opacity="0.7"/>
  <circle cx="133" cy="145" r="3.5" fill="#aaa" opacity="0.7"/>
  <circle cx="138" cy="141" r="3.5" fill="#aaa" opacity="0.7"/>
  <circle cx="143" cy="140" r="3.5" fill="#aaa" opacity="0.7"/>
  <circle cx="148" cy="137" r="3.5" fill="#aaa" opacity="0.7"/>
  <circle cx="154" cy="135" r="3.5" fill="#aaa" opacity="0.7"/>
  <circle cx="160" cy="133" r="3.5" fill="#aaa" opacity="0.7"/>
  <circle cx="165" cy="132" r="3.5" fill="#aaa" opacity="0.7"/>
  <circle cx="170" cy="132" r="3.5" fill="#aaa" opacity="0.7"/>
  <circle cx="176" cy="131" r="3.5" fill="#aaa" opacity="0.7"/>
  <circle cx="182" cy="131" r="3.5" fill="#aaa" opacity="0.7"/>
  <circle cx="188" cy="131" r="3.5" fill="#aaa" opacity="0.7"/>
  <circle cx="194" cy="132" r="3.5" fill="#aaa" opacity="0.7"/>
  <circle cx="200" cy="133" r="3.5" fill="#aaa" opacity="0.7"/>
  <circle cx="206" cy="135" r="3.5" fill="#aaa" opacity="0.7"/>
  <circle cx="212" cy="138" r="3.5" fill="#aaa" opacity="0.7"/>
  <circle cx="218" cy="141" r="3.5" fill="#aaa" opacity="0.7"/>
  <circle cx="224" cy="145" r="3.5" fill="#aaa" opacity="0.7"/>
  <circle cx="229" cy="150" r="3.5" fill="#aaa" opacity="0.7"/>
  <circle cx="234" cy="156" r="3.5" fill="#aaa" opacity="0.7"/>
  <!-- Upper edge scatter -->
  <circle cx="113" cy="160" r="3" fill="#bbb" opacity="0.55"/>
  <circle cx="130" cy="142" r="3" fill="#bbb" opacity="0.55"/>
  <circle cx="145" cy="133" r="3" fill="#bbb" opacity="0.55"/>
  <circle cx="158" cy="126" r="3" fill="#bbb" opacity="0.55"/>
  <circle cx="172" cy="123" r="3" fill="#bbb" opacity="0.55"/>
  <circle cx="186" cy="122" r="3" fill="#bbb" opacity="0.55"/>
  <circle cx="201" cy="124" r="3" fill="#bbb" opacity="0.55"/>
  <circle cx="215" cy="130" r="3" fill="#bbb" opacity="0.55"/>
  <circle cx="228" cy="140" r="3" fill="#bbb" opacity="0.55"/>
  <!-- Lower edge scatter -->
  <circle cx="115" cy="175" r="3" fill="#bbb" opacity="0.55"/>
  <circle cx="131" cy="160" r="3" fill="#bbb" opacity="0.55"/>
  <circle cx="146" cy="149" r="3" fill="#bbb" opacity="0.55"/>
  <circle cx="160" cy="142" r="3" fill="#bbb" opacity="0.55"/>
  <circle cx="174" cy="141" r="3" fill="#bbb" opacity="0.55"/>
  <circle cx="189" cy="141" r="3" fill="#bbb" opacity="0.55"/>
  <circle cx="203" cy="143" r="3" fill="#bbb" opacity="0.55"/>
  <circle cx="217" cy="150" r="3" fill="#bbb" opacity="0.55"/>
  <circle cx="229" cy="158" r="3" fill="#bbb" opacity="0.55"/>

  <!-- PC1 arrow: along the major axis of the ellipse (roughly horizontal, slight tilt) -->
  <!-- From (90, 177) to (255, 118) -->
  <defs>
    <marker id="arrowPC1" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto">
      <path d="M0,0 L0,6 L8,3 z" fill="#e05c2a"/>
    </marker>
    <marker id="arrowPC2" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto">
      <path d="M0,0 L0,6 L8,3 z" fill="#2a7ae0"/>
    </marker>
  </defs>
  <line x1="88" y1="180" x2="252" y2="117"
        stroke="#e05c2a" stroke-width="2.5" stroke-linecap="round"
        marker-end="url(#arrowPC1)"/>

  <!-- PC2 arrow: perpendicular to PC1, shorter, from centre (170,135) -->
  <line x1="170" y1="135" x2="188" y2="182"
        stroke="#2a7ae0" stroke-width="2.5" stroke-linecap="round"
        marker-end="url(#arrowPC2)"/>

  <!-- Labels -->
  <text x="258" y="114" font-size="11" fill="#e05c2a" font-weight="bold">PC1</text>
  <text x="258" y="126" font-size="10" fill="#e05c2a">(most variance)</text>

  <text x="192" y="190" font-size="11" fill="#2a7ae0" font-weight="bold">PC2</text>
  <text x="192" y="202" font-size="10" fill="#2a7ae0">(less variance)</text>

</svg>
</div>

---

## The Math

$$\mathbf{Z} = \mathbf{X} \mathbf{W}$$

where $\mathbf{W}$ is the matrix of eigenvectors (principal components) of the covariance matrix $\mathbf{\Sigma} = \frac{1}{n}\mathbf{X}^T\mathbf{X}$.

> **In plain English:** To project the data onto the principal components, you multiply the data matrix $\mathbf{X}$ by the matrix of component directions $\mathbf{W}$. Each column of $\mathbf{W}$ is an eigenvector of the covariance matrix, pointing in a direction of high variance.

<details>
<summary>Show the derivation</summary>

Centre the data by subtracting the mean of each feature. Then compute the covariance matrix $\mathbf{\Sigma} = \frac{1}{n}\mathbf{X}^T\mathbf{X}$.

Solve the eigenvalue problem $\mathbf{\Sigma}\mathbf{w} = \lambda\mathbf{w}$. The eigenvector with the largest eigenvalue $\lambda_1$ is the first principal component. It explains the most variance. Sort all eigenvectors by their eigenvalues in descending order, then take the top $k$ to form the projection matrix $\mathbf{W}$.

The fraction of total variance explained by component $i$ is $\lambda_i / \sum_j \lambda_j$. This is what a scree plot displays, and it tells you how many components to keep.

In practice, Singular Value Decomposition (SVD) of the data matrix is used instead of computing the covariance matrix explicitly, because SVD is numerically more stable and computationally efficient for large datasets.

</details>

---

## How It Learns

PCA is an unsupervised technique. It uses no labels. It centres the data, computes the covariance structure, and finds the eigenvectors of the covariance matrix. These eigenvectors define the principal component directions. To reduce dimensionality, you choose how many components to keep, typically guided by a scree plot showing cumulative explained variance, and then project the data by matrix multiplication.

There's no iterative training or loss function involved. PCA is a one-shot linear algebra operation: you run it once and get back a transformed dataset. This makes it extremely fast compared to iterative methods, and the result is fully deterministic given the same input data.

---

## When to Use It

PCA is useful as a preprocessing step when you have many correlated features and want to reduce noise, speed up training, or visualise high-dimensional data. It works best when the important structure in the data is roughly linear. When you need to visualise clusters in high-dimensional data, projecting onto the first two principal components and plotting them gives a quick, interpretable view that often reveals groupings at a glance.

The main limitation is that principal components are linear combinations of all features, so the reduced dimensions are not directly interpretable in the original feature space. If you need to explain what a component means in terms of individual variables, you can inspect the component loadings, but the story is rarely simple. For highly non-linear structure, techniques like t-SNE or UMAP may reveal patterns that PCA cannot.

---

## Try It Yourself

If you have not set up Python yet, start with the [Get Started guide](setup) first.

This code takes a dataset with 30 features and compresses it down to just 2 components, then checks how much information was kept.

Copy this into a cell and run it with Shift + Enter:

```python
from sklearn.datasets import load_breast_cancer    # 30-feature medical dataset
from sklearn.preprocessing import StandardScaler   # scale before PCA
from sklearn.decomposition import PCA              # the PCA model
import matplotlib.pyplot as plt                    # for plotting

# Load dataset: 569 samples, 30 features
data = load_breast_cancer()
X = data.data
y = data.target

# Scale features: PCA is sensitive to variance, so scaling matters
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)   # gives each feature mean=0, std=1

# Reduce 30 features to just 2 principal components
pca = PCA(n_components=2)
X_reduced = pca.fit_transform(X_scaled)   # finds the 2 directions of most variance

# How much variance do the two components explain?
print("Explained variance ratio:", pca.explained_variance_ratio_.round(3))
print(f"Total variance retained: {pca.explained_variance_ratio_.sum() * 100:.1f}%")

# Scatter plot of the two components, coloured by diagnosis
plt.figure(figsize=(8, 5))
for label, name in enumerate(data.target_names):
    mask = y == label
    plt.scatter(X_reduced[mask, 0], X_reduced[mask, 1], label=name, alpha=0.6)
plt.xlabel("Principal Component 1")
plt.ylabel("Principal Component 2")
plt.title("Breast Cancer Dataset: 30 Features reduced to 2 Components")
plt.legend()
plt.tight_layout()
plt.show()
```

**Expected output:**
```
Explained variance ratio: [0.443 0.19 ]
Total variance retained: 63.3%
```

A scatter plot appears with two overlapping but visibly separated clusters: malignant and benign tumours, projected onto two dimensions from the original 30 features.

**What each line does:**
- `StandardScaler()`: makes each feature contribute equally before we measure variance
- `PCA(n_components=2)`: creates a PCA model that will keep the 2 most informative directions
- `pca.fit_transform(X_scaled)`: finds the principal components and projects the data onto them
- `pca.explained_variance_ratio_`: shows what fraction of total variance each component captures
- `plt.scatter(...)`: plots each tumour as a dot, coloured by whether it's malignant or benign

**What just happened?**

Two numbers now describe each tumour instead of thirty. And those two numbers still capture 63.3% of the total variation in the original data. The scatter plot shows that even with this compression, the two tumour types are mostly separable. That's PCA at work.

---

## Key Takeaways

- PCA finds the directions in your data with the most variance and projects your data onto just those directions.
- The first two or three components often capture the majority of meaningful variation, making them useful for visualisation and noise reduction.
- It's unsupervised and linear, so it's fast to run and easy to interpret in terms of explained variance.
- Always scale features before running PCA, otherwise high-magnitude features will dominate.
- It's typically the first dimensionality reduction technique to try. For non-linear structure, consider t-SNE or UMAP.

---

[← K-Means Clustering](kmeans){: .btn } [Next → Intermediate Overview](intermediate){: .btn .btn-primary }
