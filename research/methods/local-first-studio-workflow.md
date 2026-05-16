## Local-First Studio Workflow

Technical companion to [*You Are Your Own Cloud*](../../position/your-own-cloud.md). This document describes the architecture, the hardware choices, the codec decisions, and the protocols I use to operate a sovereign post-production studio across Paris, Florida, and Los Angeles, with horizontal collaborators.

*Working document, May 2026. Updated as the architecture evolves.*

---

### The principle

A studio has four layers that need to be separated cleanly:

1. **Storage** — where bytes live. Optimized for capacity and resilience.
2. **Compute** — where bytes are processed. Optimized for speed and concurrency.
3. **Distribution** — how bytes travel between people. Optimized for latency and access control.
4. **Backup** — how bytes survive. Optimized for longevity, not speed.

Most NAS-based workflows collapse storage and distribution into the same appliance, then use a slow protocol (SMB over the open internet, often tunneled through a VPN that adds another layer of latency) to expose the NAS to remote collaborators. This is the architecture I had until May 2026. It does not work.

The architecture I am moving toward separates the four layers:

- **Storage** is the NAS, which holds rushes, masters, archives. The NAS sleeps when not in use.
- **Compute** is a Mac Mini M4 sitting on the same local network as the NAS, always on, with a fast CPU and unified memory.
- **Distribution** is Strada Connect, which runs on the Mac Mini and exposes the NAS as a peer-to-peer accessible volume, generating ephemeral files on demand.
- **Backup** is a 3-2-1 stack: local 4TB external, NAS, Cloudflare R2, with LTO-8 tape archive added when budget allows.

The result is that the NAS no longer has to be a server. The Mac Mini, which is two orders of magnitude more powerful than the NAS processor, does the serving.

---

### Hardware inventory and roles

| Machine | Location | Role | Specs |
|---|---|---|---|
| **Mac Mini M4** | Paris | Server hub, always on, Strada Agent, backup orchestration, transcoding | M4, 24 GB RAM, 1 TB internal |
| **MacBook Pro M5 Max #1** | With Ismaël (Paris/LA) | Primary editing FCP, color DaVinci | M5 Max, 128 GB RAM, 2 TB internal, TB5 |
| **MacBook Pro M5 Max #2** | Paris (CENTQUATRE residency from Sept 2026) | Secondary editing, color, Premiere Pro for partners | M5 Max, 128 GB RAM, 2 TB internal, TB5 |
| **MacBook Air M3** | Travel | Light editing, daily Claude Code work, offload from set | M3, 16 GB RAM, 1 TB |
| **PC Windows NOMAD** | Jacksonville, FL | Heavy AI inference, Pinokio, ComfyUI, Wan AI, encoding | Ryzen 9 9900X, RTX 5090 32 GB VRAM, 46 GB RAM |
| **PC GPU Paris** | Paris | Secondary AI inference, RTX 4090 | RTX 4090 24 GB VRAM |
| **QNAP TS-453BT3** (Thunderbolt 3) | Paris | HOT TIER, 4 bays, 40 TB RAID 5, 2× TB3, 2× 10 GbE | Intel Celeron J3455, 8 GB RAM, QTS OS, used both as DAS (Thunderbolt 3 direct-attach) and as NAS (10 GbE on the LAN) |
| **NAS Ugreen DXP8800 Pro** | Paris | COLD TIER, 8 bays, 10 GbE, NVMe cache slots, archives and masters | UGOS Pro, supports SSD cache |
| **4T_IJC_th4** | Travels with MacBook | Local working copy, Strada cache target, sessions, drafts | 4 TB external Acasis, Thunderbolt 3 |

The four-layer architecture maps onto this inventory as:

