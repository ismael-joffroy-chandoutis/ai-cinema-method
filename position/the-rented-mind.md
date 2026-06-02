## The Rented Mind

*Notes on efficiency, after a year of building a studio around models I do not own. Draft, June 2026. A working note. It will evolve as the economics do.*

---

For about a year, the culture around agentic AI ran on a single flex: burn the most tokens. *Token maxing.* You proved you were serious by how much compute you could set on fire, how many parallel agents you could keep alive, how long you could leave a model running unattended while it churned. The number going up was the point. I did it too. I left a frontier model running errands a shell script could have done, because the running itself felt like progress.

That period is ending, and the thing replacing it is more honest. Call it *efficient maxing*: not how much you spend, but what you get back for what you spend. Output per token. The question stops being "how much can I burn" and becomes "is this the cheapest path to the same result." Most of the elaborate setups people built in the first phase, mine included, turn out to be careful answers to a question nobody was really asking.

This essay is the working note from that shift. It continues an argument I have been making in this repository about sovereignty: in the [sovereignty essay](souverainete.md) about running models on local hardware, and in [You Are Your Own Cloud](your-own-cloud.md) about owning the post-production substrate. Both essays ended on the same uncomfortable admission, which I want to make the subject this time rather than the footnote. There is one layer of the studio I cannot yet own. The reasoning layer. The mind. I rent it. What follows is an attempt to think clearly about why, for how long, and on what terms.

---

### Two efficiencies, not one

The word *efficiency* hides two different problems, and conflating them is where most of the money gets wasted.

The first is operational. Inside a single session, how much do you spend to get the work done. This is the token-and-context problem, and it is mostly a question of discipline. I come back to it near the end.

The second is strategic, and it is the expensive one. It is the capital problem. What do you buy, what do you rent, and what do you refuse to buy because the thing you would buy it for is a moving target. The first wave of the AI hype told a generation of independent creators to go into debt for hardware. Buy the maxed-out workstation. Buy four of them. Build the rig. The reasoning was that compute is the constraint, so own the compute.

For one half of the work, that reasoning is correct. For the other half, it is a trap. Telling the two halves apart is the whole game.

---

### Rent the mind, own the hands

Here is the division I have arrived at, and the one this essay argues for.

**Own the hands.** Everything downstream of a decision, the parts of the work that are deterministic, repeatable, and bound to your own material, should run on hardware you control. Image generation, video synthesis, 3D rendering, upscaling, the storage that holds years of a project, the memory the system reads from, and, crucially, the headroom to run the orchestration harness itself. This is the argument of the two earlier essays and I will not repeat it. The hands have gotten cheap. A local diffusion stack on a consumer GPU costs effectively nothing per image once the card is paid for. Storage has collapsed to roughly eleven dollars per terabyte. The case for owning the hands is settled.

**Rent the mind.** The frontier reasoning model, the one that holds the architecture of a long project in its head and decides what to do next, is the single layer where renting is still the correct call in mid-2026. Not because owning it is impossible. Because owning it does not yet pay.

That distinction matters, and the discourse around it is full of lies in both directions. Let me take them one at a time.

---

### The lie of "impossible," and the lie of "full local"

The first lie is that you cannot run a frontier-class model on your own machine. This is false, and it has been false since early 2025. A 512 GB Mac Studio runs DeepSeek V3, a 671-billion-parameter model, at roughly twenty tokens per second in 4-bit, on Apple's own MLX framework. As of April 2026, the top DeepSeek V4 tier scores within a fraction of a point of a Western frontier model on SWE-bench Verified, the standard coding benchmark, and a 284-billion-parameter V4 Flash with only 13 billion active parameters runs comfortably on far less than 512 GB. The weights are open. The license is permissive. You can, literally, own a near-frontier mind.

The second lie is the mirror of the first: that you therefore can and should go *full local*. The people who say this are usually not running the workloads they claim. Two things break it.

**Speed collapses with context.** That twenty-tokens-per-second figure is for a short prompt. Push the context to sixteen or thirty-two thousand tokens, the regime any real agentic session lives in, and throughput falls off a cliff while memory use climbs toward the ceiling. A model that is pleasant for a chat becomes unusable for an agent that has to read a corpus, hold a plan, and call tools across a long horizon.

**The agentic gap is wider than the benchmark gap.** On a single-shot task the open models are within a point or two of the frontier. On multi-step agentic work, the kind where the model has to plan, call a tool, read the result, recover from its own mistake, and stay coherent across dozens of turns, the gap opens to something like seven or ten points. That is precisely the work I actually use a model for. The benchmark that flatters local inference is not the benchmark that governs orchestration.

So the honest statement is narrow, and it is this. For chat and for bounded single-shot tasks, a local near-frontier model is here today, and you should use it. For long-horizon agentic orchestration, renting the frontier is still correct in mid-2026, not on principle but on throughput and coherence. The seam I admitted in the earlier essays is real, and it sits exactly here.

