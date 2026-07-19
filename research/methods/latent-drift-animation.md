# Latent Drift Animation

**Function**: Generate animated sequences by traversing diffusion model latent space — producing images that shift, morph, and transform without fixed temporal coherence. Used for oneiric, psychic, or expressionist sequences where instability is the point.

**Used in**: [*Virus*](../projects/virus-2024.md)

---

## What problem this solves

Some sequences in a film should not be stable. The psychological interior of a character, the subjective reconstruction of a traumatic event, the space between memory and hallucination — these resist the composed, temporally coherent image. Latent drift produces sequences where figures and environments continuously reconfigure: neither fully present nor absent, hovering in a state of permanent becoming.

This is distinct from video generation (which aims for temporal coherence). Latent drift uses temporal incoherence as an expressive property.

---

## Method: AnimateDiff

AnimateDiff is an open-source model that doesn't encode video sequences directly — it encodes motion instructions. Applied over Stable Diffusion, it produces "animated" images: frames that drift through a programmed sequence of states rather than depicting a fixed world from multiple angles.

The filmmaker defines:
- A start state (image or prompt)
- A sequence of intermediate states (through keyframe prompts or image references)
- Timing and transition behavior

The model generates frames that pass through these states, dissolving and reconstituting the image between them. The level of drift is controllable: tight timing produces smooth morphs; loose timing produces surreal transformations.

### Key properties

- **No temporal ground truth**: unlike video gen models, AnimateDiff doesn't try to maintain consistent objects across frames. Things shift, multiply, dissolve.
- **Latent space movement**: the "movement" is geometric in the model's internal representation, not physical in the world. Produces movements that feel psychological rather than physical.
- **High ControlNet compatibility**: combine with ControlNet to anchor some spatial properties while letting others drift. Fix the composition while the face dissolves.

---

## Alternative: closed-form video generation

For sequences that need temporal coherence (archival images brought to life, scenes that feel plausibly real), commercial video generation models (RunwayML Gen-3, Luma Dream Machine) produce better results:

- **Image-to-video**: take a still — archival, generated, or photographed — and animate it for 4–8 seconds. Used in *Virus* to animate photographs of Mihai and internet archives of the Curtis Culwell attack.
- **Video extension**: extend an existing clip by several seconds. The model "forgets" the initial frames progressively, producing a deliquescent drift at the edges of its context window — unintentional but cinematically productive.
- **CogVideoX** (open source): lower resolution (720×480), locally runnable. Paired with Topaz for upscale and frame interpolation for quality improvement.

---

## Post-processing

Raw AnimateDiff output is typically 512–768px and 8–16fps. Post-process:
- Topaz Video AI — upscale to 2K+ without generational artifacts
- RIFE / optical flow interpolation — smooth to 24fps or slow-motion
- DaVinci Resolve / FCP — color grading to integrate with photographic footage

---

## Tools referenced

- AnimateDiff (open source) — latent animation via diffusion
- RunwayML Gen-3 — commercial image-to-video
- Luma Dream Machine — commercial image-to-video
- CogVideoX (open source) — text/image to video
- Topaz Video AI — upscaling and interpolation
- ComfyUI — pipeline management
