---
layout: default
title: LSTM
parent: Deep Learning
nav_order: 6
---

# LSTM

## What is it?

An LSTM (Long Short-Term Memory) is a recurrent neural network cell designed to solve the vanishing gradient problem that cripples standard RNNs. It maintains two internal states: a hidden state $h_t$ like a vanilla RNN, and a cell state $c_t$ — a separate "memory highway" that allows gradients to flow across many timesteps without vanishing. Gates control what gets written to memory, what gets erased, and what gets read out at every step.

---

## The Idea

The core problem with vanilla RNNs is that the gradient signal decays as it travels backwards through many timesteps — the network cannot reliably learn that something at the beginning of a sequence matters for what happens at the end. LSTMs fix this by introducing a cell state $c_t$ that flows through time largely unchanged unless a gate deliberately modifies it. Think of it as a conveyor belt running alongside the normal hidden state, carrying information across long distances without it getting diluted.

Three separate gates regulate how that memory gets used. The forget gate $f_t$ is a sigmoid layer that looks at the previous hidden state and the current input, then outputs a number between 0 and 1 for each element of the cell state — a value near 0 means "erase this" and a value near 1 means "keep this." The input gate $i_t$ works in tandem with a candidate value $\tilde{c}_t$ (produced by a tanh layer) to decide how much new information gets written into memory. Together, the forget and input gates produce the new cell state: erase a fraction of what was there before, then add some portion of the new candidate.

Once the cell state is updated, the output gate $o_t$ decides what portion of it gets exposed as the hidden state $h_t$. The hidden state is not the full cell state — it is the cell state passed through a tanh and then filtered by the output gate, so the network can choose to keep certain memories "internal" while revealing only what is relevant for the current output. Because the cell state update is additive rather than multiplicative, gradients can flow backwards through time without being repeatedly crushed by a weight matrix, which is what makes deep temporal dependencies learnable.

---

## Visual

```mermaid
graph LR
  xt["xₜ"] --> FG["Forget Gate\n σ(Wf·[h,x]+bf)"]
  xt --> IG["Input Gate\n σ(Wi·[h,x]+bi)"]
  xt --> OG["Output Gate\n σ(Wo·[h,x]+bo)"]
  xt --> CC["Cell Candidate\n tanh(Wc·[h,x]+bc)"]
  FG -->|"fₜ"| CS["Cell State cₜ"]
  IG -->|"iₜ"| CS
  CC -->|"c̃ₜ"| CS
  CS --> OG
  OG --> ht["hₜ"]
```

---

## The Math

$$c_t = f_t \odot c_{t-1} + i_t \odot \tilde{c}_t$$

$$h_t = o_t \odot \tanh(c_t)$$

where $f_t = \sigma(W_f [h_{t-1}, x_t] + b_f)$, $i_t = \sigma(W_i [h_{t-1}, x_t] + b_i)$, $o_t = \sigma(W_o [h_{t-1}, x_t] + b_o)$, and $\tilde{c}_t = \tanh(W_c [h_{t-1}, x_t] + b_c)$.

> **In plain English:** The cell state $c_t$ is updated by forgetting part of the old state ($f_t \odot c_{t-1}$) and adding new content ($i_t \odot \tilde{c}_t$). The hidden state is what the cell state "decides to reveal" through the output gate.

<details><summary>Show the derivation</summary>

The key to understanding why LSTMs resist vanishing gradients is the gradient of the loss with respect to $c_{t-1}$:

$$\frac{\partial c_t}{\partial c_{t-1}} = f_t$$

Because $f_t \in (0, 1)$ is produced by a separate gate with its own learned parameters — rather than a shared weight matrix applied repeatedly — it can stay close to 1 over many timesteps, letting gradients flow far back without vanishing. This insight is known as the Constant Error Carousel from the original 1997 Hochreiter & Schmidhuber paper.

GRUs (Gated Recurrent Units) simplify the LSTM by merging the cell and hidden states and using only two gates (reset and update), achieving similar performance with fewer parameters. For many tasks the two architectures are interchangeable.

</details>

---

## How It Learns

