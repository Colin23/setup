# Tool Inventory

## Purpose

This document is the canonical list of tools and technologies for this setup repository.

It answers:

- What should exist on the machine
- How each item is installed
- How installation is verified

---

## Status Vocabulary

- **tier**
    - `core`: required for baseline productivity
    - `extended`: useful, but not required for day-1 setup
    - `optional`: nice-to-have, low-frequency usage

- **method**
    - `pacman` / `aur` / `flatpak` / `vendor-script` / `manual`

---

## Consolidated Inventory

### Core tools

| tool                    | category       | module           | arch method | ubuntu method | macOS method | verify                                                                              |
|-------------------------|----------------|------------------|-------------|---------------|--------------|-------------------------------------------------------------------------------------|
| bash                    | shell          | 00-system-update | pacman      | apt           | system       | `bash --version`                                                                    |
| openssh                 | vcs/auth       | 30-git-ssh       | pacman      | apt           | system       | `ssh -V`                                                                            |
| curl                    | network        | 00-system-update | pacman      | apt           | brew/system  | `curl --version`                                                                    |
| unzip                   | archive        | 00-system-update | pacman      | apt           | brew/system  | `unzip -v`                                                                          |
| zip                     | archive        | 00-system-update | pacman      | apt           | brew/system  | `zip -v`                                                                            |
| zsh                     | shell          | 20-shell         | pacman      | apt           | brew         | `zsh --version`                                                                     |
| git                     | vcs            | 30-git-ssh       | pacman      | apt           | brew         | `git --version`                                                                     |
| wget                    | network        | 10-core-cli      | pacman      | apt           | brew         | `wget --version`                                                                    |
| tree                    | utility        | 10-core-cli      | pacman      | apt           | brew         | `tree --version`                                                                    |
| cloc                    | utility        | 10-core-cli      | pacman      | apt           | brew         | `cloc --version`                                                                    |
| htop                    | utility        | 10-core-cli      | pacman      | apt           | brew         | `htop --version`                                                                    |
| vim                     | editor         | 10-core-cli      | pacman      | apt           | brew         | `vim --version`                                                                     |
| firefox                 | browser        | 50-browser       | pacman      | apt           | brew-cask    | `firefox --version`                                                                 |
| fastfetch               | utility        | 10-core-cli      | pacman      | apt           | brew         | `fastfetch --version`                                                               |
| bat                     | cli ux         | 10-core-cli      | pacman      | apt           | brew         | `bat --version`                                                                     |
| fzf                     | cli ux         | 10-core-cli      | pacman      | apt           | brew         | `fzf --version`                                                                     |
| starship                | shell prompt   | 20-shell         | pacman      | apt           | brew         | `starship --version`                                                                |
| eza                     | cli ux         | 10-core-cli      | pacman      | manual        | brew         | `eza --version`                                                                     |
| zoxide                  | cli ux         | 10-core-cli      | pacman      | manual        | brew         | `zoxide --version`                                                                  |
| ufw                     | firewall       | 40-firewall      | pacman      | apt           | manual       | `sudo ufw status verbose` shows active, deny (incoming), allow (outgoing), no rules |
| carapace-bin            | cli completion | 20-shell         | aur         | manual        | brew         | `carapace --version`                                                                |
| zsh-autosuggestions     | shell plugin   | 20-shell         | oh-my-zsh   | oh-my-zsh     | oh-my-zsh    | plugin file exists                                                                  |
| zsh-syntax-highlighting | shell plugin   | 20-shell         | oh-my-zsh   | oh-my-zsh     | oh-my-zsh    | plugin file exists                                                                  |
| shellcheck              | testing/lint   | 90-dev-quality   | pacman      | apt           | brew         | `shellcheck --version`                                                              |
| shfmt                   | testing/lint   | 90-dev-quality   | pacman      | apt           | brew         | `shfmt --version`                                                                   |

### Extended tools

| tool              | category          | module      | arch method                      | ubuntu method | macOS method       | verify                                                                           |
|-------------------|-------------------|-------------|----------------------------------|---------------|--------------------|----------------------------------------------------------------------------------|
| Jetbrains Toolbox | IDEs              | 99-manual   | manual                           | manual        | manual             | app exists                                                                       |
| asdf              | version manager   | 81-runtimes | aur                              | manual        | brew               | `asdf --version`                                                                 |
| java              | runtime           | 81-runtimes | asdf                             | asdf          | asdf               | `java --version`                                                                 |
| kotlin            | runtime           | 81-runtimes | asdf                             | asdf          | asdf               | `kotlin -version`                                                                |
| gradle            | build             | 81-runtimes | asdf                             | asdf          | asdf               | `gradle --version`                                                               |
| maven             | build             | 81-runtimes | asdf                             | asdf          | asdf               | `mvn --version`                                                                  |
| nodejs            | runtime           | 81-runtimes | asdf                             | asdf          | asdf               | `node --version`                                                                 |
| npm               | runtime           | 81-runtimes | asdf                             | asdf          | asdf               | `npm --version`                                                                  |
| dotnet            | runtime           | 81-runtimes | asdf                             | asdf          | asdf               | `dotnet --version`                                                               |
| terraform         | infra             | 81-runtimes | asdf                             | asdf          | asdf               | `terraform --version`                                                            |
| opentofu          | infra             | 81-runtimes | asdf                             | asdf          | asdf               | `tofu --version`                                                                 |
| go                | runtime           | 81-runtimes | asdf                             | asdf          | asdf               | `go version`                                                                     |
| erlang            | runtime           | 81-runtimes | asdf                             | asdf          | asdf               | `erl -eval 'erlang:display(erlang:system_info(otp_release)), halt().'  -noshell` |
| elixir            | runtime           | 81-runtimes | asdf                             | asdf          | asdf               | `elixir --version`                                                               |
| gleam             | runtime           | 81-runtimes | asdf                             | asdf          | asdf               | `gleam --version`                                                                |
| docker            | containers        | future      | pacman                           | apt           | brew+colima        | `docker --version`                                                               |
| docker compose    | containers        | future      | pacman (`docker-compose` plugin) | apt plugin    | brew plugin        | `docker compose version`                                                         |
| rust (rustup)     | runtime           | future      | vendor-script                    | vendor-script | vendor-script/brew | `rustc --version`                                                                |
| uv                | python tooling    | future      | vendor-script                    | vendor-script | brew/vendor        | `uv --version`                                                                   |
| python            | runtime           | future      | uv-managed                       | uv-managed    | uv-managed         | `python3 --version`                                                              |
| flatpak           | app platform      | future      | pacman                           | apt           | brew/manual        | `flatpak --version`                                                              |
| keepassxc         | security/password | future      | flatpak                          | flatpak       | brew-cask          | `keepassxc --version`                                                            |
| obsidian          | notes             | future      | flatpak                          | flatpak       | manual             | app exists                                                                       |
| onlyoffice        | office            | future      | flatpak                          | flatpak       | manual             | app exists                                                                       |

### Optional tools

| tool             | category    | arch method | ubuntu method | macOS method | verify                    |
|------------------|-------------|-------------|---------------|--------------|---------------------------|
| azure-cli        | cloud       | pacman      | vendor-script | brew         | `az version`              |
| argocd cli       | gitops      | pacman      | manual        | brew         | `argocd version --client` |
| tekton cli (tkn) | ci/cd       | pacman      | manual        | brew         | `tkn version`             |
| hcloud cli       | cloud       | pacman      | manual        | brew         | `hcloud version`          |
| ventoy           | usb tooling | aur         | manual        | manual       | binary exists             |

---
