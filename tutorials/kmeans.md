---
layout: default
title: K-Means Clustering
parent: ML Basics
nav_order: 12
---

# K-Means Clustering

Spotify groups its 600 million users into listener types so it can recommend the right playlists. Nobody labelled each user manually. The algorithm found the groups on its own. That is clustering, and K-Means is the most popular way to do it.

---

## What is K-Means Clustering?

K-Means Clustering is a way to automatically group data into $k$ clusters, where similar items end up in the same group. You do not tell it what the groups should be. You just say how many groups you want, and the algorithm figures out the rest.

**New word: unsupervised** means the algorithm works without labels. It does not know what the "right answer" is. It just looks for natural groups in the data.

**New word: centroid** is the centre point of a cluster. Think of it as the average member of the group.

---

## A simple way to think about it

Imagine dropping 100 grains of rice onto a table at random. You want to sort them into 3 piles.

Here is what K-Means does. It places 3 imaginary pins on the table at random positions. Each grain of rice joins the pile belonging to the nearest pin. Then each pin moves to the middle of its pile. Then every grain re-checks which pin is now closest and rejoins accordingly. The pins keep moving until they stop.

The result is 3 tidy piles where each grain is grouped with the grains most similar to it. Nobody told the algorithm which grains belong together. It worked it out by repeatedly asking: "which centre is closest?"

The only question you have to answer beforehand is: how many piles do you want? That number is $k$.

---

## How it works, step by step

1. Choose how many clusters you want ($k$) and place $k$ starting centre points at random positions.
2. Assign every data point to the nearest centre.
3. Move each centre to the average position of all the points assigned to it.
4. Repeat steps 2 and 3 until no point changes its cluster assignment.
5. The algorithm has converged. The clusters are ready.

---

## See it visually

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

Each colour is a cluster. The star in the middle of each cluster is the centroid, the average position of all points in that group. Every dot has been assigned to the nearest star.

---

## The maths (do not panic)

K-Means tries to minimise this:

$$\arg\min_S \sum_{i=1}^{k} \sum_{\mathbf{x} \in S_i} \|\mathbf{x} - \boldsymbol{\mu}_i\|^2$$

> **In plain English:** For each cluster $S_i$, measure the distance from every point in it to the cluster's centre $\boldsymbol{\mu}_i$. Square those distances and add them all up. K-Means tries to find the grouping that makes this total as small as possible. Smaller means every point is close to its cluster's centre, which means tighter, cleaner clusters.

<details>
<summary>Show more detail</summary>

Finding the perfect grouping is extremely hard to do in one go. K-Means takes a shortcut by alternating between two steps, each of which improves the answer.

In the assignment step, every point is sent to its nearest centre. This is the best possible assignment given the current centres.

In the update step, each centre moves to the average position of its assigned points. This is the best possible position for a centre given the current assignment.

Because each step makes the total squared distance smaller (or keeps it the same), and because the total cannot go below zero, the loop is guaranteed to stop eventually.

One practical tip: always run K-Means several times with different random starting positions and keep the best result. This reduces the chance of landing in a poor solution caused by unlucky starting positions.

</details>

---

## Run the code yourself

This code groups iris flowers into 3 clusters without looking at the species labels. Then it checks how well those discovered groups match the true species.

**Step 1:** Open [Google Colab](https://colab.research.google.com) and create a new notebook.

**Step 2:** Copy this code into a cell:

```python
# Import the tools we need
from sklearn.cluster import KMeans                      # the clustering model
from sklearn.datasets import load_iris                  # 150 flowers, 4 measurements each
from sklearn.preprocessing import StandardScaler        # rescale features before clustering
from sklearn.metrics import adjusted_rand_score         # measures how good the clusters are

# Load iris data - we will NOT use the species labels during clustering
iris = load_iris()
X = iris.data
y_true = iris.target   # we will only use this at the end to check our results

# Rescale features: K-Means uses distances, so fair scaling matters
X_scaled = StandardScaler().fit_transform(X)

# Create and run K-Means: find 3 clusters, try 10 different starting positions
km = KMeans(n_clusters=3, init='k-means++', n_init=10, random_state=42)
km.fit(X_scaled)   # runs the assign-then-move loop until clusters stop changing

# Report how tight the clusters are and how well they match true species
print(f"Inertia: {km.inertia_:.2f}")   # lower = tighter clusters
print(f"Adjusted Rand Score: {adjusted_rand_score(y_true, km.labels_):.3f}")  # 1.0 = perfect match
```

**Step 3:** Press **Shift + Enter** to run it.

You should see:
```
Inertia: 139.82
Adjusted Rand Score: 0.730
```

**What each line does:**
- `load_iris()`: loads 150 flower measurements (the labels are set aside and not used during clustering)
- `StandardScaler().fit_transform(X)`: rescales all measurements so none dominate the distance calculation
- `KMeans(n_clusters=3, init='k-means++')`: creates a K-Means model, with smarter-than-random starting positions
- `n_init=10`: runs the whole process 10 times with different starts, then keeps the best result
- `km.fit(X_scaled)`: runs the assign-and-move loop until the clusters stop changing
- `km.inertia_`: the total squared distance from each point to its cluster centre (lower is better)
- `adjusted_rand_score(...)`: compares the discovered clusters to the true species labels. A score of 1.0 is a perfect match and 0 is no better than random.

**What just happened?**

The algorithm never saw the flower species labels. It just looked at 4 measurements and grouped similar flowers together. The Adjusted Rand Score of 0.73 means its groupings match the true species much better than chance. It is not perfect because two of the three iris species look very similar in measurement space, but it found real structure without any guidance.

---

## Quick recap

- K-Means groups data into $k$ clusters by repeatedly assigning each point to the nearest centre and then moving the centre to the average of its group.
- You must choose $k$ before you start. A common approach is to try several values and pick the one where adding more clusters stops helping much.
- Always rescale your features first. K-Means uses distances, so features with large numbers will dominate otherwise.
- It works best when the natural groups in the data are roughly round and similar in size.
- It is usually the first clustering method to try on any new dataset because it is fast and easy to understand.

---

[← Naive Bayes](naive-bayes){: .btn } [Next → PCA](pca){: .btn .btn-primary }
