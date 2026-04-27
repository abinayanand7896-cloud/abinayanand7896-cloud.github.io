---
layout: default
title: Get Started
nav_order: 1
---

# Get Started: Run Your First ML Code

Every tutorial in this series has live, runnable code. Before you hit that code, you need a place to run it. This takes about 5 minutes.

Pick one of the two options below.

---

## Option A: Google Colab (Recommended for Beginners)

Google Colab is a free coding environment that runs entirely in your browser. No installation, no setup, works on any computer or phone.

1. Open your browser and go to [colab.research.google.com](https://colab.research.google.com)
2. Sign in with a Google account (Gmail works)
3. Click **New notebook** in the top left
4. You will see a blank cell, like an empty text box
5. Click inside the cell and type this:

```python
print("Hello, ML!")
```

6. Press **Shift + Enter** to run it

You should see `Hello, ML!` appear right below the cell. That is it. You now have a working Python environment.

To run the code from any tutorial, just copy it, paste it into a new cell, and press Shift + Enter.

---

## Option B: Python on Your Own Computer

If you prefer to work offline, use Anaconda. It installs Python and everything you need in one go.

**Step 1: Download Anaconda**

Go to [anaconda.com/download](https://www.anaconda.com/download) and download the version for your system (Windows, Mac, or Linux). Run the installer and click through the defaults.

**Step 2: Check it worked**

Open **Anaconda Prompt** (Windows) or your **Terminal** (Mac or Linux). Type:

```bash
python --version
```

You should see something like `Python 3.11.5`. If you do, Python is installed.

**Step 3: Open Jupyter Notebook**

Jupyter Notebook is your coding workspace, similar to Google Colab but on your own machine. Start it by typing:

```bash
jupyter notebook
```

A browser window opens showing your files. Click **New**, then **Python 3 (ipykernel)**. A blank notebook opens. Same experience as Colab.

---

## Install the Libraries (Local Python Only)

If you are using Google Colab, skip this. Colab has everything pre-installed.

If you chose Option B, open your terminal and run:

```bash
pip install numpy pandas scikit-learn matplotlib
```

This downloads four libraries that every tutorial uses. It takes about a minute.

---

## What Each Library Does

You will see these imports throughout every tutorial. Here is what they actually do:

| Library | Plain-English description |
|---------|--------------------------|
| `numpy` | Does math on large lists of numbers, very fast |
| `pandas` | Works with tables of data, like Excel but in code |
| `scikit-learn` | Ready-made ML models you can train in two lines of code |
| `matplotlib` | Draws charts and graphs from your data |

---

## Your First ML Program

Let us make sure everything works. Copy and paste this into a cell and run it:

```python
# Import the tools we need
import numpy as np
from sklearn.linear_model import LinearRegression

# Our training data: house size (sq ft) and price ($k)
# We are showing the model two examples so it can learn the pattern
sizes = np.array([[1000], [2000]])
prices = np.array([200, 400])

# Train the model on our two examples
model = LinearRegression()
model.fit(sizes, prices)

# Now ask the model: what would a 1500 sq ft house cost?
prediction = model.predict([[1500]])[0]
print(f"Predicted price for a 1500 sq ft house: ${prediction:.0f}k")
```

Expected output:

```
Predicted price for a 1500 sq ft house: $300k
```

**What just happened?**

You trained a machine learning model. It looked at two examples (1000 sq ft = $200k and 2000 sq ft = $400k), figured out the pattern (every extra 100 sq ft adds $20k), and used that pattern to predict that a 1500 sq ft house costs $300k. That is the entire idea of machine learning, compressed into 10 lines.

You did not write any rule telling it "add $20k per 100 sq ft." It worked that out by itself. That is the magic.

---

## Troubleshooting

**"ModuleNotFoundError: No module named sklearn"**
You need to install the libraries. Run `pip install scikit-learn` in your terminal.

**"I don't have a Google account"**
Create a free one at [accounts.google.com](https://accounts.google.com) or use the local Python option.

**"The cell is not running"**
Make sure you clicked inside the cell first, then pressed Shift + Enter (both keys at the same time).

---

You are set up. Let us start learning.

[Next: What is Machine Learning?](what-is-ml){: .btn .btn-primary }
