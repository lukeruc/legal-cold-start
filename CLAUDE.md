# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## About

This is the **infrastructure layer** of the Legal AI Workstation. The `coldstart/` directory is the installer — users copy it to their work directory, run the cold-start interview, and get a complete legal working environment (CLAUDE.md, .claude/profiles/, playbook/, sessions/, records/, archive/).

Design documentation is maintained separately from the project files.

Function modules live in `modules/` during development. When mature, they are extracted to independent repos and installed as Claude Code skills under `.claude/skills/` in the user's work directory. Modules consume the infrastructure layer via path and field contracts defined in the design documentation.

## Development — no build, no tests, no lint

This is a pure configuration project. There is no build step, no test suite, no CI. Development consists of editing Markdown and JSON files.

## Key files and their relationships

- **`coldstart/CLAUDE.md`** — The agent instructions that ship to users. Four sections: personality, routing (profile check → coldstart or normal operation), operating procedures, and workspace map.
- **`coldstart/scripts/in-house.md`** — Cold-start interview script. Contains conversation outline, seed-file extraction guide, and six-domain profile creation guidelines with example. Self-deletes after cold-start completes.
- **`coldstart/playbook/`** — Operating procedures. process/ (3 files, loaded every session) defines baseline rules. contracts/ (README + review/ + templates/) grows with use.
- **`coldstart/.claude/`** — profiles/ (empty, filled by coldstart) and settings.local.json (permissions).

## Design invariants

- **Scripts self-delete after cold-start.** The cleanup step removes `scripts/`. `.claude/profiles/` stays — it's the user's permanent profile. `CLAUDE.md`, `playbook/`, `sessions/`, `records/`, `archive/`, and `.claude/settings.local.json` also remain.
- **Profiles only record what changes model output behavior.** Not contact lists, not org charts, not reference material. If a field wouldn't change how Claude responds, it doesn't belong in the profile.
- **In-house counsel only (Phase 1).** The project starts with company counsel. Law firm and academic roles are deferred to later phases.
- **Guardrails are in playbook/process/, not CLAUDE.md.** Operational rules live in `coldstart/playbook/process/` (3 files: approach, information, role). CLAUDE.md only contains personality, routing, operating procedures reference, and the workspace map.
- **Tool names are never hardcoded in guardrails.** Per commit `158a627`, all references to specific tools use placeholder terminology. The actual tool names come from the user's profile.
