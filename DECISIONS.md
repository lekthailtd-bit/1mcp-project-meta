# Decisions

Durable cross-chat decisions for the Lek Thai 1MCP project.

## D001 — Separate source and project metadata

**Decision:** Keep 1MCP source changes in `lekthailtd-bit/agent` and sanitized project continuity/operational metadata in `lekthailtd-bit/1mcp-project-meta`.

**Reason:** Preserve an upstream-compatible source fork while giving ChatGPT/Codex a separate continuity surface.

**Status:** Active

## D002 — Meta continuity lives on the default branch

**Decision:** Maintain ordinary project continuity records on the meta repository's default `main` branch. Do not use Git tags for evolving chat/state data and do not create a permanent parallel metadata branch merely to store chat history.

**Reason:** Tags are immutable snapshots and a second long-lived metadata branch would create another reconciliation problem. The meta repository already provides the required separation from the source fork.

**Status:** Active

## D003 — Git outranks conversational memory

**Decision:** ChatGPT Project memory and prior conversations are convenience context only. Current Git state and the authority split documented in `AGENTS.md` are canonical.

**Reason:** Individual chats can terminate, truncate, misremember tool semantics, or contain superseded state.

**Status:** Active

## D004 — 1MCP nested namespace semantics

**Decision:** `lt` is the outer Lekthai2 gateway identifier. The `server` argument of lazy 1MCP metatools is a downstream server name returned by discovery. Never infer `server:"lt"` from the outer gateway name.

Unknown discovery starts broad with `lt_tool_list({limit:20})`. Filtered zero results must be broadened before diagnosing gateway failure. Glob substring matching uses `*term*`.

**Reason:** The same incorrect outer-gateway/downstream-server inference reproduced across multiple model/reasoning configurations and caused false terminal diagnoses.

**Status:** Active

## D005 — Supported lazy architecture, not local hybrid patching

**Decision:** Work with upstream's supported lazy metatool model. Do not invent a local hybrid package patch around deprecated/ignored lazy selectors.

**Reason:** Upstream already removed/ignored historical hybrid/direct-expose selectors; the maintainable opportunity is discovery/instruction quality.

**Status:** Active

## D006 — Side-by-side release safety

**Decision:** Preserve the existing live 1MCP `0.32.2` installation as a fallback while building and validating side-by-side stock-upstream and Lek Thai-fork `0.37.0` releases. Production activation is a separate explicit step.

**Reason:** Allows clean A/B comparison and rollback without overwriting the known live installation.

**Status:** Active

## D007 — Exact validation identity

**Decision:** A validation claim must identify the exact source commit. If the tested tree contains uncommitted changes, also record a deterministic diff hash.

**Reason:** Prevents a later commit/reformat/fix from being incorrectly treated as the tree that actually passed.

**Status:** Active

## D008 — Keep ChatGPT Project bootstrap tiny

**Decision:** ChatGPT Project-level instructions should be no more than 10 lines and should point agents to this repository's `AGENTS.md` and `CURRENT_STATE.md` rather than duplicating operational rules.

**Reason:** Avoid instruction drift and save context in every fresh conversation.

**Status:** Active

## D009 — Public-safe meta repository

**Decision:** Treat `lekthailtd-bit/1mcp-project-meta` as public-safe unless a later explicit decision records a deliberate visibility change. Do not commit internal hostnames/IPs, private URLs, infrastructure paths, raw chat exports, credentials/auth state, or customer/business-sensitive data.

**Reason:** The repository was discovered during initial sanity review to be publicly visible. Continuity must not depend on sensitive internal details.

**Status:** Active
