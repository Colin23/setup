# Bootstrap and Onboarding (Fresh Machine)

## Goal

Define the first 30–60 minutes on a brand-new machine so setup is reliable, repeatable, and does not depend on
pre-existing SSH credentials.

---

## High-Level Flow

1. Install minimal prerequisites manually (small fixed set).
2. Clone public setup repo via HTTPS.
3. Run bootstrap flow.
4. Bootstrap establishes SSH identity and access.
5. Pull private dotfiles repo.
6. Run setup profiles/modules.
7. Re-run to confirm idempotency.

---

## Arch First-Time Onboarding

## Step 0 — Minimal manual prerequisites

Install only what is necessary to start automation:

- git
- curl
- openssh (if not already)
- sudo configured for user

## Step 1 — Clone setup repo (HTTPS)

Use HTTPS for first clone so it works before SSH keys exist.

## Step 2 — Start bootstrap

Run bootstrap command from this repo (to be implemented under `bootstrap/`).

Bootstrap responsibilities:

- detect OS (Arch expected in v1)
- verify network connectivity
- ensure required base tools exist
- generate SSH key if missing
- print public key + guidance to add in GitHub/GitLab
- wait/confirm user action
- test SSH auth
- optionally switch setup repo remote to SSH

## Step 3 — Dotfiles acquisition

After SSH is confirmed:

- clone private dotfiles repo (recommended) OR
- initialize/update private submodule

Recommended approach: direct clone for simpler first-run auth handling.

## Step 4 — Apply setup profile

Run main setup, e.g. `minimal`, then `laptop/full`.

## Step 5 — Validate convergence

Immediately run setup a second time:

- should complete successfully
- should not repeat destructive steps
- should only reconcile drift/missing state

---

## Repository Strategy: Public Setup + Private Dotfiles

## Public repo

Contains:

- runner
- modules
- docs
- non-secret machine logic

## Private dotfiles repo

Contains:

- personal shell/editor configs
- private defaults
- non-public preferences

## Integration options

1. **Preferred:** bootstrap clones private dotfiles repo after SSH setup.
2. **Alternative:** git submodule (works, but first-run auth UX is slightly more complex).

---

## Bootstrap Design Requirements

- Must be safe to rerun.
- Must not overwrite SSH keys blindly.
- Must provide clear prompts for manual key registration.
- Must fail fast with actionable errors.
- Must log each step.

---

## Safety Notes

- Never store secrets in this repository.
- Keep secret material outside source control.
- Dotfile application should use backup/replace policy (defined in implementation).
- Manual steps should be explicit and isolated in a dedicated module.

---

## Future Extensions

- bootstrap `--non-interactive` mode (for CI/VM testing)
- automatic detection of Git provider and key upload helpers
- support for Ubuntu/macOS bootstrap variants
