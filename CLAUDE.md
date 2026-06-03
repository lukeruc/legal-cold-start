# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## About

This is the **infrastructure layer** of the Legal AI Workstation. The `coldstart/` directory is the installer — users copy it to their work directory, run the cold-start interview, and get a complete legal working environment (CLAUDE.md, .claude/profiles/, playbook/, sessions/, records/, archive/).

Design documentation is maintained separately from the project files.

Five function modules (contract-review, contract-analyze, rule-builder, evidence-organizer, document-drafter) are separate repos that consume this infrastructure via path and field contracts. They live as Claude Code skills under `.claude/skills/` in the user's work directory.

## Development — no build, no tests, no lint

This is a pure configuration project. There is no build step, no test suite, no CI. Development consists of editing Markdown and JSON files.

## Key files and their relationships

- **`coldstart/CLAUDE.md`** — The main agent instructions that ship to users. Contains the startup state machine (check `STATUS: UNCONFIGURED`, placeholders, pause markers), the shared guardrails (citation provenance, currency trigger, jurisdiction detection, etc.), and the profile update mechanism.
- **`coldstart/scripts/*.md`** — Cold-start interview script (in-house). Loaded by CLAUDE.md only during cold-start. Covers: seed-file extraction guide, dialogue outline, tool verification protocol, pause support, and the final write-to-profile template.
- **`coldstart/scripts/in-house.md`** — Cold-start interview script. Contains conversation outline, seed-file extraction guide, and profile creation guidelines (six domains). Self-deletes after cold-start completes.
- **`coldstart/.claude/settings.local.json`** — Permissions for the deployed template (currently only allows `git` commands).

## Design invariants

- **Scripts self-delete after cold-start.** The cleanup step removes `scripts/`. `.claude/profiles/` stays — it's the user's permanent profile. `CLAUDE.md`, `playbook/`, `sessions/`, `records/`, `archive/`, and `.claude/settings.local.json` also remain.
- **Profiles only record what changes model output behavior.** Not contact lists, not org charts, not reference material. If a field wouldn't change how Claude responds, it doesn't belong in the profile.
- **In-house counsel only (Phase 1).** The project starts with company counsel. Law firm and academic roles are deferred to later phases.
- **Guardrails are in playbook/process/, not CLAUDE.md.** Per the concept design (Phase 1), operational rules live in `coldstart/playbook/process/` (6 files). CLAUDE.md only contains personality, routing, and the system map. Guardrails apply whether or not a profile has been configured.
- **Tool names are never hardcoded in guardrails.** Per commit `158a627`, all references to specific tools use placeholder terminology. The actual tool names come from the user's profile.
