# opencode_hermes

Open-source lab for autonomous AI agent experiments.

This repository is for comparing, adapting, and testing open-source autonomous agent ideas in the spirit of Hermes Agent, OpenClaw-style projects, and adjacent tool-using agent systems. The first priority is learning quickly while keeping experiments reproducible and easy to review.

## Goals

- Track useful open-source autonomous agent patterns.
- Run small, reversible experiments before promoting anything to a larger implementation.
- Check agent context against the GitHub repository at the beginning of each project conversation or work session.
- Record hypotheses, outcomes, and follow-up tasks in a format future agents can reuse.

## Context Refresh

GitHub is the source of truth for this project. At the start of a new conversation or work session in this project, run:

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File scripts/update-context.ps1
```

The script fetches the latest repository state, fast-forwards the local branch when it is safe, and writes a local context snapshot to `.agent-context/latest.md`.

This is a session-start check, not a scheduled background refresh.

The `.agent-context` directory is intentionally ignored by Git. It is a local working memory cache for Codex, OpenCode, and other agents, not project history.

## Experiment Flow

1. At session start, refresh context with `scripts/update-context.ps1`.
2. Create or update an experiment note under `experiments/`.
3. Keep changes small enough to inspect.
4. Record assumptions, commands, observations, and next steps.
5. Open issues for follow-up work that should survive beyond the current session.

## Guides

- [Hermes Agent beginner guide, Korean](docs/hermes-agent-beginner-guide-ko.md): detailed copy-paste setup and first-use guide for Windows, macOS, and Ubuntu.

## Repository Layout

- `AGENTS.md`: project instructions for AI coding agents.
- `docs/context.md`: durable project context and decision log.
- `experiments/`: experiment plans, notes, and outcomes.
- `scripts/update-context.ps1`: local context refresh script.

## License

MIT
