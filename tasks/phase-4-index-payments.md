# Tasks — Phase 4: Economics & Discovery (Profiles PAY + FED)

*Goal: native metering with pluggable settlement, and federated transparency + capability search. References: spec §§3 (federation), 8.3, 10; `docs/02-ARCHITECTURE.md` §3.3; `docs/04-SECURITY-MODEL.md` T2/T7/T9.*

---

## Track PAY — Economics

### T4.1 — Metering engine (Gateway)
Per-tier/per-resource/per-action pricing from Owner Policy; 402 challenge with `Ajar-Price` + settlement-adapter negotiation headers (x402-compatible); free-quota grants; simulate-call pricing. **DoD:** pricing matrix tests; challenge/response interop with a stock x402 client.

### T4.2 — Settlement adapter interface + x402 adapter
Adapter trait: challenge → payment proof → verification → receipt linkage; x402 first (testnet/sandbox). **DoD:** paid read and paid action E2E on sandbox; settlement proof embedded/reference-linked in receipt.

### T4.3 — AP2-card adapter (interface + pilot)
Map Ajar mandate + offer → AP2 mandate flow for card rails; document impedance (user-key vs agent-key binding). **DoD:** design-complete + sandbox pilot or explicitly parked with ADR documenting blockers.

### T4.4 — Kernel payment flow
Budget-aware 402 handling inside Monitor accounting (payments count against caps); principal payment-method custody rules; per-payment vault entries. **DoD:** budget-drain adversarial tests (T9) — cumulative spend can never exceed `caps.total` across mixed reads/actions/settlements.

### T4.5 — Owner revenue console
Earnings by tier/operator/resource; payout config; reconciliation export against receipts. **DoD:** console totals equal receipt-derived totals in tests.

## Track FED — Discovery

### T4.6 — Transparency log reference implementation
Append-only Merkle log (CT-style): submission (manifest publications/updates/revocations), STH issuance, inclusion + consistency proofs; gossip endpoint. **DoD:** proofs verify against independent implementation of the verifier; split-view detection demonstrated between two test logs.

### T4.7 — Monitor service
Watches logs for: conflicting manifests per domain, unexpected key changes, revocation anomalies; owner subscription alerts (Console integration). **DoD:** injected impersonation manifest detected + alerted within target window in E2E test.

### T4.8 — Gateway log integration
Publish on sign/rotate/revoke; embed `latest_sth` reference; retry/consistency handling. **DoD:** FED conformance vectors pass; log outage degrades gracefully (serving continues; publication queued).

### T4.9 — Index search node
Log crawler → manifest fetch+verify → capability text/embedding index → query API returning candidates + log proofs; pluggable embedding backend; node operation playbook. **DoD:** relevance benchmark: correct site in top-k for a curated query set over ≥1k indexed manifests, <1s.

### T4.10 — Kernel Resolver v2
Multi-node query, proof verification, cross-node result reconciliation, node reputation heuristics; privacy notes (OQ-4 interim mitigations). **DoD:** eclipse-resistance test — a malicious node omitting/forging results cannot cause acceptance of an unverifiable site.

### T4.11 — Federation bootstrap
Recruit/operate ≥2 independent log operators + ≥2 index nodes (universities/companies/community); publish operator playbooks. **DoD:** interop demonstrated across independently operated infrastructure.

---

**Phase exit gate:** ROADMAP Phase-4 criteria green; threat-model review (T2 full, T7, T9, T10) recorded; discovery + paid-action demos published.
