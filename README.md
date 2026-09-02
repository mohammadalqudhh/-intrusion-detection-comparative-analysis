# Cross-Domain Cyber Threat Detection and Malware Classification
### Comparative Benchmarking of Machine Learning Models, Feature Selection Methods, and Hyperparameter Optimization Costs

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)
[![Domain](https://img.shields.io/badge/Domain-Cybersecurity%20%7C%20Machine%20Learning-black.svg)]()
[![Optimization](https://img.shields.io/badge/Tuning-GridSearchCV-blueviolet.svg)]()
[![Frameworks](https://img.shields.io/badge/Frameworks-Scikit--Learn%20%7C%20XGBoost-orange.svg)]()

---

## Project Overview
Modern cybersecurity architectures require a rigorous balance between detection efficacy and runtime execution efficiency. This benchmark investigates the trade-offs across classification metrics (Accuracy, Weighted F1-Score, Macro F1-Score) and computational complexity (training duration and inference latency per sample)[cite: 9, 10].

The experimental pipeline evaluates the cross-domain robustness of three machine learning paradigms (Gradient Boosting, Kernel Machines, and Artificial Neural Networks) across two fundamentally different attack spaces: packet-level tabular network flows and dynamic mobile syscall execution frequencies[cite: 9, 10].

---

## Benchmark Datasets & Preprocessing

| Benchmark Dataset | Problem Domain | Data Modality | Original Features | Processed Dimension | Target Classes |
|---|---|---|---|---|---|
| **NSL-KDD** | Network Intrusion Detection (NIDS) | Tabular connection flows, protocol types, network flags, and host-based error rates[cite: 3] | 41 attributes[cite: 3] | 117 features (Post One-Hot Encoding & `MinMaxScaler`)[cite: 3] | 5 Classes: Normal, DoS, Probe, R2L, U2R[cite: 3] |
| **CIC-MalDroid2020** | Mobile Malware Attribution | Dynamic behavior logs tracking execution counts of Linux System Calls (Syscalls) and IPC Binders[cite: 5] | 471 attributes[cite: 5] | 470 normalized numeric frequency features[cite: 5] | 5 Classes: Benign, Adware, Banking, SMS Malware, Riskware[cite: 5] |

---

## Evaluated Architectures & Hyperparameter Optimization

### 1. Model Families & Parameter Tuning
Each architecture was evaluated under two distinct operational paradigms: **Default Baseline** (untuned default estimators) and **Optimized** (exhaustive search via `GridSearchCV` over stratified folds)[cite: 9, 10].

| Model Family | Estimator | Baseline Setup | GridSearchCV Optimization Parameter Space |
|---|---|---|---|
| **Gradient Tree Boosting** | `XGBoost` | Default boosting parameters with early stopping[cite: 9, 10] | Tuning tree depth (`max_depth`), learning rate (`learning_rate`), number of estimators (`n_estimators`), and regularization (`subsample`, `colsample_bytree`)[cite: 1, 2] |
| **Kernel Machines** | `SVM` | Linear Kernel & standard RBF Kernel ($\gamma=\text{scale}$)[cite: 9, 10] | Kernel selection (`linear` vs. `rbf`), penalty parameter ($C$), and kernel coefficient ($\gamma$)[cite: 9, 10] |
| **Neural Architectures** | `MLPClassifier` | Single hidden-layer baseline with standard Adam optimizer[cite: 9, 10] | Hidden layer topology (`hidden_layer_sizes`), activation functions (`relu`, `tanh`), L2 regularization penalty (`alpha`), and learning rate schedules[cite: 1, 2] |

### 2. The Role of GridSearchCV in This Research
Hyperparameter tuning via `GridSearchCV` was benchmarked as a core evaluation axis to measure return-on-investment (ROI) for computational resources[cite: 9, 10]:
* **System Performance vs. Computational Overhead:** Measuring whether exhaustive search across parameter combinations produces statistically meaningful gains in predictive accuracy or Macro F1 compared to baseline defaults[cite: 9, 10].
* **Impact Across Estimators:** Observing how optimization affects models differently (e.g., stabilizing convergence in MLPs and SVMs versus marginal gains in tree-based ensembles)[cite: 9, 10].
* **Latency Inflation:** Quantifying wall-clock training expansion (e.g., scaling training from sub-second baselines up to hundreds of seconds under cross-validation grids)[cite: 9, 10].

---

## Feature Selection Strategies

| Selection Paradigm | Method Identifier | Mechanism & Selection Logic | Selected Feature Count |
|---|---|---|---|
| **Filter** | `Filter_MI` | Ranks non-linear dependence between features and target labels via Mutual Information gain[cite: 4, 5] | Top 30 features[cite: 4, 5] |
| **Filter** | `Filter_Chi2` | Non-parametric Chi-Square ($\chi^2$) statistical independence testing on normalized distributions[cite: 4, 5] | Top 30 features[cite: 4, 5] |
| **Wrapper** | `Wrapper_RFECV` | Recursive Feature Elimination with Stratified Cross-Validation using an XGBoost estimator to isolate the optimal subset[cite: 4, 5] | Dynamic: **50 features** (NSL-KDD)[cite: 4] / **152 features** (MalDroid)[cite: 5] |
| **Wrapper** | `Wrapper_Forward` | Sequential Forward Selection (SFS) greedy search evaluating iterative subset accuracy[cite: 4, 5] | Top 20 features[cite: 4, 5] |
| **Baseline Benchmark** | `Full_Features` | Complete normalized feature space without selection applied[cite: 9, 10] | 117 features (NSL-KDD)[cite: 3] / 470 features (MalDroid)[cite: 5] |

---

## Experimental Objectives

* **Dimensionality Reduction Impact:** Quantify detection degradation or improvement when reducing dimensions from full attribute spaces down to statistical and wrapper subsets[cite: 9, 10].
* **Filter vs. Wrapper Trade-offs:** Measure whether wrapper methods justify their higher subset selection cost compared to fast statistical filter techniques[cite: 9, 10].
* **GridSearchCV Cost-Benefit Analysis:** Determine the empirical boundary where exhaustive parameter search becomes counterproductive due to computational complexity[cite: 9, 10].
* **Cross-Domain Transferability:** Evaluate model consistency when transitioning from structured network tabular counters to high-dimensional behavioral frequency distributions[cite: 9, 10].

---

## Evaluation Metrics

| Metric | Evaluation Scope | Methodological Significance |
|---|---|---|
| **Overall Accuracy** | Total correct predictions over all evaluation instances[cite: 9, 10] | Primary standard indicator for overall benchmark capability[cite: 9, 10] |
| **Weighted F1-Score** | Multi-class harmonic mean weighted by ground-truth support per class[cite: 9, 10] | Accounts for class distribution variances across dominant categories[cite: 9, 10] |
| **Macro F1-Score** | Unweighted arithmetic mean across individual class F1-scores[cite: 9, 10] | Critical indicator penalizing performance drops on low-frequency classes (e.g., U2R, R2L)[cite: 9, 10] |
| **Training Latency** | Wall-clock execution time (in seconds) required for model convergence[cite: 9, 10] | Evaluates practical retraining feasibility in dynamic production environments[cite: 9, 10] |
| **Inference Latency** | Test batch prediction time (in seconds)[cite: 9, 10] | Evaluates operational readiness for real-time network and host packet inspection[cite: 9, 10] |
