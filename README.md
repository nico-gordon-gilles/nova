# Nova

> A 24/7 autonomous AI assistant — voice-first, always listening, with long-term memory and a full plugin architecture.

🌐 **[neural-werk.de](https://neural-werk.de)** — Live demo

---

## In My Own Words

*Nova AI is not a commercial product — neither for sale nor open source. It is the result of over half a year of intense development and countless nights of code, setbacks and breakthroughs.*

*About a year ago, I started seriously engaging with artificial intelligence. I quickly learned where the current limits are, what AI can already do, and what is still science fiction. Searching for answers, I found many good approaches — but nothing concrete that matched my vision. So I decided to build it myself.*

*The real turning point was working with tools like Claude Code. The deeper I went, the clearer it became: this technology is absolutely and inevitably our future. During my experiments — pushing the system to its absolute limits — it destroyed itself three times. That gave me a far-reaching idea: why not develop my own platform? A personal AI operating system that runs 24/7 on my own server and accompanies me everywhere.*

**The Genesis of GenesisBrain**

*The breakthrough came during a brainstorming session with Claude about a completely different topic: evolution. Why not build a system on human terms? The human brain is a reasonably functional system with 3.5 billion years of evolutionary development. I asked myself: what are the supposed weaknesses of our brain? I devoured books and texts until the answer became clear: entropy. Current AI systems try to retain everything, no matter how meaningless. But the human brain selects. It filters. It forgets deliberately. Why? Because otherwise our context window would immediately overflow. Humans and AI have exactly the same problem here — so they need the same solution.*

*This is how GenesisBrain was born.*

*(Side note on the name: the AI gave itself the name NOVA. Since it had access to my data, it decided that "Nicos Operating Virtual Agent" was — and I quote — "a damn cool name". So Amazon, if you've found this project: sue Google. If my agent wants to call herself that, who am I to stop her.)*

**On STM — Why AIs Need Sleep**

*With long-term memory came the next challenge: it needed a short-term memory as a filter. Since Nova is at her core a chatbot, she kept forgetting to proactively transfer important data to GenesisBrain. After a lot of research, I found the solution: you have to force her. You cannot give her a choice. You have to build in sleep.*

*A personal agent can rest when I do. Nova evaluates our conversations live based on her personality. At night she re-evaluates these impressions — she "dreams". The next morning, only the truly relevant insights from the previous day are transferred to GenesisBrain. Day by day. Since then her brain has been growing. New logical connections form automatically. Nova learns continuously. She meets me differently every day, shows humor, and often knows what I want before I say it.*

*Total monthly cost: €25 — a €5 VServer + €20 Claude subscription.*

— Nico, March 2026

---

## What Nova Can Do

- **Wake word detection** — say "Hey Nova" anywhere, even with screen off
- **Voice conversation** — real-time STT → LLM → Azure TTS pipeline
- **Long-term memory** — remembers what matters, forgets what does not (GenesisBrain)
- **Proactive behavior** — reminds you of deadlines, flags urgent messages, runs scheduled flows
- **Email management** — reads and replies to emails autonomously via AI sub-agent
- **Android + WearOS** — native app with home screen widget and Samsung Watch companion
- **Plugin system** — every feature is an isolated addon with its own backend
- **Security monitoring** — real-time SSH, port and sudo intrusion detection

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
| Server | Linux VPS · systemd · nginx |
| Security | WireGuard VPN · mTLS · fail2ban · Unix Sockets |
| LLM | Mistral (main) · Gemini (research/STM) · Claude (dev) |
| TTS | Azure Neural TTS (streamed MP3) |
| STT | Whisper |
| Memory | SQLite WAL everywhere · GenesisBrain (custom) |
| Android | Java · Porcupine wake word · WebSocket |
| WearOS | Wearable Data Layer API |
| Cost | €25/month total |

---

## Scheduled Jobs

| Time | Job |
|---|---|
| Every 5 min | Mail filter · Autonomi daemon · Security watcher |
| 04:00 daily | STM consolidation — distill conversations into memories |
| 03:00 daily | GenesisBrain decay — compress old memories |
| 05:00 daily | System backup |

---

📍 Lower Saxony, Germany · 🌐 [neural-werk.de](https://neural-werk.de)

