# Setup

A repo-driven machine setup system that bootstraps and continuously reconciles a developer environment. The repository
is the single source of truth for desired machine state.

Running `setup` is safe, repeatable, and convergent: each run brings the machine closer to the declared state, and a
second run is a no-op.

---

## Quick Start

### On a fresh machine

Follow [docs/new-machine-setup.md](docs/new-machine-setup.md) — total time ~15–30 minutes, ~2 minutes of manual
intervention.

### On an existing machine (ongoing reconciliation)

```bash
./run                  # full run
./run --skip firewall  # skip firewall (e.g. on managed work laptops)
./run --dry-run        # preview without changes
./run --only shell     # run only matching modules
```

---

## Repository structure

```text
setup/
├── run                    main entrypoint
├── lib/common             shared helper library
├── runs/NN-name           modules, executed in numeric order
├── scripts/lint           static analysis (ShellCheck + shfmt)
├── tests/                 Docker smoke tests
└── docs/                  documentation
```

---

## Documentation

- [architecture.md](docs/architecture.md) — how the system works (runner, modules, two-repo strategy)
- [new-machine-setup.md](docs/new-machine-setup.md) — step-by-step bootstrap on a fresh laptop
- [contributing.md](docs/contributing.md) — adding tools, updating dotfiles, changing employers, testing
- [tool-inventory.md](docs/tool-inventory.md) — canonical catalog of all tools and how they're installed

## Platform Support

Primary: Arch-based Linux
Secondary: Ubuntu / KDE Neon (some tools warn-only when not in apt)
Future: macOS

## Inspiration

The shape of this repo was originally inspired by ThePrimeagen's [dev repo](https://github.com/ThePrimeagen/dev/). It
has since diverged significantly to support multiple Linux distros, a private dotfiles repo, and an ArgoCD-style
reconciliation model.