- **Storage layer (HOT)**: QNAP TS-453BT3 in Thunderbolt 3 direct-attach to the Mac Mini = working storage for active rushes and current projects.
- **Storage layer (COLD)**: NAS Ugreen DXP8800 Pro on the 10 GbE LAN = archives, delivered masters, secondary backup.
- **Compute layer**: Mac Mini M4 (server hub, runs Strada Agent against both storage tiers), MBP M5 Max (editing), PC GPUs (AI inference).
- **Distribution layer**: Strada Agent on Mac Mini exposes both tiers as peer-to-peer accessible volumes. 10 GbE local switch for in-studio clients.
- **Backup layer**: 4T external (travel copy + Strada cache), Ugreen NAS (cold), Cloudflare R2 (offsite cold), LTO-8 future (archive masters).

The architectural insight that changes everything: **the QNAP TS-453BT3, connected by Thunderbolt 3 to the Mac Mini, is functionally equivalent in its DAS mode to the OWC ThunderBay 4 that Michael Cioni uses in his own studio.** Cioni explicitly told me he does not use a NAS at all — he runs his Mac Studio against direct-attached OWC Thunderbolt RAIDs because the NAS operating system "adds a layer" that costs 40% more for the same throughput. The QNAP TS-453BT3 can operate in DAS mode over Thunderbolt 3, bypassing its own QTS layer when mounted directly to the Mac Mini. It can also operate in parallel as a regular NAS over 10 GbE for in-studio clients that need direct LAN access. I have used it as a NAS for five years. The Thunderbolt 3 link as the primary path for remote access via Strada is the new piece.

---

### The wiring diagram

```
INTERNET
   │
[FreeBox / router]                    ← currently 2.5 GbE WAN, planned upgrade to 10 GbE
   │ 2.5 GbE today (10 GbE later)
   ▼
[MikroTik CRS304-4XG-IN 10 GbE switch]
   │
   ├─── 10 GbE ──► [Mac Mini M4]
   │                    │
   │                    ├── Thunderbolt 3 DIRECT ──► [QNAP TS-453BT3]
   │                    │   (~700-1000 MB/s real on RAID 5 HDD,    HOT TIER 40 TB RAID 5
   │                    │    up to ~1500 MB/s if SSDs in bays)     also accessible on LAN via 10 GbE
   │                    │
   │                    └── (Strada Agent exposes both volumes via P2P to remote collaborators)
   │
   ├─── 10 GbE ──► [Ugreen DXP8800 Pro] (COLD TIER, archives, masters)
   │
   ├─── 10 GbE ──► [MBP M5 Max #1] + OWC Express 4M2 Ultra TB5 (scratch direct, 6,600 MB/s)
   ├─── 10 GbE ──► [MBP M5 Max #2]
   └─── 1 GbE  ──► [PC Windows GPU Paris]

Strada Connect → routes through router to peer machines worldwide (Paris collaborators, remote partners).
NEVER expose the QNAP or Ugreen directly to the router. All external access goes through the Mac Mini's Strada Agent.
```

### The non-negotiable upgrade: 10-gigabit local network

The single highest-leverage upgrade is the local network. The Mac Mini, the QNAP, and the Ugreen all have 10 GbE ports. The default Gigabit Ethernet switch most people buy throttles them to one tenth of their capacity. A documented benchmark on my old setup measured 1.26 MiB/s over SMB through Tailscale, against a theoretical NAS throughput on 10 GbE of approximately 800-1000 MB/s in real-world SMB. The bottleneck was the switch and the tunneling protocol, not the storage hardware.

Recommendation: **MikroTik CRS304-4XG-IN**, four 10GBase-T ports plus one gigabit management port, two hundred euros. Desktop form factor, no rack required, no fans. Connects: Mac Mini, two MacBook Pros (via USB-C to 10 GbE adapter, OWC or Aquantia, hundred euros each), the Ugreen NAS, and the studio GPU PC if needed. Five to six devices, one switch, the studio fabric runs at the speed of local memory.

