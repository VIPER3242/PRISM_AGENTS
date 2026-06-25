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

PRISM is a private, fully local, multi-agent AI system built to run on self-hosted hardware inside a single home. Not a product. Not a platform. Every model runs locally. Every piece of data stays on the home network. Privacy is architectural.

Six specialist agents handle distinct domains. One orchestrator — **Calista** — is the sole interface to the owner. All other agents are infrastructure.

---

## Design Principles

| Principle | Detail |
|---|---|
| **Local-only inference** | All LLMs via Ollama on Apple Silicon. No cloud API ever called. |
| **Privacy is architectural** | No email content, camera footage, or conversation data leaves the network. |
| **Clean domain separation** | Each agent owns one domain. No overlap, no exceptions. |
| **Approval-gated actions** | Sending messages, pushing code, running automation — all require explicit owner approval. |
| **Simulation before build** | Minimum 30-scenario flaw simulation before any production code is written. |
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

## How It Works

### Communication — MQTT

All inter-agent communication routes through a single Mosquitto MQTT broker on Maia's NAS. Topic ACLs enforced at broker level — each agent publishes only to its own topics and subscribes only to what it needs.

```
Owner
  ↓ voice / Telegram / Discord
Calista (Mac Studio)
  ↓ MQTT
  ├── Iris    → research
  ├── Sasha   → security alerts
  ├── Tessa   → development
  └── Cleo    → email

Maia (NAS)
  ├── MQTT broker (Mosquitto)
  ├── Heartbeat aggregation
  ├── Thermal monitoring (SSH)
  ├── NAS storage
  └── LoRA orchestration
```

### Memory

| Store | Writer | Contents |
|---|---|---|
| `calista.db` — SQLite, Mac Studio | Calista only. Sasha: `irl_people` face data only. | All programmatically queried data — memory, journal, contacts, schedules. |
| Obsidian Vault — Mac Studio | Calista | Narrative/lore/personality content. Read as context. Never queried programmatically. |
| Iris Vault — NAS | Iris (write), Tessa (read-only) | Research notes, auto-refreshed trend notes. |
| Agent Vaults — NAS | Per-agent, own vault only | Tessa: project archives. Cleo: email logs. |

**Rule: programmatically queried + frequently written → SQLite. Narrative context + rarely written → Obsidian vault.**

### FastAPI Layer

Calista exposes a versioned FastAPI layer for all external integrations.

| Endpoint | Purpose |
|---|---|
| `POST /v1/message` | Text input → text response |
| `POST /v1/voice` | Audio input → audio response |
| `GET /v1/state` | Emotion state, agent statuses, thermals |
| `POST /v1/command` | Trigger a named action |
| `GET /v1/stream` | SSE stream of real-time output events |
| `WS /v1/live` | WebSocket for VTuber avatar bridge |
| `POST /v1/context` | Inject context into conversation window |
| `POST /v1/file` | Send document/image for VLM processing |
| `GET /v1/agents` | All agent statuses via Maia heartbeat data |
| `GET /v1/dashboard` | Room terminal kiosk dashboard |

Reachable from any authorised device via Tailscale.

---

## Hardware

| Node | Machine | Memory | Storage | Always-On |
|---|---|---|---|---|
| Calista | Mac Studio M4 Max | 64GB unified | 1TB NVMe | ✅ |
| Iris | Mac Mini M4 Pro | 48GB unified | 512GB NVMe | ❌ |
| Sasha | Mac Mini M4 | 16GB unified | 512GB NVMe | ✅ |
| Tessa | Mac Mini M4 Pro | 48GB unified | 512GB NVMe | ❌ |
| Cleo | Mac Mini M4 | 16GB unified | 512GB NVMe | ✅ |
| Maia (NAS) | Intel N100 mini-ITX | 16GB DDR4 | 256GB NVMe + 4TB IronWolf | ✅ |

All nodes on wired Ethernet. No WiFi. Inter-node communication via Tailscale.

**Physical hardware (Calista's domain):** Room terminal (10–13" wall-mounted touchscreen, wide-FOV camera, mic), desk camera, ceiling-mounted robotic arm (Pi 5 + AI HAT, serial bus servos, parallel jaw gripper, COB LED), shoulder-mounted robot (LCD, camera, speaker, mic — active when owner is out), TP-Link Kasa + Govee smart home, room mic array + dual-zone speakers, UPS.

---

## Tech Stack

| Category | Technologies |
|---|---|
| **Inference** | Ollama, CoreML |
| **LLMs** | qwen3:8b, qwen3:32b, qwen3:4b, qwen2.5-vl:7b, llava:7b, Qwen3 32B, Qwen2.5-Coder 32B |
| **Real-time CV** | YOLO v8 nano (CoreML), MediaPipe Holistic, face_recognition/dlib, OpenCV |
| **Voice** | openWakeWord, faster-whisper, Kokoro-82M, VITS2 + RVC |
| **Inter-agent** | Mosquitto MQTT, paho-mqtt |
| **Memory** | SQLite (WAL mode), Obsidian vaults |
| **Integrations** | Google Workspace API, Gmail API, Spotify API, Notion API, GitHub API |
| **Networking** | Tailscale, RTSP, ONVIF |
| **Infrastructure** | OpenMediaVault, Docker, launchd, NUT (UPS) |
| **API layer** | FastAPI, SSE, WebSocket |
| **Coding agent** | Aider, pyautogui, rapidfuzz |
| **VTuber** | VTube Studio WebSocket API, Live2D |
| **Smart home** | python-kasa, govee-api-laggat |
| **Robotics** | Feetech STS servos, MOSFET PWM, INA219, Raspberry Pi 5 + Hailo-8L / Google Coral |

---

## Contributing

Pull requests only. Read [CONTRIBUTING.md](./CONTRIBUTING.md) before
submitting anything — it covers what is in scope, what is not, and
how to approach the process.

---

## Contact

**overpowered41.23@gmail.com**
