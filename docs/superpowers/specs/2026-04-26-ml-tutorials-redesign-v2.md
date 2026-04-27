# ML Tutorials Redesign v2 — "AI From the Ground Up"

**Date:** 2026-04-26  
**Scope:** Redesign of all 20 existing tutorials (ML Basics + Deep Learning tracks only). NLP, Transformers, and LLMs & GPT tracks are future phases.

---

## Goals

- Replace AI-sounding, bullet-heavy content with human-written, conversational prose
- Add mathematical formulas with plain-English translations and collapsible derivations
- Add one visual per tutorial (Mermaid or HTML/CSS SVG)
- Rename the site to "AI From the Ground Up"
- Set up a 5-track navigation structure that can grow over time
- Make the site a one-stop resource for anyone learning ML — beginner to intermediate

---

## Site-Level Changes

### Title
**Old:** Zero to Deep Learning  
**New:** AI From the Ground Up

### Navigation — 5 Flat Tracks
| Track | Status | Contents |
|-------|--------|----------|
| ML Basics | Active | what-is-ml, foundations, linear-regression, logistic-regression, decision-tree, random-forest, gradient-boosting, xgboost, svm, knn, naive-bayes, kmeans, pca, intermediate |
| Deep Learning | Active | neural-networks-intro, mlp, deep-learning, cnn, rnn, lstm |
| NLP | Coming soon | placeholder |
| Transformers | Coming soon | placeholder |
| LLMs & GPT | Coming soon | placeholder |

Each "Coming soon" track gets a placeholder index page with a brief description of what will be covered. `what-is-ml` and `foundations` sit at the top of ML Basics as the entry point. `intermediate` is the signpost/overview page at the end of ML Basics linking forward to Deep Learning.

### Math Rendering — KaTeX
- Load KaTeX via CDN in the Jekyll layout (no plugin required, works on GitHub Pages)
- Add to `_layouts/default.html` or equivalent:
  ```html
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/katex.min.css">
  <script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/katex.min.js"></script>
  <script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/contrib/auto-render.min.js"
    onload="renderMathInElement(document.body, {delimiters: [{left:'$$',right:'$$',display:true},{left:'$',right:'$',display:false}]})"></script>
  ```
- Inline math: `$formula$`
- Display math (centred block): `$$formula$$`

---

## Tutorial Structure

Every tutorial follows this exact 8-section order:

| # | Section | Description |
|---|---------|-------------|
| 1 | **What is it?** | Plain-English definition + one real-world example to anchor the concept. 2–4 sentences. |
| 2 | **The Idea** | Deeper conversational explanation of how and why it works. No bullet lists. Paragraph prose only. |
| 3 | **Visual** | One diagram — Mermaid for flow/structure, HTML/CSS SVG for spatial/graphical content. |
| 4 | **The Math** | Key formula (KaTeX) + plain-English translation in a callout box + collapsible derivation. |
| 5 | **How It Learns** | How the training process works. Conversational. Connects back to the formula where relevant. |
| 6 | **When to Use It** | Strengths and limitations written as paragraphs, not bullet lists. |
| 7 | **Try It Yourself** | Minimal working Python code snippet using scikit-learn or equivalent. |
| 8 | **Key Takeaways** | 3–5 warm sentences summarising the most important points. No bullet list. |

### Section 4 — The Math: Template

```markdown
## The Math

$$\hat{y} = w_1 x_1 + w_2 x_2 + \dots + b$$

> **In plain English:** The predicted value is a weighted sum of your inputs plus a bias term.
> The model's job during training is to find the weights that make this prediction as accurate as possible.

<details>
<summary>Show the derivation</summary>

...step-by-step derivation here...

</details>
```

---

## Voice Guidelines

- Write as a knowledgeable friend explaining over coffee — clear, direct, warm
- Never use bullet lists inside explanation sections (sections 1, 2, 5, 6, 8)
- Avoid passive voice where possible ("the model learns" not "the weights are updated")
- No filler phrases: "In this tutorial we will...", "As we can see...", "It is important to note..."
- Math terms get introduced once with a plain-English label, then used naturally
- Sentences vary in length — mix short punchy statements with longer explanations

---

## Visual Guidelines

| Visual type | Use when | Tool |
|-------------|----------|------|
| Mermaid flowchart | Algorithm flow, decision paths, sequential steps | Mermaid |
| Mermaid graph | Network structures, tree structures | Mermaid |
| HTML/CSS SVG | Scatter plots, curves, grids, spatial data | Raw HTML/CSS in markdown |

