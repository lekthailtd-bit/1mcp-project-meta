# Current State

Last updated: 2026-09-15

This file is intentionally concise and should be **edited in place** as state changes rather than becoming an append-only diary.

## Repositories

- Meta/continuity: `lekthailtd-bit/1mcp-project-meta`
- Lek Thai source fork: `lekthailtd-bit/agent`
- Upstream: `1mcp-app/agent`

## Source baseline

- Upstream target version: 1MCP `0.37.0`
- Upstream base commit used for current fork work: `50af86018c6582f04de97212dec5072e54df6b46`
- Current source feature branch: `feat/lazy-instructions-metatool`
- Current remote feature HEAD: `f03d8aa239608696d3ada5747bf538a799d388ce`
- Integration checkout: `/opt/1mcp/src/agent`
- Stock-upstream comparison worktree: `/opt/1mcp/src/upstream`

The current integration checkout also contains validated-but-not-yet-committed strengthening work. Inspect Git before relying on this summary.

## Feature goal

Improve lazy-mode discovery reliability for smaller/faster agents without abandoning the supported metatool architecture.

Current feature work includes:

- new `tool_instructions` lazy metatool;
- four lazy metatools: `tool_instructions`, `tool_list`, `tool_schema`, `tool_invoke`;
- explicit downstream-server versus outer-gateway semantics;
- explicit glob semantics and filtered-zero recovery guidance;
- reuse of one authoritative `LAZY_TOOLS` set across secondary runtime surfaces;
- downstream server instructions surfaced selectively rather than dumping all instructions by default.

## Validation achieved on the strengthened candidate

- focused affected tests: 239/239 passed;
- full static/lint/type/build/SDK gate: passed;
- targeted lazy-loading E2E: 25/25 passed;
- browser-smoke E2E after staging Chromium install: 23/23 passed;
- full unit regression: 362/362 files, 5,370/5,370 tests passed;
- full admin regression: 19/19 files, 201/201 tests passed.

A final full E2E rerun is currently detached with output/status persisted under `/tmp/1mcp-e2e-latest.*`. Do not claim that final full E2E gate is green until its saved exit/result is read.

At detached-run launch, the tested source state was:

- base HEAD: `f03d8aa239608696d3ada5747bf538a799d388ce`
- uncommitted diff SHA-256: `d6ace3ad1cffb0b32e9b2b5afb0faf330d68e290e6aa20aa3aa0363e584172de`

## Production

Production remains deliberately separate:

- live 1MCP version: `0.32.2`
- service: `business-mcp.service`
- current source/fork validation must not be treated as a production deployment.

Do not switch production until the stock 0.37.0 build, Lek Thai fork build, release layout, switcher, smoke tests, and rollback path are all verified and the user explicitly agrees to activation.

## Upstream interaction

Upstream issue #406 is the current benchmark/discovery discussion.

Maintainer feedback requested a fresh-chat before/after result and sanitized discovery trace covering zero-result recovery, glob semantics, and nested namespaces. A sanitized follow-up has been drafted but should be posted only after the current candidate's validation state is accurately known.

See `upstream/issue-406.md`.

## Immediate next actions

1. Read the detached full E2E status/log and record the final result.
2. If green, inspect the source diff, commit and push the strengthened candidate normally with hooks enabled.
3. Update this file with the resulting commit SHA and final validation state.
4. Post the sanitized #406 benchmark follow-up.
5. Continue the planned dual-install release/switcher work without altering live production until its isolated validation is complete.
