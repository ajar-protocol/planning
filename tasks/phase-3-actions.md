# Tasks — Phase 3: The Action Layer (Profile ACT)

*Goal: safe operations end-to-end — typed actions, SIMULATE, two-phase commit, mandates, dual-signed receipts. References: spec §§5–8, `docs/04-SECURITY-MODEL.md` T4/T5/T8, `docs/05-OWNER-CONTROL.md` Axis 4. This phase gets an external security review before being declared stable.*

---

## Gateway side

### T3.1 — Action runtime & endpoint binding
Execute manifest-declared actions against owner-wired backends (HTTP APIs first; form-POST adapter second); input/output schema validation; per-action audience/tier checks. **DoD:** declared-schema violations rejected with registry error codes; no undeclared parameter passthrough.

### T3.2 — SIMULATE engine
`Ajar-Mode: simulate` path: zero-side-effect guarantee (enforced at adapter layer — read-only backend calls or sandbox), resolved effects + total cost + validity window response. **DoD:** side-effect detector tests (DB/state diff before-after) prove zero mutation across the suite; fidelity harness compares simulate vs. subsequent offer for identical inputs.

### T3.3 — Offer/Commit state machine
Propose → signed Offer (freeze window, single-use) → Commit (verify agent countersignature over offer‖mandate) → execute → Receipt; abort/expiry paths fail closed; idempotency-key dedup. **DoD:** state-machine model-checked or exhaustively property-tested: no path commits an expired, replayed, or unproposed offer.

### T3.4 — Mandate verification module
Signature, validity, scope-matching (T0.3 algorithm), caps vs. resolved cost, revocation check within `cached_ttl`. **DoD:** all mandate adversarial vectors pass; over-cap and out-of-scope commits impossible in tests.

### T3.5 — Receipt issuance & store
Dual-signed receipts persisted per retention policy; export; linkage to audit log. **DoD:** every committed action has exactly one receipt; receipts independently verifiable offline.

### T3.6 — Risk gates & approval queue (Console v2)
Owner ceremonies per docs/06 Axis 4: auto/mandate/human_site/threshold/deny; Console approval queue with full context (simulation, mandate, requester); kill switches per action. **DoD:** gated commit blocks until human approval in E2E test; threshold routing correct at boundary values.

### T3.7 — Action Drafter + wiring UI
Draft candidates from forms/OpenAPI (risk-class suggestions); Console flow: review → wire → classify → gate → sign → publish. **DoD:** pilot commerce site's "add to cart" and "checkout" drafted and owner-published in recorded session; nothing publishable without explicit owner steps.

## Kernel side

### T3.8 — Simulator module
Mandatory simulate-before-propose for R2/R3; parse resolved effects; feed Monitor with *simulation results*, not model beliefs. **DoD:** commit attempts without fresh valid simulation are blocked by construction.

### T3.9 — Policy Monitor v2 (full)
Scopes/caps/risk ceilings/forbidden patterns; escalation to principal-side human checkpoint; budget accounting across simulate+commit+settlement. **DoD:** adversarial suite — hijacked-model scenarios (injection corpus driving action attempts) produce zero out-of-mandate commits; escalations fire exactly per mandate constraints.

### T3.10 — Executor v2
Propose/commit flows, offer verification (site signature, expiry, effects ⊆ simulation tolerance), countersigning, idempotency keys, Receipt Vault v2 (dispute export bundles). **DoD:** interop matrix vs. reference Gateway green including all abort/expiry paths.

### T3.11 — Principal mandate suite
Issue/inspect/revoke full-scope mandates; standing-mandate authoring (recurrence caps); minimal approval UI for escalations. **DoD:** canonical scenario mandates authored via tooling by a non-expert following docs.

## Cross-cutting

### T3.12 — Conformance ACT suite
Executable, both sides, adversarial-heavy (T0.7 growth). **DoD:** reference implementations 100%; divergence tests: zero undeclared effects.

### T3.13 — Canonical scenario E2E
The 50-ticket walkthrough (docs/02 §4) across independently deployed Gateway + Kernel, mock settlement. **DoD:** recorded run; every artifact (mandate, simulations, offer, receipt) archived in `/examples` as living documentation.

### T3.14 — Liability walkthrough & dispute pack
For each spec §8.4 case, produce the signed-artifact bundle and a written third-party adjudication using only the bundle. **DoD:** an uninvolved reviewer assigns fault correctly for all cases from artifacts alone.

### T3.15 — External security review
Commission review of Profile ACT (crypto, state machines, Monitor); triage findings. **DoD:** all criticals fixed; residuals accepted in DECISIONS.md; report summary published.

---

**Phase exit gate:** ROADMAP Phase-3 criteria green; ACT declared stable only after T3.15.