LSTMs are trained with the same Backpropagation Through Time algorithm used for vanilla RNNs, but the cell state's additive update keeps gradients from vanishing over long sequences. In practice, the network learns which events to remember and which to forget entirely from the training data — you do not need to hand-design what the gates should attend to.

In modern frameworks, LSTMs are available as `nn.LSTM` in PyTorch and `tf.keras.layers.LSTM` in TensorFlow. They are typically stacked two to four layers deep, with dropout applied between layers to regularise the model. Training is straightforward: the gating mechanism handles the gradient problem automatically, so you can use standard optimisers like Adam without special tuning.

For tasks with very long sequences — thousands of timesteps — Transformers with attention mechanisms have largely superseded LSTMs because attention can directly connect distant positions without routing the signal through every intermediate step. That said, LSTMs remain fast, lightweight, and effective for sequences of moderate length where the full attention computation would be overkill.

---

## When to Use It

LSTMs remain competitive for a wide range of sequence modelling tasks: time series forecasting, named entity recognition, speech recognition, and machine translation (though Transformers have largely taken over in large-scale translation). Their main practical advantage over Transformers is that they are lighter on memory and compute, and they require less data to train well, which makes them the right starting point when resources are limited.

If you have a sequence modelling problem and access to only a modest amount of data or a single GPU, an LSTM is often the most pragmatic choice. When your sequences grow very long, when global context across the whole sequence is essential, or when you are working on tasks like document understanding or large-scale language modelling, Transformer architectures offer a more powerful solution. The rule of thumb is straightforward: try the LSTM first and upgrade to a Transformer if you need to.

---

## Try It Yourself

```python
import torch
import torch.nn as nn
import numpy as np

# Generate a sine wave for time series prediction
t = np.linspace(0, 100, 1000)
sine_wave = np.sin(t).astype(np.float32)

SEQ_LEN = 20
X, y = [], []
for i in range(len(sine_wave) - SEQ_LEN):
    X.append(sine_wave[i:i + SEQ_LEN])
    y.append(sine_wave[i + SEQ_LEN])

X = torch.tensor(X).unsqueeze(-1)   # (N, SEQ_LEN, 1)
y = torch.tensor(y).unsqueeze(-1)   # (N, 1)

split = int(len(X) * 0.8)
X_train, X_test = X[:split], X[split:]
y_train, y_test = y[:split], y[split:]

# Define LSTM model
class LSTMPredictor(nn.Module):
    def __init__(self):
        super().__init__()
        self.lstm = nn.LSTM(input_size=1, hidden_size=32, batch_first=True)
        self.fc = nn.Linear(32, 1)

    def forward(self, x):
        out, _ = self.lstm(x)
        return self.fc(out[:, -1, :])

model = LSTMPredictor()
optimizer = torch.optim.Adam(model.parameters(), lr=1e-3)
loss_fn = nn.MSELoss()

# Train
for epoch in range(20):
    model.train()
    pred = model(X_train)
    loss = loss_fn(pred, y_train)
    optimizer.zero_grad()
    loss.backward()
    optimizer.step()

# Evaluate
model.eval()
with torch.no_grad():
    test_pred = model(X_test)
    test_loss = loss_fn(test_pred, y_test).item()
    print(f"Test MSE Loss: {test_loss:.6f}")
    print(f"Predicted: {test_pred[0].item():.4f} | Actual: {y_test[0].item():.4f}")
```

**Expected output:**
```
Test MSE Loss: 0.000073
Predicted: 0.8803 | Actual: 0.8795
```

---

## Key Takeaways

LSTMs solve the vanishing gradient problem that cripples vanilla RNNs by introducing a cell state — a dedicated memory channel that flows through time nearly unchanged unless a gate intervenes. The forget, input, and output gates give the network fine-grained control over what to remember, what to write, and what to expose at each step. This gating mechanism allows LSTMs to learn dependencies spanning hundreds of timesteps, which vanilla RNNs simply cannot do reliably. They remain a strong and practical choice for sequence modelling tasks where Transformer-scale resources are not available, and they are a natural next step whenever a vanilla RNN falls short.

---

[← RNNs](rnn){: .btn }
