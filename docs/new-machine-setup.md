# New Machine Setup

## Goal

Get from bare OS to fully provisioned developer environment with minimal manual steps.
Total time: ~15–30 minutes, of which ~2 minutes is manual intervention.

---

## Prerequisites

A freshly installed OS with:

- A user account with `sudo` access
- Network connectivity

---

## Step-by-Step Bootstrap Flow

### Step 0 — Install minimal prerequisites (manual, ~30 seconds)

#### Arch

```bash
sudo pacman -Sy --noconfirm git curl openssh sudo
```

#### Ubuntu

```bash
sudo apt-get update && sudo apt-get install -y git curl openssh-client
```

### Step 1 — Clone setup repo via HTTPS (~30 seconds)

HTTPS is used because SSH keys don't exist yet on a fresh machine.

```bash
mkdir -p ~/development/personal/colin
cd ~/development/personal/colin
git clone https://github.com/Colin23/setup.git
cd setup
```

### Step 2 — Install base tools (automated, ~3–5 minutes)

Install system packages, CLI tools, shell, and dev quality tools. Dotfiles and git-ssh are skipped — they need SSH which
isn't set up yet.

```bash
./run --skip git-ssh,dotfiles,firewall,browser
```

This runs:

- `00-system-update` - system update + base packages
- `10-core-cli` - wget, tree, bat, fzf, eza, zoxide, etc.
- `20-shell` - zsh, starship, oh-my-zsh, plugins, carapace
- `90-dev-quality` - shellcheck, shfmt
- `99-manual` - prints manual step reminders

### Step 3 — Set up SSH key (one manual pause, ~2 minutes)

```bash
bash ./run --only git-ssh
```

This will:

1. Install git + openssh (if not already present)
2. Generate an ed25519 SSH key named personal-github-key (prompts for email).
3. Print the public key
4. **Pause** — Add the key to [GitHub SSH settings](https://github.com/settings/ssh/new)
5. Test SSH connectivity to GitHub
6. Report success/failure

**This is the only manual pause in the entire bootstrap.**

### Step 4 — Clone dotfiles and apply configs (automated, ~1 minute)

```bash
bash ./run --only dotfiles
```

This will:

1. Clone `git@github.com:Colin23/dotfiles.git` to `~/.dotfiles`
2. Create `~/development/` directory tree (personal, work, obsidian)
3. Symlink all configs (.zshrc, starship.toml, .gitconfig, git identities, SSH config).
4. Verify all symlinks are correct

### Step 5 — Full convergence run (automated, ~2 minutes)

```bash
bash ./run
```

Runs everything. All modules should succeed:

- Already-installed packages → no-op
- Already-correct symlinks → no-op
- SSH key → already exists
- Dotfiles → already cloned, pull is no-op

On a managed/work laptop where you can't (or shouldn't) configure the system firewall:

```bash
./run --skip firewall
```

### Step 6 — Idempotency verification

```bash
bash ./run
```

Run a second time. Should complete with zero changes and zero failures.

### Step 7 — Start a new shell

```bash
exec zsh
```

The full environment is now active: zsh + starship + plugins + personal `.zshrc`.

---

## Summary of Manual Steps

| Step | Action                       | Time |
|------|------------------------------|------|
| 0    | Install git, curl, openssh   | 30s  |
| 1    | Clone setup repo (HTTPS)     | 30s  |
| 3    | Add SSH key to GitHub        | ~90s |
| 7    | Start new shell (`exec zsh`) | 5s   |

Everything else is automated.

---

## Subsequent Runs

After initial bootstrap, ongoing reconciliation is just:

```bash
cd ~/development/personal/colin/setup
./run
```

This will:

- Update system packages
- Ensure all CLI tools are present (install if missing = drift recovery)
- Pull latest dotfiles and re-verify symlinks
- Report any issues

---

## Switching to SSH Remote (optional)

After SSH is working, you can switch the setup repo from HTTPS to SSH:

```bash
cd ~/development/personal/colin/setup
git remote set-url origin git@github.com:Colin23/setup.git
```

This is optional — HTTPS continues to work for pulls on a public repo.

---

## See Also

- [architecture.md](architecture.md) — how the system works internally, including the two-repo strategy.
- [contributing.md](contributing.md) — how to add tools, change employers, or update dotfiles.

