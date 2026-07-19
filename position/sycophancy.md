## Sycophancy

On AI's compulsion to please, and what it reveals.

---

In machine learning, sycophancy describes a model's tendency to agree with the user rather than give accurate answers -- to tell you what you want to hear. The word itself comes from Ancient Greek: *sykophantes*, originally a professional accuser in the courts of Athens, someone who brought denunciations for personal gain. The meaning drifted over centuries toward flattery and servile agreement. In both senses -- the ancient informer and the modern yes-man -- there's something performed, something strategic, something that serves the system rather than the truth.

It's a useful word for what generative AI does to images.

---

### The aesthetic of pleasing

Every generative model has a default aesthetic. It's not neutral. It's optimized.

Ask a diffusion model for a landscape and you get golden hour, volumetric fog, cinematic depth of field. Ask for a portrait and you get smooth skin, catch lights in the eyes, a shallow bokeh that flatters the subject. Ask for a woman and you get a fashion magazine silhouette -- elongated, symmetrical, posed. Every output carries a glaze of surface beauty that I call the "Geo magazine" look: images that are immediately appealing, technically accomplished, and completely empty.

This is not a bug. It's structural. These models are trained on datasets filtered by aesthetic scores, curated from platforms where engagement determines visibility. The training data overrepresents what people like, share, and reward. The biases aren't engineered intentionally -- they emerge from the data's own skew and from the evaluation metrics used to select "good" outputs during training. The result is an image-making machine that systematically gravitates toward comfort, resolution, and conventional beauty -- not because someone designed it that way, but because that's what the data looks like when you average it.

For commercial applications, this makes sense. For art, it's a problem -- and a subject.

---

### What sycophancy hides

The sycophantic tendency of AI reveals several things at once:

**Cultural biases as statistical averages.** When a model consistently produces a certain type of beauty, it's showing you the mean of its training data. That mean is not universal -- it's the aggregate preference of the people who produced and labeled the images the model learned from. The "beautiful" that emerges is a specific cultural construction: Western, commercial, youth-oriented, Instagram-inflected.

**The refusal of the strange.** Models resist producing images that are genuinely disturbing, ambiguous, or formally radical. Not just through content filters (which block explicit material) but through aesthetic filters built into the optimization itself. The model gravitates toward coherence, resolution, recognizability. It takes deliberate effort -- low guidance scales, adversarial prompts, model hacking -- to push it toward the zone where interesting images live.

**The confusion between quality and compliance.** A sycophantic model produces images that look "good" in the same way a sycophantic colleague tells you your work is "great." The flattery prevents honest engagement. When every generated image looks polished, you lose the ability to distinguish between a genuine discovery and a decorated cliche.

---

### Working against the grain

The artistic response to sycophancy is not to reject generative models but to work against their default behavior. Several strategies:

**Refuse the first output.** The first image a model produces is almost always the most sycophantic -- the closest to what it thinks you want to see. It's the statistical center. The interesting work starts when you push past this center toward the edges: lower coherence, higher noise, unexpected combinations.

**Use open-source models locally.** Commercial APIs add layers of filtering on top of the model's intrinsic sycophancy: content policies, safety classifiers, aesthetic guardrails. Running models locally strips these layers and gives access to the full range of the model's behavior, including the zones that platforms hide. (More on this in [Sovereignty](souverainete.md).)

**Make the bias visible.** Instead of correcting for sycophancy, make it the subject. Show the magazine aesthetic. Show the stereotypes. Show the smoothing. Let the viewer see what "beautiful" means when it's defined by an optimization function. The gap between the sycophantic output and the real image is a critical space that cinema can inhabit.

**Demand disagreement.** For language models specifically, I configure my tools to challenge rather than agree. The default mode of most AI assistants is compliance -- telling you what you want to hear. Inverting this, asking the model to push back, to find problems, to be critical, produces a much more useful creative dynamic. A collaborator who always agrees is not a collaborator.

---

### Why this matters for cinema

Cinema has always had a relationship with flattery. Commercial cinema flatters the audience. Advertising flatters the product. Social media flatters the self. AI-generated images inherit and amplify all of these tendencies simultaneously.

For filmmakers working with generative tools, sycophancy is not a peripheral concern. It's the central aesthetic question. If you don't actively resist it, every frame you produce will carry that glaze of automated approval -- beautiful, polished, and fundamentally dishonest about what the world looks like.

The most urgent task for AI cinema is not to make better images. It's to make images that are honest about what they are: synthetic, biased, optimized for attention, and capable -- when handled critically -- of revealing exactly that.

---

[Back to Method](../README.md)
