# Tasks — Phase 1: Gateway MVP (Read Layer, Profile CORE)

*Goal: owner goes from ordinary website → agent-ready in under an hour, self-hosted. References: `docs/02-ARCHITECTURE.md` §3.1, `docs/05-OWNER-CONTROL.md`, `ajar-gateway/CONVERSION-PIPELINE.md`. Language/stack per ADR-009 (Rust core recommended). Each task lists dependencies; parallelize freely otherwise.*

---

## Track A — Core serving

### T1.1 — Gateway skeleton (reverse-proxy shape)
Single binary + Docker image; config file; proxies the origin site untouched by default; health/metrics endpoints. **DoD:** existing site behind Gateway is byte-identical for browsers; deploy docs tested on clean VM.

### T1.2 — Manifest serving & lifecycle
Serve `/.well-known/ajar.json` from signed store; sequence/expiry handling; `Link` header advertisement. Depends: T0.4. **DoD:** conformance vectors for manifest retrieval/verification pass.

### T1.3 — Content negotiation layer
Same-URL negotiation: `Accept: application/ajar+json | text/markdown` → semantic view; else pass-through. Chunked responses, stable IDs, per-chunk hashes, `Ajar-Content-Signature`, ETag + chunk-diff sync. Depends: T1.5/T1.6 output format. **DoD:** negotiation matrix tests pass; diff sync returns only changed chunks in test scenario.

### T1.4 — View index endpoint
Machine sitemap: chunk map + hashes + change hints. **DoD:** a client can plan a full or incremental sync from the index alone.

## Track B — Conversion pipeline

### T1.5 — Harvester (Tier 1)
Collectors: sitemap.xml, RSS/Atom, JSON-LD/schema.org, OpenGraph, robots. Normalize into internal content model. **DoD:** on pilot sites with structured data, harvested fields populate views without Tier-2 parsing; provenance recorded per field.

### T1.6 — Extractor (Tier 2)
Crawl-once (politeness + budget); JS rendering only where needed via CDP interface (pluggable engine per ADR-010), cached; boilerplate stripping; HTML→semantic-markdown; **template clustering** by DOM signature; per-template rule application; stable chunk-ID derivation. **DoD:** clustering assigns ≥95% of pages of a pilot commerce site to ≤20 templates; extraction accuracy vs. hand-labeled ground truth ≥ threshold per template.

### T1.7 — Inducer (Tier 3, build-time LLM)
Provider-pluggable LLM step: label fields on N samples/cluster → emit deterministic extraction rules + draft descriptions; held-out validation; confidence flags for owner review. Hard rule: output is config; nothing model-dependent in serve path. **DoD:** on a template Tier 2 can't fully parse, induced rules reach threshold accuracy on held-out pages; rules are inspectable text artifacts.

### T1.8 — Drift detection & re-induction
Scheduled spot-render + hash/field sanity comparison; on drift: quarantine affected views, trigger re-induction, notify owner. **DoD:** synthetic template change is detected within one cycle; stale views never served silently.

## Track C — Owner sovereignty

### T1.9 — Key ceremony & signer
Guided owner-key generation (offline guidance), operational key issuance/rotation/revocation, signing service used by all modules; OS keystore/HSM hooks. **DoD:** ceremony documented + tested; no module signs outside the signer; revocation propagates to manifest + logs stub.

### T1.10 — Owner Policy engine (read subset)
Parse/validate policy doc (schemas from T0.4); enforce per-request: exposure match, audience tier (RFC 9421 verification), rate limits, path exclusions; authenticated-area exclusion by default with tests. **DoD:** policy conformance vectors pass; the `/account/**` class of content is unexposable without explicit override + warning.

### T1.11 — Console v1
Web UI: coverage report, plain-language exposure summary ("agents can read X, Y; nothing else"), policy editor with preview/revert, draft-manifest review + sign flow, audit log viewer, kill switch. **DoD:** the Phase-1 owner journey (docs/06 §4 steps 1–5) is completable start-to-finish without touching config files.

### T1.12 — Audit log
Append-only interaction log (who/tier/what/verdict), retention config, export. **DoD:** every agent-facing request appears exactly once; tamper-evidence (hash chain) verified in tests.

## Track D — Ecosystem & proof

### T1.13 — WordPress plugin (Tier-1 flagship)
Reads posts/pages/products via WP internals; emits views + draft manifest; hooks for freshness; bundles or fronts the Gateway serving layer. **DoD:** fresh WP site → agent-ready CORE in <15 min in recorded demo.

### T1.14 — Conformance suite (executable) for Gateway/CORE
Turn T0.7 vectors into an executable harness any Gateway implementation can run. **DoD:** reference Gateway passes 100%; harness usable standalone by third parties.

### T1.15 — Pilot program & benchmark report
Deploy on 10 structurally diverse real sites (blog, docs, e-commerce, news, SPA...); measure coverage, owner effort, token cost vs. raw HTML. **DoD:** published report hitting ROADMAP Phase-1 exit numbers, or documented gap analysis feeding fixes.

---

**Phase exit gate:** ROADMAP Phase-1 exit criteria all green; threat-model review (T1–T3, T6) recorded in DECISIONS.md; SECURITY.md published.
