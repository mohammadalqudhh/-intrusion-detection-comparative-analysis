# Cross-Domain Cyber Threat Detection and Malware Classification
### Comparative Benchmarking of Machine Learning Models, Feature Selection Methods, and Hyperparameter Optimization Costs

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)
[![Domain](https://img.shields.io/badge/Domain-Cybersecurity%20%7C%20Machine%20Learning-black.svg)]()
[![Optimization](https://img.shields.io/badge/Tuning-GridSearchCV-blueviolet.svg)]()

---

## Project Overview
Modern cybersecurity architectures require a rigorous balance between detection efficacy and runtime execution efficiency. This benchmark investigates the trade-offs across classification metrics (Accuracy, Weighted F1-Score, Macro F1-Score) and computational complexity (training duration and inference latency per sample).

The experimental pipeline evaluates the cross-domain robustness of three machine learning paradigms (Gradient Boosting, Kernel Machines, and Artificial Neural Networks) across two fundamentally different attack spaces: packet-level tabular network flows and dynamic mobile syscall execution frequencies.

---
## Repository Roadmap & File Architecture

```text
├── notebooks/
│   ├── nsl_kdd.ipynb                                # End-to-end pipeline covering all experiments on NSL-KDD
│   ├── CICMalDroid_Main_Pipeline.ipynb              # End-to-end primary pipeline covering all experiments on CIC-MalDroid
│   ├── CICMalDroid_L2_Normalization.ipynb           # Experimental pipeline variant (80/20 split & L2 normalization)
│   ├── CICMalDroid_MinMax_Scaling.ipynb             # Experimental pipeline variant (70/30 split & MinMax scaling)
│   └── Cross_Domain_Comparison_NSL_vs_CIC.ipynb     # Cross-domain synthesis & comparative benchmark notebook
│
├── reports/
│   ├── NSL_KDD_Results_&_Evaluation.pdf             # Graphical analysis, confusion matrices & metrics for NSL-KDD
│   ├── MalDroid_Results_&_Evaluation.pdf            # Graphical analysis, feature distributions & metrics for MalDroid
│   └── Cross_Domain_Comparative_Analysis.pdf        # Comprehensive cross-dataset comparison report with unified charts
│
├── .gitignore
├── LICENSE
└── README.md
```
## Benchmark Datasets & Preprocessing

| Benchmark Dataset | Problem Domain | Data Modality | Original Features | Processed Dimension | Target Classes |
|---|---|---|---|---|---|
| **NSL-KDD** | Network Intrusion Detection (NIDS) | Tabular connection flows, protocol types, network flags, and host-based error rates | 41 attributes | 117 features (Post One-Hot Encoding & `MinMaxScaler`)| 5 Classes: Normal, DoS, Probe, R2L, U2R |
| **CIC-MalDroid2020** | Mobile Malware Attribution | Dynamic behavior logs tracking execution counts of Linux System Calls (Syscalls) and IPC Binders | 471 attributes | 470 normalized numeric frequency features| 5 Classes: Benign, Adware, Banking, SMS Malware, Riskware |

## Evaluated Architectures & Hyperparameter Optimization

### 1. Model Families & Parameter Tuning
Each architecture was evaluated under two distinct operational paradigms: **Default Baseline** (untuned default estimators) and **Optimized** (exhaustive search via `GridSearchCV` over stratified folds).

| Model Family | Estimator | Baseline Setup | GridSearchCV Optimization Parameter Space |
|---|---|---|---|
| **Gradient Tree Boosting** | `XGBoost` | Default boosting parameters with early stopping | Tuning tree depth (`max_depth`), learning rate (`learning_rate`), number of estimators (`n_estimators`), and regularization (`subsample`, `colsample_bytree`) |
| **Kernel Machines** | `SVM` | Linear Kernel & standard RBF Kernel ($\gamma=\text{scale}$) | Kernel selection (`linear` vs. `rbf`), penalty parameter ($C$), and kernel coefficient ($\gamma$) |
| **Neural Architectures** | `MLPClassifier` | Single hidden-layer baseline with standard Adam optimizer| Hidden layer topology (`hidden_layer_sizes`), activation functions (`relu`, `tanh`), L2 regularization penalty (`alpha`), and learning rate schedules |

### 2. The Role of GridSearchCV in This Research
Hyperparameter tuning via `GridSearchCV` was benchmarked as a core evaluation axis to measure return-on-investment (ROI) for computational resources:
* **System Performance vs. Computational Overhead:** Measuring whether exhaustive search across parameter combinations produces statistically meaningful gains in predictive accuracy or Macro F1 compared to baseline defaults.
* **Impact Across Estimators:** Observing how optimization affects models differently (e.g., stabilizing convergence in MLPs and SVMs versus marginal gains in tree-based ensembles).
* **Latency Inflation:** Quantifying wall-clock training expansion (e.g., scaling training from sub-second baselines up to hundreds of seconds under cross-validation grids).

---

## Feature Selection Strategies

| Selection Paradigm | Method Identifier | Mechanism & Selection Logic | Selected Feature Count |
|---|---|---|---|
| **Filter** | `Filter_MI` | Ranks non-linear dependence between features and target labels via Mutual Information gain | Top 30 features |
| **Filter** | `Filter_Chi2` | Non-parametric Chi-Square ($\chi^2$) statistical independence testing on normalized distributions | Top 30 features |
| **Wrapper** | `Wrapper_RFECV` | Recursive Feature Elimination with Stratified Cross-Validation using an XGBoost estimator to isolate the optimal subset| Dynamic: **50 features** (NSL-KDD) / **152 features** (MalDroid) |
| **Wrapper** | `Wrapper_Forward` | Sequential Forward Selection (SFS) greedy search evaluating iterative subset accuracy | Top 20 features |
| **Baseline Benchmark** | `Full_Features` | Complete normalized feature space without selection applied | 117 features (NSL-KDD) / 470 features (MalDroid) |

---

## Experimental Objectives

* **Dimensionality Reduction Impact:** Quantify detection degradation or improvement when reducing dimensions from full attribute spaces down to statistical and wrapper subsets.
* **Filter vs. Wrapper Trade-offs:** Measure whether wrapper methods justify their higher subset selection cost compared to fast statistical filter techniques.
* **GridSearchCV Cost-Benefit Analysis:** Determine the empirical boundary where exhaustive parameter search becomes counterproductive due to computational complexity.
* **Cross-Domain Transferability:** Evaluate model consistency when transitioning from structured network tabular counters to high-dimensional behavioral frequency distributions.

---

## Evaluation Metrics

| Metric | Evaluation Scope | Methodological Significance |
|---|---|---|
| **Overall Accuracy** | Total correct predictions over all evaluation instances | Primary standard indicator for overall benchmark capability |
| **Weighted F1-Score** | Multi-class harmonic mean weighted by ground-truth support per class | Accounts for class distribution variances across dominant categories |
| **Macro F1-Score** | Unweighted arithmetic mean across individual class F1-scores[cite: 9, 10] | Critical indicator penalizing performance drops on low-frequency classes (e.g., U2R, R2L) |
| **Training Latency** | Wall-clock execution time (in seconds) required for model convergence[cite: 9, 10] | Evaluates practical retraining feasibility in dynamic production environments |
| **Inference Latency** | Test batch prediction time (in seconds)[cite: 9, 10] | Evaluates operational readiness for real-time network and host packet inspection |


---

## Key Experimental Results & Benchmarks

### 1. NSL-KDD (Network Intrusion Detection)

| Model & Configuration | Selection Variant | Tuning Status | Accuracy | Macro F1 | Train Latency | Inference Latency |
|---|---|---|---|---|---|---|
| **MLP (Top Overall)** | Filter_Chi2 | Baseline | **78.33%** | **0.5661** | 9.11s | 0.02s |
| **XGBoost (Fastest Optimal)** | Wrapper_RFECV | Baseline | 77.79% | 0.5409 | **0.47s** | **0.01s** |
| **SVM (RBF)** | Wrapper_RFECV | Optimized | 77.52% | 0.5504 | 174.86s | 1.55s |
| **XGBoost** | Full_Features | Baseline | 77.74% | 0.5429 | 0.78s | 0.03s |
| **XGBoost** | Wrapper_RFECV | Optimized | 77.56% | 0.5276 | 135.57s | 0.03s |

* **Key Takeaway:** MLP utilizing `Filter_Chi2` delivered the highest multi-class generalization (0.5661 Macro F1), effectively detecting minority intrusions (U2R/R2L). Baseline XGBoost combined with `Wrapper_RFECV` achieved near-identical accuracy (77.79%) within a sub-second training footprint (**0.47s**)].

---

### 2. CIC-MalDroid2020 (Android Malware Attribution)

| Model & Configuration | Selection Variant | Tuning Status | Accuracy | Macro F1 | Train Latency | Inference Latency |
|---|---|---|---|---|---|---|
| **XGBoost (Top Overall & Fastest)** | Wrapper_RFECV | Baseline | **95.04%** | **0.9428** | **1.05s** | **0.003s** |
| **XGBoost** | Full_Features | Baseline | 95.04% | 0.9423 | 2.28s | 0.030s |
| **MLP** | Wrapper_RFECV | Optimized | 89.14% | 0.8723 | 90.45s | 0.004s |
| **SVM** | Wrapper_RFECV | Optimized | 87.97% | 0.8559 | 180.45s | 1.860s |
| **XGBoost** | Filter_MI | Baseline | 92.54% | 0.9089 | 0.56s | 0.010s |

* **Key Takeaway:** XGBoost paired with `Wrapper_RFECV` dominated across both accuracy (**95.04%**) and latency (**1.05s**)[cite: 10]. Wrapper selection reduced dimensionality from 470 down to 152 syscall features without any metric degradation.

---

## Empirical Insights & Trade-Off Analysis

* **The GridSearchCV Efficiency Penalty:** 
  Exhaustive grid search across XGBoost on MalDroid scaled training latency by **228x** (from 1.05s to 240.32s) without yielding any accuracy improvement over baseline defaults (remained static at 95.04%)[cite: 10]. Conversely, grid optimization was essential for SVM, boosting accuracy by **+13.8%** on MalDroid.
* **Filter vs. Wrapper Efficacy:** 
  `Wrapper_RFECV` consistently outperformed filter methods in classification metrics across both datasets. However, univariate filters (`Filter_Chi2`) proved exceptionally fast and effective for neural models on tabular data.
* **Operational Recommendation:** 
  For production inline network inspection and automated APK triage, **Baseline XGBoost with RFECV** provides the superior Pareto-optimal frontier between detection power and execution overhead.

# Network Intrusion & Android Malware Detection Using Machine Learning

An end-to-end machine learning pipeline for detecting network anomalies and classifying Android malware behaviors using **NSL-KDD** and **CICMalDroid 2020** datasets.

