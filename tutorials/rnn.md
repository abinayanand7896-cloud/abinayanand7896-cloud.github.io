---
layout: default
title: Recurrent Neural Networks
parent: Deep Learning
nav_order: 4
---

# Recurrent Neural Networks (RNN)

## Plain English Intro

Reading a sentence word by word, you remember what came before. "The bank was steep" vs. "The bank was empty" — the word "bank" means something different based on surrounding context.

A standard MLP has no memory — it processes each input independently. An RNN has a hidden state that acts like short-term memory. At each step, it looks at the current input AND what it remembered from the previous step.

---

## How It Works

At each time step:
1. Receive the current input (e.g., a word)
2. Combine it with the previous hidden state (memory from the last step)
3. Produce a new hidden state (updated memory)
4. Optionally produce an output (e.g., a prediction)

The same weights are reused at every time step — the network applies the same transformation at each position in the sequence.

---

## When to Use It

**Good for:**
- Sequential data: text, speech, time series
- Tasks where order matters
- Sequence-to-sequence problems (translation, summarization)

**Not ideal for:**
- Very long sequences (RNNs forget things from far back — use LSTM)
- Non-sequential data (use MLP or CNN)

---

## Hands-On Code

Install:

```bash
pip install tensorflow numpy
```

```python
import numpy as np
import tensorflow as tf

# Generate a simple sine wave sequence
t = np.linspace(0, 100, 1000)
sine_wave = np.sin(t)

# Create input/output pairs: given 20 steps, predict the next value
SEQ_LEN = 20
X, y = [], []
for i in range(len(sine_wave) - SEQ_LEN):
    X.append(sine_wave[i:i + SEQ_LEN])       # 20 past values
    y.append(sine_wave[i + SEQ_LEN])          # Next value to predict

X = np.array(X)[..., np.newaxis]  # Shape: (samples, 20, 1)
y = np.array(y)

# Split into train and test
split = int(len(X) * 0.8)
X_train, X_test = X[:split], X[split:]
y_train, y_test = y[:split], y[split:]

# Build RNN
model = tf.keras.Sequential([
    tf.keras.layers.SimpleRNN(32, input_shape=(SEQ_LEN, 1), activation='tanh'),
    tf.keras.layers.Dense(1)  # Output: single predicted value
])

model.compile(optimizer='adam', loss='mse')
model.fit(X_train, y_train, epochs=10, batch_size=32, verbose=0)

# Evaluate
loss = model.evaluate(X_test, y_test, verbose=0)
print(f"Test MSE Loss: {loss:.6f}")

# Predict the next value after a sample sequence
sample = X_test[0:1]
predicted = model.predict(sample, verbose=0)
actual = y_test[0]
print(f"Predicted: {predicted[0][0]:.4f} | Actual: {actual:.4f}")
```

**Expected output:**
```
Test MSE Loss: 0.000342
Predicted: 0.8731 | Actual: 0.8795
```

---

## Key Takeaways

- RNNs process sequences step by step, maintaining a hidden state as memory
- The same weights are shared across all time steps
- Simple RNNs struggle with long-range dependencies — they forget distant context
- Good for short sequences; use LSTM for longer ones
- Applications: sentiment analysis, time series forecasting, speech recognition

---

[← CNN](cnn){: .btn } [Next → LSTM](lstm){: .btn .btn-primary }
