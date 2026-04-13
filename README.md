# Nova

> A 24/7 autonomous AI assistant — voice-first, always listening, with long-term memory and a full plugin architecture.

🌐 **[neural-werk.de](https://neural-werk.de)** — Live demo

Nova is a personal AI assistant I built from scratch and run on my own server. It is not a wrapper around ChatGPT. Every component — memory, voice pipeline, Android app, plugin system, agent communication — is custom built.

---

## What Nova Can Do

- **Wake word detection** — say "Hey Nova" anywhere, even with screen off
- **Voice conversation** — real-time STT → LLM → Azure TTS pipeline
- **Long-term memory** — remembers what matters, forgets what does not (GenesisBrain)
- **Proactive behavior** — reminds you of deadlines, flags urgent messages, runs scheduled flows
- **Email management** — reads and replies to emails autonomously via AI sub-agent
- **Android + WearOS** — native app with home screen widget and Samsung Watch companion
- **Plugin system** — every feature is an isolated addon with its own backend

---

## System Architecture

```
┌──────────────────────────────────────────────────────────┐
│                     VServer (Linux)                      │
│                                                          │
│  ┌────────────────────────────────────────────────────┐  │
│  │              claude-panel (Node.js)                │  │
│  │                                                    │  │
│  │  Backend Core          Addon System                │  │
│  │  ├── Auth (mTLS)       ├── bridge-connector        │  │
│  │  ├── File Manager      ├── autonomi                │  │
│  │  ├── Push System       ├── com-hub                 │  │
│  │  ├── Addon Manager     ├── mail-filter             │  │
│  │  └── Addon API         ├── security-monitor        │  │
│  │                        └── ...8 more addons        │  │
│  │                                                    │  │
│  │  GenesisBrain (Python)                             │  │
│  │  └── Doorwatcher (FastAPI, Unix Socket)            │  │
│  └────────────────────────────────────────────────────┘  │
│                                                          │
│  nginx (HTTPS + mTLS)  ·  fail2ban  ·  WireGuard VPN    │
└──────────────────────────────────────────────────────────┘
                           │
                    WireGuard VPN
                           │
          ┌────────────────────────────┐
          │     Android (Nova Bridge)  │
          │  Wake Word · TTS · Voice   │
          │  WebSocket · WearOS        │
          └────────────────────────────┘
```

---

## Components

| Component | Description | Repo |
|---|---|---|
| 🧠 GenesisBrain | Long-term memory with 15 levels, decay, immutability | [nova-genesisbrain](https://github.com/nico-gordon-gilles/nova-genesisbrain) |
| 🤖 Autonomi | Autonomous layer: goals, mood, inbox, flows, daemon | [nova-autonomi](https://github.com/nico-gordon-gilles/nova-autonomi) |
| 💾 STM | Short-term memory consolidation via LLM pipeline | [nova-stm](https://github.com/nico-gordon-gilles/nova-stm) |
| 📱 Bridge App | Android voice client: wake word, TTS, WearOS | [nova-bridge-app](https://github.com/nico-gordon-gilles/nova-bridge-app) |
| 📬 Mail Filter | AI email pipeline with sub-agent and circuit breaker | [nova-mail-filter](https://github.com/nico-gordon-gilles/nova-mail-filter) |
| 🔌 Platform | Plugin architecture, addon API, file manager | [nova-platform](https://github.com/nico-gordon-gilles/nova-platform) |
| 🛡️ Security Monitor | SSH, port and sudo intrusion detection | [nova-security-monitor](https://github.com/nico-gordon-gilles/nova-security-monitor) |
| 🔀 Com Hub | Multi-agent router with loop detection and approvals | [nova-com-hub](https://github.com/nico-gordon-gilles/nova-com-hub) |

---

## Infrastructure

| Layer | Technology |
|---|---|
| Server | Linux VPS, systemd services |
| Reverse Proxy | nginx (HTTPS + mTLS) |
| VPN | WireGuard (all voice traffic tunneled) |
| Security | fail2ban, mTLS client certificates, Unix Sockets |
| LLM | Mistral (main) · Gemini (research/STM) · Claude (dev) |
| TTS | Azure Neural TTS (streamed MP3) |
| STT | Whisper |
| Memory | SQLite (WAL) everywhere · GenesisBrain (custom) |
| Android | Java · Porcupine wake word · WebSocket |
| WearOS | Wearable Data Layer API |

---

## Scheduled Jobs

| Time | Job |
|---|---|
| Every 5 min | Mail filter — check and reply to emails |
| Every 5 min | Autonomi daemon — check goals, inbox, flows |
| Every 5 min | Security watcher — SSH/port/sudo monitoring |
| 04:00 daily | STM consolidation — distill conversations into memories |
| 03:00 daily | GenesisBrain decay — compress old memories |
| 05:00 daily | System backup |
| Sat 09:00 | Brain health monitor |

---

## Security Design

- All voice traffic runs over **WireGuard VPN** — WebSocket ports not exposed publicly
- **mTLS** — every device has a client certificate, revocable via CRL
- **Unix Sockets** for all internal IPC — nothing exposed as TCP unnecessarily
- **GenesisBrain Doorwatcher** — only write path into the memory database
- **Read-only security watcher** — observes, never blocks or modifies

---

## Why I Built This

I wanted a real AI assistant — not a demo, not a prototype. Something that runs 24/7, knows who I am, remembers what matters, and can act on my behalf when I ask it to.

After 8 years of web development and experience as a SAP administrator, I started building Nova as a way to go deep on AI systems. Every architectural decision in this project was made because I had a real problem to solve.

---

📍 Lower Saxony, Germany · 🌐 [neural-werk.de](https://neural-werk.de)

