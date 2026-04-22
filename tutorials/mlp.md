---
layout: default
title: Multilayer Perceptron
parent: Deep Learning
nav_order: 2
---

# Multilayer Perceptron (MLP)

## Plain English Intro

An MLP is the simplest kind of neural network — a stack of layers where every unit in one layer is connected to every unit in the next. It's the "vanilla" neural network. Before CNNs and RNNs, nearly all neural network work used this design.

Think of it as a pipeline: data goes in one end, passes through several transformation layers, and a prediction comes out the other end.

---

## How It Works

1. Input layer receives your data (one unit per feature)
2. Hidden layers apply learned transformations (matrix multiplication + activation function)
3. Output layer produces predictions (one unit per class for classification)
4. During training, backpropagation adjusts all weights to minimize prediction error

Common activation functions:
- **ReLU** — "If the value is negative, set it to 0. Otherwise, keep it." (Most widely used in hidden layers)
- **Sigmoid** — Squashes output to 0–1. Used in binary output layers.
- **Softmax** — Converts outputs to probabilities that sum to 1. Used for multi-class output layers.

---

## When to Use It

**Good for:**
- General classification and regression when you have enough data
- As a baseline neural network before trying more specialized architectures
- When input is a flat feature vector (not an image or sequence)

**Not ideal for:**
- Images (use CNN — it understands spatial patterns)
- Sequences (use RNN/LSTM — they understand order)

---

## Hands-On Code

Install:

```bash
pip install tensorflow
```

```python
import tensorflow as tf
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
import numpy as np

# Load Iris dataset
data = load_iris()
X = data.data.astype(np.float32)
y = tf.keras.utils.to_categorical(data.target, num_classes=3)  # One-hot encode labels

# Scale features
scaler = StandardScaler()
X = scaler.fit_transform(X)

# Split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Build the MLP
model = tf.keras.Sequential([
    tf.keras.layers.Input(shape=(4,)),           # 4 input features
    tf.keras.layers.Dense(16, activation='relu'), # Hidden layer: 16 units, ReLU
    tf.keras.layers.Dense(8, activation='relu'),  # Hidden layer: 8 units, ReLU
    tf.keras.layers.Dense(3, activation='softmax')# Output: 3 classes, probabilities
])

# Compile: define optimizer, loss function, and metric to track
model.compile(optimizer='adam', loss='categorical_crossentropy', metrics=['accuracy'])

# Train for 50 rounds (epochs)
model.fit(X_train, y_train, epochs=50, verbose=0)

# Evaluate on test set
loss, accuracy = model.evaluate(X_test, y_test, verbose=0)
print(f"Test Accuracy: {accuracy * 100:.1f}%")
```

**Expected output:**
```
Test Accuracy: 100.0%
```

---

## Key Takeaways

- MLP is the simplest neural network: input → hidden layers → output
- Each layer applies a learned transformation followed by an activation function
- ReLU is the default activation for hidden layers; Softmax for multi-class output
- Always scale your input features before training a neural network
- Adam is a good default optimizer — it adapts the learning rate automatically

---

[← What are Neural Networks?](neural-networks-intro){: .btn } [Next → CNN](cnn){: .btn .btn-primary }
