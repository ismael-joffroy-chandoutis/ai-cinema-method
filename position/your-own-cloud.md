## You Are Your Own Cloud

Notes on building a sovereign post-production studio, after a meeting with Michael Cioni.

*Draft, May 2026. A working document. This will evolve as the practice does.*

---

I have spent eighteen months building a private cloud to compensate for a NAS that could not serve files remotely fast enough. I spent thousands of euros on egress, on bandwidth, on cron jobs that synced one entrepôt to another at three in the morning. I wrote shell scripts to mount SMB shares over Tailscale and watched the throughput drop to one and a quarter megabytes per second, and I called this *my infrastructure*.

It worked, in the way a bicycle works as a car. You can get somewhere on it. You will be tired when you arrive.

Then I had a one-hour video call with Michael Cioni in early May 2026, and one sentence rearranged the architecture I had been patching for a year and a half.

> The NAS is not the right place for playback.

The NAS has a weak processor. It was designed to store, not to serve. My Mac Mini M4 was sitting next to it on the same shelf, two orders of magnitude more powerful, doing nothing while my collaborators in Paris waited four minutes to scrub a ProRes clip living three meters away from it.

The problem was never my hardware. The problem was that I had inherited a mental model where the NAS was the *server* and the network was the *protocol*, and I had been trying to make a storage appliance behave like a media platform.

This essay is the working note from the shift. It is also a longer argument I have been trying to formulate for years, about what *sovereignty* means when you no longer accept the cloud as the default substrate of cinema.

---

### What I already knew, what I didn't apply

I had written about sovereignty already, in [the previous essay in this repo](souverainete.md), about running AI models on local hardware, owning the weights, refusing the censorship of commercial APIs. The argument was clear: for cinema that touches the body, war, the margins of human experience, the platforms will not engage with my subject matter. I needed the full expressive range, which meant local inference.

What I had not extended that argument to was the *rest of the production chain*. The rushes. The editing timelines. The collaborative access. The way a film travels between Paris and Los Angeles, between my collaborators at Films Grand Huit and me, between an SSD on a film set in Florida and a NAS in a residency in the Marais. For all of that, I had quietly accepted the cloud as the universal solvent. Cloudflare R2 for the rushes I needed to share. Tailscale for remote access. Frame.io for the producer reviews on past films. WeTransfer when nothing else worked.

The pattern I could not name was this: I had built sovereign AI inference inside a non-sovereign post-production environment. The center was free. The edges were rented.

The reason I had not seen it was that the alternative did not exist, or did not exist *as a coherent proposition*. Local AI inference had Pinokio, ComfyUI, llama.cpp, a whole ecosystem of tools that allowed me to stop paying for API calls. Local post-production had a NAS, which is to say it had storage without a protocol for remote access that worked at speeds compatible with editing.

That is what shifted in May 2026.

---

### The meeting

The meeting was not technical. Cioni did not sell me anything. He listened to my architecture, asked two or three questions, and then said the thing about the NAS not being for playback. He said it the way a doctor names a symptom that you have been describing for a year without realizing it was a single thing.

The Mac Mini is the server. The NAS is the disks. Strada is the protocol that makes the Mac Mini accessible from anywhere on the open internet, peer-to-peer, encrypted, without ever uploading a frame to a third-party data center.

What he said next was more radical, and I caught it only on re-reading the transcript:

> I don't use NAS because NAS is really expensive. Strata gives you remote access without NAS. So I just use OWC RAID. And RAID is like 40% cheaper. And it's the same speed, but it's cheaper. And I can invite as many people as I want to the RAID.

Cioni himself doesn't use a NAS. He uses an OWC ThunderBay RAID connected by Thunderbolt directly to his Mac Studio. The NAS operating system, which is the layer that justifies the price difference between a NAS and a RAID, is precisely the layer he bypasses. The RAID holds the bytes. The Mac Studio is the server. Strada is the protocol. Three components, no NAS OS in the middle.

