# LE CS Agent

**LE CS Agent** is a self-hosted AI workstation for running and comparing many LLM providers from one interface. It combines model routing, persona control, persistent session memory, benchmarking utilities, and optional shell-backed agent mode for hands-on operator workflows.

Private source: [logicencoder/le-cs-agent](https://github.com/logicencoder/le-cs-agent). API keys live in `.env` on your machine — never in this overview repo.

## Tech stack

| Layer | Technologies |
|-------|--------------|
| Backend | Node.js server |
| UI | Single-page chat and tools interface |
| Providers | OpenRouter, Hugging Face router, Groq, Cerebras, Mistral, Google, NVIDIA, and others |
| Persistence | DuckDB session and benchmark storage |
| Agent runtime | `node-pty` terminal bridge for shell execution mode |

## Multi-provider chat desk

Streaming chat responses, model switching, and persona presets are handled in one desk. This lets operators move between reasoning-heavy and speed-focused models without rebuilding prompts in separate tools.

The provider abstraction is designed for real operator usage: when one API is degraded, the session can continue on another provider with minimal friction.

## Model lab and benchmark utilities

The model lab endpoints test candidates and store pass/fail plus latency metadata. This keeps a historical view of which model IDs are currently usable and which ones have become unstable.

That data is used to choose practical defaults for other Logic Encoder automation projects where free-tier and low-cost model quality changes frequently.

## Session memory and history

DuckDB-backed storage keeps session threads durable across browser reloads. Operators can revisit previous runs, compare outputs from different models, and export chats for follow-up implementation work.

## Agent mode with embedded terminal

LE CS Agent can run shell commands through a terminal bridge in agent mode. This supports workflows where model responses need verified command output or direct filesystem interaction instead of pure text suggestions.

Because this mode executes real commands, it is intended for trusted environments and operator-controlled machines.

## Hybrid thinking stream

For models that return reasoning content, the interface can display thinking output separately from final answers. This makes model behavior easier to evaluate during debugging, prompt tuning, and provider comparisons.

## Quick start

```bash
npm install
cp .env.example .env   # provider API keys
npm start              # default port 3010 — node server.js
```

Open the served **`index.html`** — chat, model picker, personas, terminal tab, and live server logs in one layout.

See [REPOS.md](REPOS.md) for repository links.

---

**Made by [Logic Encoder](https://logicencoder.com)** · [GitHub](https://github.com/logicencoder) · [Contact](https://logicencoder.com/contact/)
