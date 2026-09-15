# 1MCP Project Meta

Private continuity and operational metadata for Lek Thai's 1MCP work.

This repository is **not** the 1MCP source fork. Source code lives in:

- Lek Thai fork: `lekthailtd-bit/agent`
- Upstream: `1mcp-app/agent`

Start with `AGENTS.md`, then `CURRENT_STATE.md`.

## Purpose

Keep durable project context outside individual ChatGPT conversations without polluting the upstream-compatible source fork.

This repository may contain internal operational context, but must never contain credentials, tokens, private keys, session cookies, browser profiles, or other replayable secrets.

## Layout

- `AGENTS.md` — canonical instructions for agents working on this project.
- `CURRENT_STATE.md` — concise, current operational/project state and next actions.
- `DECISIONS.md` — durable cross-chat decisions and supersessions.
- `chats/` — lightweight chat TL;DR and handover records.
- `upstream/` — notes for upstream issues, PRs, benchmarks, and contribution evidence.
- `deployment/` — non-secret deployment/runtime architecture and validation notes.
