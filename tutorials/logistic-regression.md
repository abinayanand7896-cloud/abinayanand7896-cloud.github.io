---
layout: default
title: Logistic Regression
parent: ML Basics
nav_order: 4
---

# Logistic Regression

Have you ever wondered how your email app decides whether to put a message in your inbox or your spam folder? It does not just say "this is spam." It calculates a probability, like "this has a 95% chance of being spam", and then makes a decision. That is exactly what Logistic Regression does.

---

## What is Logistic Regression?

Logistic Regression is a method for answering yes/no questions. Given some information about something, it predicts the probability (which means the chance, as a number from 0 to 1) that it belongs to one category or another.

For example: is this email spam or not? Is this tumour likely to be harmful or harmless? Should this bank approve or reject this loan? These are all questions Logistic Regression can answer, and it gives you a confidence score alongside every answer.

Despite the word "regression" in the name, this is a classification method (which means it puts things into categories, not a method that predicts a continuous number like a price).

---

## A simple way to think about it

Imagine you are deciding whether to bring an umbrella tomorrow. You look at several clues: the sky is grey, the forecast says 80% chance of rain, and it rained the last three Tuesdays. You weigh up all those clues and arrive at a number in your head, something like "there is a 75% chance I will need an umbrella."

Logistic Regression does exactly that. It looks at several pieces of information (called features), weighs each one, and produces a final probability. If the probability is above 50%, it predicts "yes". Below 50%, it predicts "no". You can also adjust that 50% cutoff if your problem needs it.

The key ingredient is something called a **sigmoid function** (which means a function that squashes any number into the range 0 to 1, producing an S-shaped curve). This is what turns the model's internal calculation into a proper probability.

---

## How it works, step by step

1. The model starts with random guesses for how important each feature is
2. It multiplies each feature value by its importance score and adds them all up (just like Linear Regression)
3. It passes that total through the sigmoid function to squash it into a number between 0 and 1
4. It compares that probability to the correct answer and measures how wrong it was
5. It adjusts the importance scores slightly to reduce the error
6. It repeats steps 2 to 5 thousands of times until the predictions are as accurate as possible

---

## See it visually

<div style="background:#f8fafc;border:1px solid #e2e8f0;border-radius:8px;padding:20px;margin:20px 0;">
<svg viewBox="0 0 320 160" xmlns="http://www.w3.org/2000/svg" style="width:100%;max-width:400px;display:block;margin:auto;">
  <line x1="20" y1="130" x2="300" y2="130" stroke="#94a3b8" stroke-width="1.5"/>
  <line x1="160" y1="10" x2="160" y2="145" stroke="#94a3b8" stroke-width="1.5" stroke-dasharray="4"/>
  <text x="150" y="155" font-size="10" fill="#64748b">0</text>
  <text x="10" y="134" font-size="10" fill="#64748b">0</text>
  <text x="10" y="24" font-size="10" fill="#64748b">1</text>
  <text x="130" y="155" font-size="10" fill="#64748b">z →</text>
  <line x1="20" y1="125" x2="60" y2="123" stroke="#3b82f6" stroke-width="2" fill="none"/>
  <path d="M 60 123 Q 100 120 120 110 Q 140 95 160 80 Q 180 65 200 50 Q 220 38 260 28 L 300 25"
        stroke="#3b82f6" stroke-width="2.5" fill="none"/>
  <line x1="20" y1="20" x2="60" y2="20" stroke="#94a3b8" stroke-width="1" stroke-dasharray="3"/>
  <line x1="260" y1="20" x2="300" y2="20" stroke="#94a3b8" stroke-width="1" stroke-dasharray="3"/>
  <circle cx="160" cy="80" r="3" fill="#ef4444"/>
  <text x="165" y="78" font-size="9" fill="#ef4444">σ(0) = 0.5</text>
  <text x="200" y="155" font-size="10" fill="#3b82f6">sigmoid curve</text>
</svg>
</div>

This is the sigmoid curve. No matter what number you feed into it, the output always stays between 0 and 1. The middle point is 0.5. Large positive inputs push the output close to 1 (very likely "yes"). Large negative inputs push it close to 0 (very likely "no").

---

## The maths (do not panic)

Here is the formula that makes this work. We will break down every part.

$$\hat{y} = \sigma(\mathbf{w}^T \mathbf{x} + b), \quad \text{where} \quad \sigma(z) = \frac{1}{1 + e^{-z}}$$