---

### The moving target, and why not to pre-buy it

The tempting move, for someone who values sovereignty, is to buy the big box now as a hedge. Spend ten thousand euros on a maxed-out machine so that the day a true frontier model is small enough to run well locally, you are ready.

Do not do this, and the reason is the same logic that made the hands cheap. The target is moving away from you faster than you can buy toward it. Models are getting more capable per active parameter, not less. The mixture-of-experts trend means a model can carry hundreds of billions of parameters but activate only a small slice of them per token, so the effective cost of running real capability keeps falling. At the same time the machines keep getting better and the price for a given capability keeps dropping. If you buy the ten-thousand-euro box today as a bet on a model that does not exist yet, then on the day that model ships, a better and cheaper box will ship with it. You will have paid a premium to be early to a party that had not started.

The correct reason to buy a large machine is the work it does today, not the model it might run tomorrow. If a big box earns its cost now, in rendering, in video, in holding and serving your material, buy it, and treat any future local-inference capability as a bonus you did not pay for. If its only justification is the hypothetical local frontier model, wait. The waiting gets cheaper every quarter. This is the one place where the sovereignty instinct, left unchecked, leads you to spend against your own interest.

---

### The flip, and what readiness means

None of this means the flip is far away. The night I am drafting this, Microsoft used its Build 2026 keynote to release MAI-Thinking-1, a reasoning model trained from scratch with no distillation, mid-size at thirty-five billion parameters, reported to match a leading Western frontier model on coding benchmarks. A mid-size, non-distilled model reaching frontier coding quality is exactly the signal that the line between the rented mind and the owned one is about to move. It will not be the last. The arrival of capable models small enough to own is a question of months, not years, and a working studio should expect the ground to shift under this essay while the essay is still online.

So readiness is not pre-buying. It is two disciplines held at once.

The first is to watch for the flip and be ready to act *at* it, not ahead of it. When a model you can genuinely own reaches the quality your work needs, the hardware that runs it well may not be sitting on a shelf waiting for you. Capability that confers real power tends to rarefy: the machines everyone suddenly wants are the ones that get back-ordered, allocated, and repriced. The readiness that matters is financial and logistical slack at the moment of the flip, not a box bought a year early and depreciating while you wait for the reason to use it.

The second is to insist that the mind you own is actually ownable. MAI-Thinking-1, for all that it signals, launched in private preview on a hosted platform, not as open weights. You cannot finetune it, you cannot inspect or correct its biases, you cannot re-align it to your work, you cannot retrain it on your own material, and you cannot keep it if the platform changes its terms. A model you run through someone else's preview is rented with extra steps. The whole reason to own the mind rather than rent it is the freedom to modify it: to finetune, to re-align, to audit the bias, to retrain. If a model is not open-weight, owning the hardware under it buys you almost nothing the API did not already give you. Open weights are the precondition that turns local hardware into sovereignty instead of into an expensive way to run someone else's closed model.

This extends to the hardware. The machine worth owning at the flip is the repairable, resaleable, general-purpose one, not the sealed appliance tuned to a single vendor's stack. The flip rewards optionality: parts you can replace, a platform you can repurpose, resale value that funds the next machine. Sovereignty is not a single purchase. It is keeping the freedom to move.

---

### The cheap-mind wave

While the local question stays unresolved, something else has shifted the economics underneath it: the price of *renting* a near-frontier mind has fallen through the floor, and the floor is in China.

As of mid-2026, the strongest low-cost models for agentic work come from Chinese labs. The DeepSeek V4 tiers and the MiniMax M2 line deliver coding and agentic performance close to the Western flagships at a small fraction of the price. A Western frontier model can cost twenty-five to seventy-five dollars per million output tokens. The Chinese near-equivalents land around one to three. The output-per-dollar gap is not a few percent. It is an order of magnitude, sometimes more. For a working creator on a real budget, that is not a detail. It is the difference between an agentic practice you can afford to run all day and one you have to ration.

I think this wave is structural, not promotional. The labs that ship the cheapest capable models set the floor everyone else has to price against, and right now those labs are not in California. For the bulk of agentic work, the economically literate move in mid-2026 is to route most of it to the cheapest model that clears the quality bar, and to reserve the expensive frontier model for the work that genuinely needs it.

But routing by price alone is a mistake, and it is the mistake nobody writing about efficiency seems to name.

---

### Route by capability and by sensitivity

This is the part I have not seen said cleanly, and it is the reason this essay is not just an optimization guide.

When you send text to a hosted API, you send it to someone else's servers, under someone else's terms, with no enforceable guarantee about retention, training use, or disclosure. For most work that is an acceptable trade. For some work it is a breach dressed up as an efficiency.

