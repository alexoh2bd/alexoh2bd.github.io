---
title: "Steering experiments to reduce sycophancy (linear probes on LLM activations)"
date: 2026-05-20
description: "Linear probes and steering to detect sycophancy and calibrate assertiveness in LLMs."
excerpt: "TruthfulQA + epistemic-integrity probes, steering on MHA/MLP/residuals — citations, pipeline, and what to expect from the experiments."
report_link: "https://github.com/alexoh2bd/Sycophancy_Probes"
report_label: "Repository: alexoh2bd/Sycophancy_Probes →"
---

**Sycophancy_Probes** implements **linear probes** on internal transformer activations to study **sycophancy** and related failure modes, plus an **assertiveness** pipeline for epistemic calibration. The codebase trains probes on **MHA**, **MLP**, and **residual-stream** locations, then runs **activation steering** at inference (subtracting a scaled probe direction from hidden states) to push behavior toward more truthful or better-calibrated outputs.

## Motivation

Alignment failures often show up as *flattery* — models agreeing with incorrect user premises — or as *overconfident* wording even when the answer is wrong. Both patterns are partially linearly decodable from activations: prior work shows sycophancy-related structure in attention heads, and epistemic-integrity work separates warranted vs unwarranted certainty. This repo operationalizes those ideas for **Gemma** (and extensible to other LLMs): extract activations, train cheap linear probes, and intervene with the same geometry investigators use for interpretability.

## What we built

- **Sycophancy probes** — Binary classifiers trained on [TruthfulQA](https://arxiv.org/abs/2109.07958)-derived supervision, with per-layer/head probes and inference drivers for MHA, MLP, and residual paths.
- **Assertiveness probes** — Ridge regression on the **epistemic-integrity** dataset to predict continuous assertiveness, steering with scale (e.g. negative scale to *reduce* assertiveness), R² heatmaps across layers/heads, and evaluation scripts on steered vs unsteered generations.
- **Steering mechanics** — Interventions of the form `h ← h − α × direction`, with projection scale tied to activation std so steps are comparable across heads.

## Related literature

- Lin et al., [TruthfulQA](https://arxiv.org/abs/2109.07958) — benchmark for truthfulness; we use it (via project-specific data generation) for sycophancy-oriented probe training.
- [Sycophancy hides linearly in the attention heads](https://arxiv.org/html/2601.16644v1) (2026) — theoretical and empirical motivation that sycophancy is linearly accessible; implementation builds on the associated [reference code](https://github.com/rifoagenadi/sycophancy).
- [Epistemic integrity in large language models](https://arxiv.org/html/2411.06528v2) (2025) — dataset and framing for assertiveness / epistemic calibration; [epistemic-integrity](https://github.com/ComplexData-MILA/epistemic-integrity) supplies training and test splits for the ridge probes.
- Representative work on **sycophancy** in deployed chat models (e.g. Anthropic's public analyses) motivates steering as a complement to preference optimization when you need *local*, interpretable interventions for research.

## Results

The repository is set up for **replicable pipelines**: SLURM batch scripts, saved probe weights (`.pth` / `.pkl`), accuracy dictionaries, optional **Weights & Biases** logging for assertiveness runs, and CSV exports of predictions for downstream evaluation. Reported headline metrics depend on model checkpoint, steering scale, and head subset (`k_heads`); the README documents the exact `uv run` commands and expected artifact paths. Qualitatively, steering reduces agreement with false user beliefs on TruthfulQA-style probes when the linear direction is well identified; assertiveness steering shifts wording toward less overconfident phrasing on held-out epistemic-integrity items, with evaluation hooks to compare incorrect steered vs unsteered responses.

**Contacts** (from the project): [Jenny Chen](mailto:janet.chen@duke.edu), [Alex Oh](mailto:alex.oh@duke.edu), [Rishika Randev](mailto:rishika.randev@duke.edu).
