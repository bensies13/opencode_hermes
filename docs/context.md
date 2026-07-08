# Durable Project Context

This file stores stable project context that should remain useful across sessions. For volatile repository state, run `scripts/update-context.ps1` and read `.agent-context/latest.md`.

## Project Scope

`opencode_hermes` is a public open-source lab for autonomous AI agent experiments. The project focuses on practical agent loops, tool-use policies, reproducible experiment notes, and lightweight evaluation methods.

## Current Assumptions

- The repository should stay implementation-neutral until a concrete experiment needs code.
- Experiment notes should be easy for another agent or human maintainer to continue.
- GitHub issues and pull requests should become the durable source for task-level context.
- Local generated context should not be committed.

## Decision Log

| Date | Decision | Reason |
| --- | --- | --- |
| 2026-07-08 | Use GitHub as the source of truth and generate local agent context with `scripts/update-context.ps1`. | Keeps Codex/OpenCode sessions aligned with the latest remote state without creating noisy commits. |
