# Upstream Issue #406 — Lazy Discovery Benchmark

Upstream: `1mcp-app/agent#406`

## Relevant maintainer feedback

The maintainer welcomed the real-world datapoint and requested:

- a fresh-chat before/after result;
- a sanitized discovery trace;
- benchmark coverage for zero-result recovery, glob semantics, and nested namespaces;
- measurement before changing defaults.

## Sanitized observed failure

The recurring failure shape was:

1. agent wants a downstream browser capability;
2. filtered discovery uses an overly narrow/exact pattern and returns zero;
3. agent confuses the outer gateway identifier with the inner/downstream `server` filter;
4. another valid zero result is misdiagnosed as an empty/broken gateway.

Correct recovery is broad discovery first:

`lt_tool_list({limit:20})`

Returned downstream server names are authoritative for subsequent `server` filtering.

For substring discovery, glob semantics require forms such as:

`*browser*`

## Before/after benchmark shape

**Before guidance:** filtered zero -> outer gateway reused as downstream server -> zero -> false unavailability diagnosis -> user correction required.

**After explicit guidance:** imperfect filtered zero may still occur -> agent broadens discovery itself -> downstream server recovered -> exact schema fetched -> task continues.

One successful exported retry executed with extended thinking, so it must not be represented as proof of a strict no-thinking/cheap path.

## Current local contribution direction

The Lek Thai fork is testing:

- `tool_instructions` as a fourth lazy metatool;
- explicit downstream-server namespace wording in metatool schemas/descriptions;
- explicit glob semantics and broad filtered-zero recovery;
- selective downstream server instructions;
- one authoritative lazy-meta-tool set reused across runtime surfaces.

Do not post private environment details or unsanitized traces upstream.
