---
title: "Crafter rollout collector: a web app for human & agent data"
date: 2026-05-20
description: "Crafter rollout collector web app — human & agent data, policy demos, world-model imagination."
excerpt: "Full-stack Crafter rollouts, policy demos in the browser, and world-model imagination side-by-side with real frames — motivation, stack, and literature."
report_link: "https://github.com/alexoh2bd/crafter-rollout-app"
report_label: "Repository: alexoh2bd/crafter-rollout-app →"
---

**crafter-rollout-app** is a full-stack web application for collecting rollouts in the **Crafter** open-world survival environment, demoing trained policy checkpoints in the browser, and visualizing **world-model imagination** rollouts next to real frames. It is designed so that dataset building, qualitative evaluation, and communication of model behavior stay in one loop instead of scattered scripts.

## Motivation

Goal-conditioned and model-based RL in procedurally generated worlds generate long, rich trajectories that are painful to inspect when everything runs headless. We wanted: (1) **reproducible human play** with automatic saving; (2) **agent demos** with lightweight inference overlays for debugging policies; and (3) a place to **compare imagined rollouts** from a latent dynamics model against the live environment — matching how practitioners validate world models in papers but rarely ship as a single UI.

## What it does

- **Human play** — Play Crafter in the browser; rollouts are saved when the session ends.
- **Agent demo** — Run a trained policy with real-time inference overlays.
- **Imagination demo** — Visualize world-model rollouts side-by-side with the real frame stream.

The stack is **FastAPI** + **Crafter** / **PyTorch** on the backend (with **Supabase** for persistence and **Railway** for hosting), and **Vite · React · TypeScript · Tailwind** on the frontend (deployed on **Vercel**). A live deployment is linked from the [GitHub README](https://github.com/alexoh2bd/crafter-rollout-app) (*crafter-rollout-app.vercel.app*).

## Related work

- Hafner, [Crafter](https://danijar.com/project/crafter/) — the benchmark environment for open-world agent learning that motivates the whole toolchain.
- World models & imagination — latent dynamics for rollouts build on the Dreamer line of work (e.g. [Dreamer](https://arxiv.org/abs/1912.01603)); the UI is meant to make those qualitative checks routine.
- Goal-conditioned RL & procedural worlds — connecting data collection to generalization in sparse-reward settings (see also recent GCRL and Crafter-based baselines in the literature).

## Results (what to expect)

The project is explicitly marked **work in progress**; the engineering outcome is a deployable pipeline (Docker, Railway + Vercel, documented in the repo's `docs/`) rather than a single leaderboard number. The useful "results" for now are: saved human/agent trajectories in a consistent format, working policy demos in the browser, and imagination visualizations that make it obvious when the world model diverges from reality. For build details, see [`docs/BUILD_SPEC.md`](https://github.com/alexoh2bd/crafter-rollout-app/blob/main/docs/BUILD_SPEC.md) and deployment notes in the same folder.