> **In plain English:** The model multiplies each feature by its importance weight, adds them all up with a bias number, and then passes the result through the sigmoid function. The sigmoid converts that total into a probability between 0 and 1.

<details><summary>Show more detail</summary>

To train the model, we need a way to measure how wrong a prediction is. For probabilities, we use something called **binary cross-entropy** (which means a penalty score for how far off a probability prediction was from the correct answer of 0 or 1).

$$\mathcal{L} = -\frac{1}{n}\sum_{i=1}^{n}\left[y_i \log(\hat{y}_i) + (1-y_i)\log(1-\hat{y}_i)\right]$$

Here is the intuition. When the correct answer is 1 and the model predicts a probability close to 1, the penalty is nearly zero. When the model is confidently wrong (say it predicts 0.01 when the answer is 1), the penalty is very large. Confident wrong answers are punished much harder than uncertain ones.

Unlike Linear Regression, there is no single formula to jump straight to the best answer. The model has to improve gradually, step by step. Each step adjusts the weights a little in the direction that reduces the penalty. This process is called **gradient descent** (which means taking small steps downhill toward the lowest possible error).

</details>

---

## Run the code yourself

This code will train a model to tell two types of iris flowers apart. At the end, it will show you not just the answer, but how confident the model was for each flower it looked at.

**Step 1:** Open [Google Colab](https://colab.research.google.com) and create a new notebook. (Or use Jupyter if you followed the [Get Started guide](setup).)

**Step 2:** Copy this code into a cell:

```python
from sklearn.datasets import load_iris                 # loads the 150-flower dataset
from sklearn.linear_model import LogisticRegression    # the model we will train
from sklearn.model_selection import train_test_split   # splits data into training and test sets
from sklearn.metrics import accuracy_score             # measures how often we are right

# Load the iris dataset and turn it into a two-category problem
data = load_iris()
X = data.data                              # the measurements for each flower
y = (data.target != 0).astype(int)         # 0 = setosa, 1 = any other species

# Hold out 20% of the flowers to test the model after training
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Create the model (max_iter=200 means allow up to 200 rounds of improvement)
model = LogisticRegression(max_iter=200)
model.fit(X_train, y_train)                # train the model on the training flowers

predictions = model.predict(X_test)        # predict the species for each test flower
print(f"Accuracy: {accuracy_score(y_test, predictions) * 100:.1f}%")

# Show the predicted probabilities for the first five test flowers
probs = model.predict_proba(X_test[:5])    # get probability scores, not just labels
for i, (p0, p1) in enumerate(probs):
    print(f"Sample {i+1}: P(setosa)={p0:.2f}  P(not setosa)={p1:.2f}")
```

**Step 3:** Press **Shift + Enter** to run it.

You should see:
```
Accuracy: 100.0%
Sample 1: P(setosa)=0.00  P(not setosa)=1.00
Sample 2: P(setosa)=0.00  P(not setosa)=1.00
Sample 3: P(setosa)=0.98  P(not setosa)=0.02
Sample 4: P(setosa)=0.00  P(not setosa)=1.00
Sample 5: P(setosa)=0.00  P(not setosa)=1.00
```

**What each line does:**
- `(data.target != 0).astype(int)`: converts the three-species problem into a simpler two-category problem: setosa vs. everything else
- `train_test_split(...)`: puts 80% of the flowers aside for training and keeps 20% hidden for testing
- `model.fit(X_train, y_train)`: trains the model by repeatedly adjusting the weights to reduce the penalty score
- `model.predict(X_test)`: predicts a category (0 or 1) for each test flower
- `model.predict_proba(X_test[:5])`: returns actual probabilities instead of just the final label

**What just happened?**

The model learned which flower measurements best separate setosa from other species. Look at sample 3: the model said there is a 98% chance it is setosa, not a confident 100%. That is the power of probability outputs. You can see exactly how certain the model is, not just what it decided. A doctor or a bank would find that confidence score just as useful as the final answer.

---

## Quick recap

- Logistic Regression predicts the probability that something belongs to a category, as a number from 0 to 1
- The sigmoid function is what turns any number into a proper probability
- The model trains by adjusting its weights step by step to reduce its penalty score
- Each weight tells you directly how much that feature pushed the prediction toward one category
- It is fast, reliable, and easy to explain, which is why it is widely used in medicine and finance

---

[← Linear Regression](linear-regression){: .btn } [Next → Decision Trees](decision-tree){: .btn .btn-primary }
