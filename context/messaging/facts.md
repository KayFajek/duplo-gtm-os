# Duplo: authorized copy facts

This file is the ONLY source of names, numbers and claims for prospect-facing copy (the `/campaign-copy` FACTS GATE). Anything not listed here renders as `{{placeholder}}`. Never pull a fact from memory, and never invent one.

**Last updated:** 2026-11-12, approved by the GTM lead in session.

- Built from: `knowledge-base/playbooks/templates/facts-template.md`.
- Sources: `context/docs/research/` and `context/docs/case-studies.md`.

---

## Sender identity

| Field | Value |
|---|---|
| Sender name(s) | Per inbox, resolved by the sequencer signature (`{{sender_signature}}`) |
| Title | Per inbox |
| Signature format | Set per inbox in the sequencer |
| Sender persona notes | First person singular ("I"). "We" means Duplo, and appears only in the value-prop line |

## What we do (locked value prop)

The value-prop line states one of these, flat. The lead-in is verbatim; the tail may adapt per route.

- **Variant A (platform):** "Duplo runs payments, spend, NRS e-invoicing and treasury on one platform."
- **Variant B (e-invoicing):** "Duplo is licensed by the NRS as both a Systems Integrator and an Access Point Provider, so invoices are cleared, paid and reconciled in one system."

Product one-liner (context, not copy): "The financial operating system for African businesses" (Duplo homepage). Lagos-based, serving Nigeria with South Africa as the expansion market.

## Approved claims and numbers (use these exact phrasings)

| Claim | Approved phrasing | Source | Approved by / date |
|---|---|---|---|
| NRS licences | "licensed by the NRS as both a Systems Integrator and an Access Point Provider" | techeconomy.ng, 2026-03-20; research/company.md | GTM lead, 2026-11-12 |
| Bulk payouts | "pay up to 500 recipients at a time" | tryduplo.com homepage | GTM lead, 2026-11-12 |
| Global reach | "80+ currencies across 160+ countries" | tryduplo.com homepage | GTM lead, 2026-11-12 |
| Scale | "1,000+ businesses" and "over $1bn in annualised payment volume" | CEO, TechCabal via brandiconimage.com, 2026-07 | GTM lead, 2026-11-12 |
| Security | "ISO 27001 and 22301", "PCI DSS", "NDPR compliant", "CBN PSSP licence" | tryduplo.com homepage | GTM lead, 2026-11-12 |

### Market facts (regulation, not Duplo claims)

**Status:** APPROVED 2026-11-12 by the GTM lead, with Email 1. These come from `context/docs/research/market.md`, and copy may only state them in the phrasing below.

| Fact | Approved phrasing | Source |
|---|---|---|
| Phased rollout | "NRS e-invoicing is being phased in by business size" | Sovos, 2026-02-26; NRS notice 2026-02-17 |
| Reference number | "each invoice needs an NRS reference number before it goes out" | Beancount guide; Duplo NRS page (IRN) |
| Accredited route | "connecting to the NRS has to go through an accredited Systems Integrator or Access Point Provider" | Financial Nigeria, 2026-07-19 |
| Input VAT | "an invoice that isn't cleared can cost the buyer the input VAT on it" | Beancount guide |

**Formatting rules for proof**
- Write customer names exactly as "Eat'N'Go", "Prunet Digital", "PressOne Africa", "Maersk", "DP World", "Krones", "Heineken".
- Write money as "$1bn" and "₦" amounts in full (₦200,000). Write the authority as "NRS", never FIRS or NIRS.

## Numbers to keep fuzzy (never quote as hard numbers)
- Any customer result not listed above. That includes the "up to 85%" cost and "up to 50% faster" close claims, which are NOT approved.
- 2022 growth figures (1,000%, 4,200%, $380,000 processed).
- Prospect counts that come from scraped data, such as outlet counts and job counts. Use clusters only, per the workflow spec.

## Authorized social-proof names

Only these customer names may appear in prospect-facing copy:

| Name | What it proves | Matches | Caveat |
|---|---|---|---|
| Eat'N'Go | A 200+ store operator runs store payments and approvals on Duplo ("over 70 stores" on Duplo) | S2 multi-location QSR and retail | Approved phrasing: "Eat'N'Go, which operates Domino's Pizza, Cold Stone Creamery and Pinkberry, runs payments and approvals for over 70 of its stores on Duplo." No other result claimed |
| Prunet Digital | A Lagos agency moved approvals off WhatsApp and pays salaries by upload. Quote: "Duplo gave us our time back." | S4 B2B services | Agency, small. Don't imply enterprise scale |
| PressOne Africa | A B2B SaaS company that grew "from managing 10 businesses to supporting thousands" uses Duplo. Quote: "Invest in structure early." | S4 SaaS | The quote is advice, not a result. Logo plus quote only |
| Maersk, DP World, Krones | Logistics, port and industrial companies use Duplo | S3 | Logo-drop only. No results, and no claim about which product they use |
| Heineken | An FMCG and beverage company uses Duplo | S1 | Logo-drop only. No results, and no claim about which product they use |

**Source note (2026-11-12):** Maersk, DP World, Krones and Heineken are not on tryduplo.com.
- Heineken comes from one press reprint of a CEO interview (brandiconimage.com, 2026-07).
- Maersk, DP World and Krones come from one Techeconomy article (2026-03-20).

The GTM lead decided to keep all four as approved logo-drops. Re-confirm with Duplo before scaling sends.

Everyone else stays out of copy until added here, including SmartCiti, MDS, Kredete, Riz Remit and the other homepage logos.

## Proof caveats
- Eat'N'Go proves multi-store payments and approvals. It does NOT prove e-invoicing or treasury.
- The enterprise logos prove the company uses Duplo, nothing more. Never attach a metric or a product to them.
- No customer is approved as proof for NRS e-invoicing. Tax routes lean on the licence claim, not on a customer.

## Banned

### Banned angles
- Claiming which e-invoicing deadline applies to the prospect. Their turnover band is unknown (W3 spec).
- Fear or penalty-led openers. Penalties may be stated as fact only where a route earns it, never as a threat.
- Implying their finance team is failing or behind (iron law 14 and the insult test).
- Price, discount or "free" framing.
- Knocking competitors by name.

### Banned words and phrasing (company-specific)
The global list is `knowledge-base/playbooks/copy-bans.yaml`. Specific to Duplo:
- "FIRS" and "NIRS". Use "NRS".
- "bank" or "neobank" to describe Duplo.
- "CBN licence" for e-invoicing (those licences are from the NRS).
- Corporate cards and payroll as Duplo products.
- Duplo's own site phrases that hit the global ban list ("streamlined", "seamlessly"). Don't quote them.

### Never in copy, under any circumstance
- Funding amounts, investors, break-even or profitability statements.
- The Wema Bank revenue share or partnership terms.
- Pricing.

## Voice notes
- Register: plain, peer-to-peer, benefit-led (`context/brand-kit/brand.md`).
- Hedges about the prospect: "my guess is", "could be off", "probably", "not sure if".
- Subjects: lowercase, 3 to 5 words, no variables except `{{company_name}}`, same subject across the thread.

## CTA ladder
- **Primary:** a question that confirms their situation ("Is that on your plate yet?", "How are you handling that today?").
- **Secondary (final email):** a referral question ("Is someone else closer to this?").
- **Never:** calendar links, "book a call", demo asks in Email 1.

## Internal only (never in prospect copy)
- Pricing, customer roster beyond the authorized names, funding, partnership terms, campaign performance, reply rates.