I was sitting on the equivalent piece of hardware without knowing it. In 2020 I bought a QNAP TS-453BT3, a four-bay NAS with a modest Intel Celeron CPU and two Thunderbolt 3 ports, configured with 40 TB of RAID 5 storage. I had been using it as a 10 GbE NAS on my local network, and as a Thunderbolt-attached drive when I wanted speed, but I had been working around its slow *remote* access by routing everything through Cloudflare R2 anyway. The weakness was never the Thunderbolt link, which is fast. The weakness was that the QNAP's tiny Celeron CPU choked the moment a remote collaborator tried to scrub a clip across Tailscale. Strada removes that bottleneck entirely: the QNAP holds bytes, the Mac Mini does the serving work, the collaborator gets local-feeling playback.

The same hardware, used differently, becomes the architecture Cioni described.

This is what Strada calls *being your own cloud*. The phrase is Cioni's, from a public talk:

> You are your own cloud, because you have your own drives instead of renting Jeff Bezos' drives. You have your own computer instead of renting Jeff Bezos' computers. And you just plug that into your internet at home or the office, and that's always available.

When I heard it the first time, I dismissed it as marketing. After the meeting, I recognized it as architectural. The cloud, in this framing, is not a place. It is a *function*: data made accessible from anywhere. Functions can be provided locally or remotely. The question is who owns the substrate.

For cinema, the answer matters in ways it does not for a spreadsheet. A film is not a transient artifact. It is rushes, masters, archives, decisions, drafts, every state of an editing timeline preserved across two years of doubt. To make that travel through AWS, where the egress fees alone can exceed the cost of the local hardware, where the terms of service can change between projects, where the company can deprecate a service or raise prices or simply disappear, is to make the work hostage to a substrate I do not control.

It is also, simply, slow and expensive.

---

### A convergence I didn't see coming

What Cioni handed me was a vocabulary for an intuition I had been carrying since I started running Stable Diffusion locally in 2023. The intuition came from three sources I had been reading independently, and which only after the meeting I recognized as a single argument coming from three angles.

The first source was the Ink & Switch essay on *local-first software*, published years before I read it but which I encountered as scaffolding for a feeling I already had. The thesis was simple:

> Consider a significant personal creation, such as a PhD thesis or the raw footage of a film — for these you might be willing to take responsibility for storage and backups in order to be certain that your data is your data.

The argument was not against the cloud. It was against the *cloud as default*. Cloud apps offer collaboration, real-time sync, access from anywhere. The cost is that the data is hostage to APIs, terms of service, and the continued existence of the provider. For things that matter, you own the bytes. The bytes are on your machine. Everything else is a derivative.

The second source was Cioni himself, whose Strada pivot story I learned during the meeting and have confirmed in his public talks since. Strada started in 2023 as an AI tool: semantic search across rushes, automatic tagging, transcription, the things every editor wishes existed. It plateaued at one hundred signups per month. Cioni listened to the users. The problem was not finding the media. The problem was *accessing* the media without paying cloud rent.

They pivoted to a peer-to-peer architecture in spring 2025. One hundred signups per day by summer. Thirtyfold growth. The empirical validation of the local-first thesis, in the specific case of video post-production.

The third source was my own beliefs, written down in March 2026, before the Cioni meeting:

> Data sovereignty. Data stays local. Models run local when possible. No dependency on a cloud service that can shut off tomorrow.

Three angles. One thesis. Software, post-production, AI inference. *You own your data, in spite of the cloud.*

The reason I am writing this essay now is that the three angles have converged on a single architecture I can actually build, with hardware that exists in May 2026, at prices that fit a documentary budget.

---

### The collapse that makes it necessary

This shift would matter less if the institutional context were stable. It is not.

Ted Hope, one of the lucid witnesses of American independent cinema for the last thirty years, formulated the structural diagnosis in IndieWire earlier this year:

> As global streaming platforms replaced territorial licensing with worldwide rights acquisitions, regional distributors and broadcasters lost their role in the ecosystem. Without those buyers competing for rights territory by territory, one of the primary financing engines for independent cinema disappeared — international sales.

The middle layer that allowed films like the ones I want to make to *exist economically* has dissolved. Miramax, Fine Line, October Films, their equivalents in France and across Europe: gone, absorbed, or hollowed. The financing engine is broken. The distribution lanes are narrower. The number of buyers competing for the same film has dropped by an order of magnitude.

