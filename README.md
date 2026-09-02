# -intrusion-detection-comparative-analysis
Comparative analysis of ML pipelines for network intrusion and Android malware detection on NSL-KDD and CIC-MalDroid2020 using advanced feature selection
# 🛡️ Cross-Domain Cyber Threat Detection & Malware Classification
### Comparative Benchmarking of Machine Learning Pipelines, Feature Selection & Computational Trade-offs

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Domain](https://img.shields.io/badge/Domain-Cybersecurity%20%7C%20Machine%20Learning-red.svg)]()
[![Models](https://img.shields.io/badge/Models-XGBoost%20%7C%20SVM%20%7C%20MLP-green.svg)]()

---

## 📌 Executive Overview
This repository provides an end-to-end empirical study evaluating optimized Machine Learning (ML) architectures across two distinct cybersecurity domains:
1. **Network Intrusion Detection (NIDS):** Classifying network anomalies using the **NSL-KDD** benchmark dataset.
2. **Mobile Security (Android Malware):** Detecting and attributing malicious application behaviors using syscalls and binder transaction frequencies from the **CIC-MalDroid2020** dataset.

The core focus of this research is measuring the trade-offs between **predictive power (Accuracy & Macro F1)**, **feature dimensionality reduction**, and **computational overhead (training & inference latency)** across multiple algorithmic paradigms (Tree-based Ensembles, Kernel Methods, and Neural Architectures).

---

## 🔬 Core Evaluation Objectives
- **Feature Selection Benchmarking:** Comparing univariate statistical methods (**Filter:** Mutual Information, $\chi^2$) against iterative wrapper algorithms (**Wrapper:** RFECV, SFS).
- **Hyperparameter Optimization Impact:** Analyzing whether exhaustive parameter tuning (`GridSearchCV`) justifies the surge in computational training time compared to optimized baselines.
- **Cross-Domain Generalization:** Assessing how identical algorithmic pipelines adapt to tabular network traffic vs. high-dimensional Android behavioral signatures.

---
