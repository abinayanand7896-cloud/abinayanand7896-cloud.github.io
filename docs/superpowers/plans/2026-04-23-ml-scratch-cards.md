# ML Scratch Card Homepage Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the current `index.html` table-based homepage with a grouped peek/expand scratch card grid covering all 17 ML tutorials.

**Architecture:** Single file replacement — `index.html` keeps its Jekyll front matter so the existing just-the-docs nav/header renders unchanged. All CSS lives in a `<style>` block inside the file. No new files, no layout changes, no build config changes.

**Tech Stack:** Jekyll (just-the-docs theme), HTML, CSS (no JS required)

---

## Files Changed

| File | Change |
|---|---|
| `index.html` | Full replacement — new hero + CSS + 3 level sections of cards |

---

### Task 1: Replace `index.html` with the complete new homepage

**Files:**
- Modify: `index.html` (full replacement)

This is a static site — there are no automated tests. Correctness is verified by running Jekyll locally in Task 2.

- [ ] **Step 1: Back up the current file**

```bash
cp ~/abinayanand7896-cloud.github.io/index.html \
   ~/abinayanand7896-cloud.github.io/index.html.bak
```

- [ ] **Step 2: Write the new `index.html`**

Replace the entire contents of `index.html` with:

```html
---
layout: home
title: Home
nav_order: 1
---

<style>
/* ── Hero ─────────────────────────────────────────── */
.page-hero {
  text-align: center;
  padding: 40px 20px 28px;
  border-bottom: 1px solid #e2e8f0;
  margin-bottom: 36px;
}
.page-hero h1 {
  font-size: 28px;
  font-weight: 800;
  color: #0f172a;
  margin: 0 0 8px;
}
.page-hero p {
  font-size: 15px;
  color: #64748b;
  margin: 0 0 18px;
}
.hero-cta {
  display: inline-block;
  background: #1e293b;
  color: #fff;
  padding: 9px 20px;
  border-radius: 6px;
  font-size: 14px;
  font-weight: 600;
  text-decoration: none;
  transition: background 0.2s;
}
.hero-cta:hover { background: #334155; color: #fff; }

/* ── Level sections ───────────────────────────────── */
.level-section { margin-bottom: 40px; }

.level-header {
  display: flex;
  align-items: center;
  gap: 12px;
  margin-bottom: 16px;
  padding-bottom: 10px;
  border-bottom: 2px solid #e2e8f0;
}
.level-badge {
  font-size: 11px;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  padding: 3px 10px;
  border-radius: 20px;
  color: #fff;
}
.badge-l1 { background: #64748b; }
.badge-l2 { background: #334155; }
.badge-l3 { background: #0f172a; }

.level-title {
  font-size: 17px;
  font-weight: 700;
  color: #1e293b;
}
.level-count {
  font-size: 12px;
  color: #94a3b8;
  margin-left: auto;
}

/* ── Card grid ────────────────────────────────────── */
.cards-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
  gap: 14px;
}

/* ── Card ─────────────────────────────────────────── */
.tcard {
  border-radius: 10px;
  overflow: hidden;
  cursor: pointer;
  box-shadow: 0 2px 8px rgba(0,0,0,0.10);
  transition: box-shadow 0.2s, transform 0.2s;
  text-decoration: none;
  display: block;
}
.tcard:hover {
  box-shadow: 0 6px 20px rgba(0,0,0,0.18);
  transform: translateY(-2px);
}
.tcard-header {
  padding: 18px 14px 14px;
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  min-height: 96px;
  justify-content: center;
}
.tcard-icon { font-size: 26px; margin-bottom: 7px; }
.tcard-name {
  font-weight: 700;
  font-size: 12px;
  color: #fff;
  line-height: 1.35;
}

/* Level colour modifiers */
.l1 .tcard-header { background: linear-gradient(135deg, #64748b, #94a3b8); }
.l2 .tcard-header { background: linear-gradient(135deg, #334155, #475569); }
.l3 .tcard-header { background: linear-gradient(135deg, #0f172a, #1e293b); }

/* Peek strip */
.tcard-peek {
  background: #f8fafc;
  color: #475569;
  font-size: 11px;
  line-height: 1.5;
  padding: 0 12px;
  max-height: 0;
  overflow: hidden;
  transition: max-height 0.35s ease, padding 0.25s ease;
  border-top: 2px solid #e2e8f0;
}
.tcard:hover .tcard-peek {
  max-height: 80px;
  padding: 10px 12px;
}
</style>

<!-- Hero -->
<div class="page-hero">
  <h1>ML Tutorials</h1>
  <p>From Zero to Deep Learning — plain English, real code, no fluff.</p>
  <a class="hero-cta" href="{{ '/tutorials/what-is-ml' | relative_url }}">Start with What is ML? →</a>
</div>

<!-- Level 1 -->
<div class="level-section">
  <div class="level-header">
    <span class="level-badge badge-l1">Level 1</span>
    <span class="level-title">ML Foundations</span>
    <span class="level-count">10 tutorials</span>
  </div>
  <div class="cards-grid">

    <a class="tcard l1" href="{{ '/tutorials/what-is-ml' | relative_url }}">
      <div class="tcard-header">
        <div class="tcard-icon">❓</div>
        <div class="tcard-name">What is Machine Learning?</div>
      </div>
      <div class="tcard-peek">The big picture — what ML actually is, why it matters, and where it shows up in the real world.</div>
    </a>

    <a class="tcard l1" href="{{ '/tutorials/linear-regression' | relative_url }}">
      <div class="tcard-header">
        <div class="tcard-icon">📈</div>
        <div class="tcard-name">Linear Regression</div>
      </div>
      <div class="tcard-peek">Predict numbers using a straight line — the simplest and most widely used idea in all of ML.</div>
    </a>

    <a class="tcard l1" href="{{ '/tutorials/logistic-regression' | relative_url }}">
      <div class="tcard-header">
        <div class="tcard-icon">🔵</div>
        <div class="tcard-name">Logistic Regression</div>
      </div>
      <div class="tcard-peek">Sort things into two buckets — spam or not, sick or healthy — using a surprisingly simple trick.</div>
    </a>

    <a class="tcard l1" href="{{ '/tutorials/knn' | relative_url }}">
      <div class="tcard-header">
        <div class="tcard-icon">👥</div>
        <div class="tcard-name">K-Nearest Neighbors</div>
      </div>
      <div class="tcard-peek">Classify anything by asking: what do its closest neighbours look like?</div>
    </a>

    <a class="tcard l1" href="{{ '/tutorials/naive-bayes' | relative_url }}">
      <div class="tcard-header">
        <div class="tcard-icon">🎲</div>
        <div class="tcard-name">Naive Bayes</div>
      </div>
      <div class="tcard-peek">Make smart predictions using probability — fast, simple, and surprisingly good at text.</div>
    </a>

    <a class="tcard l1" href="{{ '/tutorials/decision-tree' | relative_url }}">
      <div class="tcard-header">
        <div class="tcard-icon">🌳</div>
        <div class="tcard-name">Decision Trees</div>
      </div>
      <div class="tcard-peek">Make decisions step by step — like a flowchart your computer can learn from data.</div>
    </a>

    <a class="tcard l1" href="{{ '/tutorials/random-forest' | relative_url }}">
      <div class="tcard-header">
        <div class="tcard-icon">🌲</div>
        <div class="tcard-name">Random Forests</div>
      </div>
      <div class="tcard-peek">Build hundreds of trees and let them vote — together they're far smarter than any one tree alone.</div>
    </a>

    <a class="tcard l1" href="{{ '/tutorials/svm' | relative_url }}">
      <div class="tcard-header">
        <div class="tcard-icon">✂️</div>
        <div class="tcard-name">Support Vector Machines</div>
      </div>
      <div class="tcard-peek">Find the widest possible boundary between two groups — works even when data isn't linearly separable.</div>
    </a>

    <a class="tcard l1" href="{{ '/tutorials/kmeans' | relative_url }}">
      <div class="tcard-header">
        <div class="tcard-icon">🔵</div>
        <div class="tcard-name">K-Means Clustering</div>
      </div>
      <div class="tcard-peek">Group similar data points together without any labels — the machine figures out the clusters itself.</div>
    </a>

    <a class="tcard l1" href="{{ '/tutorials/pca' | relative_url }}">
      <div class="tcard-header">
        <div class="tcard-icon">📉</div>
        <div class="tcard-name">PCA</div>
      </div>
      <div class="tcard-peek">Squash high-dimensional data down to fewer dimensions while keeping the most important patterns.</div>
    </a>

  </div>
</div>

<!-- Level 2 -->
<div class="level-section">
  <div class="level-header">
    <span class="level-badge badge-l2">Level 2</span>
    <span class="level-title">Intermediate ML</span>
    <span class="level-count">2 tutorials</span>
  </div>
  <div class="cards-grid">

    <a class="tcard l2" href="{{ '/tutorials/gradient-boosting' | relative_url }}">
      <div class="tcard-header">
        <div class="tcard-icon">⚡</div>
        <div class="tcard-name">Gradient Boosting</div>
      </div>
      <div class="tcard-peek">Build a powerful model by training each new tree to fix what the last one got wrong.</div>
    </a>

    <a class="tcard l2" href="{{ '/tutorials/xgboost' | relative_url }}">
      <div class="tcard-header">
        <div class="tcard-icon">🚀</div>
        <div class="tcard-name">XGBoost</div>
      </div>
      <div class="tcard-peek">A faster, smarter take on Gradient Boosting — the algorithm that wins most Kaggle competitions.</div>
    </a>

  </div>
</div>

<!-- Level 3 -->
<div class="level-section">
  <div class="level-header">
    <span class="level-badge badge-l3">Level 3</span>
    <span class="level-title">Deep Learning</span>
    <span class="level-count">5 tutorials</span>
  </div>
  <div class="cards-grid">

    <a class="tcard l3" href="{{ '/tutorials/neural-networks-intro' | relative_url }}">
      <div class="tcard-header">
        <div class="tcard-icon">🧠</div>
        <div class="tcard-name">What are Neural Networks?</div>
      </div>
      <div class="tcard-peek">The idea that changed everything — loosely inspired by the brain, but really just clever maths.</div>
    </a>

    <a class="tcard l3" href="{{ '/tutorials/mlp' | relative_url }}">
      <div class="tcard-header">
        <div class="tcard-icon">🔗</div>
        <div class="tcard-name">Multilayer Perceptron (MLP)</div>
      </div>
      <div class="tcard-peek">Your first real neural network — stack some layers, train it on data, watch it learn.</div>
    </a>

    <a class="tcard l3" href="{{ '/tutorials/cnn' | relative_url }}">
      <div class="tcard-header">
        <div class="tcard-icon">🖼️</div>
        <div class="tcard-name">CNN</div>
      </div>
      <div class="tcard-peek">Neural networks that see — how computers learn to recognise cats, faces, and X-rays.</div>
    </a>

    <a class="tcard l3" href="{{ '/tutorials/rnn' | relative_url }}">
      <div class="tcard-header">
        <div class="tcard-icon">🔄</div>
        <div class="tcard-name">RNN</div>
      </div>
      <div class="tcard-peek">Networks with memory — built for sequences like text, speech, and time-series data.</div>
    </a>

    <a class="tcard l3" href="{{ '/tutorials/lstm' | relative_url }}">
      <div class="tcard-header">
        <div class="tcard-icon">💡</div>
        <div class="tcard-name">LSTM</div>
      </div>
      <div class="tcard-peek">A smarter RNN that can remember things from way earlier in a sequence without forgetting.</div>
    </a>

  </div>
</div>
```

