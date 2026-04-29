# Tool Inventory (Single Source of Truth Catalog)

## Purpose

This document is the canonical list of tools and technologies for this setup repository.

It answers:

- What should exist on my machine
- How each item is installed
- How installation is verified
- In which implementation phase each item will be automated

---

## Status Vocabulary

- **tier**
    - `core`: required for baseline productivity
    - `extended`: useful, but not required for day-1 setup
    - `optional`: nice-to-have, low-frequency usage

- **automation**
    - `m1`: automate in Milestone 1 (Arch MVP)
    - `m2`: automate in Milestone 2
    - `later`: automate later
    - `manual`: intentionally manual for now

- **method**
    - `pacman` / `aur` / `flatpak` / `vendor-script` / `manual`

---

## Consolidated Inventory

### Core tools

| tool                    | category       | module           | arch method | ubuntu method (future) | macos method (future) | verify                 | automation |
|-------------------------|----------------|------------------|-------------|------------------------|-----------------------|------------------------|------------|
| bash                    | shell          | 00-system-update | pacman      | apt                    | system                | `bash --version`       | m1         |
| openssh                 | vcs/auth       | 30-git-ssh       | pacman      | apt                    | system                | `ssh -V`               | m1         |
| curl                    | network        | 00-system-update | pacman      | apt                    | brew/system           | `curl --version`       | m1         |
| unzip                   | archive        | 00-system-update | pacman      | apt                    | brew/system           | `unzip -v`             | m1         |
| zip                     | archive        | 00-system-update | pacman      | apt                    | brew/system           | `zip -v`               | m1         |
| zsh                     | shell          | 20-shell         | pacman      | apt                    | brew                  | `zsh --version`        | m1         |
| git                     | vcs            | 30-git-ssh       | pacman      | apt                    | brew                  | `git --version`        | m1         |
| wget                    | network        | 10-core-cli      | pacman      | apt                    | brew                  | `wget --version`       | m1         |
| tree                    | utility        | 10-core-cli      | pacman      | apt                    | brew                  | `tree --version`       | m1         |
| cloc                    | utility        | 10-core-cli      | pacman      | apt                    | brew                  | `cloc --version`       | m1         |
| htop                    | utility        | 10-core-cli      | pacman      | apt                    | brew                  | `htop --version`       | m1         |
| vim                     | editor         | 10-core-cli      | pacman      | apt                    | brew                  | `vim --version`        | m1         |
| firefox                 | browser        | 50-browser       | pacman      | apt                    | brew-cask             | `firefox --version`    | m1         |
| fastfetch               | utility        | 10-core-cli      | pacman      | apt                    | brew                  | `fastfetch --version`  | m1         |
| bat                     | cli ux         | 10-core-cli      | pacman      | apt                    | brew                  | `bat --version`        | m1         |
| fzf                     | cli ux         | 10-core-cli      | pacman      | apt                    | brew                  | `fzf --version`        | m1         |
| starship                | shell prompt   | 20-shell         | pacman      | apt                    | brew                  | `starship --version`   | m1         |
| eza                     | cli ux         | 10-core-cli      | pacman      | manual                 | brew                  | `eza --version`        | m1         |
| ufw                     | firewall       | 40-firewall      | pacman      | apt                    | manual                | `ufw status`           | m1         |
| carapace-bin            | cli completion | 20-shell         | aur         | manual                 | brew                  | `carapace --version`   | m1         |
| zsh-autosuggestions     | shell plugin   | 20-shell         | oh-my-zsh   | oh-my-zsh              | oh-my-zsh             | plugin file exists     | m1         |
| zsh-syntax-highlighting | shell plugin   | 20-shell         | oh-my-zsh   | oh-my-zsh              | oh-my-zsh             | plugin file exists     | m1         |
| bats                    | testing        | 90-dev-quality   | pacman      | apt/manual             | brew                  | `bats -v`              | m1         |
| shellcheck              | testing/lint   | 90-dev-quality   | pacman      | apt                    | brew                  | `shellcheck --version` | m1         |
| shfmt                   | testing/lint   | 90-dev-quality   | pacman      | apt/manual             | brew                  | `shfmt --version`      | m1         |

### Extended tools