The QNAP connects to the Mac Mini directly via Thunderbolt 3 cable. It does *not* need to be on the 10 GbE switch in the primary path, because the Thunderbolt 3 direct-attach link is faster than any 10 GbE network. The QNAP's 10 GbE port can be used as a secondary backup path if redundancy is desired.

Without the 10 GbE switch, every other piece of hardware is operating below capacity.

---

### Two-tier storage configuration

#### HOT TIER — QNAP TS-453BT3 over Thunderbolt 3

The QNAP TS-453BT3 was bought in 2020 and configured with 40 TB usable in RAID 5 (four bays). It has two Thunderbolt 3 ports and two 10 GbE ports, with an Intel Celeron J3455 CPU and 8 GB of RAM. The Celeron is the weak point: in pure-NAS mode over 10 GbE, this CPU caps real-world throughput well below the network ceiling. In Thunderbolt 3 DAS mode, the CPU is bypassed entirely — the Mac Mini reads the volume directly through the SATA controller, so the storage performance is what the disks can do, not what the QNAP OS can serve.

Operating modes used in parallel:

- **Primary path: Thunderbolt 3 direct-attach to the Mac Mini.** The QNAP appears in the Mac Mini's Finder as a fast external volume. Real-world throughput in RAID 5 on HDD: ~700-1,000 MB/s, equivalent to 5-7 simultaneous streams of ProRes 422 HQ at 4K. If two bays are eventually populated with SSDs, the throughput would rise to ~1,500 MB/s.
- **Secondary path: 10 GbE NAS for in-studio clients.** The QNAP simultaneously exposes the same volume over SMB on the local network for any machine that needs to mount it directly (the MacBook Pros at the studio, the GPU PC). This is the legacy mode I have used for five years.

This is the working storage for active rushes, current editing projects, and anything that needs to be accessible through Strada to remote collaborators. It stays awake during working hours and sleeps overnight.

#### COLD TIER — Ugreen DXP8800 Pro over 10 GbE

The Ugreen DXP8800 Pro has eight HDD bays and two NVMe slots for read/write cache. It is the secondary storage layer, optimized for capacity over speed.

Recommended configuration:
- **Bays 1-6**: 6× WD Red Plus 12 TB or Seagate IronWolf Pro 16 TB in RAID 6 (~60-80 TB usable, archives and delivered masters)
- **Bays 7-8**: reserved for future expansion, hot spare, or backup pool
- **NVMe slots**: 2× Samsung 980 Pro 2 TB or WD Black SN850X 2 TB, read/write cache

The Ugreen connects to the 10 GbE switch. The Mac Mini sees it as a SMB mount. Strada Agent exposes its contents alongside the QNAP volume — the collaborator browsing through Strada sees both as folders without knowing which tier they live on.

The Ugreen runs in low-power mode most of the time. It wakes when a file is accessed. Most of the time only the hot tier is active.

#### Why two tiers and not one

Could I put everything on the QNAP and forget the Ugreen? Yes, if 40 TB is enough. It is not. *The Goldberg Variations* alone occupies more than 13 TB of rushes from the August 2024 shoot, plus 2-3 TB of processed proxies, plus several terabytes of institutional archives and reference material, plus production documents. Past films (*Madotsuki*, *Vues Lumière*, *Maalbeek*, *Swatted*, *Ondes Noires*, *Virtual Kintsugi*) occupy another 30+ TB in finished masters that should not be deleted.

Two tiers separates the *working set* (what I actively edit, 5-15 TB typically) from the *long memory* (what I keep but rarely touch, 40+ TB and growing). The working set goes on the fast Thunderbolt tier. The long memory goes on the capacity tier.

This is the same architecture every professional facility uses, scaled to a one-person studio.

---

### The Strada layer

Strada Agent runs on the Mac Mini M4. It mounts both the QNAP (Thunderbolt 3 direct-attach) and the Ugreen NAS (SMB over 10 GbE) and exposes their contents to authorized users as peer-to-peer accessible volumes.

