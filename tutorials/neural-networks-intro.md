---
layout: default
title: What are Neural Networks?
parent: Deep Learning
nav_order: 1
---

# What are Neural Networks?

## Plain English Intro

Your brain is made of ~86 billion neurons — cells that send electrical signals to each other. When you learn something new, the connections between neurons get stronger.

Artificial neural networks are a very simplified computer model of this. Instead of billions of neurons, we use thousands or millions of simple mathematical units arranged in layers, passing numbers to each other. By training on data, the connections (weights) between units get tuned — and the network learns to recognize patterns.

---

## How It Works

A neural network has three types of layers:

| Layer | What it does |
|-------|-------------|
| **Input layer** | Receives raw data (e.g., pixel values of an image) |
| **Hidden layers** | Transform the data, learning increasingly abstract features |
| **Output layer** | Produces the final prediction (e.g., "cat" or "dog") |

Each unit in a layer:
1. Receives numbers from the previous layer
2. Multiplies each by a learned **weight**
3. Adds them up
4. Passes the result through an **activation function** (adds non-linearity)
5. Sends the output to the next layer

**Training** adjusts the weights using an algorithm called **backpropagation** — it measures the error and works backward through the network, nudging each weight to reduce the error.

### Why "Deep" Learning?

A network with many hidden layers is called a **deep** neural network. More layers = ability to learn more complex patterns.

---

## Types of Neural Networks (covered in this series)

| Model | Best for |
|-------|---------|
| **MLP** (Multilayer Perceptron) | General classification/regression |
| **CNN** (Convolutional Neural Network) | Images |
| **RNN** (Recurrent Neural Network) | Sequences (text, time series) |
| **LSTM** | Long sequences where long-term memory matters |

---

## When to Use Neural Networks

**Good for:**
- Images, audio, video, and raw text
- Very large datasets (neural networks need lots of data to shine)
- Problems where simpler models fall short

**Not ideal for:**
- Small datasets (classic ML models often beat neural networks here)
- Tabular data (try XGBoost first — it's often better and much faster)
- When you need to explain the model's decisions easily

---

## Key Takeaways

- Neural networks are layers of simple mathematical units that learn from data
- Training adjusts millions of weights using backpropagation
- "Deep learning" means many hidden layers — allowing complex pattern recognition
- Neural networks excel at images, audio, and text — but need large datasets
- They are the foundation of modern AI: image recognition, speech, language models

---

[← XGBoost](xgboost){: .btn } [Next → Multilayer Perceptron](mlp){: .btn .btn-primary }
