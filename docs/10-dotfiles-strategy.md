# Dotfiles Strategy

## Goal

Define how personal configuration files (dotfiles) are managed alongside the setup system,
ensuring a clean separation of concerns while maintaining a seamless reconciliation loop.

---

## Two-Repo Architecture

| Repo                | Visibility | Contains                                                              | Cloned to                 |
|---------------------|------------|-----------------------------------------------------------------------|---------------------------|
| `setup` (this repo) | Public     | Runner, modules, lib, docs, tests                                     | User's preferred location |
| `dotfiles`          | Private    | Shell configs, git identity configs, SSH config, personal preferences | `~/.dotfiles`             |

### Relationship

- The **setup repo** is the authority on **machine state** — what's installed, where configs are placed.
- The **dotfiles repo** is the authority on **config content** — what those configs say.
- The `80-dotfiles` module is the **bridge**: it clones/pulls the dotfiles repo and symlinks files to their targets.

Together they define the complete desired state of the machine.

---

## Why Not Submodules or a Monorepo?

| Approach           | Rejected because                                                                                                |
|--------------------|-----------------------------------------------------------------------------------------------------------------|
| **Submodule**      | Adds friction (detached HEAD, `--recursive` dance, pointer bumps). No real benefit for a single-person project. |
| **Monorepo**       | Setup repo would need to be private. Loses HTTPS-first bootstrap (no SSH keys on fresh machine).                |
| **Separate clone** | ✅ Chosen. Simple, decoupled git histories, clean bootstrap flow, full reconciliation via `80-dotfiles`.         |

---

## How It Works

### Symlink mappings

The `80-dotfiles` module maintains a declarative list of symlink mappings:

Each mapping is processed by `ensure_symlink` from `lib/common`, which:

- Creates the symlink if missing
- Is a no-op if the symlink is already correct
- Backs up any existing file before replacing

### Directory structure

Git identity selection via `.gitconfig` `includeIf` requires a specific directory layout
under `~/development/`. The `80-dotfiles` module creates these directories if they don't exist.

### Reconciliation

On each `./run`:

1. `20-shell` installs zsh, starship, oh-my-zsh, plugins (tools exist)
2. `30-git-ssh` ensures personal SSH key exists (auth works)
3. `80-dotfiles` clones/pulls dotfiles, creates directories, symlinks configs

Tools are installed before configs are placed. On rerun, everything is a no-op if correct.

---

## Changing Employers

When switching jobs, update two things:

1. **Dotfiles repo:** Update/replace work-specific git identity files and `.gitconfig` `includeIf` entries.
2. **`80-dotfiles` module:** Update the work-specific symlink section and directory list.

The personal baseline (shell config, prompt, personal git identity) never changes.

---

## Adding or Removing a Dotfile

1. Add/remove the file in the dotfiles repo.
2. Add/remove the `ensure_symlink` line in `runs/80-dotfiles`.
3. Add/remove the path from the verification list in the same module.
4. Run `./run --only dotfiles` to apply.

The symlink inventory lives **in code** (`runs/80-dotfiles`), not in documentation.
This avoids drift between docs and reality.

---

## Bootstrap Interaction

On a fresh machine, dotfiles are acquired at step 4 of the bootstrap flow.
SSH must be set up first (step 3) because the dotfiles repo is private.

See `docs/09-bootstrap-and-onboarding.md` for the complete step-by-step.
