# dotai

A minimal, opinionated template for working effectively with AI agents on software projects across multiple sessions. Designed for multi-LLM setups where different models handle different tasks.

## The problem it solves

AI agents have no persistent memory between sessions. Without structure, every session starts from scratch — losing context, repeating mistakes, and drifting from the project's direction. This template externalizes the agent's memory into the repository itself, with support for multiple AI models working together.

## Structure

```text
your-repo/
├── CLAUDE.md              # AI's briefing — read first, every session
└── .ai/
    ├── AGENTS.md          # Multi-LLM agent configuration
    ├── commands/
    │   ├── start.md       # /start — begin a session
    │   └── end.md         # /end — close a session and update context
    ├── CONVENTIONS.md     # Project-specific coding conventions
    ├── CONTEXT.md         # Current project state (updated by AI)
    ├── DECISIONS.md       # Architectural decisions / ADRs (human-approved)
    ├── ERRORS.md          # Past mistakes and systemic fixes (updated by AI)
    ├── TASKS.md           # Task list and status (updated by AI)
    ├── plans/             # Implementation plans (for plan → implement → review workflow)
    └── architecture/      # Architectural documentation
```

The `example/` directory contains a fully filled-in version for reference.

## Multi-LLM Agent System

This template supports a **plan → implement → review** pipeline where different AI models handle different tasks:

- **Planning** (Opus/advanced model) — Architecture, exploration, planning
- **Implementation** (Sonnet/fast model) — Writing code from plans
- **Review** (Opus/advanced model) — Code review, safety checks
- **Research** (specialized) — Web research, crate evaluation

See `.ai/AGENTS.md` for the full agent configuration.

## File ownership

| File | Who updates it |
|---|---|
| `CLAUDE.md` | Human only |
| `.ai/DECISIONS.md` | Human only (AI may propose, not commit) |
| `.ai/CONTEXT.md` | AI, at the end of each productive session |
| `.ai/ERRORS.md` | AI, when a significant mistake occurs |
| `.ai/TASKS.md` | AI, during active work |

## How to use

**Option A — Let an AI bootstrap it (recommended).** Open Claude Code or OpenCode inside your repo and paste this prompt:

> Read https://github.com/marcodellemarche/dotai — study the template structure and the `example/` directory — then create `CLAUDE.md` and `.ai/` in this repo. Fill in the project-specific sections based on what you can infer from the codebase. Ask about anything you're uncertain of, or leave it as a placeholder.

**Option B — Manual setup.**

1. Copy `CLAUDE.md` and `.ai/` into your repo
2. Fill in `CLAUDE.md` with your project's specifics

**Starting a session.** Type `/start` in your AI tool (requires `.ai/commands/start.md` to be present). Or use the equivalent prompt manually: *"Read CLAUDE.md and CONTEXT.md, summarize the current state, and ask what we're working on today."*

**Ending a session.** Type `/end` to trigger the end-of-session ritual: AI updates `CONTEXT.md` and `TASKS.md`, and drafts any ADRs in the chat for your review.

## Key principles

- **Short sessions beat long ones.** A fresh session with good context outperforms a bloated one.
- **Errors are assets.** Document them in `ERRORS.md` so they don't repeat.
- **One task per session.** Scope creep is the main source of loops and drift.
- **Checkpoints matter.** Every ~10 exchanges, explicitly ask: *"Are we still on track?"*
- **Multiple models, multiple strengths.** Use expensive models for thinking, cheap models for typing.
