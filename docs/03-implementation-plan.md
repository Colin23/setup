# Implementation Plan

## Objective

Build a rerunnable machine setup reconciler where this repository is the single source of truth for developer
environment state.

---

## Phase 0 — Documentation Baseline (done/ongoing)

- Finalize architecture and operating model docs.
- Finalize tool inventory (full list + prioritization).
- Define module naming and contract.
- Define Arch MVP scope.

---

## Phase 1 — Bootstrap & Identity (new machine entrypoint)

## Why this phase exists

A fresh machine often has:

- no SSH keys
- no trusted host setup
- no private repo access

This phase guarantees a reliable onboarding path.

## Deliverables

- `bootstrap/` onboarding script(s) and docs
- HTTPS-first clone flow for public setup repo
- SSH key generation if missing
- guided pause to register key in Git provider
- SSH connectivity check
- optional remote URL switch HTTPS -> SSH
- optional private dotfiles repo clone step

## Acceptance

- Fresh machine can start with only minimal manual commands and complete bootstrap safely.

---

## Phase 2 — Runner + Shared Library

- Implement robust runner:
    - deterministic order (`runs/NN-name`)
    - `--dry-run`, `--profile`, `--only`, `--skip`
    - module summary output
    - non-zero exit on failures
- Implement shared helpers:
    - logging
    - OS detection
    - command checks
    - dry-run command wrapper
    - safe file/symlink helpers

## Acceptance

- Runner executes selected modules predictably and reports actionable results.

---

## Phase 3 — Arch MVP Modules

Implement prioritized modules from inventory (Milestone 1 list), e.g.:

- system update + base tools
- shell tools
- git/ssh tooling
- core runtimes
- dotfiles integration
- manual steps module

## Acceptance

- Fresh Arch run succeeds and provisions core environment.

---

## Phase 4 — Test Harness & Safety

- lint/format checks (ShellCheck, shfmt)
- basic bats tests for helper behavior
- smoke tests in disposable environment
- idempotency test: run twice
- drift correction test (remove tool, rerun)

## Acceptance

- Changes can be validated without touching primary laptop.

---

## Phase 5 — Hardening

- better retry behavior for network installers
- structured logs/artifacts
- module timing summary
- optional strict/plan modes
- optional lock/state metadata

---

## Phase 6 — Ubuntu Support

- package abstraction/adapters
- OS guards per module
- ubuntu smoke tests

---

## Phase 7 — macOS Support

- brew-based adapters
- macOS-specific modules
- onboarding and verification parity

---

## Implementation Strategy Rules

1. Inventory first, implementation second.
2. One module at a time; test immediately.
3. Keep modules rerunnable and side-effect aware.
4. Prefer package manager installs over curl scripts.
5. Keep manual tasks explicit and isolated.
