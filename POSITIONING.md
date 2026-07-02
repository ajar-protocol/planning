# POSITIONING - Open Collaboration

Read this before writing public copy or designing a feature that overlaps another protocol. Ajar should be adoptable by any company or project, and it should work with the standards already in use.

---

## 1. The one-sentence position

Ajar is the owner-controlled declaration and authority layer for agent access to websites. It says what a site offers, who may act, with what proof, and at what price. It integrates with standards that already handle transport, tool wiring, commerce semantics, settlement, and agent-to-agent coordination.

## 2. Protocol-by-protocol composition map

| Protocol / project | What it owns | How Ajar composes | Where Ajar complements it |
|---|---|---|---|
| **HTTP/TLS, RFC 9421, Web Bot Auth** | Transport, request signing, agent identity on the wire | Ajar is *defined as* HTTP semantics; agent requests sign with `tag="ajar"` interoperably | The site-side signed declaration + policy those identities are checked against |
| **MCP** | Agent↔tool wiring, the dominant tool wire format | Manifest actions may declare `transport: mcp`; the Kernel presents Ajar sites *as MCP resources/tools* to existing agents | Web-scale late binding (discover→verify→bind), owner policy, risk classes, SIMULATE, receipts |
| **WebMCP** | In-page, client-side tools | Manifest may point at WebMCP tools for interactive flows | Signed site identity, mandates, effect typing above those tools |
| **ACP (Agentic Commerce Protocol)** | Checkout flows, Shared Payment Tokens, PSP integration | An Ajar `commerce.checkout` action can *bind to* an ACP flow; Ajar receipts wrap ACP transactions | Owner-controlled exposure/gating, SIMULATE before checkout, generalized (non-payment) actions, liability rules |
| **UCP** | Full commerce journey semantics (Google/Shopify orbit) | Same pattern: UCP as the commerce semantics behind Ajar-declared actions on Shopify-class merchants | Vendor-neutral self-hosted layer for everyone *outside* that orbit; unified manifest across commerce + content + custom actions |
| **AP2** | Payment authorization mandates (user-signed) | Ajar's PAY profile lists `ap2-card` as a settlement adapter; AP2 mandates satisfy the payment leg of an Ajar R3 commit | Mandates for *all* action types, agent-key binding, standing org↔org mandates, mechanical liability |
| **x402 / MPP** | HTTP-native machine payments (stablecoin/multi-rail) | Ajar's 402 metering is x402-compatible on the wire; `x402` is the first settlement adapter | The owner pricing policy and the receipt/liability layer above raw payment |
| **A2A** | Agent-to-agent tasks and discovery | Phase-5 bridge: Ajar sites/actions invocable from A2A systems; agent cards to manifest mapping | The site side of agent-to-website access |
| **UCAN** | Capability delegation cryptography | Mandate delegation chains are UCAN-compatible | The web-protocol packaging: scopes registry, caps, risk binding, receipts |
| **llms.txt / markdown negotiation / NLWeb / schema.org** | Agent-readable content | The Gateway harvests them (Tier 1) and Ajar Views supersede-but-honor them; NLWeb `/ask` can be declared as an R0 action | Signatures, chunk diffs, unified manifest, and everything beyond reading |
| **CDN crawl controls (pay-per-crawl etc.)** | Read-layer owner controls inside one vendor | Fully compatible in front of a Gateway; CDNs are *invited* to ship managed Ajar Gateways | Open, self-hosted, vendor-neutral, and extends control from reading to **acting and transacting** |
| **ERC-8004 / NANDA / registries** | Agent identity & reputation registries | Optional identity anchoring; Index interop notes | Site-capability transparency + semantic capability search (the other half of discovery) |

## 3. Ajar's Focus

1. **The unified signed *site* manifest** for owner-controlled site capabilities.
2. **SIMULATE + R0–R3 effect taxonomy** as standardized web protocol primitives.
3. **Generalized mandates with mechanical liability** across payment and non-payment actions.
4. **The owner policy model** — exposure/audience/economics/gates/observability as one declarative, signed surface.
5. **The deterministic Kernel** as a standardized open client (research prototypes exist; no standard).
6. **Capability transparency + semantic search over site manifests** (registries do agents, not sites).

When another standard already covers a capability, Ajar should integrate with it rather than duplicate it.

## 4. Communication rules (public writing, talks, README copy)

- Present adjacent protocols and companies as partners in a layered system; the sentence is always "Ajar carries X" / "X settles what Ajar authorizes" / "X transports what Ajar declares."
- Recognize strengths specifically, and describe differences technically: cite the exact boundary (e.g., "AP2 binds mandates to the user key; Ajar adds agent-key binding") without broad superiority claims.
- The conformance suite defines "Ajar Compatible"; our implementations get no special status in messaging.
- Highlight third-party implementations prominently; `awesome-ajar` exists to list work outside the reference repos.
- When a major company ships overlapping functionality, publish an interoperability note quickly and constructively.

## 5. Standing review

Re-check this map quarterly against `ajar/docs/01-RESEARCH.md` updates. Triggers for an AEP or ADR: an adjacent protocol adds owner-side controls, a manifest standard emerges from W3C/IETF, or a settlement rail consolidates. The preferred response is a bridge, and when appropriate, deprecating duplicate Ajar surface area in favor of the stronger standard.

## License

This document is licensed under CC-BY-4.0. See `LICENSE`.
