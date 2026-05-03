# Contributing

This is a single-person project, but consistent workflows still matter. This doc covers the
recurring tasks: adding tools, updating dotfiles, changing employers, and validating changes.

---

## Definition of Done

A change (module, tool, dotfile, etc.) is done when:

### Functional

- Fresh run succeeds on the target platform.
- Immediate rerun succeeds (idempotency baseline).
- Drift recovery scenario validated (where applicable).

### Quality

- Script passes `./scripts/lint` (ShellCheck + shfmt).
- Logs are readable and actionable.
- Failure messages identify the module and command.

### Documentation

- Module purpose and verification documented in the module header.
- Platform support declared via `require_os`.
- Any manual steps are clearly documented in `99-manual` or `new-machine-setup.md`.

### Safety

- No destructive behavior without explicit opt-in.
- Dotfile overwrites use `ensure_symlink` (which backs up existing files).

---

## Adding or Removing a Tool

1. Add or remove the package install line in the appropriate `runs/NN-*` module.
2. Add or remove the tool from that module's verification loop.
3. If it is a new module, name it `runs/NN-name` with a free numeric prefix.
4. Run lint:
   ```bash
   ./scripts/lint
   ```
5. Run the Docker smoke tests:
   ```bash
   ./tests/docker-arch
   ./tests/docker-ubuntu
   ```
6. Run the module on the current actual machine to verify:
   ```bash
   ./run --only <module-substring>
   ```

### Tool Catalog

Authoritative tool data lives in the modules themselves.
Check [tool-inventory.md](tool-inventory.md) for details.

---

## Adding or Removing a Dotfile

Symlink mappings live in `runs/80-dotfiles` (the code), not in docs. To change them:

1. Add/remove the file in the dotfiles repo.
2. Add/remove the `ensure_symlink` line in `runs/80-dotfiles`.
3. Add/remove the path from the verification list in the same module.
4. Apply:
   ```bash
   ./run --only dotfiles
   ```

`ensure_symlink` backs up any existing non-symlink file before replacing it, so this is
safe to run on a machine that already has files in those locations.

---

## Changing Employers

When switching jobs:

1. **Dotfiles repo:** update or replace the work-specific git identity files and the
   `includeIf` entries in `.gitconfig`.
2. **`runs/80-dotfiles`:** update the work-specific symlink section and the
   `DEV_DIRS` list.

The personal baseline (shell config, prompt, personal git identity) doesn't change.

---

## Testing

### What is being tested

| Layer        | How                                               | What it covers                                                    |
|--------------|---------------------------------------------------|-------------------------------------------------------------------|
| Static       | `./scripts/lint`                                  | ShellCheck + shfmt on all shell scripts                           |
| Integration  | `./tests/docker-arch` and `./tests/docker-ubuntu` | Fresh install, idempotency, drift recovery for non-system modules |
| Real machine | `./run` (current laptop)                          | Modules that need GUI, systemd, or SSH to private repos           |

### What can't be tested in Docker

| Module        | Why                                | How to test          |
|---------------|------------------------------------|----------------------|
| `30-git-ssh`  | Interactive prompts, SSH to GitHub | Real machine         |
| `40-firewall` | Requires systemd + iptables        | VM or real machine   |
| `50-browser`  | GUI application                    | Real machine         |
| `80-dotfiles` | Requires SSH to clone private repo | Real machine (or VM) |

`99-manual` is safe in Docker — it only prints text.

### Idempotency test cases

1. Fresh install run succeeds.
2. Immediate second run is a no-op (no changes, no failures).
3. Simulated drift (e.g., remove `bat`) is corrected on rerun.
4. Partial failure can be rerun to recover.

### Testing dotfiles on a real machine

The dotfiles module is safe on an existing machine because `ensure_symlink` backs up any
existing files before replacing them. Preview first:

```bash
./run --only dotfiles --dry-run
```

Then apply:

```bash
./run --only dotfiles
```

### Full bootstrap test (VM)

To validate the entire fresh-machine flow without using the actual real laptop:

1. Create an Arch VM (GNOME Boxes or virt-manager).
2. Snapshot immediately after clean OS install.
3. Follow `docs/new-machine-setup.md` step by step.
4. Revert to the snapshot and repeat to confirm reproducibility.

### macOS

No macOS Docker image exists due to Apple licensing. Test on real hardware or a macOS CI
runner (e.g., GitHub Actions `macos-latest`).

---

## Platform Support

- **Primary:** Arch-based Linux (full support).
- **Secondary:** Ubuntu / KDE Neon (works, but some tools are warn-only when not available
  via apt — for example fastfetch, eza, zoxide).
- **Future:** macOS (planned, not implemented).
