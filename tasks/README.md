# /tasks - Phase task tickets

These six files are the master task breakdowns for Phases 0–5. They live here in `planning` as the single backlog; as each phase starts, convert its tasks into GitHub issues **in the repo that owns them**:

| File | Destination repo(s) |
|---|---|
| phase-0-spec.md | `ajar` (spec) + `conformance` (T0.7) |
| phase-1-gateway.md | `ajar-gateway` (+`ajar-woocommerce` for T1.13) |
| phase-2-client.md | `ajar-kernel` |
| phase-3-actions.md | split: `ajar-gateway` / `ajar-kernel` / `ajar` (spec) / `conformance` |
| phase-4-index-payments.md | `ajar-index` (create the repo at Phase-4 start) + `ajar-gateway`/`ajar-kernel` |
| phase-5-ecosystem.md | `planning` + respective repos |

Path convention: references like `docs/03-PROTOCOL-SPEC.md` resolve inside the public **`ajar` spec repo**; `CONVERSION-PIPELINE.md` will live in **`ajar-gateway`**; `ROADMAP.md` lives here in **`planning`**.
