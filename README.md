# Cross-Domain Cyber Threat Detection & Malware Classification
### Empirical Benchmarking of Machine Learning Architectures, Feature Selection Strategies, and Computational Latency

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue.svg)]([https://www.python.org/](https://www.python.org/))
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Domain](https://img.shields.io/badge/Domain-Cybersecurity%20%7C%20Machine%20Learning-red.svg)]()
[![Models](https://img.shields.io/badge/Architectures-XGBoost%20%7C%20SVM%20%7C%20MLP-brightgreen.svg)]()

---

## Project Overview
Modern intrusion detection and malware analysis systems demand high classification metrics along with minimal inference latency and resource consumption. This research presents a comparative study evaluating the trade-offs between predictive accuracy, feature space dimensionality, and computational execution cost (training and prediction latency).

The pipeline validates how well three distinct algorithmic paradigms adapt across two different cybersecurity problem spaces: network packet-level anomalies and mobile system-call behavioral sequences.

---

## Target Domains & Benchmarks
The framework evaluates real-world cyber threat vectors across two standardized benchmarks:

* **Network Intrusion Detection System (NIDS) — NSL-KDD:**
* **Objective:** Detect and classify anomalous network interactions into five traffic classes: Normal, Denial of Service (DoS), Probing/Scanning, Remote to Local (R2L), and User to Root (U2R).
* **Nature of Data:** Tabular network traffic metrics, connection durations, service flags, and protocol-level counters.

* **Mobile Malware Attribution — CIC-MalDroid2020:**
* **Objective:** Identify, distinguish, and categorize Android application intent across five software profiles: Benign, Adware, Banking Malware, SMS Fraud, and Riskware.
* **Nature of Data:** High-dimensional dynamic behavior capturing system calls (syscalls) and inter-process communication (binder) transaction frequencies.

---

## What We Did & Compared
To determine the optimal balance between performance and operational overhead, we conducted structured experiments comparing:

1. **Full Dimensionality vs. Targeted Subsets:**
* Baseline performance utilizing all engineered attributes.
* Filter-based statistical extraction against wrapper-driven iterative selection.
2. **Filter vs. Wrapper Feature Selection Paradigms:**
* **Filter:** Statistical and dependency-driven ranking independent of any single model.
* **Wrapper:** Search-guided subset generation directly coupled to tree-based and sequential evaluation engines.
3. **Out-of-the-Box vs. Tuned Models:**
* **Baseline Configurations:** Standard default estimators evaluated for rapid prototyping.
* **Optimized Configurations:** Exhaustive grid exploration via GridSearchCV across defined hyperparameter bounds.
4. **Accuracy vs. Computational Latency Trade-offs:**
* Quantifying whether marginal improvements in accuracy or Macro F1 justify exponential increases in training wall-clock time and prediction latency.

---

## Algorithms & Methodological Pipeline

* **Algorithmic Paradigms:**
* **Gradient Boosted Trees:** XGBoost evaluating tree depth, subsampling rates, and regularization penalties.
* **Kernel Machines:** Support Vector Machines (SVM) using Linear kernels for high-dimensional efficiency and Radial Basis Function (RBF) kernels for non-linear boundary separation.
* **Neural Architectures:** Multi-Layer Perceptron (MLP) feedforward neural networks evaluating layer depth, activation dynamics, and solver convergence.

* **Hyperparameter Optimization (GridSearchCV):**
* Exhaustive parameter exploration using stratified k-fold cross-validation across defined grids, tuning parameters such as regularization parameter C, kernel coefficient gamma, learning rates, and tree depths.
* Benchmarked directly against untuned estimators (Baseline vs. Optimized) to evaluate empirical returns against compute overhead.

* **Feature Selection Implementations:**
* **Mutual Information (Filter_MI):** Measures non-linear dependency and information gain between features and target classes (Top 30 features).
* **Chi-Square (Filter_Chi2):** Non-parametric statistical dependency testing for categorical and non-negative normalized distributions (Top 30 features).
* **Recursive Feature Elimination with Cross-Validation (Wrapper_RFECV):** Employs an XGBoost estimator to systematically prune redundant attributes and extract optimal subsets.
* **Sequential Forward Selection (Wrapper_Forward / SFS):** A forward search isolating an optimal 20-feature subset.

* **Evaluation Metrics:**
* Multi-class classification performance: Overall Accuracy, Weighted F1-Score, and Macro F1-Score (prioritizing detection across rare attack categories).
* Resource overhead: Training Time (seconds) and Per-sample Inference Latency (seconds).
