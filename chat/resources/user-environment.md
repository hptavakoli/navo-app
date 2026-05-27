# User environment — stable / device-level facts

**Status:** Personal / user-global. Sync this file across the user's projects from a canonical copy; do not duplicate content into individual project runbooks.

**Defaults captured:** 2026-05-10. The defaults below reflect the file owner's primary dev machine and stable preferences. If a future project bootstraps on a different machine / OS / architecture, the relevant items must be re-confirmed at bootstrap rather than treated as automatic defaults.

> Records the file owner's stable working environment — operating system, hardware shape, editor, package managers, language runtime install conventions, shell preferences. AI sessions read this to ground assumptions about the user's machine, so the user does not need to re-explain the local environment in every conversation.
>
> This file is **NOT project-specific.** Project-specific environment information (venv inventory, mode flags, project commands) belongs in [`../../docs/04-runbook.md`](../../docs/04-runbook.md), which extends this file with project-specific overlays.

This file is dual-purpose: it is **both** a template scaffold (so a fresh project can see what fields belong here) **and** a pre-filled starting point with the file owner's personal defaults. Each line carries one of these tags:

- `[default — user-personal]` — pre-filled personal default. Override at bootstrap if a project deviates.
- `[fill at bootstrap]` — system-inspectable fact captured fresh at project bootstrap (use safe read-only commands; never read shell rc files, `.env`, keychain, or secrets).
- `[machine-scoped]` — machine-specific facts (CPU / RAM / install paths). Treat as descriptive, not load-bearing.
- `[TODO(user)]` — preference items with no reasonable default. Only used when neither the template nor the user can pre-supply an answer.

---

## 1. Operating system

- `[default — user-personal]` macOS, Apple Silicon (`arm64`).
- `[fill at bootstrap]` Specific macOS version → `sw_vers`.
- `[fill at bootstrap]` Kernel version → `uname -srm`.
- `[machine-scoped]` Specific machine specs (CPU model, RAM size) → `sysctl -n machdep.cpu.brand_string`, `sysctl -n hw.memsize`. Fill if relevant; otherwise omit.
- `[default — user-personal]` No known workflow-affecting system settings reported. AI should not assume FileVault, Focus filters, security tooling, or notification behavior unless explicitly confirmed at bootstrap.

---

## 2. Editor / IDE

- `[default — user-personal]` Primary editor: **VS Code** (Apple Silicon build at `/usr/local/bin/code`).
- `[fill at bootstrap]` Specific VS Code version → `code --version`.
- `[default — user-personal]` Notable VS Code extensions assumed available:
  - `anthropic.claude-code` — Claude Code in-IDE integration
  - `github.vscode-pull-request-github` — PR review inside the editor
  - `ms-python.python`, `ms-python.vscode-pylance`, `ms-python.debugpy`, `ms-python.vscode-python-envs` — Python toolchain
  - `docker.docker`, `ms-azuretools.vscode-docker`, `ms-azuretools.vscode-containers` — Docker integration
  - `eamodio.gitlens`, `mhutchie.git-graph` — Git history / blame / graph
  - `bierner.markdown-mermaid` — Mermaid in Markdown preview
  - `figma.figma-vscode-extension` — Figma integration
  - `codeium.codeium` — AI completion (alongside Claude)
- `[default — user-personal]` Terminal-based AI tooling: **Claude Code CLI** at `~/.local/bin/claude`. Specific version → `claude --version`.
- `[default — user-personal]` VS Code is the **primary** editor and the only one part of the regular default workflow. Other editors (Xcode, JetBrains, Cursor, vim / neovim, etc.) should only be assumed if a project explicitly introduces them.
- `[default — user-personal]` No strong global formatter / linter preference. Use project-specific formatter / linter conventions — Black / Ruff for Python, Prettier / ESLint for JS / TS — only when the project chooses them. Not global hard rules.

---

## 3. Shells / terminals

