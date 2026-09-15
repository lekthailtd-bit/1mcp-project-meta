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

The 0.38.0 delta from the pinned base does not currently overlap the lazy-patch source files, but do **not** silently rebase or merge upstream into the validated candidate. Finish/record the current candidate first, then decide explicitly whether a rebase/update is required.

The active source checkout contains validated-but-not-yet-committed strengthening work. Its current uncommitted diff SHA-256 remains:

`d6ace3ad1cffb0b32e9b2b5afb0faf330d68e290e6aa20aa3aa0363e584172de`

An unrelated untracked admin type-probe file is also present in the checkout. Do not accidentally include it in the feature commit.

## Feature goal

Improve lazy-mode discovery reliability for smaller/faster agents without abandoning the supported metatool architecture.

Current feature work includes:

- new `tool_instructions` lazy metatool;
- four lazy metatools: `tool_instructions`, `tool_list`, `tool_schema`, `tool_invoke`;
- explicit downstream-server versus outer-gateway semantics;
- explicit glob semantics and filtered-zero recovery guidance;
- reuse of one authoritative `LAZY_TOOLS` set across several secondary runtime surfaces;
- downstream server instructions surfaced selectively rather than dumping all instructions by default.

## Validation achieved on the strengthened candidate

- focused affected tests: 239/239 passed;
- full static/lint/type/build/SDK gate: passed;
- targeted lazy-loading E2E: 25/25 passed;
- browser-smoke E2E after installing its staging-only browser prerequisite: 23/23 passed;
- full unit regression: 362/362 files, 5,370/5,370 tests passed;
- full admin regression: 19/19 files, 201/201 tests passed;
- 2026-09-15 audit rerun: `MetaToolProvider` 53/53 passed;
- 2026-09-15 audit rerun: template-server MetaToolProvider suite 9/9 passed;
- `git diff --check`: passed.

The detached final full E2E run for the exact candidate identity was **interrupted**, not green:

- base HEAD: `f03d8aa239608696d3ada5747bf538a799d388ce`
- uncommitted diff SHA-256: `d6ace3ad1cffb0b32e9b2b5afb0faf330d68e290e6aa20aa3aa0363e584172de`
- persisted exit code: `129`
- log ended while tests were still passing and contained no Vitest completion summary or explicit test failure.

Treat that run as inconclusive and rerun the full E2E gate after the audit findings below are corrected.

## 2026-09-15 patch audit status

**Upstream-readiness verdict: REJECT until patch-local corrections are made.**

No source or production changes were made during this audit.

Must-fix findings:

1. `tool_instructions` derives visible servers and `toolCount` directly from the raw `ToolRegistry` instead of the existing `CapabilityCatalog` visibility/policy path. This can disagree with `tool_list`, count source-disabled tools, lose exact connection-key filtering, and aggregate tools from same-name template instances that are outside the caller's request visibility.
2. The server-instruction hook is wired through `InstructionAggregator.getServerInstructions(server)`, which is a public-name-level raw cache. The existing instruction subsystem already has `getEffectiveServerInstructions(outboundKey, serverName)` to preserve source-qualified/template-instance identity and configured instruction overrides. The new path therefore risks returning instructions from the wrong same-name template instance and bypasses configured overrides.

Hardening before upstream submission:

- define whether `tool_instructions` represents only servers with visible tools or the full visible downstream server namespace; current wording and implementation are not fully aligned for zero-tool servers;
- remove remaining drift points around the lazy-tool set, especially hard-coded `isMetaTool` membership and the missing `tool_instructions` assertion in that test block;
- avoid duplicate `tool_instructions` tool-definition metadata drifting between the internal factory and `MetaToolProvider`;
- add tests that reproduce same-name per-session template isolation, disabled-tool counts, configured instruction overrides, and disconnected visibility;
- replace the current “significantly reduced token count” assertion that only counts tools with an actual serialized/token-budget measurement;
- keep the unrelated untracked admin probe out of any feature commit.

## Production

Production remains deliberately separate on live 1MCP `0.32.2`.

Current source/fork validation must not be treated as a production deployment.

Do not switch production until the stock/fork release layout, switcher, smoke tests, rollback path, and selected target version are all verified and the user explicitly agrees to activation.

## Upstream interaction

Upstream issue #406 is the current benchmark/discovery discussion.

Maintainer feedback requested a fresh-chat before/after result and sanitized discovery trace covering zero-result recovery, glob semantics, and nested namespaces. A sanitized follow-up has been drafted but should be posted only after the current candidate's audit findings are corrected and validation state is accurately known.

See `upstream/issue-406.md`.

## Immediate next actions

1. Correct the two must-fix issues strictly within the lazy patch.
2. Add focused regression tests for visibility, per-session template isolation, and effective instruction overrides.
3. Tighten the smaller hardening items without unrelated 1MCP refactoring.
4. Rerun focused/static/unit/admin/targeted E2E as appropriate, then rerun the full E2E gate to a real completion summary.
5. Commit and push the strengthened 0.37.0-based candidate normally with hooks enabled.
6. Update this file with the resulting commit SHA and final validation state.
7. Post the sanitized #406 benchmark follow-up only after the candidate is genuinely green.
8. Compare the pinned candidate with current upstream 0.38.0 and decide explicitly whether to rebase/update before further release work.
9. Continue the planned dual-install release/switcher work without altering live production until its isolated validation is complete.