Cioni, in his HPA Tech Retreat keynote, brought the same diagnosis in numbers. Production down thirty-seven percent year on year. On-set hours at a thirty-year low. The stages of Los Angeles, empty. And the question he asks, which is the question I sit with:

> If storytelling isn't broken, why are so many people out of work?

The answer, in his framing, is that the industry built its competitive moat around friction — specialized talent, bespoke technology, sophisticated distribution, standardized practices, professional organizations — and that the same friction is now the reason the industry is uncompetitive against creators who have access to enough of the tools to make work without the moat.

This is not an argument for becoming a YouTuber. The films I make do not fit YouTube. The point is structural: the conditions that made *being a working independent filmmaker* a sustainable middle-class position have eroded. The infrastructure that used to be paid for by the system now has to be paid for by the filmmaker, or it does not exist.

The cloud was sold to us as the new substrate. It is not. It is a new tax. Frame.io for review. AWS for transcoding. LucidLink for collaboration. Each of them affordable in isolation, lethal in aggregation. The numbers Cioni cites for cloud storage in M&E workflows are forty to eighty dollars per terabyte per month, plus egress, plus per-seat fees, plus the cost of moving in and the cost of moving out. For a 4K documentary at one hundred terabytes of rushes, you cross the cost of a Mac Studio within a year.

The local substrate, by contrast, has collapsed in price. Hard drive storage is now at roughly eleven dollars per terabyte. A Thunderbolt 5 NVMe enclosure with four slots costs under six hundred euros. A ten-gigabit Ethernet switch costs two hundred. The cost curves of storage and the cost curves of cloud crossed somewhere around 2023, and the crossing went unnoticed because cloud was already mentally installed as the default.

This is the material condition for the shift Cioni's tool makes available. The hardware exists. The pricing makes sense. The only thing missing was the protocol that let local hardware be remotely accessible without uploading to a third party. That is what Strada Connect provides, with patented *ephemeral files* generated on the fly from the source media, streamed peer-to-peer at the latency of two hundred milliseconds, debayering BRAW in the browser on a show-floor connection of half a megabit per second.

The whole point of the demo at NAB 2026 was to prove that the cloud is no longer necessary for what we used to need the cloud for.

---

### The Affleck paradox

I want to write down a question that haunts the entire argument, because it is the question I cannot evade.

In March 2026, Netflix acquired *InterPositive*, a startup founded by Ben Affleck and a team of sixteen people, for a sum reported up to six hundred million dollars. InterPositive built an AI system that lets filmmakers train a model on the dailies of their own production and then use that model in post: to remove wires, reframe shots, enhance backgrounds, shape lighting. Affleck described it in interviews as a tool for *filmmaker autonomy*. The pitch was the same pitch I am making in this essay: take back control of post-production, do not depend on external VFX vendors, build infrastructure that serves the work.

Netflix bought the company. The tool became proprietary to Netflix. It is not available commercially. Independent filmmakers who do not work with Netflix will never have access to it. Elizabeth Stone, Netflix's CPTO, framed the acquisition in the public statement:

> Our approach to AI has always been focused on meaningfully serving the needs of the creative community and our members. Innovation should empower storytellers, not replace them.

I do not doubt the sincerity of the statement. I doubt the structure it inhabits.

The tool of autonomy was bought by the platform against which autonomy was the answer. The independent filmmaker who built the tool became a senior advisor at Netflix. The sixteen engineers became Netflix employees. The codebase that promised to free filmmakers from VFX vendors became the property of the largest streaming platform in the world.

This is the paradox of the autonomous studio: the tools that promise independence are, by the logic of venture capital, also the tools that have an exit. An exit means an acquirer. The acquirer is rarely a federation of independent filmmakers. The acquirer is a platform.

The question this poses for me is precise: *how do I build a sovereign studio without being bought?*

I do not think the answer is to refuse all tools that have venture capital behind them. Strada has venture capital behind it. So does ComfyUI, which raised thirty million dollars in 2025 to keep its open source pipeline going. So does most of the local AI ecosystem I depend on. To refuse them on principle is to refuse the infrastructure that makes the work possible.

