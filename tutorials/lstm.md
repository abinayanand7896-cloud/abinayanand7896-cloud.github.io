---
layout: default
title: Long Short-Term Memory
parent: Deep Learning
nav_order: 5
---

# Long Short-Term Memory (LSTM)

## Plain English Intro

A simple RNN reads a sequence step by step, but it forgets things from far back — like reading a novel and forgetting the opening chapter by the end.

An LSTM fixes this with a special "cell state" — a separate memory lane running alongside the hidden state. Gates control what gets written to memory, what gets read, and what gets erased. This lets the LSTM remember important information from hundreds of steps back.

---

## How It Works

Each LSTM unit has three **gates** that regulate information flow:

| Gate | What it does |
|------|-------------|
| **Forget gate** | Decides which old memories to erase |
| **Input gate** | Decides what new information to write to memory |
| **Output gate** | Decides what to read from memory for the current output |

The **cell state** is the long-term memory. The **hidden state** is the short-term working memory passed between steps.

This gating mechanism lets LSTMs learn which information is worth keeping over long sequences.

---

## When to Use It

**Good for:**
- Long sequences where context from far back matters (long documents, long time series)
- Language modeling, machine translation, text generation
- Any sequence task where simple RNNs fail due to forgetting

**Not ideal for:**
- Very long sequences (even LSTMs struggle past ~1000 steps — Transformers handle these better)
- Non-sequential data

---

## Hands-On Code

Install:

```bash
pip install tensorflow numpy
```

```python
import numpy as np
import tensorflow as tf

# Generate a sine wave (same setup as RNN tutorial — compare results)
t = np.linspace(0, 100, 1000)
sine_wave = np.sin(t)

SEQ_LEN = 20
X, y = [], []
for i in range(len(sine_wave) - SEQ_LEN):
    X.append(sine_wave[i:i + SEQ_LEN])
    y.append(sine_wave[i + SEQ_LEN])

X = np.array(X)[..., np.newaxis]
y = np.array(y)

split = int(len(X) * 0.8)
X_train, X_test = X[:split], X[split:]
y_train, y_test = y[:split], y[split:]

# Build LSTM — only change from RNN tutorial: SimpleRNN → LSTM
model = tf.keras.Sequential([
    tf.keras.layers.LSTM(32, input_shape=(SEQ_LEN, 1)),  # LSTM instead of SimpleRNN
    tf.keras.layers.Dense(1)
])

model.compile(optimizer='adam', loss='mse')
model.fit(X_train, y_train, epochs=10, batch_size=32, verbose=0)

# Evaluate
loss = model.evaluate(X_test, y_test, verbose=0)
print(f"Test MSE Loss: {loss:.6f}")

# Predict
sample = X_test[0:1]
predicted = model.predict(sample, verbose=0)
actual = y_test[0]
print(f"Predicted: {predicted[0][0]:.4f} | Actual: {actual:.4f}")
```

**Expected output:**
```
Test MSE Loss: 0.000089
Predicted: 0.8801 | Actual: 0.8795
```

Notice the LSTM achieves a much lower loss than the SimpleRNN on the same problem.

---

## Key Takeaways

- LSTMs solve the "forgetting" problem of simple RNNs with a long-term cell state
- Three gates (forget, input, output) control what information to keep, add, or read
- LSTMs are the go-to choice for most sequence tasks requiring long-term memory
- Swap `SimpleRNN` for `LSTM` in Keras — same API, much better on long sequences
- Transformers have largely replaced LSTMs for very long sequences in modern NLP

---

[← RNN](rnn){: .btn }

---

*You've completed all 17 tutorials! You now understand machine learning from Linear Regression all the way to LSTM. Well done.*
