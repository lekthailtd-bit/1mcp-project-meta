# Current State

Last updated: 2026-09-15

This file is intentionally concise and should be **edited in place** as state changes rather than becoming an append-only diary.

## Repositories

- Meta/continuity: `lekthailtd-bit/1mcp-project-meta`
- Lek Thai source fork: `lekthailtd-bit/agent`
- Upstream: `1mcp-app/agent`

## Source baseline

Current patch candidate:

- pinned source version: 1MCP `0.37.0`
- pinned upstream base commit: `50af86018c6582f04de97212dec5072e54df6b46`
- current source feature branch: `feat/lazy-instructions-metatool`
- current remote feature HEAD: `f03d8aa239608696d3ada5747bf538a799d388ce`

During the 2026-09-15 meta-repository sanity review, upstream `main` was observed at package version `0.38.0`, commit `0971cc1d87b0b1e0155cd3bf2a355bc2ab3fe449`.

Therefore the active candidate is intentionally behind current upstream. Do **not** silently rebase or merge upstream into the validated candidate. Finish/record the current candidate first, then inspect upstream changes and decide explicitly whether a rebase/update is required.

The active source checkout also contains validated-but-not-yet-committed strengthening work. Inspect source Git before relying on this summary.

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
- browser-smoke E2E after installing its staging-only browser prerequisite: 23/23 passed;
- full unit regression: 362/362 files, 5,370/5,370 tests passed;
- full admin regression: 19/19 files, 201/201 tests passed.

A final full E2E rerun has been launched detached on the authorized integration environment. Do not claim that final full E2E gate is green until its persisted exit/result is read.

At detached-run launch, the tested source identity was:

- base HEAD: `f03d8aa239608696d3ada5747bf538a799d388ce`
- uncommitted diff SHA-256: `d6ace3ad1cffb0b32e9b2b5afb0faf330d68e290e6aa20aa3aa0363e584172de`

## Production

Production remains deliberately separate on live 1MCP `0.32.2`.

Current source/fork validation must not be treated as a production deployment.

Do not switch production until the stock/fork release layout, switcher, smoke tests, rollback path, and selected target version are all verified and the user explicitly agrees to activation.

## Upstream interaction

Upstream issue #406 is the current benchmark/discovery discussion.

Maintainer feedback requested a fresh-chat before/after result and sanitized discovery trace covering zero-result recovery, glob semantics, and nested namespaces. A sanitized follow-up has been drafted but should be posted only after the current candidate's validation state is accurately known.

See `upstream/issue-406.md`.

## Immediate next actions

1. Read the detached full E2E result and record the final outcome.
2. If green, inspect the source diff, commit and push the strengthened 0.37.0-based candidate normally with hooks enabled.
3. Update this file with the resulting commit SHA and final validation state.
4. Post the sanitized #406 benchmark follow-up.
5. Compare the pinned candidate with current upstream 0.38.0 and decide explicitly whether to rebase/update before further release work.
6. Continue the planned dual-install release/switcher work without altering live production until its isolated validation is complete.
