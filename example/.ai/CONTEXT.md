# Project Context

> Updated by Claude at the end of every productive session.
> Last updated: 2025-03-05

---

## Current state

Core parsing pipeline is complete and tested. PDF generation works for single-client invoices. Multi-client batching is partially implemented — the aggregation logic in `cmd/batch.go` is written but not yet wired to the PDF generator. All existing tests pass.

---

## Next steps (in order)

1. Wire `batch.go` aggregation output to `pdf/generator.go`
2. Add integration test: multi-client JSON → multiple PDFs
3. Handle edge case: client with zero billable hours in a month (currently panics)

---

## Recent decisions

- [2025-03-04] Switched from `signintech/gopdf` to `jung-kurt/gofpdf` — see DECISIONS.md ADR-002
- [2025-02-28] Deferred multi-currency support until after v1.0 is stable

---

## Active warnings

- `cmd/batch.go` is incomplete — do not treat it as working code
- Zero-hours edge case causes a nil pointer panic in `pdf/generator.go:112` — known, not yet fixed
