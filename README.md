# LE CS Agent

**LE CS Agent** (LogicEncoder Coding Station Agent) is a self-hosted chat workstation for trying and operating many LLM APIs from one dark UI. Private source: [le-cs-agent](https://github.com/logicencoder/le-cs-agent).

Runs on **SOL** or a dev machine — **not** on the logicencoder.com WordPress host.

## What it is

**What:** A Node.js server plus a single-page web UI. You pick a model and persona, chat with streaming replies, inspect provider credits, benchmark free models, and optionally let the model run shell commands in agent mode.  
**Why:** Scattered free-tier APIs (OpenRouter, Hugging Face, Groq, Cerebras, etc.) are painful to compare manually; one desk consolidates routing, logging, and session memory.  
**Who:** The operator building or debugging LogicEncoder automation and tooling — not public site visitors.

## Multi-provider chat

**What:** Streaming completions over WebSocket; sidebar lists models from OpenRouter and other providers; personas change system behavior (engineer, hacker, normal, …).  
**Why:** Different tasks need different models and prompts; switching must be instant without new CLI tools.  
**Who:** Operator gets faster iteration; no direct end-user audience.

## Model lab

**What:** REST endpoints to list free providers, test a single model (`/api/test-one`), store pass/fail latency in DuckDB `model_scores`.  
**Why:** Free model lists rot weekly; automated probes show what still answers.  
**Who:** Operator avoids broken model IDs in downstream pipelines (e.g. karaoke Automate, other LE apps).

## Session memory

**What:** DuckDB file stores messages per session, export to Markdown, list sessions.  
**Why:** Long debugging threads must survive browser refresh without external DB ops.  
**Who:** Operator reviewing what was asked and which model replied.

## Agent mode + terminal

**What:** Optional tab with real bash (via `node-pty` on Linux/WSL). In agent mode the model emits `<shell>command</shell>` tags; the server runs them and feeds output back into the chat.  
**Why:** Coding agents need verified filesystem state, not guessed paths.  
**Who:** Operator automating local fixes; **security:** only run on trusted machines — full shell access.

## Hybrid thinking stream

**What:** When the model returns reasoning tokens, the UI shows a separate “thinking” stream before the final answer.  
**Why:** Reasoning models (R1-style) are unreadable if reasoning and answer are merged blindly.  
**Who:** Operator judging model quality on hard prompts.

## Related repositories

| Repo | Role |
|------|------|
| [le-cs-agent](https://github.com/logicencoder/le-cs-agent) | Private code |
| [le-cs-agent-overview](https://github.com/logicencoder/le-cs-agent-overview) | This page |

See [REPOS.md](REPOS.md).

## Licensing

© LogicEncoder. Third-party APIs subject to their terms.
