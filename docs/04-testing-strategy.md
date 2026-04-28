# docs/04-testing-strategy.md

# Testing Strategy

## Goals

- Validate changes without touching primary machine.
- Fast feedback loop during early development.
- Confidence that reruns are safe and convergent.

## Test Pyramid

### 1) Static/Unit-like (fastest)

- Shell linting (e.g., ShellCheck)
- Formatting checks (e.g., shfmt)
- Small helper function tests (Bats)

### 2) Integration (fast)

- Run selected modules in disposable containers where possible.
- Validate command presence and expected file outputs.

### 3) System Smoke (realistic)

- Use disposable VMs with snapshots:
    - Restore snapshot -> run setup -> verify -> rollback.
- Best for system-level packages, services, desktop-related behavior.

## Recommended Practical Setup (Arch-first)

- **Daily dev loop**: lint + targeted container smoke tests.
- **Pre-merge / milestone**: VM snapshot test on Arch.
- **Before real machine apply**: full VM run twice (verify idempotency).

## Why Not Reinstall OS Repeatedly?

Too slow. Prefer:

- VM snapshots (near-instant rollback)
- Disposable containers for non-systemd/non-desktop modules
- Optional spare laptop only for occasional end-to-end validation

## Idempotency Test Cases (must-have)

1. Fresh install run succeeds.
2. Immediate second run performs no harmful changes and still succeeds.
3. Simulated drift is corrected on rerun.
4. Partial failure can be rerun to recover.

## Acceptance Output

Each test run should produce:

- module pass/fail/skip summary
- timing
- key assertions (tool installed, version, config exists)
