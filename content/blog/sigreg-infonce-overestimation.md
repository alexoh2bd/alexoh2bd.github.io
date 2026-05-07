---
title: "Correcting overestimation bias with SIGReg + InfoNCE"
date: 2026-05-15
description: "SIGReg + InfoNCE for overestimation bias in JaxGCRL — Alex Oh"
excerpt: "Main takeaway from JaxGCRL experiments: pairing Sketched Isotropic Gaussian Regularization with InfoNCE reins in optimistic contrastive signal."
report_link: "https://wandb.ai/aho13-duke-university/JaxGCRL_testing/reports/stand-still-please---VmlldzoxNTkzMzY5Mg"
report_label: "Full report on Weights & Biases →"
---

**Main takeaway.** In our **JaxGCRL** experiments, combining **InfoNCE** with **SIGReg** (Sketched Isotropic Gaussian Regularization — distribution matching that pushes representations toward an isotropic Gaussian structure and limits collapse) helps **correct overestimation bias** that shows up when the contrastive signal is left unconstrained. The regularizer does not replace InfoNCE; it *calibrates* the representation geometry so the contrastive objective does not become misleadingly optimistic about how aligned or informative the embeddings are during training.

## Why overestimation shows up next to InfoNCE

InfoNCE is built as a lower bound on mutual information between views. In practice, with finite batches, sampling effects, and the nonlinear encoder, that bound can be loose: optimization can chase a contrastive score that *looks* better than what the representation geometry actually supports — the empirical analogue of overestimation. In goal-conditioned RL settings that lean on contrastive shaping, that bias can leak into unstable targets or overly confident alignment between states and goals.

## What SIGReg changes

SIGReg adds an explicit signal that embeddings should not collapse or drift into pathological shapes: sketching-based tests encourage slices of the representation to match a Gaussian reference. That trades a bit of headroom on raw contrastive margin for more honest, well-conditioned features — which is where the combination with InfoNCE pays off in the runs summarized in the report.

## How to read the linked report

The Weights & Biases report ("stand still please") walks through the runs where we compared objectives side by side. Use it for figures, sweeps, and exact configs; treat this page as the one-sentence-to-one-paragraph summary you can point collaborators to.

[Open the W&B report](https://wandb.ai/aho13-duke-university/JaxGCRL_testing/reports/stand-still-please---VmlldzoxNTkzMzY5Mg)
