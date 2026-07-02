# INTEGRATION STORIES - Example Adoption Scenarios

These scenarios explain how Ajar should work for real owners and implementers. Each story names the actor, the starting problem, the Ajar pieces involved, and the existing technology Ajar works with. Use them for demos, case studies, and acceptance scenarios.

---

## Story 1 - The WooCommerce merchant

**Actor:** A Surat saree store on WordPress/WooCommerce, 4,000 products, one owner-operator.
**Before:** Agent shoppers from major assistant surfaces either can't parse her theme, get blocked as bots, or hallucinate prices from stale crawls. She sees rising bot traffic, zero attributable revenue.
**With Ajar:** She installs the `ajar-woocommerce` plugin. Tier-1 harvest reads products, prices, and stock from the Woo database. The Console shows: "4,000 products mapped; draft actions: `search_catalog` (R0), `add_to_cart` (R1), `checkout` (R3)." She publishes search free for everyone, cart for signed agents, and checkout gated at `threshold(₹20,000)`. Above that amount, she approves from her phone. Her signed manifest is live at `/.well-known/ajar.json`.
**Result:** Agents get exact real-time data at ~10% of the token cost; she gets attributable agent revenue, per-operator analytics, and a kill switch. **Composes with:** WordPress internals (Tier 1), x402 metering if she later charges crawlers, ACP if her PSP prefers it for the payment leg.

## Story 2 - The Shopify brand + UCP

**Actor:** A D2C skincare brand on Shopify already reachable through Google-orbit agent surfaces via UCP.
**Problem UCP doesn't solve for them:** exposure control ("agents may browse the catalog but never the wholesale price list"), non-commerce actions (`book_consultation`, `check_ingredient_compatibility`), and one policy across *all* agent surfaces, not one ecosystem.
**With Ajar:** The `ajar-shopify` app serves the manifest via app proxy. Commerce actions bind to the existing UCP/ACP checkout. Ajar adds owner gates, SIMULATE for exact landed cost before cart mutation, and dual-signed receipts around the rail the merchant already uses.
**Result:** The brand gets one signed policy surface for agent access while UCP continues to handle checkout.

## Story 3 - The SaaS docs site

**Actor:** A dev-tools company whose docs get hammered by coding agents; also **us** (ajarprotocol.org).
**Before:** Agents fetch JS-heavy HTML pages, burn ~50k tokens to answer "what are the rate limits," and still miss the answer; the company's server bills rise for traffic that never converts.
**With Ajar:** A Next.js docs site serves Ajar Views from the same URLs. An agent requesting `Accept: application/ajar+json` gets chunk-addressed semantic content with stable IDs; returning agents fetch only diffs. `search_docs` is declared as a free R0 action. The manifest license block says read=allowed, train=denied.
**Result:** The same question is answered correctly with fewer tokens and no HTML parsing.

## Story 4 - The coding agent implementing Ajar

**Actor:** A developer at any company, told "make our site agent-ready," using an AI coding agent.
**With Ajar:** They add `ajar-docs-mcp` to their coding agent. The agent calls `search_spec("manifest required fields")`, `get_section("5.1")`, and `get_checklist("gateway-core")`, then pulls examples and conformance vectors while implementing a manifest and views.
**Result:** The implementation can be checked against the conformance suite before it is shipped. **Composes with:** MCP.

## Story 5 - Org-to-org standing mandate

**Actors:** A logistics company and its packaging vendor, invoicing monthly for years.
**Before:** PDFs, email threads, three-way matching by humans, occasional disputes with no clean evidence.
**With Ajar:** The vendor's ERP-fronting Gateway exposes `submit_invoice` and `query_po` for the `contracted` tier. The buyer's finance head signs a standing mandate: "pay vendor X up to ₹5,00,000/month against invoices matching open POs, valid 12 months, revocable." Each cycle: vendor agent submits, buyer agent SIMULATEs the payment, the system proposes, commits, issues a dual-signed receipt, and settles through the declared rail.
**Result:** The invoice cycle has signed evidence for what was submitted, checked, approved, and paid. **Composes with:** AP2/x402 settlement, A2A if the two sides' agent fleets coordinate further.

## Story 6 - The hosting provider or agency

**Actor:** A managed-WordPress host, or a Gujarat web agency maintaining 300 SME sites.
**With Ajar:** The host offers "Agent-Ready" as a managed Gateway. The agency standardizes an onboarding runbook: install, review coverage, owner signs, publish. Each site keeps its own keys and policy.
**Result:** Adoption can move through the teams that already manage owner relationships.

## Story 7 - The agent-framework developer

**Actor:** A team running framework-based agents that currently scrape, break weekly, and once bought the wrong thing.
**With Ajar:** They wrap tools in `ajar-kernel` or use its MCP surface. Their agents verify sites cryptographically, read more efficiently, act only within principal-signed mandates, and produce receipts their compliance team can audit.
**Result:** Fewer action errors, clearer oversight, and fallback mode for non-Ajar sites.

---

## Using these stories

- **Demos:** Stories 1 & 3 are the launch demos (`ajar-examples`); Story 4 ships with `ajar-docs-mcp`.
- **Case studies:** each story graduates to a numbers-backed case study as pilots complete (ROADMAP T1.15, T2.11).
- **Acceptance tests:** conformance scenarios mirror Stories 1, 3, 5.
- Add new stories by PR. Each story must name the existing protocols it works with. Stories should not frame Ajar as a replacement for a healthy standard; see `POSITIONING.md`.

## License

This document is licensed under CC-BY-4.0. See `LICENSE`.