- `[default — user-personal]` Default login shell: **zsh** at `/bin/zsh`.
- `[fill at bootstrap]` Specific zsh version → `zsh --version`.
- `[default — user-personal]` Preferred interactive terminal: **Warp**. Also available and acceptable: VS Code integrated terminal, iTerm2, Terminal.app. **No workflow should depend specifically on Warp** — AI may provide commands that work in a standard shell or in the VS Code integrated terminal.
- `[default — user-personal]` No known aliases that change command semantics. AI should write explicit full commands and avoid relying on aliases (no `g`, `gst`, custom `brew` / `pip` / `docker` wrappers, etc.).
- `[default — user-personal]` No confirmed prompt customization. AI should not assume Starship / Powerlevel10k / active-venv display / Node-version display in the prompt unless explicitly confirmed at bootstrap.

---

## 4. Package managers

### System-level

- `[default — user-personal]` **Homebrew** at `/opt/homebrew` (Apple Silicon prefix). Specific version → `brew --version`.
- `[fill at bootstrap]` Top-level (leaf) brew formulas → `brew leaves` (representative leaf set on the file owner's machine includes `fzf`, `gh`, `pandoc`, `pyenv`, `rsync`, `tree`, `wget`, plus a handful of taps; specifics drift over time).
- `[default — user-personal]` Xcode Command Line Tools assumed installed at `/Library/Developer/CommandLineTools`.

### Language-specific

- `[default — user-personal]` **pyenv** for Python version management. Active version → `pyenv version` or `python3 --version`.
- `[default — user-personal]` **nvm** for Node version management. Active version → `node --version`.
- `[default — user-personal]` Per-language CLIs assumed available: `pip`, `npm`, `pnpm`, `yarn` (project chooses which).
- `[default — user-personal]` Default Python toolchain for **new** projects: **pip + venv**. Keep `uv` open as a future option if a project clearly benefits from it. Do not default to Poetry unless a specific project calls for it.
- `[default — user-personal]` Default Node toolchain for **new** projects: **pnpm**. `npm` is acceptable for simple projects or compatibility needs. `yarn` should only be used if a specific project or template requires it.

---

## 5. Language runtimes and versions

| Runtime | Manager | Version | Notes |
|---|---|---|---|
| Python | pyenv `[default — user-personal]` | `[fill at bootstrap]` → `python3 --version` | Per-project venvs created with the active interpreter unless a project pins otherwise. |
| Node.js | nvm `[default — user-personal]` | `[fill at bootstrap]` → `node --version` | |

- `[default — user-personal]` **Python and Node are the only default assumed runtimes.** Other runtimes (Go, Rust, Ruby, Java / Kotlin, Dart / Flutter) should be checked per project before use. The presence of related VS Code extensions alone is **NOT proof** that those runtimes are installed and ready.
- `[machine-scoped]` Pyenv install path: `~/.pyenv/versions/<version>/`; nvm install path: `~/.nvm/versions/node/<version>/`. AI may reference these paths for diagnostics; do not embed them into project task files (project commands should resolve through `python3` / `node` on PATH or through a project-local venv).

---

## 6. Developer tools (CLI)

Tools assumed available on PATH (specific versions captured at bootstrap, not pinned):

- `[default — user-personal]` **Git** (Apple-shipped at `/usr/bin/git`). Version → `git --version`.
- `[default — user-personal]` **GitHub CLI (`gh`)** via Homebrew. Version → `gh --version`. Used for read-only PR / issue inspection by AI; write operations follow each project's `docs/02-git-workflow.md` §1.1 (operation ownership — track varies per project).
- `[default — user-personal]` **Docker** at `/usr/local/bin/docker`. Version → `docker --version`.
- `[default — user-personal]` Docker policy: Docker is installed and acceptable to use. Do **not** introduce Docker by default for simple projects. Use Docker when it provides clear value — isolating services, reducing setup risk, or supporting required infrastructure.
- `[default — user-personal]` Other CLI tools assumed available: `jq`, `ripgrep` (`rg`), `make` (Apple-shipped GNU Make), `curl`, `pandoc`, `tree`, `wget`, `fzf`, `rsync`.
- `[default — user-personal]` **No commit signing requirement** by default. Workflow should not be blocked on signed commits. (`gpg` is also not assumed on PATH.) Revisit only if a project becomes public, team-based, security-sensitive, or organization policy requires verified commits.

---

## 7. Default development conventions

These are conventions to assume across the file owner's projects unless a specific project's runbook overrides them.

- `[default — user-personal]` Per-project Python virtual environments follow the `.venv/<purpose-slug>/` pattern. Each project's runbook should document what each venv is for.
- `[default — user-personal]` `.env` discipline: `.env` files are gitignored. `.env.example` is tracked when the project requires environment variables. Real secrets are **never committed**. Required environment variables are documented in `.env.example` and / or the project runbook.
- `[default — user-personal]` Docker is acceptable but not the default for simple projects (see §6).
- `[default — user-personal]` Default Python and Node toolchain preferences for new projects (see §4).
- `[default — user-personal]` Commit-signing policy (see §6).
- `[default — user-personal]` Branch-naming and commit-format conventions are **project-specific**. Each project's Git workflow document is the source of truth (the template's lives at [`../../docs/02-git-workflow.md`](../../docs/02-git-workflow.md)). Do not record a separate user-global default here.
- `[default — user-personal]` Default base branch for new repos: **`main`**.
- `[default — user-personal]` Pre-commit hooks are **per-project**. Do not assume pre-commit hooks by default. Add them only when a project clearly benefits from them.

