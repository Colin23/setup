# docs/02-operating-model.md

# Operating Model (ArgoCD-like Reconciliation)

## Desired State

The repository expresses:

- Which tools must exist
- Which versions/channels are preferred (where practical)
- Which configs/symlinks/files must exist
- Which services should be enabled (optional, phased)

## Reconciliation Behavior

On each run:

1. Detect current state.
2. Compare against desired state encoded in scripts/config.
3. Apply only missing/drifted pieces.
4. Re-check and report convergence status.

## Idempotency Rules

- Package managers use idempotent flags where available.
- Installers guarded by pre-checks (`command -v`, version checks, file existence checks).
- Symlinks/files managed with safe replace semantics (backup or explicit overwrite policy).
- No blind append-to-config without guards.

## Drift Handling

If machine drifts (manual uninstall/config edit):

- Next run restores desired state.
- Optional `--strict` mode can fail when drift is detected but not auto-fixable.

## Safety Modes

- `--dry-run`: show planned actions only.
- `--plan`: structured preview (future enhancement).
- `--apply`: default real execution.