- [ ] **Step 3: Delete the backup**

```bash
rm ~/abinayanand7896-cloud.github.io/index.html.bak
```

---

### Task 2: Verify locally with Jekyll

**Files:** (none changed)

- [ ] **Step 1: Install dependencies if needed**

```bash
cd ~/abinayanand7896-cloud.github.io
bundle install
```

Expected: `Bundle complete!` (or already up to date)

- [ ] **Step 2: Start the local server**

```bash
bundle exec jekyll serve --livereload
```

Expected output includes:
```
Server address: http://127.0.0.1:4000
Server running... press ctrl-c to stop.
```

- [ ] **Step 3: Open in browser and verify**

Open `http://127.0.0.1:4000` and check:

| Check | Expected |
|---|---|
| Hero section visible | "ML Tutorials" heading + subtitle + CTA button |
| Level 1 section | Badge "LEVEL 1", title "ML Foundations", count "10 tutorials", 10 cards |
| Level 2 section | Badge "LEVEL 2", title "Intermediate ML", count "2 tutorials", 2 cards |
| Level 3 section | Badge "LEVEL 3", title "Deep Learning", count "5 tutorials", 5 cards |
| Hover a card | Card lifts + peek strip slides open showing description |
| Click a card | Navigates to the correct tutorial page |
| Mobile width (resize to ~375px) | Cards reflow to 2 columns naturally |
| Site nav (top/sidebar) | Unchanged — still shows all tutorials in sidebar |

- [ ] **Step 4: Stop the server**

Press `Ctrl+C` in the terminal running Jekyll.

---

### Task 3: Commit and push

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Stage and commit**

```bash
cd ~/abinayanand7896-cloud.github.io
git add index.html
git commit -m "Replace homepage table with scratch card grid"
```

- [ ] **Step 2: Push to GitHub Pages**

```bash
git push origin main
```

- [ ] **Step 3: Verify live site**

Wait ~60 seconds, then open `https://abinayanand7896-cloud.github.io` and repeat the checks from Task 2 Step 3 on the live site.
