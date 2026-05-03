# Tests

Smoke tests for the setup system. Run from project root.

## Docker smoke tests (fresh install + idempotency + drift)

```bash
# Arch Linux
./tests/docker-arch
```

```bash
# Ubuntu 24.04 (proxy for KDE Neon)
./tests/docker-ubuntu
```

## Modules Skipped in Docker Tests

| Module        | Why                            | How to test          |
|---------------|--------------------------------|----------------------|
| `30-git-ssh`  | Interactive prompts, SSH agent | Real machine         |
| `40-firewall` | Requires systemd + iptables    | VM or real machine   |
| `50-browser`  | GUI application                | Real machine         |
| `80-dotfiles` | Skipped by Docker smoke tests  | Real machine (or VM) |

`80-dotfiles` is currently skipped in Docker smoke tests (`tests/docker-arch` and
`tests/docker-ubuntu` via `--skip git-ssh,dotfiles,firewall,browser`).
`99-manual` remains safe in Docker because it only prints text.

## Testing dotfiles on the current machine

The dotfiles module is safe to run on an existing machine thanks to `ensure_symlink`'s
backup behavior. Preview with dry-run first:

```bash
./run --only dotfiles --dry-run # Preview what would happen
```

```bash
./run --only dotfiles # Apply (backs up existing files)
```

## Full bootstrap test

To test the complete fresh-machine flow, use a VM:

1. Create an Arch VM (GNOME Boxes or virt-manager)
2. Snapshot immediately after clean OS install
3. Follow [new-machine-setup.md](../docs/new-machine-setup.md) step by step
4. Revert to snapshot and repeat to validate

## macOS

No macOS Docker image exists (Apple licensing). Test on real hardware or a macOS CI runner
(e.g., GitHub Actions `macos-latest`).

## Design decisions

- The repo is mounted **read-only** (`:ro`) and copied inside the container, so tests never mutate the working tree
- Each test is self-contained — a single `docker run` per scenario
- Drift test removes `bat` specifically because it is a leaf package with no dependents
