# Architecture

## Vision

A repo-driven machine setup system that bootstraps and continuously reconciles a developer
environment. The repository is the single source of truth for desired machine state.
Running setup is safe, repeatable, and convergent: each run brings the machine closer to
the declared state, and a second run is a no-op.

Bash-first, minimal framework overhead, modular by concern, observable by default.

---

## Components

1. **Runner** (`run`) — orchestrates module execution, parses flags, prints summary.
2. **Module scripts** (`runs/NN-name`) — idempotent units (packages, shell, tools, dotfiles).
3. **Shared library** (`lib/common`) — logging, OS detection, command wrappers, package helpers, symlink helpers.
4. **Tests** (`tests/`) — Docker-based smoke tests for fresh install, idempotency, drift.
5. **Lint** (`scripts/lint`) — ShellCheck + shfmt across all shell scripts.

### Directory layout

```text
setup/
├── run main entrypoint
├── lib/common shared helper library
├── runs/NN-name modules, executed in numeric order
├── scripts/lint static analysis runner
├── tests/ Docker smoke tests
└── docs/ this directory
```

---

## Module Contract

Each module must:

- Use `set -euo pipefail`.
- Source `lib/common`.
- Be safe to run multiple times (idempotent).
- Exit non-zero on failure.
- Avoid destructive changes unless explicitly flagged.
- Honor `DRY_RUN` via `run_cmd`.
- Have a verification step that confirms the desired state.

Modules are discovered from `runs/`, sorted by filename prefix, and executed in order.
Each runs in a subshell so a `set -e` exit doesn't kill the runner.

---

## Reconciliation Model

Inspired by ArgoCD-style declarative reconciliation.

### Desired state

The repository expresses:

- Which tools must exist
- Which configs/symlinks must exist
- Which directories must exist
- Which services should be enabled (where applicable)

### Behavior on each run

1. Detect current state.
2. Compare against desired state encoded in modules.
3. Apply only missing or drifted pieces.
4. Re-verify and report convergence.

### Idempotency rules

- Package managers use idempotent flags (`--needed` for pacman, default behavior for apt).
- Installers are guarded by pre-checks (`has_cmd`, file existence).
- Symlinks managed by `ensure_symlink`, which backs up existing files before replacing.
- No blind append-to-config without guards.

### Drift handling

If the machine drifts (manual uninstall, config edit), the next run restores desired state.
For example, removing `bat` and rerunning will reinstall it. This is validated in the
Docker drift test.

### Safety modes

- `--dry-run` — preview which modules would execute, no changes made.
- `--only <a,b,c>` — substring-match modules to run only matching ones.
- `--skip <a,b,c>` — substring-match modules to skip.

---

## Two-Repo Strategy

This project uses a **public setup repo** and a **private dotfiles repo**.

| Repo                | Visibility | Contains                                                              | Cloned to                 |
|---------------------|------------|-----------------------------------------------------------------------|---------------------------|
| `setup` (this repo) | Public     | Runner, modules, lib, docs, tests                                     | User's preferred location |
| `dotfiles`          | Private    | Shell configs, git identity configs, SSH config, personal preferences | `~/.dotfiles`             |

### Relationship

- The **setup repo** is the authority on **machine state** — what's installed, where configs are placed.
- The **dotfiles repo** is the authority on **config content** — what those configs say.
- The `80-dotfiles` module is the **bridge**: it clones/pulls the dotfiles repo and symlinks files to their targets.

Together they define the complete desired state of the machine.

### Why not submodules or a monorepo?

| Approach           | Rejected because                                                                                           |
|--------------------|------------------------------------------------------------------------------------------------------------|
| **Submodule**      | Adds friction (detached HEAD, `--recursive` dance, pointer bumps). No benefit for a single-person project. |
| **Monorepo**       | Setup repo would need to be private. Loses HTTPS-first bootstrap (no SSH keys on fresh machine).           |
| **Separate clone** | ✅ Chosen. Simple, decoupled git histories, clean bootstrap flow, full reconciliation via `80-dotfiles`.    |

### Directory structure for git identity

`.gitconfig` uses `includeIf "gitdir:..."` to select a git identity based on which directory
a repo lives in. The `80-dotfiles` module creates the required `~/development/` tree and
symlinks per-directory identity configs into place. The exact paths and identity files live
in the dotfiles repo and the `80-dotfiles` module — they are not duplicated here to avoid
drift between docs and reality.

### Reconciliation order on each run

1. `20-shell` installs zsh, starship, oh-my-zsh, plugins (tools exist).
2. `30-git-ssh` ensures personal SSH key exists and GitHub auth works.
3. `80-dotfiles` clones/pulls dotfiles, creates directories, symlinks configs.

Tools are installed before configs are placed. On rerun, everything is a no-op if correct.
