# Swarm Molecule Type Validation - Findings

## Problem
`bd swarm create` fails with: *“invalid issue type: molecule”*

Even after configuring `types.custom = molecule`, the command still rejects molecule type.

## Evidence
- Issue #1402 in `steveyegge/beads` already tracks this bug:  
  > "bd import batch validation rejects molecule/agent types that bd create accepts"  
  > Created: 2026-01-30 · Status: OPEN
- `bd types` output does not list `molecule` even with `types.custom = molecule`
- Issue #1402 is still open after ~3 weeks

## Root Cause
The BD codebase expects `molecule` to be a **native issue type** in the core type system, not a custom type added via configuration.

## Temporary Fix (Workaround)
Local configuration works for other BD commands:
```bash
bd config set types.custom "molecule"
```
But `bd swarm create` validation doesn't check custom types - it validates against core types only.

## Recommended Fix
Add `molecule` as a **native issue type** in BD's type system so:
- `bd swarm create` validates correctly
- `bd mol` commands work as intended
- No configuration workaround needed

## Test Evidence
- `bd mol list` shows templates after `types.custom = molecule`
- `bd mol pour` creates issues successfully
- `bd swarm create` still fails with validation error

## Notes
- BD v0.49.1
- Tested on 2026-02-17
- Configuration workaround: `bd config set types.custom "molecule"` allows local work but doesn't fix upstream validation
