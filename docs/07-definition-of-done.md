# docs/07-definition-of-done.md

# Definition of Done

A phase/module is done when:

## Functional

- Fresh run succeeds on target platform.
- Immediate rerun succeeds (idempotency baseline).
- Drift recovery scenario validated.

## Quality

- Script passes lint/format checks.
- Logs are readable and actionable.
- Failure messages identify module and command.

## Documentation

- Module purpose and verification documented.
- Platform support declared.
- Any manual steps clearly documented.

## Safety

- No destructive behavior without explicit opt-in.
- Dotfile overwrites follow documented policy.
