# LE CS Agent — Logic Encoder Coding Station

**A self-hosted AI workstation that combines multi-provider LLM chat, model benchmarking, DuckDB session memory, editable personas, and an embedded terminal where the model can run real shell commands — built for daily coding on your own machine.**

**LE CS Agent** (Logic Encoder Coding Station Agent) is a single-page tool that routes chat across **OpenRouter free models**, **Hugging Face Inference Router**, and eight additional providers in the model lab. Stream responses, switch personas, attach files and screenshots, benchmark which models are online today, and — in the Terminal tab — let the model execute WSL/bash commands via `<shell>…</shell>` agent mode with live output fed back into the conversation.

Built for **operators who want one desk** instead of separate browser tabs per provider. API keys live in `.env` on your machine; this overview describes product behaviour only.

**Made by [Logic Encoder](https://logicencoder.com)**

Private source: [logicencoder/le-cs-agent](https://github.com/logicencoder/le-cs-agent)

---

## What you can do

| Area | In plain language |
|------|-------------------|
| **Multi-provider chat** | Stream from OpenRouter `:free` models or HF router; pinned Perplexity Sonar for web search |
| **Thinking mode** | Chain-of-thought prompts plus native reasoning tokens for supported models |
| **Vision & files** | Attach text files as context; paste or upload screenshots for vision models |
| **Agent shell mode** | Model wraps commands in `<shell>` tags; output runs in xterm and returns to chat history |
| **Embedded terminal** | Full WSL/bash xterm with distro picker, Start/Kill, resizable split with AI sub-chat |
| **Model lab** | Test OpenRouter, HF, Groq, Cerebras, Mistral, GitHub Models, NVIDIA NIM, Google, Pollinations |
| **Session memory** | DuckDB stores threads; switch conversations, export Markdown, clear per session |
| **Personas** | Eight seeded presets plus custom CRUD — engineer, auditor, teacher, and more |
| **Live logs** | WebSocket/API log viewer with level filter for debugging provider routing |
| **Key monitoring** | OpenRouter usage, limits, and balance chips in the header |

Chat streams over **WebSocket**; the terminal uses the same connection for PTY I/O.

---

## Feature examples (two per capability)

#### Multi-provider free chat
1. You pick `qwen/qwen3-coder:free` from the sidebar and ask for a Python refactor; tokens stream into the message pane with latency stats.
2. You add an HF model like `Qwen/Qwen3-Coder-30B-A3B-Instruct` to the dropdown; chat routes to the Hugging Face router instead of OpenRouter.

#### Web search (Sonar)
1. You select pinned **Sonar (web search)** and ask "What changed in Node 22 LTS?" — the UI shows a "Searching web…" badge.
2. The server logs the search path and streams an answer from Perplexity Sonar free tier without a separate search tool.

#### Hybrid thinking / reasoning
1. You enable Thinking Mode with a Qwen3 model; native `reasoning` delta tokens appear in a collapsible **REASONING** block.
2. On a non-native model with Thinking ON, the model uses `<think>…</think>` tags parsed inline in the UI.

#### Vision and screenshots
1. You attach a PNG via the toolbar and send "What error is in this screenshot?" to a vision-capable model.
2. You Ctrl+V paste a screenshot from the clipboard; image preview appears and base64 is sent with the WebSocket payload.

#### File context injection
1. You attach `server.js` as a text file and ask "Where is rate-limit fallback handled?"
2. File content is wrapped in `<file_context>` and prepended to your message server-side before the model sees it.

#### Agent shell execution
1. In the Terminal tab you ask "List files in this directory"; the model returns `<shell>ls -la</shell>`; command runs in WSL xterm and output appears in chat.
2. Multiple `<shell>` blocks run sequentially; each gets a collapsible output block with live polling during execution.

#### Embedded WSL terminal
1. You start the terminal, pick an Ubuntu distro from the distro list, and interact manually in xterm.
2. You hit **Kill**; the PTY process stops server-side and you start fresh for a clean environment.

#### Terminal split and AI sub-chat
1. You drag the divider to resize the terminal vs the bottom chat pane during a long compile.
2. You send a message in the term-chat area; it uses the active sidebar model in a separate `term-chat-{session}` DuckDB session.

#### Session memory and conversations
1. You reload the page; WebSocket reconnect loads the last 40 messages for the current session from DuckDB.
2. You click an older conversation in the sidebar; `loadSession` restores the full thread.

#### New chat without data loss
1. You click **+ New chat**; fresh session ID, empty UI; the prior session still appears in Conversations.
2. You export the old session as Markdown before starting a new chat for a client handoff.

#### Personas
1. You select **Security Auditor** persona; a code review returns severity-ranked findings in the expected format.
2. You create custom persona "Rust Mentor" in the Personas tab; it appears in the sidebar dropdown immediately.

#### Model lab — OpenRouter sweep
1. You open Test Models, run all tests on loaded OR free models; the grid shows OK/FAIL plus latency.
2. You click a green **OK** row to jump that model into the Chat tab for immediate use.

#### Model lab — FREE provider catalog
1. You click **Load FREE Providers**; Groq, Cerebras, Google, and others appear in the grid (skips providers without keys).
2. The header chip updates `FREE: X/Y` showing how many providers have keys configured in `.env`.

#### HF benchmarking
1. You run the HF benchmark; `/api/benchmark-hf` tests all HF models and saves scores to DuckDB.
2. After benchmark, the header **HF:** chip reflects the updated model count from `/api/hf-models`.

#### Rate-limit resilience
1. You hit 429 on a popular free model during chat; the server marks it failed and retries with a fallback model.
2. The UI shows a retry badge on the model tag and a warning bubble "rate limited — retrying with …".

#### Live operational logs
1. You switch to the Logs tab and see WebSocket connect, `[SEND]`, `[→ OR]`, and `[FINAL]` lines streamed from the server.
2. You filter logs to Errors only while debugging a failed model test in the lab grid.

#### Code block workflow
1. The model returns fenced code; the UI renders each block with **Copy** and **Save** actions.
2. You click **Save ZIP** to bundle all code blocks from the current chat into one download.

#### Account and key monitoring
1. The sidebar shows OpenRouter key usage and limit from the status endpoint after each chat.
2. The balance chip loads from the credits endpoint (management key) and updates when a turn completes.

#### Markdown export
1. You click **Export MD** in the chat stats bar; the browser downloads `chat-{session}.md`.
2. The exported file includes per-message model tags and timestamps from DuckDB.

#### REST operate endpoint
1. An external tool POSTs to `/api/operate` with `prompt` plus `screenshotBase64`; the server tries fallback free models.
2. You pass `manualModel` to force a specific model for a one-shot non-streaming answer in automation.

#### Header status chips
1. OpenRouter balance turns red — you switch to a `:free` model before starting a long refactor session.
2. WebSocket indicator drops; you open Logs to confirm reconnect without losing DuckDB history.

#### Provider keys in model lab
1. You add `GROQ_KEY` to `.env`, restart, load FREE providers — Groq models appear in the test grid.
2. A provider without a key is skipped silently; the FREE chip shows configured vs total providers.

---

## What it does not do

- **Not** a cloud SaaS — runs on your workstation; you supply API keys
- **Not** full round-robin chat across all nine providers — streaming chat uses OpenRouter and HF router; the lab exercises the rest
- **Not** sandboxed agent execution — shell mode runs real commands; use on trusted machines only
- **Not** a replacement for a full IDE — complements your editor with chat, terminal, and model comparison

API keys and session database stay on your machine — not published in this overview repo.

---

## Tech stack

| Layer | Technologies |
|-------|----------------|
| Runtime | Node.js |
| HTTP server | Fastify 5 |
| Real-time | `ws` WebSocket on the same port |
| Database | DuckDB (`cas_memory.duckdb` — sessions, personas, model scores) |
| Terminal | `node-pty` + xterm.js + Fit addon |
| Frontend | Vanilla HTML/CSS/JS — no build step |
| Utilities | JSZip for code-block ZIP export |
| Providers | OpenRouter, Hugging Face Router, Groq, Cerebras, Mistral, GitHub Models, NVIDIA NIM, Google AI Studio, Pollinations |
| Quality | Playwright scripts for agent-mode verification (dev) |

---

## Quick start

```bash
npm install
cp .env.example .env   # provider API keys (OPENROUTER_KEY required)
npm start              # default port 3010 — node server.js
```

Open the served **index.html** — chat, model picker, personas, terminal tab, and live server logs in one layout.

See [REPOS.md](REPOS.md) for repository links.

---

## Related repositories

| Repository | Role |
|------------|------|
| [le-cs-agent](https://github.com/logicencoder/le-cs-agent) | Private application code |
| [le-cs-agent-overview](https://github.com/logicencoder/le-cs-agent-overview) | This product overview |

See [REPOS.md](REPOS.md).

---

**Made by [Logic Encoder](https://logicencoder.com)** · [GitHub](https://github.com/logicencoder) · [Contact](https://logicencoder.com/contact/)
