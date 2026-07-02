# Tasks — Phase 0: Specification & Foundations

*Goal: freeze Ajar v0.1-draft well enough to implement against. Reference: `ROADMAP.md` Phase 0 and the planned `docs/03-PROTOCOL-SPEC.md`. Every task is written with a concrete Definition of Done (DoD).*

---

### T0.1 — Spec review pass & issue log
Read `docs/03-PROTOCOL-SPEC.md` end-to-end against `docs/02-ARCHITECTURE.md` and `docs/04-SECURITY-MODEL.md`; file one issue per ambiguity, contradiction, or missing MUST. **DoD:** issue list triaged; spec updated or issues explicitly deferred with rationale in DECISIONS.md.

### T0.2 — Canonicalization & signature profile
Specify exact signing procedure: JCS (RFC 8785) canonical form, Ed25519, `kid` resolution, multi-signature layout, detached vs. embedded rules for each object type. **DoD:** a worked signing example (input JSON → canonical bytes → signature) for manifest, mandate, offer, receipt, added to spec appendix; no implementation-defined gaps remain.

### T0.3 — Scope registry v1
Define the dotted mandate-scope registry: `content.*`, `commerce.*`, `communication.*`, `account.*`, `data.*` with semantics, wildcard rules, and extension procedure. **DoD:** registry table in spec; ≥20 concrete scopes defined; matching algorithm (scope ⊇ requirement) specified with edge-case examples.

### T0.4 — JSON Schemas
Extract formal JSON Schema (2020-12) for: Manifest, View/Chunk, Action, Mandate, Offer, Receipt, Policy, Error. Place in `/schemas`. **DoD:** all `/examples` validate; schemas cross-reference spec section numbers; CI job validates examples on every commit.

### T0.5 — Golden examples
Author `/examples`: complete manifests for 3 archetypes (blog CORE-only; docs site CORE+metering; commerce CORE+ACT+PAY), plus mandate/offer/receipt chains for the canonical 50-ticket scenario — **and** a set of deliberately invalid examples (bad signature, expired offer, over-cap commit, rolled-back sequence) each documenting which rule it violates. **DoD:** valid examples pass schema CI; invalid examples fail for the documented reason.

### T0.6 — Error-code registry
Define RFC 9457 problem+json profile + `Ajar-Error-Code` registry covering: verification failures, policy denials, mandate violations, offer lifecycle errors, metering challenges. **DoD:** every spec MUST-fail path has a code; codes namespaced and stable.

### T0.7 — Conformance test vectors (data, not code)
For each spec §MUST, author machine-readable vectors: input artifact(s) + expected verdict + cited spec clause. Include adversarial vectors from `04-SECURITY-MODEL` (T2, T4, T5, T8 attack shapes). **DoD:** vector file per spec section; coverage table mapping MUSTs → vectors with zero gaps.

### T0.8 — Version & extension policy
Write the compatibility contract: semver rules for `ajar_version`, unknown-field tolerance tests, `x-` extension rules, deprecation procedure. **DoD:** section added to spec; two worked examples (a compatible addition; a breaking change with migration note).

### T0.9 — Repo scaffolding & CI
Issue/PR templates referencing task IDs; CI: schema validation of examples, markdown lint, link check; `SECURITY.md` draft; `CODE_OF_CONDUCT.md`; license files (CC-BY-4.0 docs / Apache-2.0 code per ADR-011). **DoD:** CI green on main; templates include the organization contribution checklist.

### T0.10 — ADR closure
Move all ADRs in `DECISIONS.md` from `proposed` → `accepted`/`rejected` with rationale; open new ADRs for anything T0.1 surfaced. **DoD:** no ADR in `proposed`; OQ list in spec §12 updated.

---

**Phase exit gate (all must hold):**
- [ ] Two independent readers hand-write a valid manifest from spec alone; schemas validate both.
- [ ] MUST→vector coverage table complete.
- [ ] CI green; ADRs closed.
