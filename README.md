# 🛡️ Cross-Domain Cyber Threat Detection & Malware Classification
### Empirical Benchmarking of Machine Learning Architectures, Feature Selection Strategies, and Computational Latency

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Domain](https://img.shields.io/badge/Domain-Cybersecurity%20%7C%20Machine%20Learning-red.svg)]()
[![Models](https://img.shields.io/badge/Architectures-XGBoost%20%7C%20SVM%20%7C%20MLP-brightgreen.svg)]()

---

## 📌 Project Overview
Modern intrusion detection and malware analysis systems demand not only superior classification metrics but also low inference latency and minimal resource consumption. This research presents an end-to-end comparative study that investigates the trade-offs between **predictive accuracy**, **feature space dimensionality**, and **computational execution cost** (training and prediction latency).

The pipeline tests and validates how well three distinct algorithmic paradigms adapt across two fundamentally different cybersecurity problem spaces: network packet-level anomalies and mobile system-call behavioral sequences.

---

## 🎯 Target Domains & Benchmarks
The framework evaluates real-world cyber threat vectors across two standardized benchmarks:

* **Network Intrusion Detection System (NIDS) — `NSL-KDD`:**
  * **Objective:** Detect and classify anomalous network interactions into five traffic classes: *Normal*, *Denial of Service (DoS)*, *Probing/Scanning*, *Remote to Local (R2L)*, and *User to Root (U2R)*[cite: 3].
  * **Nature of Data:** Tabular network traffic metrics, connection durations, service flags, and protocol-level counters[cite: 3].

* **Mobile Malware Attribution — `CIC-MalDroid2020`:**
  * **Objective:** Identify, distinguish, and categorize Android application intent across five software profiles: *Benign*, *Adware*, *Banking Malware*, *SMS Fraud*, and *Riskware*[cite: 5].
  * **Nature of Data:** High-dimensional dynamic behavior capturing system calls (syscalls) and inter-process communication (binder) transaction frequencies[cite: 5].

---

## 🔬 What We Did & Compared
To uncover the optimal balance between performance and operational overhead, we conducted structured experiments comparing:

1. **Full Dimensionality vs. Targeted Subsets:**
   * Baseline performance utilizing all engineered attributes[cite: 9, 10].
   * Filter-based statistical extraction against wrapper-driven iterative selection[cite: 9, 10].
2. **Filter vs. Wrapper Feature Selection Paradigms:**
   * **Filter:** Statistical and dependency-driven ranking independent of any single model[cite: 4, 5].
   * **Wrapper:** Search-guided subset generation directly coupled to tree-based and sequential evaluation engines[cite: 4, 5].
3. **Out-of-the-Box vs. Tuned Models:**
   * **Baseline Configurations:** Standard default estimators evaluated for rapid prototyping[cite: 9, 10].
   * **Optimized Configurations:** Exhaustive grid exploration via `GridSearchCV` across defined hyperparameter bounds[cite: 9, 10].
4. **Accuracy vs. Computational Latency Trade-offs:**
   * Measuring the real-world trade-off: does a fractional gain in accuracy or Macro F1 justify exponential increases in training wall-clock time and prediction latency[cite: 9, 10]?

---

## ⚙️ Algorithms & Methodological Pipeline

* **Algorithmic Paradigms:**
  * **Gradient Boosted Trees:** `XGBoost` (evaluating splitting depth, regularization, and subsampling efficiency)[cite: 1, 2].
  * **Kernel Machines:** `Support Vector Machines (SVM)` using both **Linear** kernels for high-dimensional efficiency and **Radial Basis Function (RBF)** kernels for non-linear decision boundaries[cite: 9, 10].
  * **Neural Representations:** `Multi-Layer Perceptron (MLP)` feedforward artificial neural networks[cite: 1, 2].

* **Feature Selection Implementations:**
  * **Mutual Information (`Filter_MI`):** Measures non-linear dependency and information gain between features and target labels (Top 30 features)[cite: 4, 5].
  * **Chi-Square (`Filter_Chi2`):** Non-parametric statistical dependency testing for categorical and non-negative normalized distributions (Top 30 features)[cite: 4, 5].
  * **Recursive Feature Elimination with Cross-Validation (`Wrapper_RFECV`):** Uses an XGBoost core estimator to prune redundant features systematically[cite: 4, 5].
  * **Sequential Forward Selection (`Wrapper_Forward / SFS`):** A greedy forward search isolating an optimal 20-feature subset[cite: 4, 5].

* **Evaluation Metrics:**
  * Multi-class classification power: **Overall Accuracy**, **Weighted F1-Score**, and **Macro F1-Score** (prioritized to measure performance on critical minority attack classes like U2R and R2L)[cite: 9, 10].
  * Operational efficiency: **Training Time (seconds)** and **Per-sample Inference Latency (seconds)**[cite: 9, 10].