The architectural insight that the Cioni meeting gave me, and which the [companion essay](../../position/your-own-cloud.md) develops, is that the Mac Mini does the *serving work*. The storage holds bytes. The Mac Mini decodes them, transcodes them on the fly if needed, and ships them peer-to-peer to whoever is reading. The CPU of any NAS (including the QNAP and the Ugreen), which is weak, is bypassed entirely. The CPU of the Mac Mini, which is fast, is used.

Cioni himself, in our conversation, was explicit on this point: *"I don't use NAS because NAS is really expensive. Strada gives you remote access without NAS. So I just use OWC RAID. And RAID is like 40% cheaper. And it's the same speed, but it's cheaper. And I can invite as many people as I want to the RAID."* His own setup is a Mac Studio M4 Max with an OWC ThunderBay direct-attached over Thunderbolt. My QNAP TS-453BT3 in DAS mode is functionally equivalent to his setup, with one advantage: it can also operate as a regular NAS over 10 GbE if I ever need that fallback.

#### Practical setup

1. Install Strada Agent on the Mac Mini.
2. Authenticate the agent with my Strada account (Gmail login is enough).
3. Add both storage paths to the agent's exposed paths:
   - `/Volumes/QNAP-TBT/` (the QNAP mounted via Thunderbolt 3)
   - `/Volumes/Ugreen-NAS/` (the Ugreen mounted via SMB over 10 GbE)
4. Define collaborator accounts (the Paris editing team, the producer, the remote partners) with folder-level access control and per-folder permissions (editor, view-only, download).
5. Each collaborator installs Strada Connect locally. The exposed folders appear in their Finder as virtual drives under `Locations → Strada`.
6. Files behave like local files to FCP, DaVinci Resolve, Premiere Pro.

#### Local cache on collaborator machines

A detail Cioni emphasized in the demo, and that I missed initially: each collaborator should configure a local Strada cache on an external drive.

In the Strada Agent settings → Manage Cache:
- Cache location: external Thunderbolt or USB drive (the 4T_IJC_th4 for me)
- Cache size: 50 GB minimum, 500 GB recommended

What this does: when a remote user opens a clip from my QNAP via Strada, the file is streamed peer-to-peer, but the bytes that arrive are also written to the local cache. The second time the file is accessed, it plays back from the local cache instead of the remote source. Background caching also runs predictively when files are loaded into a Final Cut Pro timeline.

This is the difference between *acceptable* remote editing and *transparent* remote editing on a constrained internet connection. Without the cache, every scrub re-streams. With the cache, the second pass through a clip is local-speed.

#### Receipts and chain of custody

Strada generates an xxHash checksum receipt for every transfer, automatically. In the jobs panel, every transfer shows:
- Source path (which machine, which drive, which folder)
- Destination path
- Hash (xxHash, fast non-cryptographic)
- Timestamp, file count, total size
- Who initiated the transfer

This receipt log is persistent in the Strada account. It is the Netflix-compliant chain of custody that I would otherwise have to maintain by hand with `xxhsum` and MHL files. The MHL format remains the industry standard for on-set offload, but for in-studio transfers between machines through Strada, the xxHash receipts replace the need to generate MHL manually.

#### Strict P2P, no upload

The Strada architecture means no upload to a third-party data center. The Mac Mini handles the streaming directly to the collaborator's machine. Encryption is end-to-end. The Strada signaling server is used only to establish the connection; the data itself flows peer-to-peer between the Mac Mini and the collaborator.

#### Pricing

As of May 2026: Strada Connect basic functionality is free. Paid tier is eight dollars per month with no per-user fees and no storage caps. For a studio with three to four collaborators and one hundred terabytes of rushes, this is approximately one hundred to one hundred fifty dollars per year, versus four to eight thousand dollars per year if the same workflow ran on AWS S3 + LucidLink.

