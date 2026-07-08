# Agent Instructions

This repository is an open-source autonomous AI agent experiment lab. Treat the GitHub repository as the source of truth for current project context.

## Required Conversation-Start Routine

At the start of a new conversation or work session in this repository, before making project-specific changes:

1. Run `powershell -NoProfile -ExecutionPolicy Bypass -File scripts/update-context.ps1`.
2. Read `.agent-context/latest.md`.
3. Check `git status --short --branch`.
4. If the script reports skipped sync because the worktree is dirty, do not overwrite local changes. Fetch-only context is acceptable until the user decides what to do.

The refresh script may fast-forward the current branch only when the worktree is clean and an upstream branch exists. It must not rebase, merge, reset, or discard user work.

Do not set up scheduled or background context refresh jobs unless the user explicitly asks for them.

## Project Priorities

- Prefer small experiments with clear hypotheses over broad rewrites.
- Keep reproducibility notes close to the experiment that produced them.
- Record agent behavior, tool usage, failure modes, and evaluation criteria explicitly.
- Use GitHub issues for durable follow-ups and open questions.
- Keep generated local context in `.agent-context/` and do not commit it.

## Implementation Guidance

- Preserve existing user changes.
- Use scripts and structured data where practical instead of ad hoc manual state.
- Add tests or evaluation checks when an experiment graduates into reusable code.
- Document commands that produced meaningful observations.

## Naming

Use `opencode_hermes` for the repository and project identity. Use `Hermes Agent` or `OpenClaw-style` only as inspiration labels unless this repository imports or vendors a specific upstream project.
