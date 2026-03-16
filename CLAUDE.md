# [Project Name] — AI Briefing

> This file is the first thing any AI reads at the start of every session.
> **Only the human should modify this file.**

---

## What is this project

[2–3 sentences max. What it does, who it's for, what problem it solves.]

---

## Stack & technical constraints

- [Language / runtime / version]
- [Key frameworks or libraries]
- Deploy target / environment
- Hard constraints: no X, always use Y, etc.

---

## Multi-LLM agent setup

This project uses multiple AI agents in a **plan → implement → review** pipeline. See `.ai/AGENTS.md` for the full configuration.

| Task | Agent | Model |
|---|---|---|
| Planning, architecture | planner | [model] |
| Implementation | code-writer | [model] |
| Code review | code-reviewer | [model] |
| Web research | web-researcher | [model] |

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
- **2-attempt rule.** If a problem isn't solved after 2 attempts, stop. Describe what you tried and why it didn't work. Log it in `.ai/ERRORS.md` and wait for human guidance before continuing.
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

<!-- Permanent project-level rules that apply for the life of the project.
     For temporary issues (incomplete work, fragile state), use CONTEXT.md > Active warnings instead. -->
<!-- Add project-specific pitfalls as they emerge -->
- [e.g. "Don't modify X directly, always go through Y"]
- [e.g. "File Z is fragile — don't touch without running tests first"]

---

## References

- `.ai/AGENTS.md` — multi-LLM agent configuration
- `.ai/CONTEXT.md` — current project state
- `.ai/DECISIONS.md` — architectural decisions and rationale
- `.ai/ERRORS.md` — past mistakes and systemic fixes
- `.ai/TASKS.md` — current tasks and priorities
- `.ai/plans/` — implementation plans (when using plan → implement workflow)
