# Integration / Release Validation

This is a sanitized architecture note. Internal hostnames, IPs, filesystem paths, private URLs, and credentials do not belong in this public repository.

## Current separation

- Live production: 1MCP `0.32.2`
- Source fork target: 1MCP `0.37.0`
- A stock-upstream worktree/build and a Lek Thai fork worktree/build are kept independently comparable in the authorized integration environment.

Production must remain untouched while source/build/test work is being validated unless the user explicitly approves activation.

## Intended side-by-side release model

Keep two independently identifiable 0.37.0 builds:

- stock upstream at the pinned upstream commit;
- Lek Thai fork at the tested fork commit.

A future stable selector/switcher should:

1. validate the target build;
2. atomically select it;
3. restart only when activation is explicitly approved;
4. health-check the selected runtime;
5. roll back automatically on failed health;
6. preserve the known live 0.32.2 installation until the new path is proven.

Do not silently clean historical production flags/config as part of the instructions-metatool feature.

## Validation evidence rule

For every candidate release record:

- source commit;
- diff hash if dirty;
- build result;
- static/type/lint result;
- focused tests;
- unit/admin result;
- E2E result;
- runtime smoke result;
- production state before/after if activation eventually occurs.
