# Tasks

> Updated by Claude during active work. Priorities set by human.

---

## In progress

- [ ] Wire `batch.go` aggregation output to `pdf/generator.go` — started 2025-03-05

## Up next

- [ ] Integration test: multi-client JSON → multiple PDFs
- [ ] Fix zero-hours edge case panic in `pdf/generator.go:112`

## Blocked

- [ ] CLI flag for custom output directory — blocked by: needs decision on config file format first (discuss with human)

## Done

- [x] Core JSON parsing pipeline — completed 2025-02-15
- [x] Single-client PDF generation — completed 2025-02-20
- [x] Migrate PDF library to gofpdf — completed 2025-03-04
