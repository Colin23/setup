# docs/05-platform-roadmap.md

# Platform Roadmap

## v1: Arch-based Linux (primary)

- Canonical implementation target
- All core modules first
- Strong idempotency focus

## v2: Ubuntu-based Linux

- Introduce package-manager abstraction layer
- Keep module names consistent across platforms
- Add OS guards and fallback logic

## v3: macOS

- Homebrew-first package strategy
- Separate module variants where needed
- Validate shell/config behavior differences

## Compatibility Policy

- A module must explicitly declare supported OS families.
- Unsupported modules are skipped with clear reason.
- No silent partial behavior.
