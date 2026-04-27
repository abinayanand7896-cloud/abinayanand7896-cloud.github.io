---
layout: default
title: Convolutional Neural Networks
parent: Deep Learning
nav_order: 4
---

# Convolutional Neural Networks (CNNs)

Every time you unlock your phone with your face, or Google Photos groups your pictures by person, a Convolutional Neural Network is doing the work. CNNs are the reason computers can now see.

---

## What is a CNN?

A Convolutional Neural Network is a type of neural network designed specifically for images and other grid-like data. Instead of looking at the whole image at once, it scans across the image in small patches, looking for patterns in each patch.

**New word: filter** (also called a kernel) is a small grid of learned numbers that slides across the image. Each filter is trained to recognise one specific pattern, like a vertical edge, a colour transition, or a curve.

**New word: feature map** is the result of sliding a filter across an image. It is a new grid of numbers that shows where, and how strongly, that filter's pattern appeared in the original image.

---

## A simple way to think about it

Think of how you scan a page of text. You do not read the whole page at once. Your eye jumps across a few letters at a time, left to right, top to bottom.

A CNN works the same way. A small window of weights (the filter) slides across the image, step by step. At each position it asks: "does my pattern appear here?" The answer is a single number. After the filter has slid across the entire image, all those answers form the feature map.

A CNN uses many different filters at once. The first set of filters might learn to detect edges pointing in different directions. The next set learns to detect corners and curves formed by those edges. Deeper filters combine curves into shapes, shapes into object parts, and eventually object parts into a full classification: "cat", "car", "tumour", or "handwritten 7."

Nobody tells the network what patterns to look for. It discovers them entirely on its own during training.

---

## How it works, step by step

1. The image enters as a grid of pixel values (for a colour image, one grid per colour channel: red, green, blue).
2. The first layer slides many small filters across the image and produces one feature map per filter.
3. A pooling step shrinks each feature map by keeping only the strongest activation in each small region.
4. The next layer slides filters across those feature maps, finding more complex patterns.
5. Repeat for as many layers as needed.
6. Flatten the final feature maps into a list of numbers and pass them through a standard neural network.
7. The output layer gives the final answer.

---

## See it visually

<div style="background:#f8fafc;border:1px solid #e2e8f0;border-radius:8px;padding:20px;margin:20px 0;">
<p style="text-align:center;font-size:12px;color:#64748b;margin-bottom:12px;">3×3 filter sliding over a 5×5 input image</p>
<div style="display:flex;align-items:center;justify-content:center;gap:24px;flex-wrap:wrap;">

<div>
<p style="text-align:center;font-size:11px;color:#64748b;margin-bottom:6px;">Input</p>
<div style="display:grid;grid-template-columns:repeat(5,32px);gap:2px;">
  <div style="width:32px;height:32px;background:#bfdbfe;border:2px solid #3b82f6;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;">1</div>
  <div style="width:32px;height:32px;background:#bfdbfe;border:2px solid #3b82f6;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;">0</div>
  <div style="width:32px;height:32px;background:#bfdbfe;border:2px solid #3b82f6;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;">1</div>
  <div style="width:32px;height:32px;background:#e2e8f0;border:1px solid #cbd5e1;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;color:#94a3b8;">0</div>
  <div style="width:32px;height:32px;background:#e2e8f0;border:1px solid #cbd5e1;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;color:#94a3b8;">0</div>
  <div style="width:32px;height:32px;background:#bfdbfe;border:2px solid #3b82f6;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;">0</div>
  <div style="width:32px;height:32px;background:#bfdbfe;border:2px solid #3b82f6;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;">1</div>
  <div style="width:32px;height:32px;background:#bfdbfe;border:2px solid #3b82f6;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;">1</div>
  <div style="width:32px;height:32px;background:#e2e8f0;border:1px solid #cbd5e1;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;color:#94a3b8;">0</div>
  <div style="width:32px;height:32px;background:#e2e8f0;border:1px solid #cbd5e1;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;color:#94a3b8;">1</div>
  <div style="width:32px;height:32px;background:#bfdbfe;border:2px solid #3b82f6;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;">0</div>
  <div style="width:32px;height:32px;background:#bfdbfe;border:2px solid #3b82f6;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;">0</div>
  <div style="width:32px;height:32px;background:#bfdbfe;border:2px solid #3b82f6;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;">1</div>
  <div style="width:32px;height:32px;background:#e2e8f0;border:1px solid #cbd5e1;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;color:#94a3b8;">1</div>
  <div style="width:32px;height:32px;background:#e2e8f0;border:1px solid #cbd5e1;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;color:#94a3b8;">0</div>
  <div style="width:32px;height:32px;background:#e2e8f0;border:1px solid #cbd5e1;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;color:#94a3b8;">1</div>
  <div style="width:32px;height:32px;background:#e2e8f0;border:1px solid #cbd5e1;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;color:#94a3b8;">0</div>
  <div style="width:32px;height:32px;background:#e2e8f0;border:1px solid #cbd5e1;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;color:#94a3b8;">0</div>
  <div style="width:32px;height:32px;background:#e2e8f0;border:1px solid #cbd5e1;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;color:#94a3b8;">1</div>
  <div style="width:32px;height:32px;background:#e2e8f0;border:1px solid #cbd5e1;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;color:#94a3b8;">1</div>
  <div style="width:32px;height:32px;background:#e2e8f0;border:1px solid #cbd5e1;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;color:#94a3b8;">0</div>
  <div style="width:32px;height:32px;background:#e2e8f0;border:1px solid #cbd5e1;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;color:#94a3b8;">1</div>
  <div style="width:32px;height:32px;background:#e2e8f0;border:1px solid #cbd5e1;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;color:#94a3b8;">0</div>
  <div style="width:32px;height:32px;background:#e2e8f0;border:1px solid #cbd5e1;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;color:#94a3b8;">1</div>
  <div style="width:32px;height:32px;background:#e2e8f0;border:1px solid #cbd5e1;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;color:#94a3b8;">1</div>