---

## 8. AI tooling and providers

- `[default — user-personal]` **Claude Code CLI** is the file owner's primary AI development partner.
- `[default — user-personal]` VS Code AI integrations available: Claude Code, Codeium, GitHub Copilot Pull Request integration. Copilot may also be referenced as the auditor in some prompts.
- `[default — user-personal]` ChatGPT project resource set includes the project's shared communication / operational / engineering-doctrine resources plus a per-scope project-state file (and optionally a per-scope design-status file when the design track is active). Exact resource inventory and counts are project-specific and recorded in each project's `chat/README.md` and `<scope>.project-state.md`; do not assume a fixed count or list here.

---

## What does NOT belong here

- Project-specific commands → [`../../docs/04-runbook.md`](../../docs/04-runbook.md).
- Project-specific dependency lists → project's manifest files (e.g., `requirements.txt`, `package.json`, `Cargo.toml`).
- Project-specific environment variables → project's `.env.example` and runbook.
- API keys, account credentials, GitHub tokens, SSH keys, or anything that would be a secret if leaked. Never read or record these.
- Per-project venv paths (e.g., `.venv/<phase-slug>/` instances inside a specific project) — those live in the project's runbook.
- Preferred-provider lists for hosted AI APIs — those are per-project decisions and live in each project's ADRs.

---

## How AI should use this file

- **Read at project start** to ground assumptions about the user's machine.
- **At project bootstrap, ask:** "Do these defaults still apply for this project, or are there project-specific deviations?" — and if the user notes a deviation, record the deviation in the project runbook, not in this file.
- **Treat `[default — user-personal]` items as opinions, not facts.** They reflect the file owner's preference and may be overridden at any project's bootstrap.
- **Treat `[fill at bootstrap]` items as placeholders.** Run safe read-only inspection at bootstrap to fill them; never read shell rc files, `.env`, keychain, or any other secret-bearing surface.
- **Treat `[machine-scoped]` items as descriptive, not load-bearing.** A command should not break if the user's hardware changes; AI should fall back to environment detection rather than relying on a specific path or arch.
- **Treat `[TODO(user)]` items as unresolved.** Do not assume a preference until the user confirms.
- **Do not edit this file from inside a project session unless the user explicitly asks** — changes here ripple across all the user's projects. Maintenance happens at the user's canonical copy.

---

## Confirming defaults at bootstrap

When a fresh project is bootstrapped from this template:

1. Read this file to know what the file owner's defaults are.
2. Run safe read-only inspection (`sw_vers`, `uname -m`, `which …`, `--version` flags) to fill the `[fill at bootstrap]` items.
3. Confirm with the user whether the `[default — user-personal]` items still hold for the new project, or if it deviates.
4. Record any deviations in the project's `docs/04-runbook.md` (or its renamed equivalent), not in this file.
5. If a deviation is foundational (e.g., the new project runs on a different OS or uses a different default Node toolchain), capture that reasoning in the project's ADRs alongside the runbook entry.
