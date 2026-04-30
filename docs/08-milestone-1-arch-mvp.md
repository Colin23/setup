# Milestone 1 — Arch MVP (Execution Checklist)

## Goal

Deliver a rerunnable, idempotent setup system for Arch-based systems with safe testing workflow.

## Scope

- Runner + shared lib
- Core Arch modules
- Basic test harness
- No Ubuntu/macOS implementation yet (Ubuntu runs but some tools warn-only)

## Deliverables

### 1) Runner

- [x] Deterministic module execution (`runs/NN-name` sorted)
- [x] Flags: `--dry-run`, `--only`, `--skip`
- [x] Clear summary: succeeded/failed/skipped modules
- [x] Exit non-zero if any module fails

### 2) Shared library

- [x] Logging helpers (info/warn/error/success/step)
- [x] OS detection (arch, ubuntu, debian, macos)
- [x] `has_cmd` helper
- [x] `run_cmd` wrapper honoring dry-run
- [x] Safe file/symlink helper (`ensure_symlink` with backup)
- [x] Package helpers (`pkg_install`, `aur_install`, `flatpak_install`)
- [x] Module metadata helper (`module_header`)

### 3) Module contract

- [x] Every module uses `set -euo pipefail`
- [x] Every module sources shared lib
- [x] Every module is safe to rerun (idempotent)
- [x] Every module has verification step

### 4) Arch MVP modules

- [x] 00-system-update
- [x] 10-core-cli (bat, eza, fzf, zoxide, tree, cloc, htop, vim, fastfetch, wget)
- [x] 20-shell (zsh, starship, oh-my-zsh plugins, carapace)
- [x] 30-git-ssh (git, openssh, personal SSH key, GitHub connectivity test)
- [x] 40-firewall (ufw)
- [x] 50-browser (firefox)
- [x] 80-dotfiles (clone, dev dirs, symlinks, verification)
- [x] 90-dev-quality (shellcheck, shfmt)
- [x] 99-manual

### 5) Testing

- [x] ShellCheck + shfmt configured (`scripts/lint`)
- [x] Docker smoke test: Arch (fresh + idempotency + drift)
- [x] Docker smoke test: Ubuntu (fresh + idempotency + drift)
- [ ] Dotfiles test on current machine (`./run --only dotfiles`)
- [ ] Full bootstrap test on VM or spare machine (follow `docs/09-bootstrap-and-onboarding.md`)

### 6) Documentation

- [x] Bootstrap flow documented (`docs/09-bootstrap-and-onboarding.md`)
- [x] Dotfiles strategy documented (`docs/10-dotfiles-strategy.md`)
- [x] Tool inventory maintained (`docs/06-tool-inventory.md`)
- [ ] Consolidate docs to 3 files (post-M1)

## Acceptance criteria

- [x] Fresh Arch environment: Docker smoke test passes (package modules)
- [ ] Fresh Arch environment: full bootstrap including dotfiles (VM or real machine)
- [x] Second immediate run succeeds (Docker idempotency test)
- [x] Drift recovery works (Docker drift test)
- [x] Module failures are obvious and actionable (exit codes + log messages)

## Post-M1 backlog

- [ ] Consolidate docs to 3 files (architecture, new-machine-setup, contributing)
- [ ] CI pipeline (GitHub Actions: lint + Docker tests on push)
- [ ] Hardening: retry logic for network-dependent installs
- [ ] Hardening: module timing summary
- [ ] Module-level dry-run (preview individual actions inside each module)
