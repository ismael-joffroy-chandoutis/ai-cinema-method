## Benchmarks and Alternatives for a Sovereign Studio

A comparative reading of the tools that compete for the same slot in the post-production workflow as Strada Connect, the storage solutions that compete with the NAS approach, and the open source stack that could approximate Strada's functionality.

Companion to [*You Are Your Own Cloud*](../../position/your-own-cloud.md) and [*Local-First Studio Workflow*](local-first-studio-workflow.md).

*May 2026. Numbers verified at the date of writing.*

---

### Why this document exists

Before committing to a stack, the question is: are the alternatives credible? Could I get the same result with open source? Could I avoid the Strada subscription? Could a different storage architecture eliminate the NAS entirely? This document is the working answer.

The conclusion, summarized: **there is no complete open source equivalent to Strada Connect.** The combination of peer-to-peer transport, no-upload streaming, Finder-level mounting, and NLE compatibility is architecturally unique. The closest open source approximation requires four separate tools assembled and maintained by the user. For now, Strada at eight dollars per month is the right call. The data underneath it remains mine.

---

### Strada vs. the field: feature matrix

| Tool | P2P transport | No upload required | Frame-precise review | NLE integration | Open source | Entry price |
|---|---|---|---|---|---|---|
| **Strada Connect** | Yes | Yes | Via browser | Yes (Finder mount) | No | 8 $/month |
| **LucidLink Filespaces** | No (cloud) | No | No | Yes (Finder mount) | No | 7-32 $/user/month |
| **Clapshot** (open source) | No | No (upload) | Yes | No | GPL v3 | Free |
| **Kollaborate** | No | No | Yes | Yes (NLE markers) | No | ~7 $/month |
| **Syncthing** | Yes (LAN/WAN) | No (full copy required) | No | No | MPL-2.0 | Free |
| **Resilio Sync** | Yes (BitTorrent) | No (full copy required) | No | No | No | ~60 $/month |
| **rclone** | No | No | No | No | MIT | Free |
| **MASV** | No (cloud) | No | No | No | No | 0.25 $/GB |
| **Signiant Media Shuttle** | No (cloud) | No | No | Partial | No | ~8,000 $/year |
| **IBM Aspera** | No (server) | No | No | No | No | 5,000-50,000 $+ |
| **PeerTube** | Partial (CDN) | No (upload) | No | No | AGPL-3 | Free |
| **Evercast** | No (stream) | Yes (screencast) | Yes (live) | Yes (live screen) | No | ~150 $/month |
| **ftrack** | No | No | Yes | Partial | No | ~10-20 $/user/month |
| **JuiceFS** | No | No | No | Partial (POSIX) | Apache 2 | Free |

What this table shows: no other tool combines the four features that make Strada Connect what it is. LucidLink is the closest commercial competitor but routes through the cloud and costs significantly more per user. Clapshot is the closest open source review tool but does not provide NLE-level access. Syncthing is the closest open source P2P file tool but requires full copies on every machine, defeating the on-demand streaming model.

If I had to build a Strada equivalent from open source today, it would be:

- **Syncthing** for P2P sync of working folders
- **Clapshot** for the frame-precise review layer
- **rclone** for bulk transfers and backup
- **xxHash + MHL** for integrity and chain of custody

Four tools, three configuration files, no integrated experience. This works for a single user with patience. It does not work for a four-person remote production. Eight dollars a month is the right price for the orchestration.

---

### Storage solutions compared

The question of *whether to keep the NAS* depends on the workload. For my workload (one to two simultaneous editors, occasional remote review, archive on top), the NAS makes sense. For a larger team, a SAN starts to make sense. For a solo editor with heavy active timeline, direct-attached storage on Thunderbolt 5 dominates.