The answer, I think, is structural. It has several parts, and I am still working them out:

I do not build my practice as a startup. There is no equity to dilute, no investors who need an exit, no valuation that ties my work to a future sale. My studio is *a practice*, not a company. The asset is the films, and the films are mine.

I keep the infrastructure composable from open and replaceable parts. If Strada disappears in three years, the rushes are still on my NAS, the masters are still on my LTO tapes, the editing timelines are still readable in Final Cut Pro. The protocol that connects them is fungible. Strada is the best protocol available today. If it gets bought by Adobe tomorrow, I switch to whatever has replaced it. The data does not move.

I license, I do not sell. The IP of the films stays with me, or with structures I co-own with collaborators (the producer Films Grand Huit, in the case of *The Goldberg Variations*). Distribution is a license, not a transfer. The work circulates. The ownership stays.

I treat the institutional ecosystem (CNC, ARTE, festivals, museums) as *windows*, not *gates*. They are channels through which the work reaches an audience. They are not the entity that decides whether the work exists. The autonomy is not autonomy from them. It is autonomy from depending on any single one.

This is the part I have not seen articulated cleanly anywhere else, and it is the part I am most interested in. The independent filmmaker has been told for thirty years that they must choose between the studio system and *true* independence, and that true independence means refusing institutions. This is a false binary. The institutions exist. They pay. CNC has financed the films I am proud of. The choice is not to refuse the institutions but to refuse to let the institutions define the terms of the practice.

The sovereign studio takes CNC money. It also keeps the rushes on its own NAS. It uses ARTE coproduction. It also runs its own ComfyUI on its own RTX 5090. It accepts the residency at Villa Albertine. It also publishes its working method on GitHub under an open license so that other filmmakers can learn from it without paying anyone.

The horizontality Cioni names as autonomy is not horizontality *against* institutions. It is horizontality between filmmakers. Each filmmaker as their own studio, collaborating across studios on specific projects, retaining ownership of their own infrastructure and their own IP. The film *The Goldberg Variations* is a joint project of my studio and Films Grand Huit. It is not a property of either. It is a contract between sovereign practices.

---

### What the architecture looks like

The technical description of the architecture lives in a [separate document in this repo](../research/methods/local-first-studio-workflow.md), because the cinematic argument and the engineering argument are different texts. Here is the principle.

Storage is dormant by default. Two tiers: a *hot tier* (the QNAP TS-453BT3, attached by Thunderbolt 3 to the Mac Mini, holding the current working rushes) and a *cold tier* (the Ugreen DXP8800 Pro on the 10 GbE network, holding archives and delivered masters). Most of the time both are doing nothing, drawing minimal power, waiting. The active compute is on the Mac Mini M4 that sits next to them. The Mini is the *server*: it runs Strada Agent, which exposes both volumes as virtual drives accessible from anywhere on the open internet, peer-to-peer, encrypted, generating ephemeral files on the fly so that a collaborator in Los Angeles can open a BRAW clip in DaVinci Resolve as if it were a file on their own SSD.

Local editing happens on Thunderbolt 5 direct-attached NVMe. The MacBook Pro M5 Max (shipped by Apple in March 2026) gives me three Thunderbolt 5 ports per machine at 120 Gbps each. When the editing session begins, the active timeline media is on a 6,600 megabyte per second NVMe enclosure plugged into one of those TB5 ports. The NAS is for the rest. The scratch is for the now.

The studio network is ten-gigabit. A two-hundred-euro MikroTik switch connects the Mac Mini, the two MacBook Pros, the NAS, and the GPU PC into a fabric where everything moves at the speed of local memory. This is the single most important upgrade. Without it, every other piece of hardware in the studio is running at one tenth of its capacity.

Remote collaboration happens through Strada Connect. my collaborators in Paris, me in Florida, all of us accessing the same NAS in Paris through the Mac Mini's CPU, as if the files were on our own machines. No upload to AWS. No download from R2. No waiting for a Cloudflare bucket to sync at three in the morning. The Mini is awake. The NAS wakes when asked. The collaborators see the files as if they were local.

