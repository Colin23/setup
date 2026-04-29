# docs/01-architecture.md

# Architecture

## High-level Components

1. **Runner**: orchestrates module execution.
2. **Module scripts**: idempotent units (packages, shell, language runtimes, tools, dotfiles).
3. **Shared library**: common helper functions (logging, OS detection, idempotency helpers, command wrappers).
4. **Profiles**: named subsets of modules (minimal/work/full).
5. **State/log output**: run reports for traceability.

## Directory Layout (target)

- `bootstrap/` -> first-run onboarding scripts (SSH, identity, dotfiles)
- `run` -> main entrypoint
- `lib/` -> common functions
- `runs/` -> modules (`NN-name.sh`)
- `profiles/` -> module selection sets
- `docs/` -> design + plan
- `tests/` -> smoke/integration tests
- `artifacts/` -> run logs/reports (generated)

## Module Contract

Each module must:

- Be safe to run multiple times.
- Exit non-zero on failure.
- Avoid destructive changes unless explicitly flagged.
- Support dry-run mode through shared runner behavior.
- Declare metadata (name, supported OS families, tags, dependencies if needed).

## Execution Model

- Discover modules from `runs/`.
- Sort deterministically (prefix ordering).
- Filter by profile/only/skip flags.
- Execute with centralized logging and error handling.
- Produce end-of-run summary (success, failed, skipped).