---

### Direct-attached scratch for active editing

When the MacBook Pro M5 Max sits down for a session of color grading or fine editing, the timeline media should be on a direct-attached Thunderbolt 5 NVMe enclosure, not on either storage tier. The latency and throughput difference matters when scrubbing 4K ProRes 422 HQ or BRAW 6K.

Target hardware: **OWC Express 4M2 Ultra**, four-slot NVMe M.2 2280/2242 enclosure, Thunderbolt 5, sustained 6,622 MB/s in RAID 0 with PCIe 4.0/5.0 drives. Up to 32 TB capacity per unit (4× 8 TB max per slot). Daisy-chain expandable to 128 TB. Two Thunderbolt 5 ports. Compatible with TB4/USB4 (3,200 MB/s) and TB3 (2,800 MB/s) at reduced speeds. Comes with SoftRAID Premium (3 years) for RAID 0/1/4/5/10/JBOD configuration.

Pricing: enclosure around 549 €. Add 4× 2 TB NVMe (Samsung 990 Pro or WD Black SN850X) at approximately 200 € each. Total around 1,400 € for 8 TB of scratch space at 6,600 MB/s. Can scale to 32 TB by populating with 8 TB NVMe drives (around 800 € each at current prices).

This is enough for:

- Active 4K ProRes 422 HQ timeline (220 MB/s per stream × ~30 simultaneous streams possible)
- BRAW 6K 5:1 scrubbing (194 MB/s per stream)
- ProRes 4444 master export (330 MB/s)
- ComfyUI temporary outputs while iterating on a pipeline

Workflow: sync the active project folder from the QNAP (Thunderbolt 3 → Thunderbolt 5 via the Mac Mini relay, or simply copy via the 10 GbE network) to the scratch at the start of a session. Sync back at the end. The scratch is ephemeral. The QNAP is the working source of truth. The Ugreen is the archive.

---

### Codec strategy by stage

The codec choices follow the principle of *do not transcode unless transcoding adds value*. Each stage of the production has a codec that fits its constraints.

| Stage | Codec | Bitrate | Why |
|---|---|---|---|
| Camera capture (iPhone 15 Pro Max, ProRes mode) | ProRes 422 HQ | ~220 MB/s | Native iPhone Pro capture, no transcoding loss |
| Camera capture (Blackmagic, when used) | BRAW 5:1 or 8:1 | 50-194 MB/s | Native Blackmagic |
| On-set offload | Mirror copy with xxHash64 checksum | n/a | Integrity, not compression |
| Proxy generation (Mac Mini, automated) | HEVC 10-bit 4K | 25-35 Mbps | Scrub-friendly on FCP and DaVinci, Apple Media Engine decodes natively |
| FCP timeline editing | ProRes Proxy or HEVC 10-bit | 5-12 MB/s | Smooth playback even on travel hardware |
| Color grading in DaVinci | ProRes 422 HQ original or BRAW | 220 MB/s + | Full quality for grading |
| VFX / compositing | ProRes 4444 or DPX | 330 MB/s + | Alpha channel, color depth |
| Master delivery | ProRes 4444 or ProRes 422 HQ | per spec | Festival, broadcaster spec |
| Streaming review (via Strada) | Strada ephemeral files, generated on the fly from source | n/a | Strada handles the transcoding for review playback |
| Archive | Original ProRes/BRAW + LTO-8 tape | n/a | Thirty-year shelf life, claimable as production cost |

The principle is that ProRes is the working format end to end, with HEVC proxies generated automatically on the Mac Mini for fast access during editing, and Strada handles the on-the-fly transcoding for remote review without requiring a separate proxy pipeline.

---

### Streams per machine, real numbers

A studio's capacity is measured in concurrent streams of a given codec. The numbers that matter for a multi-user editing setup:

| Codec | Per-stream MB/s | Streams over 1 GbE | Streams over 10 GbE | Streams from TB5 NVMe scratch |
|---|---|---|---|---|
| ProRes Proxy | 5.6 | 22 | 178 | 1000+ |
| ProRes LT | 12.7 | 9 | 78 | 500+ |
| ProRes 422 | 62.5 | 1-2 | 16 | 100+ |
| ProRes 422 HQ | 220 | <1 | 4-5 | 30 |
| ProRes 4444 | 330 | <1 | 3 | 20 |
| BRAW 6K 5:1 | 194 | <1 | 5 | 34 |
| HEVC 4K 10-bit proxy | 5-8 | 22+ | 178+ | 1000+ |

For my actual workflow (one to two simultaneous editors, occasional review by remote producers), 10 GbE is sufficient. The TB5 NVMe scratch handles the heavy editing locally. The NAS over 10 GbE handles the rushes and intermediate files. Strada handles the remote access without requiring full-quality streaming because the Mac Mini transcodes on the fly to fit the available bandwidth.

---

### On-set tournage workflow

Half of the work happens away from the studio. The workflow has to function with intermittent or no internet.

**Step 1, offload:** the camera card writes to a Thunderbolt SSD on the MacBook Pro (or MacBook Air for travel-light). I use a SanDisk Extreme Pro SSD as the working drive, with a parallel mirror to the 4T external Acasis drive. Both copies are made before the card is wiped.

**Step 2, integrity check:** xxHash64 checksum on every file. Generate a Media Hash List (MHL) file documenting the chain of custody. This is industry standard practice and the format CNC and broadcasters expect for delivery.

**Step 3, conditional sync:**

- If the connection is good (Strada works on 0.5 Mbps but real productivity needs more): start the Strada Agent on the laptop. The Mac Mini in Paris can immediately begin serving the files to the Paris editing team within minutes of the card being offloaded — no upload to any cloud bucket, just peer-to-peer access to the SSD attached to my MacBook.
- If the connection is bad: the files stay local. Sync happens later, when bandwidth permits. No data is held hostage in a cloud that requires upload to be useful.

**Step 4, backup:**

- The 4T external goes into a separate bag from the MacBook. Geographic separation.
- If a hotel has decent wifi, an rclone job pushes the offload to Cloudflare R2 as the third copy. This is the slow background backup, not the working access path.

**Step 5, ingest at base:**

