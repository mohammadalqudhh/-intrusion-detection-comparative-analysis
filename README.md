# Cross-Domain Cyber Threat Detection and Malware Classification
### Comparative Benchmarking of Machine Learning Models, Feature Selection Methods, and Execution Costs

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)
[![Domain](https://img.shields.io/badge/Domain-Cybersecurity%20%7C%20Machine%20Learning-black.svg)]()
[![Frameworks](https://img.shields.io/badge/Frameworks-Scikit--Learn%20%7C%20XGBoost-orange.svg)]()

---

## Project Overview
Modern intrusion detection and malware classification architectures require a balanced optimization between classification metrics and operational execution costs[cite: 9, 10]. This benchmark investigates empirical performance trade-offs across predictive metrics (Accuracy, Weighted F1, Macro F1) and computational latency (training duration and inference time per sample)[cite: 9, 10]. 

The evaluation assesses model adaptability across two distinct cybersecurity domains: network traffic connection anomalies and Android dynamic behavioral syscall traces[cite: 9, 10].

---

## Benchmark Datasets

| Benchmark Dataset | Problem Domain | Data Modality | Target Classes |
|---|---|---|---|
| **NSL-KDD** | Network Intrusion Detection System (NIDS) | Tabular connection attributes, protocol metadata, service flags, and network error counters | 5 Classes: Normal, Denial of Service (DoS), Probe, Remote to Local (R2L), User to Root (U2R) |
| **CIC-MalDroid2020** | Mobile Malware Attribution | High-dimensional behavioral frequency logs (System Calls and Binder IPC transactions) | 5 Classes: Benign, Adware, Banking Malware, SMS Malware, Riskware |

---

## Evaluated Architectures & Search Strategies

### 1. Learning Algorithms

| Model Family | Representative Estimators | Evaluated Configurations |
|---|---|---|
| **Gradient Tree Boosting** | XGBoost | Default Baseline and GridSearchCV Hyperparameter Optimized[cite: 9, 10] |
| **Kernel Machines** | Support Vector Machine (SVM) | Linear Kernel, Radial Basis Function (RBF) Kernel, and Tuned Grid Search[cite: 9, 10] |
| **Neural Representations** | Multi-Layer Perceptron (MLP) | Baseline Feedforward Network and GridSearchCV Optimized Architecture[cite: 9, 10] |

### 2. Feature Selection Pipeline

| Paradigm | Selection Method | Mathematical / Search Mechanism | Feature Target |
|---|---|---|---|
| **Filter** | Mutual Information (`Filter_MI`) | Non-linear target mutual dependency and information gain[cite: 9, 10] | Top 30 Features |
| **Filter** | Chi-Square (`Filter_Chi2`) | Non-parametric statistical dependency testing on normalized space[cite: 9, 10] | Top 30 Features |
| **Wrapper** | RFECV (`Wrapper_RFECV`) | Recursive Feature Elimination coupled with XGBoost cross-validation[cite: 9, 10] | Optimal Dynamic Subset[cite: 9, 10] |
| **Wrapper** | Forward Selection (`Wrapper_Forward`) | Sequential Forward Selection (SFS) greedy optimization[cite: 9, 10] | Top 20 Features[cite: 9, 10] |

---

## Experimental Objectives

* **Dimensionality Impact:** Comparing baseline models trained on full feature spaces against isolated statistical subsets[cite: 9, 10].
* **Selection Strategy Dynamics:** Measuring accuracy and latency trade-offs between univariate filter metrics and search-guided wrapper methods[cite: 9, 10].
* **Optimization Cost-Benefit Analysis:** Quantifying whether computational overhead introduced by GridSearchCV translates into statistically significant metric gains[cite: 9, 10].
* **Cross-Domain Generalization:** Analyzing model stability across tabular network interactions versus sparse behavioral mobile calls[cite: 9, 10].

---

## Evaluation Metrics

| Metric | Measurement Scope | Methodological Importance |
|---|---|---|
| **Overall Accuracy** | Global correct predictions | High-level dataset benchmark verification[cite: 9, 10] |
| **Weighted F1-Score** | Multi-class harmonic mean weighted by class size | Performance indicator adjusting for moderate imbalance[cite: 9, 10] |
| **Macro F1-Score** | Unweighted mean across class-level F1-scores | Critical metric penalizing classification failures on rare classes (e.g., U2R, R2L)[cite: 9, 10] |
| **Training Latency** | Wall-clock seconds for model fitting | Assessment of retraining feasibility and compute expense[cite: 9, 10] |
| **Predict Latency** | Seconds required for batch inference | Verification of real-time detection readiness[cite: 9, 10] |