One visual per tutorial, placed in section 3. No external images — all visuals are rendered inline.

---

## Implementation Strategy — Pilot First

**Phase 1 — Pilot (3 tutorials):**  
Fully redesign these three tutorials to establish and validate the template:
1. `linear-regression` — math-heavy, HTML/CSS scatter + regression line visual
2. `decision-tree` — Mermaid branching tree visual, moderate math
3. `cnn` — HTML/CSS filter-sliding-over-grid visual, more complex math

Get approval on the pattern before proceeding.

**Phase 2 — Roll out (remaining 17 tutorials):**  
Apply the approved template to all remaining tutorials using the pilot as reference.

**Phase 3 — Site structure:**  
- Rename site title in `_config.yml`
- Add KaTeX to layout
- Restructure navigation into 5 tracks
- Create "Coming soon" placeholder pages for NLP, Transformers, LLMs & GPT

---

## Per-Tutorial Plan (all 20)

| Tutorial | What is it? hook | Visual type | Key formula |
|----------|-----------------|-------------|-------------|
| what-is-ml | ML lets computers learn patterns from examples instead of hard-coded rules | Mermaid: traditional programming vs ML flow | — (conceptual) |
| foundations | Three ways a machine can learn: supervised, unsupervised, reinforcement | Mermaid: learning types roadmap | — (conceptual) |
| linear-regression | Finds the best straight line through data to predict a number | HTML/CSS: scatter plot + regression line | $\hat{y} = \mathbf{w}^T\mathbf{x} + b$ |
| logistic-regression | Predicts the probability that something belongs to a category | HTML/CSS: sigmoid curve | $\hat{y} = \sigma(\mathbf{w}^T\mathbf{x} + b)$ |
| decision-tree | Asks a series of yes/no questions to reach a prediction | Mermaid: branching decision tree | Gini impurity / information gain |
| random-forest | Builds many decision trees and lets them vote | Mermaid: trees → majority vote | — (ensemble) |
| gradient-boosting | Trains models one after another, each correcting the last one's mistakes | Mermaid: sequential error-correction chain | $F_m(x) = F_{m-1}(x) + \eta \cdot h_m(x)$ |
| xgboost | Gradient boosting with regularisation and speed optimisations | Mermaid: optimised boosting flow | Regularised objective function |
| svm | Finds the widest possible margin between two classes | HTML/CSS: margin + support vectors diagram | $\min \frac{1}{2}\|\mathbf{w}\|^2$ subject to constraints |
| knn | Classifies a point by looking at its k nearest neighbours | HTML/CSS: scatter + highlighted neighbours | Distance formula |
| naive-bayes | Estimates the probability of a class given the input features | Mermaid: probability flow | Bayes' theorem: $P(C|X) = \frac{P(X|C)P(C)}{P(X)}$ |
| neural-networks-intro | Layers of connected nodes that transform inputs into predictions | Mermaid: input → hidden → output | — (conceptual) |
| mlp | A neural network with multiple hidden layers | Mermaid: deeper layered network | Activation + forward pass |
| deep-learning | Neural networks with many layers learning hierarchical features | Mermaid: stacked feature layers | — (conceptual) |
| cnn | Scans images in small patches to detect local patterns | HTML/CSS: filter sliding over pixel grid | Convolution operation |
| rnn | Processes sequences by passing a hidden state from step to step | Mermaid: sequential timestep flow | $h_t = \tanh(W_h h_{t-1} + W_x x_t)$ |
| lstm | An RNN with gates that control what to remember and forget | Mermaid: LSTM cell with forget/input/output gates | Gate equations |
| pca | Reduces many features down to the most important directions | HTML/CSS: 3D → 2D projection | Eigenvector decomposition |
| kmeans | Groups data into k clusters by minimising within-cluster distance | HTML/CSS: clustered scatter plot | $\arg\min_S \sum_{i=1}^k \sum_{x \in S_i} \|x - \mu_i\|^2$ |
| intermediate | Overview and signpost page linking all tutorials | Mermaid: learning path map | — |

---

## Out of Scope (this phase)

- NLP tutorials
- Transformer / attention mechanism tutorials
- GPT / LLM tutorials
- Any changes to the existing code snippets beyond what's in "Try It Yourself"
- Dark mode or theme changes
- Search functionality