</div>
<p style="text-align:center;font-size:10px;color:#3b82f6;margin-top:4px;">← active patch (3×3)</p>
</div>

<div style="font-size:24px;color:#94a3b8;">→</div>

<div>
<p style="text-align:center;font-size:11px;color:#64748b;margin-bottom:6px;">Filter (learned)</p>
<div style="display:grid;grid-template-columns:repeat(3,32px);gap:2px;">
  <div style="width:32px;height:32px;background:#fef3c7;border:1px solid #f59e0b;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;">1</div>
  <div style="width:32px;height:32px;background:#fef3c7;border:1px solid #f59e0b;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;">0</div>
  <div style="width:32px;height:32px;background:#fef3c7;border:1px solid #f59e0b;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;">1</div>
  <div style="width:32px;height:32px;background:#fef3c7;border:1px solid #f59e0b;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;">0</div>
  <div style="width:32px;height:32px;background:#fef3c7;border:1px solid #f59e0b;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;">1</div>
  <div style="width:32px;height:32px;background:#fef3c7;border:1px solid #f59e0b;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;">0</div>
  <div style="width:32px;height:32px;background:#fef3c7;border:1px solid #f59e0b;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;">1</div>
  <div style="width:32px;height:32px;background:#fef3c7;border:1px solid #f59e0b;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;">0</div>
  <div style="width:32px;height:32px;background:#fef3c7;border:1px solid #f59e0b;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;">1</div>
</div>
</div>

<div style="font-size:24px;color:#94a3b8;">→</div>

<div>
<p style="text-align:center;font-size:11px;color:#64748b;margin-bottom:6px;">Feature Map</p>
<div style="display:grid;grid-template-columns:repeat(3,32px);gap:2px;">
  <div style="width:32px;height:32px;background:#d1fae5;border:1px solid #10b981;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;font-weight:600;">4</div>
  <div style="width:32px;height:32px;background:#d1fae5;border:1px solid #10b981;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;">2</div>
  <div style="width:32px;height:32px;background:#d1fae5;border:1px solid #10b981;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;">3</div>
  <div style="width:32px;height:32px;background:#d1fae5;border:1px solid #10b981;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;">2</div>
  <div style="width:32px;height:32px;background:#d1fae5;border:1px solid #10b981;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;">3</div>
  <div style="width:32px;height:32px;background:#d1fae5;border:1px solid #10b981;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;">2</div>
  <div style="width:32px;height:32px;background:#d1fae5;border:1px solid #10b981;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;">1</div>
  <div style="width:32px;height:32px;background:#d1fae5;border:1px solid #10b981;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;">3</div>
  <div style="width:32px;height:32px;background:#d1fae5;border:1px solid #10b981;border-radius:3px;display:flex;align-items:center;justify-content:center;font-size:10px;">2</div>
</div>
</div>

</div>
</div>

The blue highlighted 3x3 patch of the input is the area the filter is currently examining. The filter (yellow) is multiplied against those pixels and the results are summed to produce one number in the green feature map. The filter then slides to the next position and repeats.

---

## The maths (do not panic)

The convolution operation at position $(i, j)$ in the feature map is:

$$S(i, j) = \sum_{m=0}^{k-1} \sum_{n=0}^{k-1} I(i+m,\ j+n) \cdot K(m, n)$$

where $I$ is the input image, $K$ is the filter (kernel) of size $k \times k$, and $S(i,j)$ is the resulting activation.

