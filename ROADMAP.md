# 05 - Roadmap: Phases 0-5

Each phase is independently useful, ships a real artifact, and has explicit exit criteria. Detailed task tickets live in `/tasks/phase-N-*.md`. Durations are working estimates for a small team with automation support; sequence and exit criteria are what matter.

---

## Guiding sequence logic

1. **Spec before code** — every component implements one contract; changing contracts after code is 10x the cost.
2. **Gateway before Client** — supply creates the ecosystem; but the Client's fallback means agents get value from day one regardless.
3. **Read before act, act before pay** — trust compounds; never ask owners to expose actions before read-mode has proven safe.
4. **Index last** — discovery only matters once there is something to discover.

## Phase 0 — Specification & Foundations (~4–6 weeks)

**Goal:** freeze Ajar v0.1-draft well enough to implement against; stand up the project.

Deliverables: finalized `03-PROTOCOL-SPEC.md`; extracted JSON Schemas (`/schemas`); golden examples (`/examples`: manifests, mandates, offers, receipts — valid *and* deliberately invalid); scope registry v1; error-code registry; conformance test-vector suite (data, not code); repo/community scaffolding (CI for schema validation of examples, issue templates, SECURITY.md draft).

Exit criteria:
- [ ] Two independent people (or agents) can hand-write a valid manifest from the spec alone, and the schemas validate both.
- [ ] Every spec §MUST has at least one conformance vector.
- [ ] All ADRs 001–012 status ≠ "proposed".

## Phase 1 — Gateway MVP: the read layer (~8–12 weeks)

**Goal:** an owner goes from ordinary website → agent-ready (Profile CORE) in under an hour, self-hosted.

Deliverables: Gateway binary + Docker (reverse-proxy shape); Harvester (sitemap/RSS/JSON-LD/OpenGraph) + Extractor (template clustering, HTML→semantic view) + optional build-time Inducer; owner key ceremony + signing; Owner Policy read-subset (tiers, rate limits, path exposure); same-URL content negotiation with chunk diffs; Console v1 (coverage report, policy editor, audit log); WordPress plugin (Tier-1 path); conformance suite (executable) for Gateway/CORE.

Exit criteria:
- [ ] 10 real, structurally diverse sites (blog, docs, e-commerce, news, SPA) reach ≥90% content coverage with ≤1h owner effort each.
- [ ] Token cost of agent consumption ≤20% of raw-HTML baseline on those sites.
- [ ] A skeptical owner can answer "what exactly is exposed, to whom?" from the Console alone.
- [ ] Security: threat-model review T1–T3, T6 controls implemented; SECURITY.md published.

## Phase 2 — Client/Kernel MVP (~8–12 weeks, overlaps Phase 1 tail)

**Goal:** any agent framework can consume Ajar sites verified, and the rest of the web via consensual fallback — through one library.

Deliverables: Kernel library (Resolver + verification pipeline; Taint Boundary; Receipt Vault v1) + CLI reference agent; MCP-compatible surface (the Kernel presents sites as MCP tools/resources to existing agent stacks); Fallback Engine behind CDP (pluggable Lightpanda/Chromium) honoring robots/AIPREF/402; Policy Monitor v1 (read-scope mandates: domains, budgets, rate).

Exit criteria:
- [ ] One config change gives an existing MCP-based agent verified access to Phase-1 sites + flagged fallback elsewhere.
- [ ] Verification pipeline rejects all forged/tampered/rolled-back manifest vectors.
- [ ] Model-emitted "requests" outside mandate are provably blocked by the Monitor in adversarial tests (injection corpus).
- [ ] Side-by-side demo: identical task, Ajar site vs. raw scraping — cost, latency, reliability quantified and published.

## Phase 3 — The Action Layer (~10–14 weeks)

**Goal:** safe operations: typed actions, SIMULATE, two-phase commit, mandates, receipts (Profile ACT).

Deliverables: Gateway action runtime (endpoint binding to backend APIs/forms; SIMULATE fidelity harness; offer/commit state machine; mandate verification; receipt issuance; approval queue in Console); Action Drafter (forms/OpenAPI → drafts, owner wiring UI); Kernel: Simulator, full Policy Monitor (scopes/caps/risk gates/escalation), Executor with idempotency + dual signing, Receipt Vault v2 (disputes export); mandate tooling for principals (issue/revoke CLI + minimal UI); conformance ACT suite incl. adversarial vectors.

Exit criteria:
- [ ] End-to-end canonical scenario (50-ticket purchase walkthrough, minus real money) runs across independent Gateway and Kernel deployments.
- [ ] SIMULATE-vs-commit divergence tests: zero undeclared effects across the suite.
- [ ] External security review of Profile ACT completed; findings resolved or accepted in DECISIONS.md.
- [ ] Liability walkthrough: for each §8.4 case, the signed artifacts alone let a third party assign fault.

## Phase 4 — Economics & Discovery (~10–14 weeks)

**Goal:** metering + settlement (Profile PAY) and federated discovery (Profile FED).

Deliverables: 402 metering in Gateway with settlement adapters (x402 first; AP2-card adapter interface); Kernel budget-aware payment flow bound to receipts; transparency log reference implementation + monitor; Index search node (log crawler, capability embedding, query API + inclusion proofs); Kernel Resolver integration (multi-node query, proof checking); operational playbooks for third parties running logs/nodes.

Exit criteria:
- [ ] Real (testnet/sandbox) end-to-end paid action with settlement proof referencing the receipt.
- [ ] ≥2 independent log operators + ≥2 index nodes interoperating; forged-manifest injected into ecosystem is detected by monitors within target window.
- [ ] Capability query benchmark: relevant site discovered among ≥1k indexed manifests in <1s.

## Phase 5 — Ecosystem & Governance (ongoing)

**Goal:** make Ajar bigger than its authors.

Workstreams: org↔org standing mandates (recurring invoices reference scenario) + A2A interop; additional Gateway shapes (Shopify app, edge workers, static-site generators); framework integrations over the Kernel; WebMCP/ANP/NLWeb interop bridges; spec v1.0 hardening from field feedback; **governance transfer**: submit spec to a neutral venue (W3C CG and/or Linux Foundation — ADR-012), establish maintainer structure independent of founders; adoption program (registrar/hoster partnerships, "agent-ready" badge + public conformance directory).

Exit criteria (directional):
- [ ] ≥3 external organizations run Gateways in production; ≥2 external contributors have merge rights.
- [ ] Spec governance formally lodged outside the founding team.
- [ ] One real org↔org standing-mandate deployment (two companies, recurring settlement, zero human touches per cycle).

## Cross-phase tracks (never stop)

- **Security:** threat-model delta review at every phase gate; adversarial vectors grow monotonically.
- **Docs:** spec, glossary, and ADRs updated in the same PR as any behavior change.
- **Positioning:** quarterly refresh of 01-RESEARCH; watch Cloudflare action-layer moves, W3C AI Agent Protocol CG, MCP releases.

## North-star success metrics

| Metric | Phase 1–2 target | Phase 4–5 target |
|---|---|---|
| Owner time to agent-ready (read) | < 1 hour | < 15 minutes |
| Agent token cost vs raw HTML | ≤ 20% | ≤ 10% |
| Action success rate on conformant sites | — | > 99% (vs ~60–80% typical UI-driving) |
| Undeclared-effect incidents in conformance | 0 | 0 |
| Independent Gateway deployments | 10 pilot | 1,000+ |
| Independent log/index operators | — | ≥ 5 |