| Solution | Capacity | Read speed | Write speed | Streams 4K ProRes 422 HQ | Streams ProRes Proxy | Network | Price (capacity 32-64 TB) |
|---|---|---|---|---|---|---|---|
| **Ugreen DXP8800 Pro** (current) | 8 bays, up to ~120 TB raw (60-80 TB usable with 6 bays in RAID 6) | ~500 MB/s on 10 GbE | ~400 MB/s | 2-3 (network limited) | 50+ | 2× 10 GbE | ~700 € chassis + drives |
| **Synology DS1823xs+** | 8 bays + expander to 18 | 3,100 MB/s on 10 GbE | 2,800 MB/s | 4 (network limited) | 50+ | 10 GbE | ~1,400 € chassis + drives |
| **Synology DS3622xs+** | 12 bays + expander to 36 | 4,700 MB/s | 4,200 MB/s | 4 (network limited) | 50+ | 2× 10 GbE | ~2,800 € chassis + drives |
| **QNAP TVS-h874** | 8 bays + GPU slot | varies | varies | 4 (network limited) | 50+ | 10 GbE | ~2,500 € chassis + drives |
| **Promise PegasusPro R8** | 8 bays, DAS+NAS hybrid | 2,800 MB/s on TB3 | 2,800 MB/s | 5+ simultaneous editors | 100+ | TB3 + 10 GbE | ~5,000 € |
| **OWC Express 4M2 Ultra (TB5)** | 4 NVMe slots, 32 TB max | 6,600 MB/s | 5,900 MB/s | 30 (DAS local) | 1000+ | TB5 daisy chain | ~1,400 € + drives |
| **OWC ThunderBlade X12** | 12 NVMe, up to 96 TB | 6,500 MB/s | 5,900 MB/s | 30 (DAS local) | 1000+ | TB5 | ~3,500-5,000 € |
| **QNAP TS-453BT3 (Thunderbolt 3 NAS, already owned)** | 4 bays, 40 TB RAID 5 | ~1,500 MB/s (TB3 DAS mode, with SSDs) / ~700-1,000 MB/s (HDD RAID 5) | similar | 5-7 (TB3 DAS) | 100+ | 2× TB3 + 2× 10 GbE, Intel Celeron J3455 | already paid (2020) |

Key observations:

- On 10 GbE, all the NAS solutions plateau at the *same* throughput (the network is the bottleneck). The differences between Ugreen, Synology, and QNAP at this layer are in software ecosystem and reliability, not raw speed.
- Direct-attached Thunderbolt 5 NVMe is an order of magnitude faster than any NAS over 10 GbE. For active timeline editing, this is the right tool.
- The PegasusPro is interesting for a 3-5 editor team because it provides both DAS speed and NAS sharing in the same chassis. Overkill for my current scale.
- **The QNAP TS-453BT3 I already own** (bought 2020, used as NAS for five years) is the hidden asset in this benchmark. In Thunderbolt 3 DAS mode it provides ~700-1,000 MB/s sustained from RAID 5 HDDs, comparable to or faster than a NAS over 10 GbE, bypassing the modest Celeron J3455 CPU that was its main limitation in pure-NAS mode. Zero new spend.

**Decision: hot tier on the QNAP TS-453BT3 in Thunderbolt 3 DAS mode** (the Cioni-equivalent layer, already paid for). **Cold tier on the Ugreen DXP8800 Pro on the 10 GbE network** (archives, masters). Add the 10 GbE MikroTik switch. Add NVMe cache to the Ugreen. Add direct-attached OWC Express 4M2 Ultra TB5 (up to 32 TB per unit, 128 TB daisy-chained, 6,622 MB/s in RAID 0) for the active editing scratch when the MacBook Pro M5 Max ships. This is the right architecture at the right price.

---

### Codec performance reference

For a workflow that runs primarily in ProRes with HEVC proxies:

| Codec | Bitrate (4K 25fps) | Apple Media Engine decode | NVENC decode | Real-world streams on 10 GbE |
|---|---|---|---|---|
| ProRes Proxy | 45 Mbps (5.6 MB/s) | Native | Software | 178 |
| ProRes LT | 102 Mbps (12.7 MB/s) | Native | Software | 78 |
| ProRes 422 | 500 Mbps (62.5 MB/s) | Native | Software | 16 |
| ProRes 422 HQ | 1,760 Mbps (220 MB/s) | Native | Software | 4-5 |
| ProRes 4444 | 2,650 Mbps (330 MB/s) | Native | Software | 3 |
| ProRes RAW | variable (12-50 MB/s) | Native | Software | 25-100 |
| HEVC 10-bit 4K proxy | 25-35 Mbps | Native, hardware-accelerated | Native, NVENC | 200+ |
| H.264 10-bit 4K proxy | 25-50 Mbps | Native | Native, NVENC | 200+ |
| BRAW 4K 8:1 | ~50 MB/s | Software via plugin | Software via plugin | 20 |
| BRAW 6K 5:1 | ~194 MB/s | Software via plugin | Software via plugin | 5 |
| BRAW 6K 3:1 | ~323 MB/s | Software via plugin | Software via plugin | 3 |

For my workflow:

- iPhone 15 Pro Max in ProRes 422 HQ for primary capture (native iOS, no transcoding required).
- HEVC 10-bit 4K proxy at 30 Mbps generated automatically on the Mac Mini for FCP scrubbing.
- Original ProRes preserved for color grading in DaVinci Resolve.
- Master delivery in ProRes 4444 or per specific spec.

The Apple Media Engine on M3/M4/M5 decodes ProRes and HEVC natively in hardware. Scrubbing 4K HEVC 10-bit on a MacBook Pro M5 Max is fluid without any proxy. Proxies remain useful for the MacBook Air M3 (which has the same Media Engine but less RAM and a smaller GPU) and for collaborators on older hardware.

---

### Why xxHash64 and not MD5 or SHA-256

The integrity layer matters because films are large and the cost of detecting corruption is high. The cryptographic options (MD5, SHA-1, SHA-256) are designed for adversarial integrity (verifying that a file has not been tampered with by an adversary). They are slow.

xxHash64 is non-cryptographic. It is designed for the *non-adversarial* case of detecting accidental corruption (bit rot, transmission errors, drive failures). It runs at memory bandwidth speed, roughly twenty times faster than SHA-256, often faster than the disk can read the file.

For media workflows the choice is clear:

- xxHash64 (or xxh3) for checksum during offload, sync, archive verification
- The Media Hash List (MHL) format, an ASC industry standard, wraps xxHash or other algorithms in a portable file that travels with the rushes

The relevant tools:

