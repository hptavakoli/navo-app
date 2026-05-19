# Changelog

All notable changes to Navo App are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

For the discipline behind this file (when to add entries, what kinds of changes go where, relationship to GitHub Releases and readiness reports) see [`docs/02-git-workflow.md` §8](docs/02-git-workflow.md).

## [Unreleased]

### Changed — ChatGPT resource layout

- Per-repo project-state and design-status files renamed with `<scope>.` prefix (`app.project-state.md`, `app.design-status.md`) so they can coexist in a single shared Navo ChatGPT Project alongside the parallel files from the other two Navo repos. Other in-repo references updated accordingly.
- Shared ChatGPT resource files (`communication.claude.md`, `communication.general.md`, `playbooks.workflows.md`, `user-environment.md`) restored to scope-agnostic form using a `<scope>` placeholder, so a single upload of each shared file applies to all three Navo repos in the shared ChatGPT Project. `<scope>` convention documented in `communication.general.md` §7.
- `playbooks.workflows.md` title harmonized from `Workflows — Navo App` to `Workflows — Navo` to match shared status.

### Changed — Project-state cold-start readiness

- `chat/resources/app.project-state.md` promoted from template scaffold to first-use cold-start snapshot. Captures current repo status, bootstrap and resource-cleanup history, current `01-foundation` milestone state, next recommended action, key decisions, external pipeline / Hermes boundary, open questions, and the shared-Project upload set. Project Overview and Operational Constraints sections preserved.