Local AI inference happens on the RTX 5090 in Jacksonville and the RTX 4090 in Paris. ComfyUI, Pinokio, Wan AI, the whole open source stack that I described in the [sovereignty essay](souverainete.md). The output of these models is data, like any other data. It lives on the NAS. It travels through Strada like anything else.

Orchestration happens through Claude Code. This is the part of the stack that is *not* local, and I want to name that honestly. The reasoning layer, the conversational model that helps me hold the architecture in my head, runs on Anthropic's servers. It is the one piece of the chain that remains a cloud dependency. I accept it for now because the value is high and the alternative (running a 400-billion-parameter model locally) is not yet practical at acceptable quality for the kind of orchestration I do. If a local equivalent reaches comparable quality in the next eighteen months, I will switch. If not, this remains the single seam in the sovereignty argument, and I refuse to pretend it isn't there.

Backup follows a 3-2-1 logic that I have iterated on for two years. A local copy on a four-terabyte external drive that travels with me. A second copy on the NAS in Paris. A third on Cloudflare R2 as cold storage. Adding an LTO-8 tape archive when the budget allows, which is the only thirty-year medium I trust for masters. Strada does not replace backup. Strada is for *access*. Backup is for *survival*. The two are separate architectures.

---

### On-set, in the field

Half of my work happens away from the studio. Florida residences, Villa Albertine in Los Angeles, festivals across Europe. The architecture has to work from a film set with no internet, or with intermittent internet, or with internet good enough for messages but not for terabytes.

The pattern is:

The camera writes to a card. The card offloads to a Thunderbolt SSD on the laptop, with a checksum (xxHash64, fast, non-cryptographic, the industry standard) and a Media Hash List that documents the chain of custody. The same content is mirrored to a second SSD in parallel. This is on-set practice that has not changed in ten years and does not need to change.

If the connection is good, the laptop runs a Strada Agent. The Paris Mac Mini can already serve those files to anyone. The Paris-based editors can start logging rushes the same day. No upload. No download. The files are on the SSD next to me, and they are accessible from Paris.

If the connection is bad, nothing forces an upload. The files stay local until I can sync them. The Strada model degrades gracefully: if the source is offline, the file is unavailable, the collaborator waits or works on something else. No data is held hostage in a cloud that I cannot reach without bandwidth I do not have.

This matters for the kind of films I make. *Vues Lumière* in residency at Stéréolux Nantes, *The Goldberg Variations* with archives requiring careful chain-of-custody handling, *Virus* with a Romanian protagonist who lives in places with unreliable connection. The infrastructure has to work in conditions where the assumption of permanent broadband is wrong.

---

### What changes Monday morning

This essay is a working note, not a manifesto. I am writing it because publishing the architecture in public makes it harder for me to drift back to the patchwork. Here is what I am doing this month, in concrete terms:

I am buying a MikroTik CRS304-4XG-IN ten-gigabit switch. Two hundred euros. The single highest-leverage upgrade in the entire stack.

I am connecting the QNAP TS-453BT3 to the Mac Mini via Thunderbolt 3 cable for the first time in five years, and treating it as the hot tier. The QNAP's 10 GbE port stays as a secondary path; the primary path is the direct Thunderbolt link, which is faster than any NAS-over-network configuration.

I am installing two NVMe drives as cache in the Ugreen DXP8800 Pro to optimize the cold tier. Two hundred to three hundred euros. The NAS already supports it.

I am installing Strada Agent on the Mac Mini and exposing both volumes through it. I am testing it with my collaborators at Films Grand Huit on the Goldberg rushes, before relying on it for production. I am configuring a local Strada cache on each collaborator's machine — 500 GB on an external drive — because the Cioni demo showed that this cache is what turns the remote editing experience from *acceptable* into *transparent* on a constrained internet connection.

I am keeping Cloudflare R2 as backup. I am not deleting the existing infrastructure. The 3-2-1 backup is non-negotiable, and R2 plays the *survival* role, not the *access* role. The architecture is additive, not subtractive.

