## Sovereignty

On running AI locally, understanding every link in the chain, and why it matters for art.

---

I run AI models on local machines. Not on cloud APIs. Not through platforms that decide what I can and can't generate. On hardware I own, in a studio I control, with models whose weights sit on my hard drives.

This is not a political statement. It's a practical necessity for the kind of work I do.

---

### Why local

**No censorship.** Commercial platforms filter outputs according to their content policies. These policies are designed for the broadest possible market -- they block violence, nudity, ambiguity, anything that might generate a complaint. For consumer applications, this makes sense. For cinema, it's a fundamental constraint on expression. A filmmaker working on war, on the body, on trauma, on the margins of human experience cannot afford to have their image-making tool refuse to engage with their subject matter.

Running models locally removes these filters entirely. The full expressive range of the model is available. This doesn't mean producing offensive content for its own sake. It means having access to the complete palette, including the dark tones.

**Data sovereignty.** When you send an image to a cloud API, you lose control of it. It may be stored, logged, used for model improvement, or subject to the provider's terms of service. Documentary filmmakers work with sensitive material -- testimonies, personal archives, images of real people in real situations. Sending this material to a third-party server is not an option.

Local inference means the data never leaves the machine. The rushes, the archives, the personal corpora stay where they belong.

**Reproducibility.** Cloud APIs change. Models get updated, deprecated, replaced. A prompt that produced a specific image in March may produce something entirely different in June. For artistic work that depends on precise visual consistency -- or on the ability to revisit and extend a body of work -- this instability is unacceptable.

Local models are versioned. The same model, the same weights, the same seed, the same parameters will produce the same output indefinitely. The work is reproducible.

**Full artistic freedom.** Beyond censorship, platform constraints shape aesthetics in subtler ways. Default parameters, recommended workflows, UI design choices, model selection -- all of these nudge users toward certain kinds of images. Running locally means choosing every parameter yourself. It's more work. It's also more freedom.

---

### Understanding every link

Using AI as a black box is not an option for serious artistic work. If you don't understand what a diffusion model does -- how attention works, what embeddings represent, how the denoising process creates images -- you can't make informed decisions about your own work. You're pressing buttons and hoping for good results.

This doesn't mean becoming a machine learning researcher. It means understanding the architecture well enough to know why a certain prompt produces a certain result, why changing the guidance scale changes the image the way it does, why some LoRA adapters work and others don't, why certain aesthetic tendencies are baked into the model and can't be prompted away.

Technical literacy is the condition for creative appropriation. Without it, you're using someone else's tool the way they designed it to be used. With it, you can subvert the tool, push it beyond its intended behavior, and produce images the designers didn't anticipate.

The same applies to language models. Understanding tokenization, context windows, temperature, system prompts, retrieval-augmented generation -- these are not technical details for engineers. They're the material conditions of the creative conversation. Knowing them changes what you can ask for and what you can expect.

---

### The ecological question

The energy argument against AI art is mostly wrong, or at least wildly imprecise.

The studies cited in public debate (water consumption of ChatGPT, carbon footprint of training runs) refer to specific models at specific moments -- often GPT-3 era, 2023 data, industrial-scale training. These numbers are real but they describe one point in a rapidly changing landscape.

The distinction that matters is between **training** and **inference**. Training a large model from scratch costs millions of dollars and enormous energy. Using an already-trained model to generate images costs orders of magnitude less. A GPU generating images draws 200-400 watts during inference -- real power, but for seconds at a time, not days. Distilled models push this further: a quantized diffusion model can run on a phone, generating images while drawing less power than charging the battery. The per-image energy cost of local inference is a fraction of what the public debate suggests.

Working locally makes this visible. You can measure exactly how much power your machine draws during generation. You can track it, report it, think about it as part of the creative process. On a cloud platform, the energy cost is invisible -- abstracted behind an API call.

This transparency is itself a tool for responsibility. Not in a moralistic sense, but as a fact integrated into the practice: this image cost this much energy, took this many seconds, used this much VRAM. When the cost is visible, the choices become conscious.

None of this means AI art is ecologically innocent. It means the conversation needs to be more precise than "AI uses a lot of energy." Some AI uses a lot of energy. Some doesn't. The choice of model, hardware, and workflow determines which category you're in.

---

### Infrastructure as practice

The setup of a studio is not separate from the artistic work. The choice of hardware, network architecture, model selection, and software stack shapes what's possible and what's easy, which shapes what gets made.

Running a local AI infrastructure means maintaining machines, updating models, testing new architectures, debugging pipelines. This is unglamorous work. It's also creative work -- the infrastructure is the instrument, and building the instrument is part of playing it.

A network of GPUs connected at 10 gigabits, a NAS holding 96 terabytes of rushes and models, machines that can cluster together to pool their compute for a single heavy task or distribute across multiple users -- this is not a technical detail. It's the material condition that makes sovereign AI filmmaking possible. Without it, you're dependent on platforms that decide what you can make.

Building this infrastructure yourself, understanding every component, being able to fix it when it breaks -- this is the equivalent of a painter preparing their own pigments or a photographer developing their own film. It's not nostalgia. It's sovereignty.

---

[Back to Method](../README.md)
