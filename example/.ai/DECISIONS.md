# Architectural Decision Records

> Claude may **propose** entries here, but must not add them without human approval.

---

## ADR-001 — Use math/big.Rat for all monetary values

- **Date**: 2025-02-10
- **Status**: Accepted
- **Context**: Invoice totals involve multiplication of hourly rates by hours worked, then summing across line items. Float arithmetic causes rounding errors that appear in PDF output.
- **Decision**: All monetary values use `math/big.Rat` internally; converted to string only at PDF render time.
- **Rationale**: Exact arithmetic, no rounding surprises, standard Go library.
- **Trade-offs**: Slightly more verbose code; no performance concern at this scale.

---

## ADR-002 — Switch PDF library from signintech/gopdf to jung-kurt/gofpdf

- **Date**: 2025-03-04
- **Status**: Accepted
- **Context**: `signintech/gopdf` lacked support for multi-cell text wrapping needed for long item descriptions. Workarounds were brittle.
- **Decision**: Migrated to `jung-kurt/gofpdf` which handles text flow natively.
- **Rationale**: Better layout control, active maintenance, good documentation.
- **Trade-offs**: Required rewriting `pdf/generator.go` (~150 lines). One-time cost.
- **Superseded by**: —