| tool              | category          | arch method                      | ubuntu method (future) | macos method (future) | verify                                                                           | automation |
|-------------------|-------------------|----------------------------------|------------------------|-----------------------|----------------------------------------------------------------------------------|------------|
| Jetbrains Toolbox | IDEs              | manual                           | manual                 | manual                | app exists                                                                       | m2         |
| asdf              | version manager   | aur                              | manual                 | brew                  | `asdf --version`                                                                 | m2         |
| java              | runtime           | asdf                             | asdf                   | asdf                  | `java --version`                                                                 | m2         |
| kotlin            | runtime           | asdf                             | asdf                   | asdf                  | `kotlin -version`                                                                | m2         |
| gradle            | build             | asdf                             | asdf                   | asdf                  | `gradle --version`                                                               | m2         |
| maven             | build             | asdf                             | asdf                   | asdf                  | `mvn --version`                                                                  | m2         |
| nodejs            | runtime           | asdf                             | asdf                   | asdf                  | `node --version`                                                                 | m2         |
| npm               | runtime           | asdf                             | asdf                   | asdf                  | `npm --version`                                                                  | m2         |
| terraform         | infra             | asdf                             | asdf                   | asdf                  | `terraform --version`                                                            | m2         |
| opentofu          | infra             | asdf                             | asdf                   | asdf                  | `tofu --version`                                                                 | m2         |
| go                | runtime           | asdf                             | asdf                   | asdf                  | `go version`                                                                     | m2         |
| erlang            | runtime           | asdf                             | asdf                   | asdf                  | `erl -eval 'erlang:display(erlang:system_info(otp_release)), halt().'  -noshell` | m2         |
| elixir            | runtime           | asdf                             | asdf                   | asdf                  | `elixir --version`                                                               | m2         |
| gleam             | runtime           | asdf                             | asdf                   | asdf                  | `gleam --version`                                                                | m2         |
| docker            | containers        | pacman                           | apt                    | brew+colima           | `docker --version`                                                               | m2         |
| docker compose    | containers        | pacman (`docker-compose` plugin) | apt plugin             | brew plugin           | `docker compose version`                                                         | m2         |
| rust (rustup)     | runtime           | vendor-script                    | vendor-script          | vendor-script/brew    | `rustc --version`                                                                | m2         |
| uv                | python tooling    | vendor-script                    | vendor-script          | brew/vendor           | `uv --version`                                                                   | m2         |
| python            | runtime           | uv-managed                       | uv-managed             | uv-managed            | `python3 --version`                                                              | m2         |
| flatpak           | app platform      | pacman                           | apt                    | brew/manual           | `flatpak --version`                                                              | m2         |
| keepassxc         | security/password | flatpak                          | flatpak                | brew-cask             | `keepassxc --version`                                                            | m2         |
| obsidian          | notes             | flatpak                          | flatpak                | manual                | app exists                                                                       | m2         |
| onlyoffice        | office            | flatpak                          | flatpak                | manual                | app exists                                                                       | m2         |

### Optional tools

| tool             | category    | arch method | ubuntu method (future) | macos method (future) | verify                    | automation |
|------------------|-------------|-------------|------------------------|-----------------------|---------------------------|------------|
| azure-cli        | cloud       | pacman      | vendor-script          | brew                  | `az version`              | later      |
| argocd cli       | gitops      | pacman      | manual                 | brew                  | `argocd version --client` | later      |
| tekton cli (tkn) | ci/cd       | pacman      | manual                 | brew                  | `tkn version`             | later      |
| hcloud cli       | cloud       | pacman      | manual                 | brew                  | `hcloud version`          | later      |
| ventoy           | usb tooling | aur         | manual                 | manual                | binary exists             | later      |

---

## Phase Targets

## Milestone 1 (Arch MVP, automate now)

- bash, zsh, git, openssh, curl
- unzip, zip
- bat, eza, fzf
- nodejs, npm, uv, python
- flatpak
- bats, shellcheck, shfmt

## Milestone 2 (next)

- docker + compose
- terraform/opentofu
- azure-cli
- php + composer
- go
- firewall + selected desktop apps

## Later

- VirtualBox stack
- niche CLIs
- additional runtimes and secondary apps

---

## Rules for Adding New Tools

When adding a tool:

1. Add row here first.
2. Choose tier + automation phase.
3. Define verification command.
4. Implement module logic.
5. Add/update test assertion.
6. Mark as automated once tested.

This keeps repo truth and machine reconciliation aligned.
