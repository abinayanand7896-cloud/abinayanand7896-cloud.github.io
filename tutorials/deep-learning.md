---
layout: default
title: What is Deep Learning?
parent: Deep Learning
nav_order: 3
---

# What is Deep Learning?

## What is it?

Deep learning is machine learning with neural networks that have many layers — deep enough that each layer learns increasingly abstract representations of the input. Rather than hand-engineering features, the network discovers useful representations on its own directly from raw data. It is what powers image recognition, language models, speech synthesis, and most modern AI systems.

## The Idea

A shallow neural network with one hidden layer can theoretically approximate any function, but in practice it needs an impractically large layer to do so for complex data. Deep networks use a different strategy: instead of building one very wide layer, they stack many thinner layers. Each layer transforms the representation produced by the layer below it, learning to extract progressively higher-level features.

In a CNN processing an image, the first layers might detect edges and colour patches. The next layers combine those into textures and simple shapes. Deeper still, the network recognises eyes, wheels, or fur. The final layers combine these object parts into full classifications. No one told the network to look for edges — it learned that representation because it was useful for minimising the loss.

This hierarchical representation learning is the core insight of deep learning. The features that intermediate layers discover are not engineered by a human; they are the features that the training data itself suggests are useful. This is why deep learning works so well on domains like images, audio, and text, where the right feature representation is far from obvious.

## Visual

```mermaid
graph TD
  Raw["Raw Pixels"] --> L1["Layer 1\nEdges & Colours"]
  L1 --> L2["Layer 2\nTextures & Shapes"]
  L2 --> L3["Layer 3\nObject Parts"]
  L3 --> L4["Layer 4\nFull Objects"]
  L4 --> Out["Classification: 🐱 Cat"]
```

## The Math

$$\mathbf{a}^{(L)} = f_L(\mathbf{W}^{(L)} f_{L-1}(\cdots f_1(\mathbf{W}^{(1)}\mathbf{x} + \mathbf{b}^{(1)}) \cdots) + \mathbf{b}^{(L)})$$

> **In plain English:** The output of a deep network is a long chain of transformations — each layer takes the previous layer's output, applies a linear transformation, and passes it through an activation function. The depth of this chain is what gives the network its expressive power.

<details><summary>Show the derivation</summary>

The vanishing gradient problem: for deep networks with sigmoid activations, $\partial\mathcal{L}/\partial\mathbf{W}^{(1)}$ involves a product of many Jacobians, each with entries $\leq 0.25$ for sigmoid. The product shrinks exponentially with depth. ReLU avoids this because its derivative is 1 (for positive inputs) rather than a fraction. Batch normalisation, residual connections (ResNets), and careful weight initialisation are additional tools that allow training to remain stable at 100+ layers.

</details>

## How It Learns

Deep networks are trained with the same backpropagation and gradient descent loop as shallow networks, but at a much larger scale. The key practical ingredients are GPU acceleration for parallel matrix multiplications, mini-batch gradient descent with Adam or SGD with momentum, regularisation techniques such as dropout, weight decay, and batch normalisation, and large labelled datasets.

Transfer learning is often used to offset the data requirements. A model pre-trained on a large dataset — ImageNet for vision, or a massive text corpus for language — has already learned general-purpose features. Fine-tuning such a model on a smaller task-specific dataset is far more efficient than training from scratch, and it is the reason that state-of-the-art performance is accessible even to practitioners without vast computational resources.

## When to Use It

Deep learning excels when data is rich and unstructured — images, audio, video, text, and sequences. It tends to underperform classical ML methods such as gradient boosting on structured tabular data unless the dataset is very large. The cost is real: deep models require more data, more compute, and more hyperparameter tuning than classical methods.

When a Random Forest or Gradient Boosting model performs adequately, it is usually the better choice. Deep learning is not a universal upgrade; it is a specialised tool that pays off when the data is complex, the quantity is large, and the features that matter are not obvious enough to engineer by hand.

## Try It Yourself

```python
import torch
import torch.nn as nn
import torch.optim as optim
from torchvision import datasets, transforms
from torch.utils.data import DataLoader

# A small deep network for MNIST
class DeepNet(nn.Module):
    def __init__(self):
        super().__init__()
        self.net = nn.Sequential(
            nn.Flatten(),
            nn.Linear(784, 256), nn.ReLU(), nn.BatchNorm1d(256),
            nn.Linear(256, 128), nn.ReLU(), nn.BatchNorm1d(128),
            nn.Linear(128,  64), nn.ReLU(),
            nn.Linear( 64,  10)
        )
    def forward(self, x):
        return self.net(x)

transform = transforms.Compose([transforms.ToTensor(),
                                 transforms.Normalize((0.1307,), (0.3081,))])
train_loader = DataLoader(datasets.MNIST('.', train=True,  download=True, transform=transform), batch_size=256, shuffle=True)
test_loader  = DataLoader(datasets.MNIST('.', train=False, download=True, transform=transform), batch_size=1000)

model     = DeepNet()
optimizer = optim.Adam(model.parameters(), lr=1e-3)
criterion = nn.CrossEntropyLoss()

for epoch in range(1, 4):
    model.train()
    for X, y in train_loader:
        optimizer.zero_grad()
        loss = criterion(model(X), y)
        loss.backward()
        optimizer.step()

    model.eval()
    correct = 0
    with torch.no_grad():
        for X, y in test_loader:
            correct += (model(X).argmax(1) == y).sum().item()
    print(f"Epoch {epoch}  Test accuracy: {correct / 100:.2f}%")
```

Expected output (approximate):
```
Epoch 1  Test accuracy: 97.52%
Epoch 2  Test accuracy: 97.89%
Epoch 3  Test accuracy: 98.21%
```

## Key Takeaways

Deep learning is the practice of training neural networks with many layers, allowing the network to learn hierarchical representations of data without hand-engineered features. The key insight — that each layer can discover and build on abstractions learned by the layer below — is what makes deep networks so powerful on raw, unstructured data. ReLU activations, batch normalisation, and residual connections have made training stable at depths that were unimaginable two decades ago. Deep learning is not always the right tool, but when the data is complex and the dataset is large, it is extraordinarily capable.

---

[← Multi-Layer Perceptron](mlp){: .btn } [Next → Convolutional Neural Networks](cnn){: .btn .btn-primary }