> **In plain English:** At each position, the filter sits on top of a patch of the image. Multiply each filter weight by the pixel underneath it, then add all those products together. That sum is one number in the feature map. Slide the filter one step and repeat. Do this for the whole image and you get the full feature map.

<details>
<summary>Show more detail</summary>

For a filter of size $k \times k$ applied to an input of size $H \times W$, the output feature map has size:

$$H_{\text{out}} = H - k + 1, \quad W_{\text{out}} = W - k + 1$$

Adding **padding** (extra pixels around the border) preserves the spatial size. Adding a **stride** (sliding the filter 2 or more pixels at a time instead of 1) shrinks the output faster.

One key advantage of CNNs over plain neural networks is that the same filter is used everywhere in the image. A filter that learns to detect a vertical edge in the top-left corner will also detect vertical edges anywhere else. This is called weight sharing, and it dramatically reduces the number of parameters needed to process a high-resolution image.

</details>

---

## Run the code yourself

This code defines a small CNN architecture for classifying 28x28 pixel images of handwritten digits. You will see how convolution and pooling layers reduce the image size while extracting features.

**Step 1:** Open [Google Colab](https://colab.research.google.com) and create a new notebook.

**Step 2:** Copy this code into a cell:

```python
import torch                                        # PyTorch
import torch.nn as nn                              # tools for building neural networks

# Define a simple CNN for 28x28 grayscale digit images (10 classes: 0-9)
class SimpleCNN(nn.Module):
    def __init__(self):
        super().__init__()
        # First convolutional layer: 1 input channel, 16 filters of size 3x3
        self.conv1 = nn.Conv2d(in_channels=1, out_channels=16, kernel_size=3, padding=1)
        # Second convolutional layer: 16 channels in, 32 filters out
        self.conv2 = nn.Conv2d(in_channels=16, out_channels=32, kernel_size=3, padding=1)
        # Max pooling: takes the maximum in each 2x2 patch, halving the spatial size
        self.pool  = nn.MaxPool2d(2, 2)
        # Final layer: 32 channels x 7x7 spatial = 1568 inputs, 10 class outputs
        self.fc    = nn.Linear(32 * 7 * 7, 10)
        self.relu  = nn.ReLU()     # ReLU activation: negative values become 0

    def forward(self, x):
        x = self.pool(self.relu(self.conv1(x)))   # conv1 → ReLU → pool: produces 16 x 14 x 14
        x = self.pool(self.relu(self.conv2(x)))   # conv2 → ReLU → pool: produces 32 x 7 x 7
        x = x.view(-1, 32 * 7 * 7)               # flatten: 1568 numbers per image
        return self.fc(x)                          # output: 10 scores, one per digit class

# Create the model and count how many weights it has
model = SimpleCNN()
print(model)
print(f"\nTotal parameters: {sum(p.numel() for p in model.parameters()):,}")
```

**Step 3:** Press **Shift + Enter** to run it.

You should see:
```
SimpleCNN(
  (conv1): Conv2d(1, 16, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
  (conv2): Conv2d(16, 32, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
  (pool): MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)
  (fc): Linear(in_features=1568, out_features=10, bias=True)
  (relu): ReLU()
)

Total parameters: 21,658
```

**What each line does:**
- `nn.Conv2d(1, 16, kernel_size=3)`: creates 16 small filters, each 3x3 pixels, learning 16 different patterns
- `nn.MaxPool2d(2, 2)`: shrinks each feature map by half, keeping only the strongest signal in each 2x2 block
- `x.view(-1, 32 * 7 * 7)`: flattens the 3D feature maps into a single list of 1568 numbers
- `nn.Linear(32 * 7 * 7, 10)`: takes those 1568 numbers and produces 10 scores, one per digit
- `sum(p.numel() for p in model.parameters())`: counts every single learnable number in the network

**What just happened?**

This CNN has only 21,658 parameters and can classify 28x28 digit images into 10 classes. A standard neural network doing the same job would need far more parameters. The reason CNNs are so efficient is weight sharing: one 3x3 filter looks for its pattern everywhere in the image, so you only need 9 numbers to cover the whole image rather than one number per pixel position.

---

## Quick recap

- A CNN slides small learned filters across an image to produce feature maps that show where each pattern appears.
- Stacking layers builds up a hierarchy: early layers detect edges, later layers detect shapes, the deepest layers detect full objects.
- The same filter applies everywhere in the image, which means far fewer parameters are needed compared to connecting every neuron to every pixel.
- CNNs are the standard tool for any task involving images, including photo recognition, medical imaging, and satellite analysis.
- Transfer learning means you can use a CNN that was already trained on millions of images and fine-tune it for your specific task, even with a small dataset.

---

[← Deep Learning](deep-learning){: .btn } [Next → RNN](rnn){: .btn .btn-primary }
