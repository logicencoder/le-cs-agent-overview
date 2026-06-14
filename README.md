# LE CS Agent

**LE CS Agent** (Logic Encoder **Coding Station Agent**) is a self-hosted chat workstation for working with many LLM APIs from one dark browser UI. Pick a model, switch personas, stream replies, benchmark free providers, keep session history in DuckDB, and optionally hand the model a real shell in agent mode.

Private source: [logicencoder/le-cs-agent](https://github.com/logicencoder/le-cs-agent). API keys live in `.env` on your machine — never in this overview repo.

## The problem it solves

Free and paid model endpoints change weekly. Comparing OpenRouter, Hugging Face, Groq, Cerebras, Mistral, GitHub Models, NVIDIA, Google, and Pollinations from separate CLIs wastes time. LE CS Agent routes streaming chat, credit checks, model probes, and session export through one Node server and split-pane UI.

## Multi-provider chat

WebSocket streaming to multiple backends from a single conversation view. The sidebar lists models (with free-tier highlighting where applicable). **Personas** — engineer, hacker, normal, and custom entries stored in DuckDB — change system behavior and temperature without restarting the server.

## Model lab and provider health

REST endpoints list free providers and models, run **`/api/test-one`** probes against any configured backend, and record pass/fail latency in DuckDB **`model_scores`**. Use this when free model IDs rot and you need a quick answer on what still responds before wiring models into other Logic Encoder pipelines.

Credit and usage checks hit provider management APIs (`/api/credits`, `/api/status`) so you see balance and rate-limit headroom from the same desk.

## Session memory

Conversations persist in **`cas_memory.duckdb`** — messages per session, persona metadata, and export to Markdown via `/api/export`. Long debugging threads survive browser refresh without standing up an external database.

## Agent mode and embedded terminal

An optional terminal tab runs real bash through **node-pty** (WSL or Linux). In **agent mode** the model can emit `<shell>…</shell>` blocks; the server executes them and returns stdout/stderr into the chat loop — useful when a coding agent must verify filesystem state instead of guessing paths.

**Use agent mode only on trusted machines** — it is full shell access on the host running the server.

## Hybrid thinking stream

When a model returns separate reasoning tokens (R1-style), the UI shows a dedicated **thinking** stream before the final answer so you can judge chain-of-thought quality on hard prompts.

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
