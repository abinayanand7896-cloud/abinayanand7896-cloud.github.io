---
layout: default
title: PCA
parent: ML Basics
nav_order: 13
---

# Principal Component Analysis (PCA)

Think about a photo of your face. It contains millions of pixel values, but most of them repeat the same basic information. Your face can be described with far fewer numbers without losing the important details. PCA is the technique that figures out how to do that compression.

---

## What is PCA?

PCA (Principal Component Analysis) is a tool that takes a dataset with many features and finds a smaller set of features that captures most of the same information. It does this by discovering the directions in which the data varies the most.

**New word: feature** is just one piece of information about each example. A flower might have four features: petal length, petal width, sepal length, and sepal width. A medical dataset might have thirty features per patient.

**New word: variance** means how much a set of values spreads out. If everyone in a class scores exactly 50, the variance is zero. If scores range from 0 to 100, the variance is high.

---

## A simple way to think about it

Imagine you are looking at a long, thin sausage-shaped cloud of data points floating in 3D space.

If you shine a light from the side, the shadow on the wall is a thin sliver. You lose almost all the information. But if you shine the light from one end of the sausage, the shadow is a fat oval that shows you the full spread of the data.

PCA finds the direction that makes the biggest, most informative shadow. That direction is called the first principal component. It captures the most variation in the data.

Then PCA finds the second-best direction (at a right angle to the first), and so on. By keeping just the first two or three directions and throwing away the rest, you can represent 30 features with just 2 numbers while keeping most of the meaningful variation.

---

## How it works, step by step

1. Subtract the average of each feature so all features are centred around zero.
2. Rescale features so no single one dominates just because its numbers are bigger.
3. Find the direction in the data where the points spread out the most. That is the first principal component.
4. Find the next most-spread direction that is at right angles to the first. That is the second component.
5. Repeat until you have as many components as you want, then keep only the top few.
6. Replace the original features with these new components.

---

## See it visually

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

The grey dots are the data points. The orange arrow (PC1) points in the direction where the data spreads out most. The blue arrow (PC2) points in the next-best direction, at right angles to the first. By keeping just these two arrows, you can represent the whole cloud of data in 2D instead of however many dimensions you started with.

---

## The maths (do not panic)

Here is the formula for projecting data onto the principal components:

$$\mathbf{Z} = \mathbf{X} \mathbf{W}$$

where $\mathbf{W}$ is the matrix of eigenvectors (principal components) of the covariance matrix $\mathbf{\Sigma} = \frac{1}{n}\mathbf{X}^T\mathbf{X}$.

> **In plain English:** You take all your data ($\mathbf{X}$) and multiply it by a set of directions ($\mathbf{W}$). Each direction in $\mathbf{W}$ points along one axis of maximum spread. Multiplying rotates and compresses the data so the most important variation ends up in the first column of the result.

<details>
<summary>Show more detail</summary>

First, centre the data by subtracting the average of each feature. Then compute the covariance matrix, which is a table that measures how much each pair of features varies together.

Next, find the eigenvectors of that matrix. An eigenvector is a special direction: when you apply the covariance matrix to it, the direction stays the same and only the length changes. The eigenvector with the largest length change is PC1 because it captures the most spread. Sort all eigenvectors by their length changes from largest to smallest, then keep only the top $k$ to form your projection.

The fraction of total variance explained by each component tells you how much information you kept. You can plot these fractions and look for the point where adding more components stops helping much.

</details>

---

## Run the code yourself

This code takes a medical dataset with 30 features per patient and compresses it down to just 2 numbers per patient. Then it plots those 2 numbers to see if different diagnoses naturally separate into different regions.

**Step 1:** Open [Google Colab](https://colab.research.google.com) and create a new notebook.

**Step 2:** Copy this code into a cell:

```python
# Import the tools we need
from sklearn.datasets import load_breast_cancer    # a dataset with 30 measurements per patient
from sklearn.preprocessing import StandardScaler   # rescale features before PCA
from sklearn.decomposition import PCA              # the PCA tool
import matplotlib.pyplot as plt                    # for drawing the scatter plot

# Load dataset: 569 patients, 30 measurements each, labelled malignant or benign
data = load_breast_cancer()
X = data.data     # the 30 measurements
y = data.target   # 0 = malignant, 1 = benign

# Rescale features: PCA works by measuring spread, so scaling matters
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)   # each feature now has mean=0 and spread=1

# Compress 30 features down to just 2
pca = PCA(n_components=2)
X_reduced = pca.fit_transform(X_scaled)   # finds 2 directions of most spread

# How much information did we keep?
print("Variance kept by each component:", pca.explained_variance_ratio_.round(3))
print(f"Total variance kept: {pca.explained_variance_ratio_.sum() * 100:.1f}%")

# Plot the compressed data, coloured by diagnosis
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

**Step 3:** Press **Shift + Enter** to run it.

You should see:
```
Variance kept by each component: [0.443 0.19 ]
Total variance kept: 63.3%
```

A scatter plot will appear showing two overlapping but clearly separated clusters: malignant and benign tumours, each described by just 2 numbers instead of 30.

**What each line does:**
- `load_breast_cancer()`: loads a dataset of 569 patients with 30 tumour measurements each
- `StandardScaler()`: rescales each measurement so none of them dominate just because their numbers are bigger
- `PCA(n_components=2)`: creates a PCA tool that will keep the 2 most informative directions
- `pca.fit_transform(X_scaled)`: finds those 2 directions and converts the data to use them
- `pca.explained_variance_ratio_`: tells you what fraction of total information each component captures
- `plt.scatter(...)`: plots each patient as a dot, coloured by their diagnosis

**What just happened?**

Each patient is now described by just 2 numbers instead of 30. Those 2 numbers still capture 63.3% of all the variation in the original data. The scatter plot shows that even with this heavy compression, the two tumour types mostly separate into different regions. That is PCA reducing noise and revealing structure that was hidden in 30 dimensions.

---

## Quick recap

- PCA finds the directions in your data where the points spread out most, then uses only those directions to describe the data.
- This lets you go from 30 features to 2 (or 3) while keeping most of the meaningful variation.
- It is useful for making data easier to visualise, faster to train on, and less noisy.
- Always rescale your features before running PCA, otherwise measurements with large numbers will dominate.
- It is usually the first technique to try when you have too many features and want to simplify your data.

---

[← K-Means Clustering](kmeans){: .btn } [Next → Intermediate Overview](intermediate){: .btn .btn-primary }
