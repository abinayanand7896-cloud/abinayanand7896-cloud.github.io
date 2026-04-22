---
layout: default
title: What is Machine Learning?
parent: ML Foundations
nav_order: 1
---

# What is Machine Learning?

## Plain English Intro

Imagine you want to teach a child to recognize cats. You don't write rules like "has pointy ears, has whiskers, has fur." Instead, you show them thousands of cat photos and say "cat" each time. Eventually, they figure out the pattern themselves.

That's machine learning. Instead of writing rules by hand, you give a computer lots of examples and let it learn the pattern.

---

## How It Works

Machine learning has three core ideas:

1. **Data** — Examples you learn from (photos, numbers, text)
2. **Model** — The pattern-finding algorithm (e.g., Decision Tree, Neural Network)
3. **Training** — Feeding data to the model so it learns the pattern

After training, the model makes predictions on new data it has never seen.

### The Three Types of Machine Learning

| Type | What it does | Example |
|------|-------------|---------|
| **Supervised Learning** | Learns from labeled examples | Predict house prices, classify spam |
| **Unsupervised Learning** | Finds hidden patterns in unlabeled data | Group customers by behavior |
| **Reinforcement Learning** | Learns by trial and error with rewards | Game-playing AI, robots |

This site covers **supervised learning** (Levels 1–3) and touches on **unsupervised learning** (K-Means, PCA).

---

## When to Use Machine Learning

**Good for:**
- Problems where the rules are too complex to write by hand (face recognition)
- Problems with lots of data and a clear goal (spam detection)
- Tasks that improve with more examples

**Not ideal for:**
- Very small datasets (fewer than a few hundred examples)
- Simple rule-based problems (a few if/else statements would do)
- Cases where you must explain every single decision

---

## Hands-On: Your First ML Program

Install:

```bash
pip install scikit-learn
```

```python
# Import the tools we need
from sklearn.datasets import load_iris
from sklearn.tree import DecisionTreeClassifier

# Load a built-in dataset: 150 flower measurements, 3 species
data = load_iris()
X = data.data    # Features: petal/sepal length and width
y = data.target  # Labels: 0=setosa, 1=versicolor, 2=virginica

# Create and train a simple model
model = DecisionTreeClassifier()
model.fit(X, y)  # This is where learning happens

# Predict a new flower: [sepal length, sepal width, petal length, petal width]
new_flower = [[5.1, 3.5, 1.4, 0.2]]
prediction = model.predict(new_flower)

print(f"Predicted species: {data.target_names[prediction[0]]}")
```

**Expected output:**
```
Predicted species: setosa
```

Congratulations — you just ran your first ML program!

---

## Key Takeaways

- Machine learning lets computers learn patterns from examples instead of following hard-coded rules
- The three types are: supervised, unsupervised, and reinforcement learning
- Every ML workflow has three parts: data, a model, and training
- After training, the model predicts on new, unseen data

---

[Next → Linear Regression](linear-regression){: .btn .btn-primary }