- `xxhsum` command-line utility ([Cyan4973/xxHash on GitHub](https://github.com/Cyan4973/xxHash))
- `mhl` from Pomfort, Hedge, ShotPut Pro, or open source implementations
- Native support in some DIT tools (Silverstack, Hedge)
- **Built into Strada**: every transfer Strada handles generates a download checksum receipt in the jobs panel. The receipt records the source path, destination path, file count, hash, and timestamp. This is the Netflix-compliant chain of custody integrated at the protocol level. Confirmed in the Cioni demo on 2026-05-14: every job, every transfer, every cross-machine move is logged and reproducible.

I have built simple xxh64 + MHL generation into my offload scripts on the laptops. The MHL travels with the rushes from the camera card to the QNAP hot tier. For in-studio and remote transfers between machines, the Strada receipts replace the need to regenerate MHL manually.

---

### Cost comparison: local-first vs. cloud-first stack, five-year horizon

For a documentary studio with approximately 120 terabytes total across hot and cold tiers, and three to four collaborators:

| Layer | Local-first (my stack) | Cloud-first equivalent |
|---|---|---|
| Hot tier storage | QNAP TS-453BT3 40 TB (owned since 2020) = 0 € | AWS S3 40 TB × 0.023 $/GB/month = ~11,000 $/year |
| Cold tier storage | Ugreen DXP8800 Pro 80 TB usable (owned) + disks ~2,000 € | AWS S3 Glacier 80 TB = ~600 $/year |
| Egress / transfer | 0 (peer-to-peer Strada) | AWS egress estimated ~1,100 $/year |
| Remote access protocol | Strada Connect paid tier = ~100 €/year | LucidLink Business 4 users = ~1,500 $/year |
| Review platform | Strada built-in receipts and player = 0 | Frame.io Pro × 4 users = ~1,200 $/year |
| Local AI inference | 0 (RTX 5090, RTX 4090, M-series Macs already owned) | OpenAI / cloud equivalents = ~3,000-5,000 $/year |
| Scratch / active edit | OWC Express 4M2 Ultra + 8 TB NVMe = 1,400 € one-time | LucidLink local cache (subset of above) |
| Network | MikroTik 10 GbE switch + adapters = 400 € one-time | n/a (cloud) |
| Backup cold | Cloudflare R2 (196 GB current) = ~50 €/year | AWS Glacier 80 TB = ~600 $/year |
| **Year 1 total** | **~3,800 € + ~150 €/year** | **~19,000 $/year** |
| **Five-year total** | **~4,600 €** | **~95,000 $** |

The local-first studio costs roughly 5% of the cloud-first equivalent over five years, and the difference is even more favorable in absolute terms (€ vs $). The local-first studio also owns the hardware at the end of the five years and has the full chain of custody log of every transfer. The cloud-first studio has spent close to one hundred thousand dollars and has no asset to show for it.

Note that this comparison assumes the QNAP and Ugreen are already owned, which they are in my case. For a studio starting from zero, the upfront cost would be approximately +3,000-4,000 € more for the hot tier and +2,000-3,000 € more for the cold tier, still well under five years of cloud subscription fees.

This is the economic argument. It is real, but it is not the primary argument. The primary argument is sovereignty: the data does not depend on the continued existence of a service provider, the continued price of a subscription, or the continued availability of a particular feature.

---

### What I am buying this month (immediate roadmap)

| Item | Cost | Priority | Status |
|---|---|---|---|
| MikroTik CRS304-4XG-IN 10 GbE switch | 199 € | P0 | Order this week |
| 2× Samsung 980 Pro 2 TB NVMe (NAS cache) | 280 € | P0 | Order this week |
| 2× OWC USB-C to 10 GbE adapter | 200 € | P0 | Order this week |
| Strada Connect installation + test | 0 | P0 | This week |
| Strada Connect paid tier subscription | 8 $/month | P1 | After successful test |
| Cat6a cables (5× 2m) | 30 € | P0 | This week |
| **Subtotal immediate** | **~710 €** | | |

### What I am buying when the MacBook Pro M5 Max ships (Q3 2026)

| Item | Cost | Priority | Status |
|---|---|---|---|
| OWC Express 4M2 Ultra TB5 enclosure | 549 € | P1 | Pre-order |
| 4× WD Black SN850X 2 TB NVMe (scratch) | 800 € | P1 | Order when enclosure ships |
| Thunderbolt 5 cables (2m, 3m) | 80 € | P1 | With enclosure |
| **Subtotal Q3 2026** | **~1,430 €** | | |

### What I am budgeting for the next CNC-funded delivery

| Item | Cost | Priority | Status |
|---|---|---|---|
| OWC Mercury Pro LTO-8 tape system | ~4,500 $ | P2 | Claim as production cost on Goldberg delivery |
| 4× LTO-8 tapes (initial set) | 240 € | P2 | With system |
| LTO software license (LTFS or commercial) | 0 to 300 $ | P2 | TBD |
| **Subtotal LTO** | **~5,000 $** | | |

### What I am NOT buying

- A new NAS. The Ugreen DXP8800 Pro is recent, top-tier, becomes the cold tier. The QNAP TS-453BT3 becomes the hot tier in DAS mode. Both are already paid for.
- An OWC ThunderBay 4 or 8. The QNAP TS-453BT3 in Thunderbolt 3 DAS mode is the functional equivalent and already owned.
- A Promise PegasusPro. Overkill for current team size. Reconsider if the CENTQUATRE residency in September 2026 expands the active editor count.
- A QNAP with GPU. The GPU in the NAS does not help FCP or DaVinci, which use the client GPU. The Mac Mini M4 is enough.
- Fibre Channel SAN. Enterprise overkill, hidden costs, no benefit at this scale.
- A new Mac Mini or Mac Studio. The M4 is sufficient as a server.

---

### What the architecture does not solve

I want to be honest about the limits.

**The Claude Code dependency.** The orchestration layer of my practice runs on Anthropic's servers. This is the one cloud dependency that the local-first architecture does not eliminate. Open source LLMs at the 400-billion-parameter scale that I could realistically use as a substitute are not yet at quality parity for this workflow. I revisit this every six months.

**The festival circuit and the broadcaster ecosystem.** The local-first studio does not change who sees the films. The work still circulates through CNC, ARTE, festivals, streaming partners. Distribution is a separate problem.

**The CNC funding cycle.** Sovereignty is not financial autonomy. CNC funds the films. The architecture described here makes the production budget go further, but it does not replace the funding cycle.

**The collaboration friction.** The Paris editing team needs to install Strada Connect and learn it. The producer may not adopt it. The architecture works only if the collaborators participate in it. This is a social problem, not a technical one.

**The on-set bandwidth problem.** Some locations have no internet. Strada works at 0.5 Mbps but assumes some connectivity. For a fully disconnected location (deep wilderness, censored country), the workflow falls back to physical media transport. This is true for everyone, but it is worth naming.

---

### Update log

- **2026-05-15** — Initial document. Decisions confirmed. Hardware orders queued.
- **2026-05-15 (rev 2)** — QNAP model corrected to TS-453BT3 (4 bays, Celeron J3455, 2× TB3, 2× 10 GbE). Cost calculation aligned with the workflow document. Storage benchmark table updated to reflect the actual hardware and its real-world DAS throughput.

(Future updates will be appended here.)

---

[Back to Method](../../README.md) — [Companion essay](../../position/your-own-cloud.md) — [Technical workflow](local-first-studio-workflow.md)
