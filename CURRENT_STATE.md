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

## Full E2E diagnosis

The detached full E2E run for the exact candidate identity was **interrupted and also encountered one upstream-baseline test failure**. These are two separate issues:

- base HEAD: `f03d8aa239608696d3ada5747bf538a799d388ce`
- uncommitted diff SHA-256: `d6ace3ad1cffb0b32e9b2b5afb0faf330d68e290e6aa20aa3aa0363e584172de`
- persisted process exit code: `129`

### Runner termination

The `129` was caused by SSH/PTTY hangup, not a Vitest completion result.

Evidence:

- the long run ended at approximately the SSH session's 30-minute idle-cleanup boundary;
- a controlled reproduction using the same `nohup -> shell wrapper -> Node child` shape produced `exit_code=129` and an explicit `Hangup` when the SSH session was closed;
- a control using `setsid` survived the same SSH-session close and completed with exit code `0`.

Future long-running validation launched through SSH must use a truly session-detached launcher such as `setsid`, with persisted log/status files. `nohup` alone is insufficient in this environment.

### Upstream-baseline E2E failure

Before the hangup, `test/e2e/commands/error-scenarios.test.ts` failed:

`Resource Exhaustion Scenarios > should handle rapid repeated operations`

This is **not introduced by the lazy patch**:

- the test file is byte-identical between the feature branch and current upstream;
- untouched stock upstream 0.37.0 at `50af86018c6582f04de97212dec5072e54df6b46` reproduces the same failure with `0/20` successful operations;
- the test launches 20 CLI status processes concurrently, and the shared test runner enforces a 10-second timeout per process;
- on the current 2-logical-CPU integration runner, an explicit 10-second parallel reproduction timed out all 20 operations;
- the same 20 parallel status operations all succeed when allowed to finish without that 10-second cutoff.

Classify this as an environment-sensitive upstream-baseline E2E failure, not evidence of a lazy-patch regression. Do not weaken or modify the unrelated upstream test as part of this patch merely to make the local gate green.

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
4. Rerun focused/static/unit/admin/targeted E2E as appropriate.
5. Rerun full E2E with a verified `setsid`-style detached launcher and compare any remaining failures against the untouched stock baseline; do not require an unrelated environment-sensitive upstream test to become green through patch-local changes.
6. Commit and push the strengthened 0.37.0-based candidate normally with hooks enabled.
7. Update this file with the resulting commit SHA and final validation state.
8. Post the sanitized #406 benchmark follow-up only after the candidate is genuinely green on patch-relevant validation.
9. Compare the pinned candidate with current upstream 0.38.0 and decide explicitly whether to rebase/update before further release work.
10. Continue the planned dual-install release/switcher work without altering live production until its isolated validation is complete.