I am ordering the OWC Express 4M2 Ultra Thunderbolt 5 enclosure now that the MacBook Pro M5 Max is in hand, with the understanding that this is the scratch disk for active editing, not a replacement for the NAS. The next decision point is the Mac Studio M5 Ultra, expected in October 2026: if it makes architectural sense to upgrade the Mac Mini server role to a Mac Studio M5 Ultra by then, I will, but only if the throughput gain on Strada serving justifies the cost.

I am budgeting an OWC Mercury Pro LTO tape system for the next CNC funded film. Sixty euros per twelve-terabyte tape, thirty-year shelf life, claimable as production cost. The only archive medium I trust.

I am not changing anything about the local AI inference stack. ComfyUI, Pinokio, Wan AI, the RTX 5090, the RTX 4090. That part of the architecture already works. The Cioni meeting did not invalidate it. It extended it.

---

### What this is, and what it is not

This is not a recommendation. I am writing about my own practice, in my own conditions, with my own constraints. I have a Mac Mini M4 because I live half in Paris and half in Florida and I need a server that runs in Paris while I am away. I have a NAS because I shoot in ProRes and the files are big. I have two MacBook Pro M5 Max (released by Apple in March 2026) because the Villa Albertine residency at the CENTQUATRE in Paris, starting September 2026, will be co-led with three other artists who need access to compute. I have an RTX 5090 because the RTX 4090 was not enough for the kinds of generative pipelines I run. The Mac Studio M5 Ultra, when it ships in October 2026, is a candidate for upgrading the server role at the Paris hub.

The architecture is sized to the practice. The principles travel, the hardware doesn't.

What I am claiming is that the *principles* are now coherent in a way they were not, eighteen months ago. Local-first, peer-to-peer, sovereign data, horizontal collaboration. These were four separate ideas. They are becoming a single architecture, and the architecture is buildable today by a working filmmaker with a documentary budget.

I am also claiming that this matters beyond the question of cost or efficiency. Sovereignty is not an optimization. It is a *position* on what it means to make films in 2026, in a context where the institutions that supported independent cinema have hollowed, where the platforms that replaced them extract more than they enable, and where the tools of autonomy are being acquired by the entities autonomy was supposed to free us from.

The sovereign studio is not a refusal of the world. It is a *negotiation with the world from a position that is not begging*. The CNC files I write for Goldberg do not change because I run my own infrastructure. They get written. They get submitted. They get funded or not funded. What changes is that I do not need them to be funded for the work to continue. The work continues on the substrate I own.

This is the only definition of artistic freedom I have found that is not a slogan.

---

### Next entries

This document is the first entry in what I expect to be an ongoing journal. The architecture will change. The Strada test with my collaborators at Films Grand Huit will succeed or fail in ways I cannot predict. The Mac Studio M5 Ultra may ship in October 2026 as expected, or it may slip again due to ongoing RAM shortages. The CNC may fund the next phase of *The Goldberg Variations* in June 2026, or it may not. The residency at CENTQUATRE in September 2026 will pressure-test the multi-user architecture in ways that no amount of solo design can anticipate.

I will write the entries as the practice teaches me what was wrong about this document and what was right.

The Cioni meeting was a single point of inflection in a longer arc. The arc started with the first time I ran Stable Diffusion locally in 2023 instead of paying for an API call. It will continue past the next meeting, the next residency, the next film.

The thread is the same: every layer of the production chain that can be sovereign should be sovereign. The cloud is a service, not a substrate. *You are your own cloud.*

---

#### Sources and conversations

- Michael Cioni, public talks at HPA Tech Retreat 2025, IBC 2025, NAB 2026, and the personal meeting in May 2026.
- Ink & Switch, *Local-First Software: You Own Your Data, in Spite of the Cloud* (2019), and subsequent writing at openlocalfirst.org.
- Ted Hope on the collapse of independent film infrastructure, IndieWire 2026.
- The Strada Agents and Strada Connect product documentation, May 2026.
- Variety, NPR, Deadline coverage of the Netflix acquisition of InterPositive, March 2026.
- The companion technical document in this repository: [local-first-studio-workflow.md](../research/methods/local-first-studio-workflow.md).
- The previous essay in this series: [Sovereignty](souverainete.md).

---

[Back to Method](../README.md)
