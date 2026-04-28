# Milestone 1 — Arch MVP (Execution Checklist)

## Goal

Deliver a rerunnable, idempotent-ish setup system for Arch-based systems with safe testing workflow.

## Scope

- Runner + shared lib
- Core Arch modules
- Profiles
- Basic test harness
- No Ubuntu/macOS implementation yet

## Deliverables

### 1) Runner

- [ ] Deterministic module execution (`runs/NN-name.sh` sorted)
- [ ] Flags: `--dry-run`, `--profile`, `--only`, `--skip`
- [ ] Clear summary: succeeded/failed/skipped modules
- [ ] Exit non-zero if any module fails

### 2) Shared library

- [ ] Logging helpers (info/warn/error)
- [ ] OS detection (at least Arch detection)
- [ ] `has_cmd` helper
- [ ] `run_cmd` wrapper honoring dry-run
- [ ] Safe file/symlink helper(s)

### 3) Module contract

- [ ] Every module uses `set -euo pipefail`
- [ ] Every module sources shared lib
- [ ] Every module has `main` function
- [ ] Every module is safe to rerun
- [ ] Every module has a simple verification command

### 4) Arch MVP modules

- [ ] 00-system-update
- [ ] 10-core-cli
- [ ] 20-shell
- [ ] 30-git
- [ ] 40-runtimes (split if needed)
- [ ] 50-dev-tools
- [ ] 90-dotfiles
- [ ] 99-manual

### 5) Profiles

- [ ] `minimal`
- [ ] `laptop`
- [ ] `full`

### 6) Testing

- [ ] ShellCheck + shfmt configured
- [ ] At least one smoke test path in disposable environment
- [ ] Idempotency test: run twice
- [ ] Drift test: remove one installed tool, rerun, confirm recovery

## Acceptance criteria

- [ ] Fresh Arch environment: full profile run succeeds
- [ ] Second immediate run succeeds
- [ ] Module failures are obvious and actionable
- [ ] Docs updated with final module list and usage examples
