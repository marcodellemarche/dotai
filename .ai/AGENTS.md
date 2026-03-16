# AI Agents Architecture

> How the AI agent system is designed, why, and how to use it.

---

## Design philosophy

The agent system follows a **plan → implement → review** pipeline, with the human as the decision point between stages. This gives the human full control over routing: expensive models do the thinking, cheaper models do the typing.

Plans are persisted as files in `.ai/plans/`, making them portable between tools and sessions.

---

## Agent configuration

### planner

**Model:** [configurable] | **Mode:** Read-only (writes only to `.ai/plans/`) | **Available in:** Claude Code, OpenCode

Dual-purpose agent:

- **Explore mode** — answers "how does X work?" by reading architecture docs and tracing source code
- **Plan mode** — produces step-by-step implementation plans saved to `.ai/plans/` with exact file paths, type names, and line references

After producing a plan, presents the human with a choice of execution path.

**When to use:** Architecture questions, understanding data flows, planning features or refactors.

### code-writer

**Model:** [configurable] | **Mode:** Read-write | **Available in:** Claude Code, OpenCode

Reads plan files from `.ai/plans/` and implements them step-by-step. Runs appropriate lint/build commands after each step and tests when done. Reports deviations from the plan.

**When to use:** After the planner has produced a plan file and the human has approved it.

### code-reviewer

**Model:** [configurable] | **Mode:** Read-only | **Available in:** Claude Code, OpenCode

Systematic code review against a checklist: correctness, safety, performance, idiomatic code, error handling, concurrency.

Can run lint, typecheck, and git commands but never modifies files.

**When to use:** After implementation, before committing. Also useful for reviewing existing code.

### web-researcher

**Model:** [configurable] | **Mode:** Read-only | **Available in:** Claude Code, OpenCode

Web research using search tools. Specializes in ecosystem research: library evaluation, best practices.

Always cites sources and flags uncertain information.

**When to use:** Evaluating libraries, looking up external APIs, researching approaches before planning.

---

## Plan files

Plans are saved to `.ai/plans/` as markdown files with descriptive names (e.g., `feature-name.md`). This makes them:

- **Portable** — both Claude Code and OpenCode can read them
- **Persistent** — survive across sessions
- **Reviewable** — human can read and edit them before execution
- **Trackable** — git tracks plan history

---

## Typical workflows

### Feature implementation (delegated)

1. Human describes the feature
2. **planner** explores the codebase, saves a plan to `.ai/plans/`
3. Human reviews the plan file, chooses execution path
4. **code-writer** reads the plan file and implements step-by-step
5. **code-reviewer** reviews the result
6. Human commits

### Research-first task

1. **web-researcher** investigates approaches (e.g., "best library for X")
2. Human picks an approach
3. **planner** creates the implementation plan
4. **code-writer** implements
5. **code-reviewer** reviews

### Architecture review

1. **planner** (explore mode) traces a data flow or explains a subsystem
2. Human asks follow-up questions
3. If changes needed: **planner** switches to plan mode

---

## Tool availability

| Agent | Claude Code | OpenCode |
|---|---|---|
| planner | Yes | Yes (subagent) |
| code-writer | Yes | Yes (subagent) |
| code-reviewer | Yes | Yes (subagent) |
| web-researcher | Yes | Yes (subagent) |

All agents reference the shared `.ai/` documentation (CONVENTIONS.md, architecture docs, HOW_TO_DEVELOP.md). This means both Claude Code and OpenCode agents have the same project knowledge.

---

## Cost optimization

The model assignment is deliberate:

| Task | Complexity | Model | Why |
|---|---|---|---|
| Planning | High | Advanced | Bad plans waste everything downstream |
| Reviewing | High | Advanced | Missed bugs are expensive |
| Implementing | Medium | Fast | Following a clear plan is mechanical |
| Research | Low | Fast | Search tools do the heavy lifting |

---

## Customization

To configure specific models for your project, update the agent descriptions above with your chosen models. Common choices:

- **Thinking tasks:** Claude 4 Opus, GPT-4
- **Typing tasks:** Claude 3.5 Sonnet, GPT-4 Turbo, Gemini Pro
- **Research:** Gemini, Perplexity
