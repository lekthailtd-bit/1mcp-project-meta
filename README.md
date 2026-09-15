# 1MCP Project Meta

Durable, **public-safe** continuity metadata for Lek Thai's 1MCP work.

This repository is **not** the 1MCP source fork. Source code lives in:

- Lek Thai fork: `lekthailtd-bit/agent`
- Upstream: `1mcp-app/agent`

Start with `AGENTS.md`, then `CURRENT_STATE.md`.

## Purpose

Keep durable project context outside individual ChatGPT conversations without polluting the upstream-compatible source fork.

## Visibility / security boundary

GitHub currently reports this repository as **public**.

Therefore commit only sanitized project metadata here. Never commit credentials, tokens, private keys, cookies, browser/auth state, internal hostnames/IPs, private URLs, local infrastructure paths, customer/business-sensitive data, or raw secret-bearing logs.

If repository visibility is deliberately changed later, update the recorded decision first; do not silently relax the security boundary.

## Layout

- `AGENTS.md` — canonical instructions for agents working on this project.
- `CURRENT_STATE.md` — concise, current project state and next actions.
- `DECISIONS.md` — durable cross-chat decisions and supersessions.
- `chats/` — lightweight sanitized chat TL;DR and handover records.
- `upstream/` — notes for upstream issues, PRs, benchmarks, and contribution evidence.
- `deployment/` — sanitized release/deployment architecture and validation notes.
