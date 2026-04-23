# ML Tutorials — Scratch Card Homepage Design

**Date:** 2026-04-23
**Status:** Approved

---

## Overview

Replace the current `index.html` homepage (which shows a plain Markdown table of tutorials) with a visually engaging scratch card grid. Tutorials are grouped by level, each rendered as a peek/expand card. Hovering a card slides open a short plain-English description; clicking navigates to the tutorial page.

The site uses Jekyll with the `just-the-docs` remote theme. The new homepage keeps Jekyll front matter so the existing nav, header, and footer continue to render unchanged.

---

## Design Decisions

| Question | Choice | Reason |
|---|---|---|
| Card interaction | Peek/Expand on hover | Feels natural; reveals info without leaving the page |
| Peek content | 1–2 sentence plain-English description | Easy to read; fits the "no jargon" tone of the site |
| Grouping | Keep Level 1 / Level 2 / Level 3 labels | Already familiar to returning visitors |
| Page location | Replace `index.html` | Card grid IS the homepage; no extra pages needed |
| Color scheme | Slate Depth (light → mid → dark grey per level) | Minimal, elegant, lets content lead |
| Implementation | Pure HTML+CSS inside `index.html` | Simple, zero build changes, easy to maintain |

---

## Page Structure

```
index.html
├── Jekyll front matter (layout: home, title: Home, nav_order: 1)
├── Hero section
│   ├── Site title: "ML Tutorials"
│   ├── Subtitle: "From Zero to Deep Learning — plain English, real code, no fluff."
│   └── CTA link: "Start with What is ML? →"
├── Level 1 — ML Foundations (10 cards, slate-400/500 gradient)
├── Level 2 — Intermediate ML (2 cards, slate-600/700 gradient)
└── Level 3 — Deep Learning (5 cards, slate-800/900 gradient)
```

Each level section has:
- A header row: level badge (coloured pill) + level name + tutorial count
- A responsive card grid (CSS grid, `auto-fill`, min 150px per card)

---

## Card Component

```
.tcard (anchor tag → tutorial URL)
├── .tcard-header  (coloured gradient background)
│   ├── .tcard-icon   (emoji, 26px)
│   └── .tcard-name   (bold, white, 12px)
└── .tcard-peek   (slides open on hover, max-height transition)
    └── 1–2 sentence description (11px, slate-600)
```

**Hover behaviour:**
- Card lifts slightly (`translateY(-2px)`) and shadow deepens
- `.tcard-peek` transitions from `max-height: 0` to `max-height: 72px` over 350ms

**Click behaviour:** navigates to the tutorial's Jekyll page (e.g. `tutorials/linear-regression`).

---

## Tutorial Content (Cards)

### Level 1 — ML Foundations

| # | Icon | Name | Description |
|---|------|------|-------------|
| 1 | ❓ | What is Machine Learning? | The big picture — what ML actually is, why it matters, and where it shows up in the real world. |
| 2 | 📈 | Linear Regression | Predict numbers using a straight line — the simplest and most widely used idea in all of ML. |
| 3 | 🔵 | Logistic Regression | Sort things into two buckets — spam or not, sick or healthy — using a surprisingly simple trick. |
| 4 | 👥 | K-Nearest Neighbors | Classify anything by asking: what do its closest neighbours look like? |
| 5 | 🎲 | Naive Bayes | Make smart predictions using probability — fast, simple, and surprisingly good at text. |
| 6 | 🌳 | Decision Trees | Make decisions step by step — like a flowchart your computer can learn from data. |
| 7 | 🌲 | Random Forests | Build hundreds of trees and let them vote — together they're far smarter than any one tree alone. |
| 8 | ✂️ | Support Vector Machines | Find the widest possible boundary between two groups — works even when data isn't linearly separable. |
| 9 | 🔵 | K-Means Clustering | Group similar data points together without any labels — the machine figures out the clusters itself. |
| 10 | 📉 | PCA | Squash high-dimensional data down to fewer dimensions while keeping the most important patterns. |

### Level 2 — Intermediate ML

| # | Icon | Name | Description |
|---|------|------|-------------|
| 11 | ⚡ | Gradient Boosting | Build a powerful model by training each new tree to fix what the last one got wrong. |
| 12 | 🚀 | XGBoost | A faster, smarter take on Gradient Boosting — the algorithm that wins most Kaggle competitions. |

### Level 3 — Deep Learning

| # | Icon | Name | Description |
|---|------|------|-------------|
| 13 | 🧠 | What are Neural Networks? | The idea that changed everything — loosely inspired by the brain, but really just clever maths. |
| 14 | 🔗 | Multilayer Perceptron (MLP) | Your first real neural network — stack some layers, train it on data, watch it learn. |
| 15 | 🖼️ | CNN | Neural networks that see — how computers learn to recognise cats, faces, and X-rays. |
| 16 | 🔄 | RNN | Networks with memory — built for sequences like text, speech, and time-series data. |
| 17 | 💡 | LSTM | A smarter RNN that can remember things from way earlier in a sequence without forgetting. |

---

## CSS Architecture

All styles live in a `<style>` block inside `index.html`. No external CSS file is needed. Classes follow a simple flat naming convention:

- `.page-hero` — top hero section
- `.level-section` — one group (level)
- `.level-header`, `.level-badge`, `.level-title`, `.level-count` — header row parts
- `.cards-grid` — CSS grid container
- `.tcard`, `.tcard-header`, `.tcard-icon`, `.tcard-name`, `.tcard-peek` — card parts
- `.l1`, `.l2`, `.l3` — level colour modifiers applied to `.tcard`

---

## Responsiveness

The card grid uses `grid-template-columns: repeat(auto-fill, minmax(150px, 1fr))` so it reflows naturally on mobile — no media queries needed.

---

## Files Changed

| File | Change |
|---|---|
| `index.html` | Full replacement — new hero + card grid HTML+CSS |

No other files are touched.
