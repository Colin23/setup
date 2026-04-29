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

## What can't be tested in Docker

| Module      | Why                            | Test method        |
|-------------|--------------------------------|--------------------|
| 30-git-ssh  | Interactive prompts, SSH agent | Real machine       |
| 40-firewall | Requires systemd + iptables    | VM or real machine |
| 50-browser  | GUI application                | Real machine       |
| 80-dotfiles | Requires private repo access   | Real machine       |
| 99-manual   | Print-only, no assertions      | Visual review      |

## macOS

No macOS Docker image exists (Apple licensing). Test on real hardware or a macOS CI runner (e.g., GitHub Actions
`macos-latest`).

**Key design decisions:**

- The repo is mounted **read-only** (`:ro`) and copied inside the container, so tests never mutate the working tree
- Each test is self-contained — a single `docker run` per scenario
- Drift test removes `bat` specifically because it is a leaf package with no dependents

---

## 2. The skipped modules

Looking at the five skipped modules, here's the reality:

| Module          | Docker-testable? | Why not                                                                |
|-----------------|------------------|------------------------------------------------------------------------|
| **30-git-ssh**  | Partially        | `read -rp` prompts block non-interactive; SSH agent needs running sshd |
| **40-firewall** | ❌                | Needs systemd + kernel iptables — Docker can't do either               |
| **50-browser**  | ❌                | GUI package, no display server in container                            |
| **80-dotfiles** | Partially        | Only if `DOTFILES_REPO` is set; currently exits 0 when unset           |
| **99-manual**   | ✅                | Pure print output, always safe                                         |

`80-dotfiles` and `99-manual` are already safe — `80-dotfiles` exits cleanly when `DOTFILES_REPO` is empty, and
`99-manual` just prints text. Both should be **removed from the skip list** in the Docker tests.

For `30-git-ssh`, we can test the non-interactive parts (package install + verify) by making the interactive prompts
skip gracefully when stdin isn't a terminal:
