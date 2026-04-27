---
layout: default
title: Neural Networks
parent: Deep Learning
nav_order: 1
---

# Neural Networks

## What is it?

A neural network is a stack of layers, where each layer transforms its input by multiplying it by a set of weights, adding a bias, and passing the result through a non-linear activation function. The network learns by adjusting all its weights through backpropagation, propagating the error signal backwards from the output to every weight in the network. It is the foundation of almost all modern deep learning.

## The Idea

The inspiration is loosely biological — neurons in the brain fire when their inputs exceed a threshold, passing signals forward. In a neural network, each "neuron" computes a weighted sum of its inputs and passes the result through an activation function. A layer of neurons transforms an input vector into an output vector. Stack enough layers and the network can learn extraordinarily complex functions.

What makes neural networks powerful is the non-linearity. Without activation functions, stacking layers would just be stacking matrix multiplications — which collapses back to a single linear transformation. Non-linear activations like ReLU ($\max(0, x)$) or sigmoid allow the network to bend and warp the feature space into shapes a linear model could never achieve.

Training is what makes the whole thing work. The network makes a prediction, compares it to the true label via a loss function, and then propagates the error backwards through the network layer by layer using the chain rule. Each weight is nudged in the direction that reduces the loss — gradient descent in a space with potentially millions of dimensions.

## Visual

```mermaid
graph LR
  subgraph Input["Input Layer"]
    I1((x₁))
    I2((x₂))
    I3((x₃))
  end
  subgraph Hidden["Hidden Layer"]
    H1((h₁))
    H2((h₂))
    H3((h₃))
    H4((h₄))
  end
  subgraph Output["Output Layer"]
    O1((ŷ))
  end
  I1 & I2 & I3 --> H1 & H2 & H3 & H4
  H1 & H2 & H3 & H4 --> O1
```

## The Math

$$\mathbf{h} = f(\mathbf{W}\mathbf{x} + \mathbf{b})$$

where $\mathbf{W}$ is the weight matrix, $\mathbf{x}$ is the input vector, $\mathbf{b}$ is the bias vector, and $f$ is the activation function applied element-wise.

> **In plain English:** Each neuron computes a weighted combination of its inputs, adds a bias, and applies an activation function. A layer is just this operation applied in parallel for all its neurons.

<details><summary>Show the derivation</summary>

For a network with $L$ layers, the forward pass is:

$$\mathbf{a}^{(0)} = \mathbf{x} \quad \text{(input)}$$

Then for each layer $l = 1, \ldots, L$:

$$\mathbf{z}^{(l)} = \mathbf{W}^{(l)}\mathbf{a}^{(l-1)} + \mathbf{b}^{(l)} \quad \text{(linear combination)}$$

$$\mathbf{a}^{(l)} = f\!\left(\mathbf{z}^{(l)}\right) \quad \text{(activation)}$$

The final output is $\hat{y} = \mathbf{a}^{(L)}$. Training minimises a loss $\mathcal{L}(\hat{y}, y)$ by computing gradients $\partial\mathcal{L}/\partial\mathbf{W}^{(l)}$ via the chain rule (backpropagation) and updating weights with gradient descent:

$$\mathbf{W}^{(l)} \leftarrow \mathbf{W}^{(l)} - \eta \frac{\partial\mathcal{L}}{\partial\mathbf{W}^{(l)}}$$

</details>

## How It Learns

During training, data flows forward through the layers — the forward pass — producing a prediction. The loss function measures how wrong that prediction is relative to the true label, turning the error into a single scalar number.

Backpropagation then computes the gradient of the loss with respect to every weight in the network using the chain rule, working layer by layer from the output back to the input. A gradient descent step nudges every weight in the direction that reduces the loss. Repeat this process over many batches and epochs, and the loss gradually converges as the network settles into a set of weights that fit the training data.

## When to Use It

Neural networks excel at problems where classical machine learning tops out: images, audio, natural language, time series, and any domain where raw data contains rich structure that feature engineering struggles to capture. The trade-off is that they need more data, are harder to interpret, and require more computational resources to train.

For structured tabular data, gradient boosting usually outperforms a neural network and is far easier to tune. A good rule of thumb is to reach for a neural network when the input is high-dimensional and unstructured — pixels, waveforms, tokens — and to try simpler models first when working with clean, tabular features.

## Try It Yourself

```python
from sklearn.datasets import load_digits
from sklearn.neural_network import MLPClassifier
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler

# Load the digits dataset (8x8 images of handwritten digits)
digits = load_digits()
X, y = digits.data, digits.target

# Split and scale
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)
scaler = StandardScaler()
X_train = scaler.fit_transform(X_train)
X_test = scaler.transform(X_test)

# Train a small neural network
mlp = MLPClassifier(hidden_layer_sizes=(64, 32), max_iter=500, random_state=42)
mlp.fit(X_train, y_train)

accuracy = mlp.score(X_test, y_test)
print(f"Test accuracy: {accuracy:.4f}")
```

Expected output:

```
Test accuracy: 0.9806
```

## Key Takeaways

A neural network is a composition of linear transformations and non-linear activations, stacked layer by layer. Backpropagation and gradient descent are what allow the network to learn — the network adjusts millions of weights simultaneously to minimise a loss. Non-linearity is what makes the whole thing work: without it, the stack would collapse to a single linear model. Neural networks are the foundation of deep learning, and everything in this track builds directly on these ideas.

---

[← ML Basics](../ml-basics){: .btn } [Next → Multi-Layer Perceptron](mlp){: .btn .btn-primary }
