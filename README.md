<p align="center">
  <img src="header-banner.jpg" alt="AI Cinema Method" width="100%"/>
</p>

# AI Cinema Method

**AI as artistic method — position, research, and memory architecture from a filmmaker working across cinema, contemporary art, and AI.**

[![License: CC BY-SA 4.0](https://img.shields.io/badge/License-CC_BY--SA_4.0-lightgrey.svg)](https://creativecommons.org/licenses/by-sa/4.0/)
![Post-documentary](https://img.shields.io/badge/Post--documentary-cinema-cc0000?style=flat-square)
![AI Art](https://img.shields.io/badge/AI-as_subject-blueviolet?style=flat-square)

---

## Author

[Ismaël Joffroy Chandoutis](https://ismaeljoffroychandoutis.com) — filmmaker and artist based in Paris. **César 2022**, Cannes (Semaine de la Critique), IDFA, Hot Docs, Ars Electronica. Currently developing *The Goldberg Variations* (Villa Albertine 2026) and *Virus*.

---

## What this is

Notes on practice, not a manifesto. How and why I work with machines — and why "with" is already the wrong word.

This repo brings together three things:

1. **Position** — essays on the artistic practice: liquid writing, post-documentary, sovereignty, AI as subject
2. **Memory** — the architecture problem: how to sustain a two-year documentary project with a stateless AI
3. **Research** — methods and projects: spatial reconstruction, LoRA training, generative pipelines, tested on real films

---

## Position

**Alteration over augmentation.** I don't use AI to do things faster. I use it to displace my own thinking — to find connections I would miss alone. The goal is not optimization. The goal is displacement.

| Essay | Topic |
|-------|-------|
| [Liquid Writing](position/ecritures-liquides.md) | Research, writing, image-making, and tool-building happen simultaneously, contaminating each other |
| [Post-documentary](position/post-documentary.md) | Films that start from real events but treat the boundary between document and fabrication as the subject itself |
| [Post-photography](position/post-photography.md) | The image after the camera |
| [Sovereignty](position/souverainete.md) | Data sovereignty as artistic position, not technical preference |
| [You Are Your Own Cloud](position/your-own-cloud.md) | Building a sovereign post-production studio, after Cioni — local-first beyond AI inference |
| [On Sycophancy](position/sycophancy.md) | Why AI agreement is more dangerous than AI error |
| [Tripartite](position/tripartite.md) | Three image regimes: documentary, generative, hybrid |
| [Verbatim](position/verbatim.md) | The value of raw transcript over summary |

### Influences

| Domain | Key references |
|--------|---------------|
| Cinema | Harun Farocki, Chris Marker, Hito Steyerl, Peter Tscherkassky, Philippe Grandrieux, Forensic Architecture |
| Art | Gregory Chatonsky, Trevor Paglen, Holly Herndon & Mat Dryhurst, Ian Cheng |
| Theory | Roger Caillois, Vilém Flusser, Gilbert Simondon, Donna Haraway, Yuk Hui |

---

## Memory

The hard problem in working with AI on long-form projects: **the model has no memory**.

| Document | What it addresses |
|----------|-------------------|
| [The Filesystem is the Memory](memory/the-filesystem-is-the-memory.md) | Context engineering for a two-year documentary project. How the filesystem becomes the memory architecture for a stateless AI. |

Key insight: your `CLAUDE.md` is not a config file — it's a persistent instruction layer. Your `memory/` directory is not a folder — it's a surrogate for what the model cannot retain. Your session backups are not archives — they're the only source of truth.

---

## Research

AI methods documented from active film production. Organized by function, not by tool — tools change every six months, the underlying questions don't.

### Methods

| Method | What it solves |
|--------|---------------|
| [Spatial Reconstruction](research/methods/spatial-reconstruction.md) | Reconstruct real locations as navigable 3D spaces |
| [3D-Guided Image Generation](research/methods/controlnet-3d-guided.md) | Control image composition via 3D scene data |
| [Blender → ComfyUI Real-Time](research/methods/blender-comfyui-realtime.md) | Drive diffusion from live 3D animation |
| [Custom Model Fine-Tuning](research/methods/lora-custom-training.md) | Train location- or character-specific image models |
| [Cinematic Still Image](research/methods/sd-cinematic-image.md) | Generate stills in the register of documentary photography |
| [Latent Drift Animation](research/methods/latent-drift-animation.md) | Animate through latent space for oniric sequences |
| [Archive Animation](research/methods/animation-archives.md) | Give motion to archival photographs and footage |
| [Voice Direction](research/methods/voice-direction.md) | AI voice repair for documentary subjects |
| [Image Upscaling](research/methods/image-upscaling.md) | Cinema-grade upscaling pipeline |
| [LLM Corpus Avatar](research/methods/llm-corpus-avatar.md) | Build an AI avatar from a subject's written corpus |
| [Local-First Studio Workflow](research/methods/local-first-studio-workflow.md) | NAS as storage, Mac Mini as server, Strada as P2P protocol — the architecture documented |
| [Strada and Alternatives Benchmark](research/methods/strada-alternatives-benchmark.md) | Comparative reading of remote post-production tools, storage solutions, and open source options |

### Projects

Films and installations where these methods were developed.

| Project | Context | Year |
|---------|---------|------|
| [DamasIA](research/projects/damasIA-2023.md) | Live performance, ARTE / Gaîté Lyrique | 2023 |
| [Nemo](research/projects/nemo-biennale-2023.md) | Installation, Biennale Nemo | 2023 |
| [Ciclic Residency](research/projects/vendome-residency-2024.md) | R&D residency, Ciclic Animation, Vendôme | 2024 |
| [Virus](research/projects/virus-2024.md) | Feature documentary (in production) | 2024— |
| [Palimpseste mémoriel](research/projects/palimpseste-1972.md) | Visual research, family archive LoRA | Ongoing |
| [Farming Farocki](research/projects/farming-faroki.md) | Film / installation (in development) | In dev |

### Essays

| Essay | Topic |
|-------|-------|
| [L'IA et l'artiste : cadre critique](research/essays/ia-artiste-cadre-critique.md) | Critical framework for AI in artistic practice |
| [Dissolution des frontières médiatiques](research/essays/ia-dissolution-frontieres-mediatiques.md) | How AI dissolves the boundary between media |

---

## Status

Living document. Updated as the films progress and the methods evolve.

---

**Consolidated from:** [method](https://github.com/12georgiadis/method) · [the-filesystem-is-the-memory](https://github.com/12georgiadis/the-filesystem-is-the-memory) · [ai-cinema-research](https://github.com/12georgiadis/ai-cinema-research)
