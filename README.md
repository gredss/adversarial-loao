# Adversarial Robustness of Parameter-Efficient Fine-Tuning under Cross-Domain Shift
[![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)](#)
[![PyTorch](https://img.shields.io/badge/PyTorch-%23EE4C2C.svg?style=for-the-badge&logo=PyTorch&logoColor=white)](#)
[![HuggingFace](https://img.shields.io/badge/%F0%9F%A4%97%20Hugging%20Face-Spaces-yellow?style=for-the-badge)](#)
[![NVIDIA](https://img.shields.io/badge/nVIDIA-%2376B900.svg?style=for-the-badge&logo=nVIDIA&logoColor=white)](#)
[![LaTeX](https://img.shields.io/badge/latex-%23008080.svg?style=for-the-badge&logo=latex&logoColor=white)](#)

[![NLP](https://img.shields.io/badge/NLP-Indonesian-blue)](#)
[![Model](https://img.shields.io/badge/Backbone-IndoBERTweet-brightgreen)](#)
[![PEFT](https://img.shields.io/badge/Method-LoRA%20vs%20Full%20FT-red)](#)
[![Security](https://img.shields.io/badge/Security-Adversarial%20Robustness-blueviolet)](#)
[![Reduction](https://img.shields.io/badge/Parameters-97.6%25%20Reduced-blue)](#)
[![Security](https://img.shields.io/badge/Robustness--Gap--0.79%25-brightgreen)](#)

This repository contains the implementation and experimental results for a comparative study on the security and efficiency of Indonesian transformer models. The research evaluates the impact of Low-Rank Adaptation (LoRA) versus Full Fine-Tuning when subjected to character-level evasion attacks in a cross-domain, Leave-One-App-Out (LOAO) environment.

## Research Objectives

The primary goal of this project is to determine if Parameter-Efficient Fine-Tuning (PEFT) provides superior adversarial resilience compared to Full Fine-Tuning when handling Indonesian social media data. The study addresses three critical dimensions:

1. **Cross-Domain Generalization:** Evaluating how models trained on specific ride-hailing applications (Gojek, Grab, Maxim) generalize to unseen domains.
2. **Adversarial Security:** Testing model integrity against sensitivity-based character obfuscation.
3. **Resource Efficiency:** Measuring the trade-off between predictive accuracy and computational overhead (VRAM and Training Latency).

## Methodology

### Dataset: The Subride Corpus

The research utilizes a curated dataset of Indonesian ride-hailing reviews labeled for subjectivity (70% Subjective, 30% Objective). The dataset is partitioned using a Leave-One-App-Out (LOAO) strategy to simulate real-world domain shifts where a model must process data from an application not seen during training.

### Optimization Paradigms

The study contrasts two distinct adaptation strategies applied to the **IndoBERTweet** backbone:

* **Full Fine-Tuning:** Updates all 110.6 million parameters of the model.
* **LoRA:** Updates only 2.68 million parameters (2.42%) by injecting rank-decomposition matrices () into the attention layers.

Both paradigms utilize a custom **WeightedTrainer** to address class imbalance through a weighted cross-entropy loss function.

## Adversarial Threat Model

The security analysis employs a score-based black-box evasion attack. The adversary identifies the linguistic "Achilles' heel" of a review through Word Importance Ranking (WIR), formally defined as:

Following sensitivity analysis, character-level obfuscation (dot-insertion) is applied to the highest-ranked token (e.g., transforming "parah" to "p.a.r.a.h"). This method exploits subword tokenization vulnerabilities to induce misclassification while remaining human-readable.

## Key Findings

Experimental results demonstrate that LoRA-tuned models consistently achieve a superior security-efficiency Pareto frontier.

* **Stability:** LoRA exhibited significantly lower robustness gaps across all domains. In the Grab domain, LoRA achieved a -0.79% robustness gap, indicating nearly perfect resilience to the adversarial perturbations.
* **Efficiency:** LoRA reduced the updated parameter space by over 97.5% while maintaining clean F1-scores comparable to full fine-tuning.
* **Memory Optimization:** PEFT allowed for higher throughput and reduced GPU memory allocation, facilitating rapid iteration on low-resource hardware.