If your material is the words of people who trusted you with them, the subjects of a film, sources, anyone whose words you hold only because they let you, then the cheapest API is not the right tool, no matter how good its benchmark. The economic question, what is the cheapest model that clears the bar, has to be subordinated to a prior question, what is the most exposed place this text is allowed to go. For a practice that touches sensitive lives, routing the transcript of someone's confession through whichever API is cheapest this month is not lean. It is a failure of the duty that let you have the material in the first place.

So the routing rule has two axes, not one.

**By capability.** Send each task to the weakest, cheapest model that can actually do it. Most tasks do not need the frontier. Sending everything to the most expensive model is the same error as token maxing, just slower to notice on the invoice.

**By sensitivity.** Classify the material before you classify the task. A sensitive tier, anything entrusted to you, anything whose exposure would harm a person, stays on infrastructure you trust, which in practice means either a provider with contractual guarantees you have actually read, or local inference where the text never leaves your machine. A bulk tier, your own scaffolding, throwaway drafts, public inputs, generic code, goes to the cheapest capable model, and the cheapest capable model is increasingly the one from a lab you would never hand a confession to.

The two axes are independent. The cheap model is fine for the insensitive task and wrong for the sensitive one regardless of its quality. Efficiency that ignores the second axis is not efficiency. It is risk you have not priced yet.

---

### The operational layer: your context is a budget

Below the strategic question sits the smaller, daily one, and it is worth a section because it is the same idea at a different scale.

Inside a session, the scarce resource is not really tokens, it is the context window, and most agentic setups waste it badly. The most common waste in mid-2026 is loading dozens of integration tools into the model's context at the start of every session, each carrying a schema the model has to hold whether or not it ever uses it. A heavy integration you invoke twice a week is a standing tax on every session in between. The fix is unglamorous. Move the deterministic, scriptable operations out of the always-loaded layer and into plain command-line tools the model calls only when it needs them. You reclaim the context the schemas were occupying, and reclaimed context is reclaimed money and reclaimed reasoning headroom in equal measure.

This is the operational mirror of the strategic argument. Own the hands, rent the mind, and do not make the rented mind carry weight it does not need to carry. The same discipline, one scale down.

---

### What this is, and what it is not

This is not a recommendation, and it is not a buyer's guide. The specific numbers in it, the tokens per second, the dollars per million, the gigabytes of RAM, will be wrong within months. That is fine. The repository this essay lives in is organized around the belief that tools change every six months and the underlying questions do not. The numbers are here to date the argument, not to anchor it.

What I am claiming is structural, and I think it outlives the specifics. The work of an independent creator using these tools divides into a part that can be owned and a part that, for now, must be rented, and the line between them is drawn by throughput and coherence on long-horizon tasks, not by ideology. The honest practice owns the hands aggressively, rents the mind deliberately, routes by capability to control cost and by sensitivity to protect the people in the material, and refuses to pre-buy hardware against a future that gets cheaper while you wait.

The maximalist version of sovereignty, own everything, run everything locally, refuse every API, is a fantasy that costs money and, worse, tempts you to lie about what your setup actually does. The version I am defending is narrower, and I think it is true. Own what owning serves. Rent what renting serves. Never let the cost axis silence the sensitivity axis. The mind is rented today, and I say so plainly. The day it can be owned at the quality the work needs, on a machine bought for work it already does, it will be. Until then the seam stays visible, and naming it is the only version of sovereignty that is not a slogan.

---

#### Sources and figures

All figures are as of mid-2026 and should be re-verified before any decision. They date the argument; they do not anchor it.

- DeepSeek V3 (671B) at roughly 20 tokens per second in 4-bit on a 512 GB M3 Ultra Mac Studio, via Apple MLX. Reporting: VentureBeat, Slashdot, Hardware Corner, March 2025.
- DeepSeek V4 (April 2026): Pro and Flash tiers, large context, permissive license; the top tier reported within a fraction of a point of a Western frontier model on SWE-bench Verified at a small fraction of the output cost.
- MiniMax M2 line: open-weight, low-cost API in the same near-frontier band.
- Microsoft MAI-Thinking-1, announced at Build 2026 (June 2, 2026): a from-scratch, non-distilled mid-size (35B) reasoning model reported to match a leading Western model on coding benchmarks, launched in private preview on Foundry rather than as open weights. Reporting: Thurrott, GeekWire, TechCrunch.
- Apple MacBook Air M5 (March 2026) and the Thunderbolt tiering between the Air and the Pro, as published by Apple.
- Ink & Switch, *Local-First Software: You Own Your Data, in Spite of the Cloud* (2019).
- The prior essays in this series: [Sovereignty](souverainete.md) and [You Are Your Own Cloud](your-own-cloud.md).

---

[Back to Method](../README.md)
