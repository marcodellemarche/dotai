# Errors & Lessons Learned

> Updated by Claude when a significant mistake or wasted effort occurs.
> The goal is to make each error a one-time event.

---

## 2025-03-05 — Loop on PDF layout for multi-line cells

- **What happened**: Spent ~6 exchanges trying variations of `CellFormat` to get text wrapping right, without resolving it. Each attempt slightly changed parameters without a clear hypothesis.
- **Root cause**: No concrete example of expected output was provided. Claude was guessing at desired behavior.
- **Systemic fix**: For any layout/rendering task, always provide a concrete example of expected output (screenshot, ASCII sketch, or explicit measurements) before starting. Add to CLAUDE.md anti-patterns if it recurs.

---

## 2025-02-28 — Modified parser without running tests first

- **What happened**: Refactored `parser/parser.go` to support a new field, which silently broke existing tests. Only caught after 3 more exchanges.
- **Root cause**: Skipped the "run tests before touching fragile files" rule.
- **Systemic fix**: Added explicit anti-pattern to CLAUDE.md: "Don't modify `parser/parser.go` without first running `go test ./parser/...`"
