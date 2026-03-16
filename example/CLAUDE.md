# Invoicer — AI Briefing

> This file is the first thing any AI reads at the start of every session.
> **Only the human should modify this file.**

---

## What is this project

A CLI tool written in Go that parses timesheet JSON exports and generates PDF invoices. Targets freelancers billing multiple clients per month. Runs locally, no server.

---

## Stack & technical constraints

- Go 1.22, modules enabled
- `math/big` for all monetary arithmetic — never use float64 for money
- PDF generation via `jung-kurt/gofpdf`
- No external HTTP dependencies — fully offline
- Output files go to `./out/`, never committed to git

---

## Multi-LLM agent setup

This project uses multiple AI agents in a **plan → implement → review** pipeline. See `.ai/AGENTS.md` for the full configuration.

| Task | Agent | Model |
|---|---|---|
| Planning, architecture | planner | Opus |
| Implementation | code-writer | Sonnet |
| Code review | code-reviewer | Opus |

---

## How to start every session

1. Read this file fully
2. Read `.ai/AGENTS.md` (for agent configuration)
3. Read `.ai/CONTEXT.md` (for current state)
4. Confirm understanding: summarize current state and ask "What do you want to work on this session?"
5. Work on **one objective per session** — if the scope expands, flag it and ask

---

## Operational rules (always enforce)

- **One task per session.** Do not start a second objective without explicit confirmation.
- **2-attempt rule.** If a problem isn't solved after 2 attempts, stop. Describe what you tried and why it didn't work. Do not keep iterating blindly.
- **Checkpoint every ~10 exchanges.** Pause and ask: "Am I still heading in the right direction?"
- **When in doubt, ask.** Never assume intent on architectural or structural decisions.
- **No gold-plating.** Don't improve things that weren't asked. Stay in scope.
- **Discoveries stay in scope.** If you find something broken or fragile outside the current task, note it in `.ai/TASKS.md` or `CONTEXT.md > Active warnings` — don't fix it unless explicitly asked.
- **Use the right agent.** Planning and reviewing should use the planner/code-reviewer agents. Only use code-writer for implementation from approved plans.

---

## File ownership

AI **may** update autonomously:
- `.ai/CONTEXT.md` — at the end of every productive session
- `.ai/ERRORS.md` — whenever a significant mistake or wasted effort occurs
- `.ai/TASKS.md` — task status during active work

AI **must never** modify autonomously:
- `CLAUDE.md` — human-only
- `.ai/DECISIONS.md` — human approval required (AI may *propose* an ADR, but not add it unilaterally)

---

## Known anti-patterns

- Never use `float64` for monetary values — use `math/big.Rat` always
- Don't modify `parser/parser.go` without first running `go test ./parser/...`
- The `ClientConfig` struct is the source of truth for client data — don't duplicate fields elsewhere

---

## References

- `.ai/AGENTS.md` — multi-LLM agent configuration
- `.ai/CONTEXT.md` — current project state
- `.ai/DECISIONS.md` — architectural decisions and rationale
- `.ai/ERRORS.md` — past mistakes and systemic fixes
- `.ai/TASKS.md` — current tasks and priorities
- `.ai/plans/` — implementation plans (when using plan → implement workflow)
