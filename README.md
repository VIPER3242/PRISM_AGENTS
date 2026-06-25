# PRISM — Private Residential Intelligence System Matrix

> Fully local, multi-agent home AI. All inference on-device. No cloud.

---

## Licence & Usage

PRISM is **source-available** — not open source. The code is public
for educational study and collaboration only. Use, deployment, and
independent forking are not permitted without written permission.

Calista's character identity — her name, personality, lore, speech
design, and the distinctive feel that makes her herself — is
separately protected and is not available for reuse or adaptation
in any form.

Full terms: [LICENSE](./LICENSE) · [IDENTITY.md](./IDENTITY.md) · [CONTRIBUTING.md](./CONTRIBUTING.md)

---

## What Is PRISM?

PRISM is a private, fully local, multi-agent AI system built to run on self-hosted hardware inside a single home. It is not a product to be launched and not a platform to be used. Every model runs locally. Every piece of data stays on the home network. Privacy is architectural.

Six specialist agents handle distinct domains. One orchestrator — **Calista** — is the sole interface to the owner. All other agents are infrastructure.

---

## Design Principles

| Principle | Detail |
|---|---|
| **Local-only inference** | All LLMs via Ollama on Apple Silicon. No cloud API ever called. |
| **Privacy is architectural** | No email content, camera footage, or conversation data leaves the network. |
| **Clean domain separation** | Each agent owns one domain. No overlap, no exceptions. |
| **Approval-gated actions** | Sending messages, pushing code, running automation — all require explicit owner approval. |
| **One writer, one database** | Calista is the sole write authority on the central memory bank. Every exception is documented. |

---

## Agents

### Calista — Orchestrator
**Central Automated Local Intelligence System Task Administrator**

The only agent the owner interacts with directly. Receives all input, delegates to specialists, synthesises results, and delivers the response. Owns the central SQLite memory bank. Runs 24/7.

**Manages:** Voice, Google Calendar/Tasks, Spotify, Discord DMs, Telegram, smart home, desktop PC, room/desk cameras, robotic arm, shoulder robot, alarms, reminders, morning briefing, Goodnight routine.

**Hardware:** Mac Studio M4 Max — 64GB unified memory, 1TB SSD.

| Tier | Model | Quant | Memory | Role |
|---|---|---|---|---|
| Fast | qwen3:8b | Q5_K_M | ~7 GB | Voice, casual chat — always resident |
| Think | qwen3:32b | Q5_K_M | ~24 GB | Complex reasoning, drafts — on demand |
| Accurate Vision | qwen2.5-vl:7b | Q4_K_M | ~5.5 GB | OCR, scene understanding — on demand |
| Real-time CV | YOLO v8 nano + MediaPipe Holistic | CoreML | < 0.5 GB | Camera detection — always running |

Full load: ~46GB. 18GB buffer remaining.

---

### Iris — Research Agent
**Information Retrieval and Insight Synthesis**

Manual-trigger research only. Accepts tasks via her own Telegram bot or Calista delegation. Delivers a Telegram summary and a structured Obsidian note to the NAS vault. Three levels: Quick Overview, Slightly Deeper (30–90 min), Deep Research (2–5 hrs). Trend-sensitive notes auto-refresh nightly.

**Hardware:** Mac Mini M4 Pro — 48GB unified memory, 512GB SSD.

**Sources:** DuckDuckGo, broad web scraping, Reddit, Wikipedia, GitHub, Hacker News, arXiv, news/RSS, YouTube, social media (public, read-only). Paywalled pages skipped and logged.

---

### Sasha — Security Agent
**Surveillance and Security Awareness Hub Agent**

Ingests RTSP streams from up to 10 IP cameras. Runs continuous detection at 30–60 fps via YOLO. Matches faces against registered persons. Fires tiered Telegram alerts with thumbnails. Stores event-triggered clips locally with nightly NAS archiving. Loads a VLM on P0/P1 events for natural-language scene descriptions.

Holds the **only write exception to the central memory bank** — face registration data to `irl_people`, nothing else.

**Hardware:** Mac Mini M4 — 16GB unified memory, 512GB SSD. Always on.

**Models:** YOLO v8 nano (CoreML, always-on) + face_recognition/dlib (always-on) + llava:7b Q4 (on-demand, evicts after 120s idle).

**Alert tiers:** P0 Critical → P1 High (unknown person) → P2 Normal (known arrival) → P3 Low (motion, no face) → P4 Log only.

---

### Tessa — Development Agent
**Technical Engineering and Software Synthesis Agent**

Builds external software for the owner. Never touches PRISM's own codebase. Uses a mandatory two-model pipeline: Qwen3 32B for planning and verification, Qwen2.5-Coder 32B for execution via Aider. Every task: plan → execute → verify. Planner reloads after coding to produce `plan_vs_code_check.md` before any PR. All PRs require owner approval. No direct pushes to main.

**Hardware:** Mac Mini M4 Pro — 48GB unified memory, 512GB SSD. On-demand.

---

### Maia — Infrastructure Agent
**Maintenance and Agent Integrity Analyser**

Hosts the Mosquitto MQTT broker. Monitors heartbeats from all agents, polls thermals on all Mac nodes via SSH, tracks UPS state, manages Wake-on-LAN and remote shutdown/restart, watches Docker containers, and orchestrates LoRA fine-tuning for Calista's model. No language model. Not conversational. Pure infrastructure.

The NAS hardware and Mosquitto broker are deployed during Calista Phase 1 as a network prerequisite. Maia's full agent build is Phase 5.

**Hardware:** Intel N100 mini-ITX — 16GB DDR4, Jonsbo N2 5-bay case, 256GB NVMe boot, Seagate IronWolf NAS 4TB × 1, 2.5GbE. Always on. ~6W TDP idle.

---

### Cleo — Communications Agent
**Correspondence and Local Email Organizer**

Manages the owner's personal Gmail account only. Classifies every incoming message (P0–P4), detects and quarantines phishing, applies labels autonomously, drafts replies on request, delivers a daily digest for low-priority mail. Never sends without explicit owner approval. Never stores email body content locally — cleo.db is a metadata index only.

**Hardware:** Mac Mini M4 — 16GB unified memory, 512GB SSD. Always on.

**Models:** qwen3:4b Q4_K_M (triage, always resident) + qwen3:8b Q5_K_M (drafting, on demand).

---

## Contributing

Pull requests only. Read [CONTRIBUTING.md](./CONTRIBUTING.md) before
submitting anything — it covers what is in scope, what is not, and
how to approach the process.

---

## Contact

**overpowered41.23@gmail.com**
