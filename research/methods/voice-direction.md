# Voice Direction (Post-Production)

**Function**: Repair or redirect a subject's voice performance after recording — correcting delivery inconsistencies, transposing prosody from a reference performance onto a cloned voice, or regenerating specific lines while preserving vocal identity.

**Used in**: *Virus* (in production), *The Goldberg Variations* (in development)

---

## What problem this solves

You recorded your subject's voice. It's the right voice — theirs, irreplaceable, carrying everything the film needs. But the delivery is inconsistent: across a long session, the rhythm shifts, the register loses its hold, the pacing collapses in some takes and lands in others.

You can't go back. The recording session is closed. Maybe the subject is no longer available, or the psychological conditions that produced the right takes can't be reproduced.

The classical solutions — re-record, cut around it, hire an actor — each destroy something. Re-recording loses the conditions. Cutting limits what can be said. A replacement voice loses the subject.

**AI voice direction** is the fourth option: repair the performance without replacing the voice.

This is the same logic as ADR (Automated Dialogue Replacement), but the technology has changed what's possible. You can now:
- Fix a specific mispronounced word without touching the surrounding take
- Transfer the prosodic pattern of a reference performance onto a cloned voice
- Regenerate a line the subject never recorded, in their voice, with a specific delivery

The ethical frame is identical to ADR: declared construction, subject's approval, specific to the production context.

---

## The craft question: how much to intervene

The worst mistake is reaching for the most powerful tool first. Every layer of AI processing carries a cost — not in quality, but in authenticity. The less you modify, the more the subject's voice remains theirs.

The right approach is a hierarchy of interventions, starting with the smallest:

```
Tier 0 — Enhance the raw recording
         (no voice AI, just signal processing)
         ↓
Tier 1 — Inpaint specific failures
         (replace a word or phrase, preserve everything else)
         ↓
Tier 2 — Speech-to-speech on full takes
         (correct delivery on takes that don't work as recorded)
         ↓
Tier 3 — Full regeneration from script
         (when no usable take exists)
```

Each tier is a different relationship to the subject's voice. In Tier 0, you're restoring what was there. In Tier 3, you're building what wasn't.

---

## Method: Enhance first (Tier 0)

Before any editorial judgment, run every recording through neural audio enhancement. What sounds unusable in a bedroom recording — flat, narrow, breathy — may be cinematically perfect once brought to studio level.

**[Resemble Enhance](https://github.com/resemble-ai/resemble-enhance)** (open source, Apache 2.0): neural speech denoising and enhancement. Run this before listening critically.

```bash
pip install resemble-enhance
resemble_enhance input.wav output_enhanced.wav --denoise --enhance
```

This step alone often renders further intervention unnecessary.

---

## Method: Inpainting (Tier 1)

**[Resemble Fill](https://www.resemble.ai/fill/)**: Audio inpainting. Mark the specific segment that fails — a word, a phrase, a line. The tool regenerates only that segment using the cloned voice, preserving breath patterns, room tone, and adjacent delivery intact.

Use when: the problem is localized. One word clipped, one syllable wrong, one line delivered flat in an otherwise strong take.

---

## Method: Speech-to-speech direction (Tier 2)

When the delivery of an entire take doesn't work — the pacing, the rhythm, the register — but the content is right and the voice is needed.

Speech-to-speech (S2S) takes an audio input and outputs it in a target voice. The key: **the prosody of the input is preserved**. You're not converting text to speech; you're converting one voice's delivery to another voice's timbre.

This opens a specific directorial technique:

> Record yourself — or a stand-in — performing the line with the delivery you want. Feed that recording into S2S with the subject's voice model. The output carries the subject's timbre, your direction.

Or, when the subject has some good takes and some bad ones:

> Use a well-delivered take from the same subject as the style reference. The subject directs themselves.

**[Respeecher](https://www.respeecher.com)** is the professional benchmark for this in film. Credits include *The Mandalorian*, *Obi-Wan Kenobi*, *The Brutalist* (2024), *Emilia Pérez* (2024), and *GOLIATH* (Wilt Chamberlain posthumous narration for Showtime/Paramount+). Their Emmy-winning work on Nixon's voice for *In Event of Moon Disaster* (IDFA 2019) is the direct precedent for documentary applications.

For open-source S2S without training: **[Seed-VC](https://github.com/Plachtaa/seed-vc)** — zero-shot, 1–30 seconds of reference audio.

---

## Method: Full regeneration (Tier 3)

When no usable take exists and you need to generate the line from text, in the subject's voice, with a specific delivery.

For **inner monologue and long-form documentary narration**: **[Sesame CSM](https://github.com/SesameAILabs/csm)** (Apache 2.0). Unlike TTS models optimized for clean speech, CSM was built for conversational naturalness — micro-pauses, breath patterns, register shifts. It processes previous audio as context, maintaining coherence of voice and register across long sequences. Architecturally the closest thing to how a voice actually moves through extended narration.

For **script regeneration with emotion control**: **[Chatterbox](https://github.com/resemble-ai/chatterbox)** (MIT, Resemble AI). Best open-source TTS for this purpose as of March 2026.

---

## The reference performance question

One specific use: you have a clear idea of the *flow* you want — a register, a rhythm, a way of inhabiting the text — but the subject's recordings don't capture it. You have a reference from elsewhere (another film, another voice) that has exactly that quality.

The technical question becomes: can you transfer the prosodic pattern of that reference onto the subject's voice?

The answer is yes, with caveats. S2S preserves the source's prosody (rhythm, pitch contour, pacing). Combined with a voice clone of the subject, you get the reference's delivery in the subject's timbre.

The artistic question is subtler. The reference gives the rhythm — the shape of the phrases, the weight of the pauses. The subject's voice fills that shape with everything that's theirs. It works best when the reference shares something with the subject that goes beyond technique: a way of holding complexity, a register of restrained emotion. The form is borrowed; the presence is still the subject's.

---

## Data custody

For documentary work involving real people: verify the ToS of any cloud platform before uploading a subject's voice data.

**Respeecher** and **Resemble AI** offer on-premise deployment and privacy-first options. ElevenLabs updated their Terms of Service in 2025 to claim perpetual rights over uploaded voice data — a significant consideration for films where the subject's voice is legally or ethically sensitive.

---

## Technical documentation

Full toolkit with installation instructions and code:
→ [cinema-voice-pipeline](https://github.com/ismael-joffroy-chandoutis/cinema-voice-pipeline)
