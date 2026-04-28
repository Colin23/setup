# Vision

## Goal

Create a repo-driven machine setup system that can bootstrap and continuously reconcile my developer environment.

## Core Principles

- **Single Source of Truth**: This repository defines the desired machine state.
- **Re-runnable by design**: Running setup repeatedly converges the machine to desired state.
- **Modular**: One concern per module/script.
- **Observable**: Every run produces clear logs and a summary.
- **Testable**: Most changes are validated in disposable environments before real machines.
- **Pragmatic**: Bash-first, minimal framework overhead.

## Non-Goals (for now)

- Full config-management framework complexity.
- Perfect declarative model from day 1.
- Supporting every distro immediately.

## Initial Scope

- Primary target: Arch-based Linux.
- Secondary (later): Ubuntu-based Linux.
- Later: macOS support.
