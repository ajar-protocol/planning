# planning - What Ajar Is Trying to Achieve

This is the public planning repo for Ajar Protocol. Nothing here is normative; protocol rules live in the public [`ajar`](https://github.com/ajar-protocol/ajar) specification repo. This repo explains why Ajar exists, what order we build in, and what work is still open.

---

## 1. The goal in one paragraph

Ajar lets a website serve humans through HTML and agents through a signed semantic layer, with the website owner holding the keys. The target owner journey is: ordinary website to controlled agent access in under an hour. The target agent journey is: read, simulate, act, and produce cryptographic proof of who authorized what. If something goes wrong, the signed artifacts should explain liability better than screenshots or logs alone.

## 2. Why this must exist (the four missing primitives)

1. **Meaning** — agents burn 10–100x tokens reverse-engineering HTML built for eyes.
2. **Actions** — buttons and forms are guesses; agents need typed, declared operations with dry runs.
3. **Authority** — password-sharing and session cookies are not delegation; agents need scoped, expiring, revocable, provable mandates.
4. **Accountability** — "who pays when the agent buys 500 tickets instead of 50" currently has no answer; Ajar makes it dual-signed receipts + mechanical liability rules.

## 3. Strategy: open protocol

We measure success by adoption across independent implementations, partner products, and standards communities. Concretely:

- Spec + schemas + conformance are vendor-neutral and foundation-ready; our Gateway/Kernel are reference implementations others can use, extend, or independently implement.
- Ajar is designed for adoption by any company or project. It should work with adjacent standards: MCP/WebMCP (action transport), ACP/UCP (commerce semantics), x402/AP2/MPP (settlement), A2A (agent-to-agent), Web Bot Auth (identity), UCAN (delegation). Details: [`POSITIONING.md`](POSITIONING.md).
- Adoption is implementation-led: the Gateway provides a straightforward compliant path; `ajar-docs-mcp` will help developer tools implement Ajar directly from the spec; e-commerce plugins make the first vertical practical; demos publish measurable results.

## 4. The beachhead: e-commerce first (ADR-016)

Commerce is where agent traffic is most urgent, structured data already exists, platforms concentrate distribution (WooCommerce, Shopify), the risk ladder is vivid (browse R0 → cart R1 → checkout R3), and adjacent protocols need exactly what Ajar adds (owner control, SIMULATE, generalized mandates). Sequence: WooCommerce plugin (open, self-hosted world) → Shopify app (hosted world) → published merchant case studies with token/cost/conversion numbers.

Second vertical: documentation sites. Our own docs should be consumed protocol-first, with seed examples already in `ajar` and richer demos planned for `ajar-examples`. SaaS documentation sites are natural CORE-profile candidates.

## 5. What we build, in order

See [`BUILD-ORDER.md`](BUILD-ORDER.md) for the full dependency logic. Summary:

```
1. CORE PROTOCOL   → ajar (spec, schemas, examples, registries) + conformance vectors
2. SERVER          → ajar-gateway (read layer first)  + ajar-docs-mcp (adoption tool)
3. CLIENT          → ajar-kernel (verify, enforce, fallback)
4. PLUGGABLE APPS  → ajar-woocommerce, ajar-shopify + ajar-examples demos
5. ACTIONS→MONEY→DISCOVERY → per ROADMAP Phases 3–5 (ajar-index created at Phase 4)
```

Current status: the public `ajar` repo is prepared as the initial v0.1 draft
baseline with the spec, schemas, examples, registries, seed conformance
vectors, licenses, and local validation checks. Phase 0 is not formally exited
until the hosted validation workflow is active and the independent-reader
manifest exercise is recorded.

## 6. What's in this repo

| File | Purpose |
|---|---|
| [`ROADMAP.md`](ROADMAP.md) | Phases 0–5, exit criteria, north-star metrics |
| [`BUILD-ORDER.md`](BUILD-ORDER.md) | The core→server→client→plugins sequence and why |
| [`POSITIONING.md`](POSITIONING.md) | How Ajar works with adjacent protocols and company platforms |
| [`INTEGRATION-STORIES.md`](INTEGRATION-STORIES.md) | Concrete narratives: how Ajar helps real projects, companies, and other protocols |
| [`tasks/`](tasks/) | The master task backlog (60+ tickets with Definitions of Done), mapped to destination repos |

## 7. Success criteria (north star)

- Owner: website → agent-ready (read) in <1h, eventually <15min; every screen answers "what's exposed, to whom, at what price."
- Agent: ≤10–20% of raw-HTML token cost; >99% action success on conformant sites; zero undeclared effects in conformance.
- Ecosystem: independent implementations; independent log/index operators; spec governance formally outside the founding team (ADR-012).

## 8. Where to go next

New contributor: start with the [organization profile](https://github.com/ajar-protocol), then this repo. Implementer: read the public [`ajar`](https://github.com/ajar-protocol/ajar) spec repo; the executable `conformance` repo is next. Merchant or site owner: follow `ajar-gateway` and the platform plugin repos once they are published. Automation contributors should follow the organization `AGENTS.md` once they work in a published repo.

## License

Planning documents are licensed under CC-BY-4.0. See `LICENSE`.
