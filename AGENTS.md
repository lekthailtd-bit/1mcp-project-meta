# AGENTS.md — Lek Thai 1MCP Project Meta

These instructions govern this repository and continuity work for the Lek Thai 1MCP project.

## 1. Mandatory startup order

Before substantive 1MCP project work:

1. Read this file in full.
2. Read `CURRENT_STATE.md`.
3. Read `DECISIONS.md`.
4. Inspect the current Git state of the source repository `lekthailtd-bit/agent` before changing source. Do not assume `CURRENT_STATE.md` or chat memory is newer than Git.
5. Read only the relevant records under `chats/`, `upstream/`, and `deployment/`.

When sources disagree, stop and identify the discrepancy. Current source Git state outranks remembered conversational state for implementation facts.

## 2. Repository roles and authority

- `lekthailtd-bit/agent` — canonical Lek Thai source-code fork of `1mcp-app/agent`.
- `1mcp-app/agent` — canonical upstream project for current upstream behaviour and accepted design.
- `lekthailtd-bit/1mcp-project-meta` — canonical **sanitized** Lek Thai continuity, decisions, upstream-interaction notes, release/deployment state, and handoffs.
- ChatGPT Project memory and individual conversations are convenience context only; they never outrank Git.

Do not copy 1MCP source code into this meta repository. Do not put Lek Thai continuity/operational chat records into the upstream-compatible source fork unless they are genuinely part of an upstream-quality source change.

## 3. Lekthai2 / lazy 1MCP namespace contract

The outer Lekthai2 upstream/gateway identifier is `lt`.

Inside the 1MCP lazy metatools, `server` means a **downstream MCP server** returned by discovery, such as `playwright`, `github`, `sambapos`, `justeat`, or `ssh-session-mcp`.

Never infer `server:"lt"` merely because the outer gateway is named `lt`.

When the downstream server/tool is unknown, begin with broad discovery:

`lt_tool_list({limit:20})`

A filtered zero result does not prove the gateway is empty or unavailable. Remove/broaden filters and retry broad discovery first.

`pattern` uses glob semantics. Use `*term*` for substring matching, for example `*browser*`.

Fetch the current `tool_schema` before first invocation when the schema is not already known.

## 4. Source and upstream work

Before modifying `lekthailtd-bit/agent`:

- inspect branch, status, diff, and recent commits;
- research current upstream behaviour/issues/merged changes when materially relevant;
- prefer small upstream-quality changes over local package patches;
- preserve stock-upstream versus Lek Thai fork comparability;
- do not overwrite or patch the existing production installation as a shortcut.

Record material source branch/commit/test state in `CURRENT_STATE.md` after it changes.

## 5. Production safety

Production changes require explicit user agreement.

Unless deployment is explicitly in scope:

- do not change the live service definition;
- do not change production 1MCP configuration or arguments;
- do not replace the live binary;
- do not restart production merely to test source changes.

Staging/build/test activity must remain separable from the live installation.

## 6. Validation standard

Before calling a source change ready:

- focused tests for affected behaviour pass;
- static/lint/type/build gates pass;
- appropriate integration/E2E coverage passes;
- record the exact source commit and, when testing uncommitted work, a deterministic diff hash;
- distinguish code failures from environment/test-prerequisite failures;
- update `CURRENT_STATE.md` with the verified result.

Do not claim a test suite passed until its final exit/result is observed.

## 7. Chat continuity

Keep chat records lightweight and sanitized.

Use `chats/YYYY-MM-DD-short-topic/` only for substantive work that benefits from durable continuity. Prefer:

- `tldr.md` — current/final state, important findings, source branch/commits, validation, blockers, next action;
- `handover.md` — only when a successor needs explicit continuation instructions.

Do not reproduce full conversation transcripts here. Keep `CURRENT_STATE.md` concise and current so a fresh agent normally needs one state file, not a chain of old chats.

## 8. Decisions

Put durable cross-chat decisions in `DECISIONS.md`.

- Add a decision only when it changes how future agents should work or interpret project state.
- Mark decisions superseded rather than silently deleting historical rationale.
- Experiments and transient test output belong in chat/upstream/deployment notes, not the decision ledger.

## 9. Public-repository security boundary

Until an explicit decision records otherwise, treat this repository as public even if a client UI appears to imply otherwise.

Never commit:

- replayable secrets, tokens, cookies, passwords, private keys, credentials, or raw auth state;
- browser profiles or credential stores;
- internal hostnames/IPs, private URLs, local infrastructure paths, or secret-bearing logs;
- customer/order/personally identifying business data;
- raw conversation exports.

Use sanitized abstractions such as “integration host” or “live service” where operational context is needed.

## 10. Completion discipline

At the end of substantive work:

1. source changes belong in `lekthailtd-bit/agent`, not here;
2. update `CURRENT_STATE.md` to remove stale state and record the new verified state;
3. update `DECISIONS.md` only if a durable decision changed;
4. add/update a lightweight sanitized chat or upstream/deployment note when it materially improves continuity;
5. leave one explicit next action or state that no action remains.

The standard is: **Git is durable truth; chats are disposable working sessions.**