- When I return to Paris (or to a location with the Mac Mini accessible), the SSD is connected to the Mini directly. Files are moved (not copied) from the SSD to the QNAP hot tier, with xxHash verification (now generated automatically by Strada's transfer receipts).
- The MHL travels with the files.
- The proxy generation pipeline runs automatically on the Mini (HEVC 10-bit 4K at 30 Mbps).
- After delivery, files migrate from the QNAP hot tier to the Ugreen cold tier following a documented retention policy (typically: hot tier holds the current project + the 6 most recent, cold tier holds everything else).

This pattern has not changed substantively in five years. What changes with Strada is step 3: in the past, the only way to make rushes available remotely while in the field was either (a) upload to a cloud bucket and wait, or (b) ship the SSD. Strada makes (a) unnecessary for any session where the editor only needs *to look at* the rushes.

---

### Multi-NLE orchestration

I work with three NLEs in different roles:

- **Final Cut Pro** is the primary editing environment. The Splicekit MCP gives me programmatic control of FCP from Claude Code, which lets me build agentic editing workflows.
- **DaVinci Resolve** is the color grading environment, and increasingly the documentation/transcription environment because of its native AI features and its lossless integration with BRAW.
- **Adobe Premiere Pro** is used for specific collaborator partners who work in Premiere, and for some integration with After Effects when the workflow demands it.

Each NLE reads the same source files through Strada, so the choice of NLE does not constrain the data layer. The MCPs I have configured for Claude Code include:

- `splicekit` for FCP control
- `Adobe_Premiere_MCP_Server` for Premiere Pro
- `Adobe_After_Effects_MCP_Server` for AE
- `Adobe_Photoshop_MCP_Server` for PS

These allow me to direct edits, batch operations, and project setup from a single conversation in Claude Code, while keeping the actual NLE as the authoritative environment for the human review and decision-making.

---

### Local AI inference layer

This is documented in detail in the [Sovereignty essay](../../position/souverainete.md). Summary:

- **ComfyUI** as the visual pipeline editor for generative work (image and video).
- **Pinokio** as the one-click installer for the open source AI tools I need (Stable Diffusion variants, Wan AI, AnimateDiff, voice cloning).
- **Ollama** for local LLM inference (Qwen 3.5, Hermes, Gemma 4) on the GPUs.
- **MLX** on Apple Silicon for fast inference on the M-series chips.

The principle is the same as for the storage layer: the data never leaves the local hardware. The models are downloaded once and run indefinitely. No API calls to OpenAI, Anthropic, Google for the generative work. Claude Code remains the orchestration layer for the conversational reasoning and architecture decisions, with the explicit acknowledgment that this is the one cloud dependency in the stack.

The role of local AI in the workflow:

- Generative pipelines for *Vues Lumière* (latent space as documentary territory)
- LoRA training on Goldberg corpus for specific visual styles
- Voice cloning and transcription for the *Persona Factory* essays on Goldberg
- Spatial reconstruction pipelines for the documentary geographies

---

### Backup architecture (3-2-1 with longevity)

| Copy | Medium | Frequency | Purpose |
|---|---|---|---|
| 1 | NAS Ugreen DXP8800 Pro (Paris) | Continuous (Strada-backed) | Working master |
| 2 | 4T external Acasis (travels with me) | Every session | Geographic separation, working offline |
| 3 | Cloudflare R2 cold storage | Weekly cron | Offsite, long-term cold backup |
| 4 (future) | OWC Mercury Pro LTO-8 | Per-project archive | Thirty-year shelf life, claimable as CNC production cost |

The R2 layer is *backup, not access*. The previous architecture was confused about this and used R2 as both, which produced the worst of both worlds (slow access and expensive backup). With Strada handling access from the local NAS, R2 returns to its proper role: a low-cost (zero egress to Cloudflare Workers, fifteen dollars per terabyte per month storage) offsite cold copy.

The LTO-8 addition becomes pertinent when a project is delivered. The OWC Mercury Pro LTO writes at 300 MB/s native, twelve terabytes per tape, sixty euros per tape, thirty years of shelf life. It is the only archive medium that does not depend on the continued existence of a service provider. For *The Goldberg Variations* master, *Vues Lumière*, *Virus*, and the back catalog of previous films, the LTO archive is the only credible long-term plan.

---

### Cost summary

| Item | One-time cost | Recurring |
|---|---|---|
| MikroTik 10 GbE switch | 199 € | 0 |
| 2× NVMe cache for Ugreen NAS (2 TB each) | 250 € | 0 |
| 2× USB-C to 10 GbE adapter (OWC) | 200 € | 0 |
| OWC Express 4M2 Ultra TB5 + 4× 2 TB NVMe (Q3 2026) | 1,400 € | 0 |
| Strada Connect subscription | 0 | ~100 €/year (paid tier with all collaborators) |
| Cloudflare R2 (196 GB current, growing) | 0 | ~50 €/year storage |
| OWC Mercury Pro LTO-8 (future, claimed as production cost) | ~4,500 € | ~60 € per 12 TB tape |
| **Total studio infrastructure** | **~2,050 € immediate + ~1,400 € Q3 2026 + ~4,500 € future** | **~150 €/year** |

For the full detailed cost comparison against a cloud-first equivalent stack (LucidLink Business, AWS S3 + Glacier, Frame.io, etc.) over a five-year horizon, see the [benchmark document → Cost comparison](strada-alternatives-benchmark.md#cost-comparison-local-first-vs-cloud-first-stack-five-year-horizon).

Summary of that comparison: the local-first studio costs roughly 5% of the cloud-first equivalent over five years, owns the hardware at the end, and keeps the full chain of custody of every transfer.

---

### What doesn't change

The architecture described here does not replace:

- The CNC funding cycle (it complements it, by making the production budget go further)
- The festival circuit (the films circulate the way they always have)
- The producer relationships (Films Grand Huit on Goldberg, the collaborators on other projects)
- The human work of editing, color, sound, writing

What it changes is the *substrate* on which all of this rests. The substrate becomes mine. The work that used to be paid to AWS, Frame.io, Adobe, and LucidLink stays in my studio. The collaborators access the same files through a protocol I control. The data survives any single service shutdown.

---

### Open questions

I do not have answers to all of these. They are on the working board.

1. **What happens when a Strada beta closes a feature I depend on?** Strada is a startup. They could change the API, the pricing, the architecture. Mitigation: the data is on my NAS regardless. If Strada disappears, I lose the protocol layer, not the storage. I would switch to LucidLink, Syncthing, or whatever else fills the role.

2. **What is the right archive cadence for LTO?** Probably: one tape per delivered film, one tape per shooting block over twelve terabytes, written immediately at delivery and stored in a fireproof safe.

3. **How does this scale to the CENTQUATRE residency with three other artists?** The residency starts September 2026. Four artists, each with their own infrastructure or shared infrastructure. The CENTQUATRE provides RTX 4090 workstations for AI inference. My architecture has to interoperate with theirs without becoming a single point of failure.

4. **The Claude Code dependency.** As long as Anthropic is the orchestration layer, the stack is not fully sovereign. Open source LLMs at the 400-billion-parameter scale are not yet practical at acceptable quality for the kind of orchestration I do. This is the seam I am most uncomfortable with. I will revisit it every six months.

5. **The tape redundancy question.** LTO-8 fails. Tapes degrade. The right architecture is probably two tape copies in different physical locations, plus the R2 cold backup, plus the NAS. Four-copy strategy for masters. This is the standard archive practice for broadcast deliverables.

---

### Update log

- **2026-05-15** — Initial document. Architecture decided in the wake of the Cioni meeting. Order placed for MikroTik switch and NVMe cache drives. Strada install scheduled for next week.
- **2026-05-15 (rev 2)** — Architecture refactored after re-reading the Cioni transcript and inventorying existing hardware. The QNAP TS-453BT3 bought in 2020 (4 bays, 40 TB RAID 5, Intel Celeron J3455, 2× Thunderbolt 3, 2× 10 GbE) is now positioned as the HOT TIER in direct-attach mode to the Mac Mini. The Ugreen DXP8800 Pro is repositioned as the COLD TIER. Local cache strategy added (50-500 GB on external drive per collaborator machine). xxHash chain-of-custody confirmed automatic via Strada transfer receipts. Cinematic-mode iPhone (depth/post-focus) noted as out-of-scope for Strada.
- **2026-05-15 (rev 3)** — Council review corrections applied: QNAP model corrected from TVS-872XT (initial misidentification) to TS-453BT3 (actual hardware: 4 bays, Celeron J3455, 2× 10 GbE). Collaborator names anonymized to neutral descriptions for public publication. Mentions of specific archive provenance for *The Goldberg Variations* generalized to avoid risk to ongoing legal procedures. Mac Mini location corrected: Paris (not Paris/CENTQUATRE — the residency begins September 2026). Cost summary cross-referenced to the benchmark document instead of duplicated. Router context added: currently 2.5 GbE WAN, planned upgrade to 10 GbE.

(Future entries will be appended here as the architecture evolves.)

---

[Back to Method](../../README.md) — [Companion essay: You Are Your Own Cloud](../../position/your-own-cloud.md)
