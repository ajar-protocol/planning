# Tasks — Phase 2: Client / Kernel MVP

*Goal: one library gives any agent framework verified access to Ajar sites + consensual fallback everywhere else — with enforcement the model cannot override. References: `docs/02-ARCHITECTURE.md` §3.2, `docs/04-SECURITY-MODEL.md` §3. May overlap Phase 1 tail once T0.x artifacts are frozen.*

---

### T2.1 — Kernel skeleton & embedding surface
Library (embeddable) + sidecar mode + CLI reference agent; configuration model; structured logging. **DoD:** hello-world embed in one mainstream agent framework documented and tested.

### T2.2 — Resolver & verification pipeline
`.well-known` fetch → owner-signature check → domain binding proof → expiry/sequence → (FED-ready) inclusion-proof hook; caching with invalidation. **DoD:** all forged/tampered/rolled-back manifest vectors (T0.7) correctly rejected; TOFU behavior for non-FED sites matches spec §9 semantics.

### T2.3 — Taint Boundary
All site-derived content enters model context provenance-tagged and schema-stripped; no site string reaches tool-call arguments unmediated. **DoD:** injection corpus test — planted instructions in views never surface as executed capability requests (verified at Monitor logs, not model behavior).

### T2.4 — Policy Monitor v1 (read scope)
Deterministic pre-action check: mandate parse/verify (T0.4 schemas), domain constraints, budget accounting (global per task), rate limits; allow/deny/escalate verdicts logged. **DoD:** property-based tests — no sequence of model outputs can produce an out-of-mandate request reaching the Executor.

### T2.5 — Executor (read ops)
Signed HTTP (RFC 9421, `tag="ajar"`), negotiation, chunk-diff sync, retry/backoff, 402 recognition (defer settlement to Phase 4 — surface as policy event). **DoD:** interop against reference Gateway green across the negotiation matrix.

### T2.6 — Receipt Vault v1
Append-only, hash-chained local store: manifests seen, verdicts, requests, responses digests; export for audit. **DoD:** vault reconstruction test — a third party can replay "what did this agent do and why was it allowed" from vault alone.

### T2.7 — MCP-compatible surface
Present resolved sites as MCP resources/tools so existing MCP-based agents consume Ajar with a config change; map view index → resources, (future) actions → tools. **DoD:** stock MCP client completes a read task against a Phase-1 site with zero client code changes.

### T2.8 — Fallback Engine
CDP-pluggable headless rendering (engine per ADR-010) → extraction → same internal representation flagged `unverified`; consent guard: robots/AIPREF/402 honored, signed requests, R2/R3-equivalents require per-operation human confirmation. **DoD:** consent-guard test matrix green; unverified flag propagates to model context and vault.

### T2.9 — Mandate tooling (principal side, read scopes)
CLI to generate principal keys, issue/inspect/revoke read-scope mandates. **DoD:** round-trip: issue → Kernel loads → Monitor enforces → revoke → Monitor blocks within TTL.

### T2.10 — Conformance suite for Client
Executable harness for spec Client conformance (§11) incl. adversarial vectors. **DoD:** reference Kernel 100%; harness standalone-usable.

### T2.11 — The side-by-side demo & report
Identical multi-site research task: Ajar-enabled sites via Kernel vs. raw scraping/browser-driving; measure tokens, latency, success rate, failures. **DoD:** published quantified comparison (the Phase-2 marketing artifact) meeting ROADMAP targets.

---

**Phase exit gate:** ROADMAP Phase-2 criteria green; threat-model review (T1, T3, T4 partial, T7 hooks, T11) recorded; injection-corpus results published.
