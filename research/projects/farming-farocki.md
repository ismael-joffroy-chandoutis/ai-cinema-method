# *Farming Faroki / AI Farming* — In Development

**Type**: Film / installation (in development)
**Title**: *Farming Faroki* (working title) — also *AI Farming*, *Farming 2100*
**Reference**: Harun Farocki
**Submitted to**: Biennale Nemo (pitch)

---

## Concept

Farming Simulator as a filmmaking studio. A commercial video game — one of the most popular simulation games in the world, played mostly by people who will never farm — used as a virtual production environment for a film about agriculture, labor, and the future.

The title is a deliberate reference to Harun Farocki, the filmmaker who spent his career examining the images made by industrial and military processes — the camera not as witness but as production instrument. *Farming Faroki* applies the same logic to game engines: the simulator produces images of work that has been evacuated from the world it simulates.

---

## Why a game engine

Game engines have become the most sophisticated rendering environments available. Farming Simulator offers:
- Highly detailed agricultural environments (fields, machines, villages)
- Physically accurate weather, seasons, light
- Working vehicles and machinery
- Mutable time of day and atmospheric conditions

For a filmmaker, this is a fully controllable, infinitely re-stageable location. No weather delays. No permissions. No travel costs. A complete rural world available at any hour.

The AI layer adds: characters, faces, behaviors that the game doesn't provide natively.

---

## Pipeline: Farming Simulator → Blender → AI

### In-game capture

Game cinematics recorded from within Farming Simulator — using the game's internal camera tools and third-party cinematography mods that offer focal length, depth of field, and camera movement controls comparable to real production.

### Asset extraction and Blender integration

Character models (PED format) extracted from Farming Simulator and imported into Blender via documented conversion pipeline:
```
FS22 PED model → extraction → Blender import → rigging adjustment
→ AI-generated face texture (LoRA of specific character)
→ Animation (mocap or procedural)
→ Composite with in-game environment captures
```

### AI character generation

Game characters have generic, simulation-appropriate appearances. LoRA fine-tuning replaces or augments their visual identity:
- Faces generated from LoRA trained on interview subjects (farmers, agricultural workers)
- Skin, texture, lighting matched to the game environment via img2img

### GTA V integration (parallel research)

The same PED pipeline was tested for Grand Theft Auto V, which shares similar character formats and provides a very different environment (urban, contemporary). Research into using GTA V as a parallel visual world — the city and the field, the simulated criminal economy and the simulated agricultural economy — as contrasting registers in the same film.

---

## Research with young people

Interviews with high school students were recorded as source material — their relationship to farming, to simulation, to labor and its representations. The contrast between the game's nostalgic idealization of agricultural work and the economic reality of contemporary farming is one of the film's generative tensions.

---

## Relation to Farocki

Harun Farocki spent decades analyzing images produced by industrial, military, and surveillance systems — images that weren't made for humans to look at. Farming Simulator produces images of labor for leisure consumption: work as entertainment, the physical reality of farming translated into a relaxation game.

*Farming Faroki* turns the game's image-production apparatus back on itself — using it to make a film *about* what it simulates, looking at what the game cannot show of its own subject.

---

## Tools referenced

- Farming Simulator 22 — virtual production environment
- In-game cinematography mods — camera control
- Blender — asset integration, character animation
- GTA V — parallel environment research
- LoRA fine-tuning — character face generation (see [Custom Model Fine-Tuning](../methods/lora-custom-training.md))
- ComfyUI + SDXL — AI character augmentation
