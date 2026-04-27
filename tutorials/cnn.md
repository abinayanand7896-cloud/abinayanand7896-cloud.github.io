---
layout: default
title: Convolutional Neural Networks
parent: Deep Learning
nav_order: 4
---

# Convolutional Neural Networks (CNNs)

## What is it?

A Convolutional Neural Network is a type of neural network designed to process data that has a grid-like structure — most famously images. Instead of connecting every neuron to every pixel (which would be computationally ruinous for a 1080p image), a CNN slides a small filter across the image, looking for patterns in local patches. This makes it both efficient and powerful: the same filter that learns to detect a vertical edge in one corner of the image can detect that same edge anywhere else. CNNs are the reason computers can now recognise faces, read handwriting, and diagnose medical scans with human-level accuracy.

---

## The Idea

Think of how you scan a page of text. You don't process the entire page at once — your eye moves in small jumps, reading a few letters at a time. A CNN works similarly. A small grid of weights called a **filter** (or kernel) slides across the input image one patch at a time. At each position it multiplies its weights against the pixel values it's covering and sums the results, producing a single number. As the filter slides across the whole image, it produces a new grid of numbers called a **feature map** — a representation of where, and how strongly, the filter's pattern appears in the original image.

A CNN stacks many such filters, each learning to detect a different pattern. Early filters learn simple things like edges and corners. Deeper filters combine those simple patterns into more complex structures — textures, shapes, object parts — until the final layers can recognise a complete object. The whole thing is learned end-to-end from labelled training data, with no human hand-crafting the filters.

---

## Visual

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

---

## The Math

The convolution operation at position $(i, j)$ in the feature map is:

$$S(i, j) = \sum_{m=0}^{k-1} \sum_{n=0}^{k-1} I(i+m,\ j+n) \cdot K(m, n)$$

where $I$ is the input image, $K$ is the filter (kernel) of size $k \times k$, and $S(i,j)$ is the resulting activation.

> **In plain English:** At each position, the filter sits on top of a patch of the image. You multiply each filter weight by the pixel underneath it, then add all those products together. The result is one number in the feature map. Slide the filter across the whole image and you get the full feature map.

<details>
<summary>Show the derivation</summary>

For a filter of size $k \times k$ applied to an input of size $H \times W$, the output feature map has size:

$$H_{\text{out}} = H - k + 1, \quad W_{\text{out}} = W - k + 1$$

Adding **padding** of $p$ pixels around the input preserves the spatial size:

$$H_{\text{out}} = H - k + 2p + 1$$

With **stride** $s$ (sliding the filter $s$ pixels at a time instead of 1):

$$H_{\text{out}} = \left\lfloor \frac{H - k + 2p}{s} \right\rfloor + 1$$

The number of learnable parameters in one convolutional layer is $k \times k \times C_{\text{in}} \times C_{\text{out}}$, where $C_{\text{in}}$ is the number of input channels (e.g., 3 for RGB) and $C_{\text{out}}$ is the number of filters. This is dramatically fewer parameters than a fully connected layer of the same size — which is the key efficiency advantage of CNNs.

</details>

---

## How It Learns

A CNN is trained with standard backpropagation, exactly like any other neural network. The filter weights start randomly and are adjusted each training step to minimise the loss. What's remarkable is that nobody tells the network what to look for — it figures out on its own that early filters should detect edges, later filters should detect textures and shapes, and the final layers should combine everything into object recognition. This hierarchical feature learning is the CNN's superpower, and it emerges purely from gradient descent on labelled data.

---

## When to Use It

CNNs are the default choice for any task involving images — classification, object detection, segmentation, medical imaging, satellite imagery. They also work well on other grid-like data: audio spectrograms, time-series signals, and even text (treated as a 1D sequence). They do require more data than classical ML models to train well from scratch, but using a pretrained model (transfer learning) largely solves this — you can fine-tune a pretrained CNN on a small dataset and still get excellent results.

---

## Try It Yourself

```python
import torch
import torch.nn as nn

# Define a simple CNN for MNIST (28x28 grayscale images, 10 classes)
class SimpleCNN(nn.Module):
    def __init__(self):
        super().__init__()
        self.conv1 = nn.Conv2d(in_channels=1, out_channels=16, kernel_size=3, padding=1)
        self.conv2 = nn.Conv2d(in_channels=16, out_channels=32, kernel_size=3, padding=1)
        self.pool  = nn.MaxPool2d(2, 2)
        self.fc    = nn.Linear(32 * 7 * 7, 10)
        self.relu  = nn.ReLU()

    def forward(self, x):
        x = self.pool(self.relu(self.conv1(x)))  # -> 16 x 14 x 14
        x = self.pool(self.relu(self.conv2(x)))  # -> 32 x 7 x 7
        x = x.view(-1, 32 * 7 * 7)              # flatten
        return self.fc(x)                         # -> 10 class scores

model = SimpleCNN()
print(model)
print(f"\nTotal parameters: {sum(p.numel() for p in model.parameters()):,}")
```

Expected output:
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

---

## Key Takeaways

A CNN scans images with small learned filters, each producing a feature map that highlights where a particular pattern appears. Stacking many convolutional layers builds up a hierarchy of features — from simple edges to complex objects — entirely from data. The weight-sharing trick (the same filter applies everywhere in the image) is what makes CNNs both efficient and powerful. They are the foundation of modern computer vision, and understanding them is essential before tackling transformers applied to images.

---

[← Deep Learning](deep-learning){: .btn } [Next → RNN](rnn){: .btn .btn-primary }
