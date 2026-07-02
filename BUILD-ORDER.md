# BUILD ORDER - Core to Server to Client to Plugins to Demos

The dependency logic behind the sequence (user-mandated and correct): each stage produces the contract or proof the next stage needs.

## Stage 1 — CORE PROTOCOL (`ajar` + `conformance` vectors)
Freeze what everyone implements: spec v0.1, schemas, golden examples, scope/error registries, conformance vectors. **Gate:** two independent readers hand-write a valid manifest from the spec alone (Phase-0 exit). Nothing else starts in earnest before this gate — changing contracts after code is 10x the cost.
Immediately after the gate: **`ajar-docs-mcp` v0.1**. Its data source freezes at the same time as the spec, and implementation guidance should be available before server code starts.

**Current status:** `ajar` has the initial v0.1 draft baseline prepared and
published. Local validation and Phase 0 readiness checks pass. The stage remains
open until hosted validation is active and the independent-reader gate is
recorded.

## Stage 2 — SERVER (`ajar-gateway`)
Supply side first: owners can open doors before any special client exists (curl with an Accept header is a valid consumer — that's the point of riding HTTP). Read layer only (Profile CORE): manifest, views, policy subset, console, signing. **Gate:** Phase-1 exit (10 diverse pilot sites, <1h owner effort, ≤20% token cost, conformance CORE green).

## Stage 3 — CLIENT (`ajar-kernel`)
Demand side: verification pipeline, Taint Boundary, Policy Monitor, MCP surface, consensual fallback (so the Kernel is useful on the whole web from day one, not just Ajar sites). **Gate:** Phase-2 exit (forged-manifest vectors all rejected; injection corpus produces zero out-of-mandate requests; side-by-side benchmark published).

## Stage 4 — PLUGGABLE APPS (`ajar-woocommerce` then `ajar-shopify`) + DEMOS (`ajar-examples`)
Distribution: the beachhead vertical (ADR-016) and the public proof. WooCommerce first (self-hosted world, pure Tier-1 harvest), Shopify second (hosted world, platform constraints). Demos in parallel: `docs-site` + `blog-minimal` need only Stages 1–3; `storefront` waits for Stage 5. **Gate:** <15-minute store onboarding recorded; docs-site benchmark table published; ajarprotocol.org live as our own first conformant site.

## Stage 5 — ACTIONS → MONEY → DISCOVERY (Phases 3–5 per ROADMAP)
SIMULATE/mandates/receipts (external security review gates stability) → metering/settlement + transparency logs/index (`ajar-index` repo created here) → ecosystem/governance transfer.

## Parallelization rules
Docs-mcp, examples specs, and integration-repo design ADRs may proceed anytime (they're markdown). Implementation code respects the gates. Conformance work shadows every stage — vectors land *with* the spec sections they test, harnesses land *with* the implementations they judge.
