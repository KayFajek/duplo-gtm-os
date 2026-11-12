# Duplo campaign strategy

**Date:** 2026-11-12. Work in progress. These are ideas to validate with Duplo sales, not commitments.

**Built from:** `CLAUDE.md`, `context/icp/*`, `context/positioning/*`, `context/messaging/messaging.md`, `context/docs/case-studies.md`, `context/docs/research/*`, `context/docs/gtm-system.md`, and `knowledge-base/playbooks/workflow-anatomy.md`.

**Labels and rules**
- INFERRED: not supported by the research.
- Volume: no counts exist in the research, so volumes are bands (Low / Medium / High) with the evidence behind them. They are never invented numbers. The first run of each list measures the real figure.
- Copy blocker: customer names (Eat'N'Go, Maersk and others) can't appear in copy until `context/messaging/facts.md` approves them.

**Target audience (given):** businesses operating in Nigeria in segments S1 to S4.
- S1: FMCG and distribution
- S2: multi-location QSR and retail
- S3: logistics, ports and manufacturing
- S4: B2B services and SaaS
- S5 (fintech and remittance) is on hold. South Africa is out of v1 (`context/icp/icp.md`).

**Core value proposition:** run payments, spend, NRS e-invoicing and treasury from one platform (`messaging.md`).

**Key differentiator:** Duplo holds both NRS licences (Systems Integrator and Access Point Provider) and pays and reconciles the cleared invoice in the same system (`positioning.md`).

---

## Customer discovery analysis

Source: `context/docs/case-studies.md`. All results are Duplo's own claims.

| Company | Industry | Size | Key metric | Problem solved |
|---|---|---|---|---|
| Eat'N'Go | QSR franchise operator | 200+ stores | 70+ stores on Duplo | Vendor payments, approvals and expenses across stores |
| MDS | Not found | 44 locations | Spend transparency across 44 locations | Expense management |
| Prunet Digital | Marketing agency | Not found | "Duplo gave us our time back." | Approvals off WhatsApp, salaries by upload |
| PressOne Africa | B2B SaaS | "10 businesses to thousands" | None specific | Finance structure while scaling |
| SmartCiti | Subscription (INFERRED) | Not found | Revenue up 20% | Subscription payment attribution |
| Kredete, Riz Remit | Remittance / fintech | Not found | None | Virtual accounts, payouts, FX via API |
| Maersk, DP World, Krones, Heineken | Shipping, ports, manufacturing, beverages | Enterprise | None | No story found |

**Patterns**
- Industries: multi-location consumer businesses (QSR, retail, FMCG), logistics and ports, and SMB services.
- Sizes: from a Lagos agency to multinationals. Volume comes from the roughly 20% who are mid-market or enterprise.
- Roles that bought or used it: Operations Lead, Treasury Manager, Regional Operations Manager, CEO and Founder (testimonial titles).
- Common problem: approvals and payments spread across too many tools and locations.

**Targeting recommendation**
- The case studies show fit beyond the "₦1bn+" box. Prunet Digital is a small agency, and its CEO bought.
- Keep enterprise as the volume priority, but test a founder-led SMB services list (ideas 7 and 13).
- The evidence also points to an untapped channel: firms that advise the ICP on e-invoicing compliance (ideas 9 and 10).

**Best lookalike seeds**
1. Eat'N'Go (multi-location QSR)
2. MDS (multi-location)
3. Maersk, DP World and Krones (logistics, ports and industrial)
4. PressOne (B2B SaaS)

---

## How the ideas are ranked

Each idea gets two scores from 1 to 5:
- **Signal strength (Sig):** how strongly the signal means "needs Duplo now".
- **Data ease (Data):** how reliably and cheaply the signal can be pulled without manual research.

Ideas are ranked by Sig + Data. Ties go to the higher signal strength, then the higher volume.

The **Tier** column is a separate score from the skill's five-factor test (signal, data, volume, time to build, differentiation). It shows the build order.

## Campaign ideas (ranked)

| # | Campaign | Level | Mother intent | Tier | Segment / persona | Signal and data source | Angle (value prop type) | Volume / month | Difficulty | Sig | Data |
|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | **E-invoicing job posts** | Niche | Hiring specific roles + Regulatory changes | Tier 1: strongest reason, data exists, Duplo-only angle | S1 to S4 / economic buyer, champion | Finance or tax job post that mentions `e-invoic`, `NRS` or `Merchant Buyer`. Source: Jobberman and MyJobMag via Apify (W3 spec, cells 1 to 4) | Clear invoices with a licensed SI and APP and get paid on them in one system (mitigate risk) | Low. A subset of #4. Count unknown | Low (reuses the W3 build) | 5 | 4 |
| 2 | **Large taxpayers past 31 July** | Focused | Regulatory changes | Tier 1: time-bound, one-off list, strong reason | S1 and S3 listed firms, turnover ₦5bn+ / economic buyer, tax owner | Turnover in NGX-listed annual reports (`/crawl4ai` plus a Use AI extraction) | Deadline has passed and enforcement applies. Duplo clears invoices from the existing ERP (mitigate risk) | Low, one-off. Limited to listed firms. Count not found | Medium | 5 | 3 |
| 3 | **Medium taxpayers before January 2027** | Focused | Regulatory changes | Tier 2: strong reason, but private firms' turnover is hidden | S1 to S4, turnover ₦1bn to ₦5bn / economic buyer, tax owner | Turnover from listed filings, press revenue figures, or credit-bureau data (untested) | Enforcement starts January 2027; get onboarded through one licensed provider (mitigate risk) | Low. Most medium firms are private | High | 5 | 2 |
| 4 | **Finance, AP and tax hiring (W3)** | Focused | Hiring specific roles | Tier 1: spec approved, abundant data, reusable build | S1 to S4 / champion, economic buyer | Finance, AP, treasury or tax job post under 90 days old. Jobberman and MyJobMag via Apify | The 12 angles in the W3 spec (save time, mitigate risk) | Medium to High. Jobberman's finance category showed about 200 to 430 live listings across cached snapshots ([Jobberman](https://www.jobberman.com/jobs/accounting-auditing-finance)), before the ICP filter. ICP share unknown | Low to Medium | 3 | 4 |
| 5 | **Outlet growth (W2)** | Focused | Niche-specific play (physical world) | Tier 2: great story, but needs two monthly snapshots before the first signal | S2 chains with 10+ outlets / champion, economic buyer | Outlet count up between monthly Google Places snapshots | New stores mean more vendor bills and approvals per store; one approval flow for every store (save time) | Low. Few chains have 10+ outlets (`signal-matrix.md`) | Medium | 4 | 3 |
| 6 | **E-invoicing advisers (Creative, non-ICP)** | Focused | Niche-specific play (partner) | Tier 2: high leverage, slower partner cycle | Audit, tax and accounting advisory firms publishing NRS e-invoicing guidance / partner or tax leads | Firms with public e-invoicing articles or webinars (the research cites EY and Forvis Mazars guidance); `/crawl4ai` on their insights pages | Their clients need a licensed SI and APP. Duplo is one, and it also pays and reconciles (make money: a referral or partner offer) | Low. A small pool of firms | Low | 3 | 4 |
| 7 | **Recently funded B2B companies** | Focused | Companies fundraising | Tier 2: data easy, but only the VC-backed slice | S4, plus tech-enabled S3 / CEO or founder | Round in the last 6 months. Crunchbase via Clay Find Organization | "Invest in structure early" (PressOne): approvals and payouts before finance scales (save time) | Low. Nigerian VC rounds are limited. Count unknown | Low | 3 | 4 |
| 8 | **Competitor showcase accounts** | Niche | Competitor customers | Tier 3: tiny list and sensitive; e-invoicing angle only | Companies named on competitor sites (Bujeti lists Selar, AutoGirl, Koolboks) / economic buyer | Names from competitor testimonial pages | Keep your spend tool, add licensed e-invoicing (mitigate risk) | Very low (handful) | Low | 3 | 4 |
| 9 | **New finance leader (New Hire)** | Focused | New leadership (past 90/180 days) | Tier 2: classic signal, but Nigerian LinkedIn coverage untested | S1 to S4 / new CFO, Financial Controller, Head of Finance, Treasury Manager | Start date under 90 days. Clay Find People (title plus start date) | Set up approvals, payouts and e-invoicing compliance in the first months in the role (save time, mitigate risk) | Unknown. Measure in the first run | Medium | 3 | 3 |
| 10 | **ERP partners (Creative, non-ICP)** | Focused | Niche-specific play (partner) | Tier 2: their clients must connect the ERP to the NRS; partner cycle | SAP, Oracle, Dynamics, QuickBooks and Zoho implementers in Nigeria / partner leads | Partner directories and websites (INFERRED source, verify) | Duplo connects your clients' ERP to the NRS as a licensed SI and APP, with payments on top (make money) | Low | Medium | 3 | 3 |
| 11 | **Cross-border payers** | Focused | Website content mentions | Tier 2: good fit for global payments, signal moderate | S3 and S4 / champion, economic buyer | Job post or website mentions overseas suppliers, clients or contractors, USD or FX (regex plus a Claygent site scan) | Local and foreign-currency payouts in one run; 80+ currencies (save money, save time) | Medium (INFERRED) | Medium | 3 | 3 |
| 12 | **FMCG distributor collections (Golden ICP)** | Niche | Website content mentions | Tier 2: what a rep would research by hand; list needs building | S1 distributors / champion (finance ops), economic buyer | Distributor network, depots or retailer counts on the website (Claygent). Base list from Clay industry search | Collections from many retailers into virtual accounts, reconciled automatically. Duplo started with FMCG distributors (save time) | Low to Medium | Medium | 3 | 3 |
| 13 | **Newly licensed fintechs (S5)** | Niche | Licensing events | Tier 3: S5 on hold; CBN source unverified | S5 / engineering or payments lead | New CBN PSSP, IMTO or PSP licence (source to verify) | Atlas API for virtual accounts, payouts and FX, live in "48 hours" (make money) | Low | Medium | 4 | 2 |
| 14 | **Multi-location lookalike (Lookalike)** | Focused | Lookalike of customers | Tier 1: fast build from known chains; proof needs facts.md | S2 chains like Eat'N'Go and MDS / champion, economic buyer | Clay lookalike seeded with Eat'N'Go and MDS, plus named chains (Chicken Republic, Kilimanjaro, Bokku Mart, Addide, Market Square) | A chain like yours runs its store payments and approvals on Duplo (save time) | Low to Medium | Low | 2 | 4 |
| 15 | **Multi-bank treasury groups** | Niche | Niche-specific play | Tier 3: source INFERRED; product launched Sept 2026 | S1 groups / economic buyer, Treasury Manager | Several banks listed in the "Bankers" section of the annual report (INFERRED) | See and move cash across every bank, account and entity without replacing them (save money) | Low | High | 3 | 2 |
| 16 | **Logistics and industrial lookalike (Lookalike)** | Focused | Lookalike of customers | Tier 2: strong logos, no story behind them | S3 / champion, economic buyer | Clay lookalike seeded with Maersk, DP World and Krones | Vendor and contractor payouts plus ERP-connected e-invoicing (save time) | Low to Medium | Low | 2 | 3 |
| 17 | **Multi-branch churches and security firms** | Focused | Niche-specific play | Tier 3: Duplo blog targets them; no customer proof | Churches and security companies with many branches / administrator, finance head | Branch counts from Google Places | Collections and payouts across every branch in one place (save time) | Medium (INFERRED) | Medium | 2 | 3 |
| 18 | **Creative Ideas** | Broad | Niche-specific play | Tier 2: high volume, weakest signal | S1 to S4 base list / champion | No trigger. Claygent reads the website and writes 3 use cases from Duplo's four pillars | "I had an idea for {{company_name}}..." with 3 specific uses (save time) | High (base list) | Low | 1 | 4 |
| 19 | **South Africa Xero users** | Focused | Companies using specific software | Tier 3: SA out of v1; Xero hard to detect | SA businesses / finance lead | Xero usage (hard to detect; job posts mentioning Xero) | Two-way Xero sync, payments and reconciliation (save time) | Unknown | High | 2 | 2 |

**Considered and dropped**
- Brass customers moving to Paystack: no list exists.
- Wema ALAT Business users: Wema owns the list, so this is inbound only.
- Suppliers to large taxpayers: no data.
- Accounting-software users in Nigeria: BuiltWith can't detect backend tools.

These match the kills in `signal-matrix.md`.

**Required types**

| Type | Ideas |
|---|---|
| Creative Ideas | #18 |
| New Hire | #9 |
| Lookalike | #14, #16 |
| Creative (non-ICP) | #6, #10 |
| Golden ICP | #12 |
| Creative stretch | #1, #8, #10, #15 |

---

## Campaign overviews (copywriter handoff)

Each overview is meaning, not copy. Write the copy with `/campaign-copy` after `/build-workflow-spec`.

1. **E-invoicing job posts.**
   - Hook: they're hiring for e-invoicing work while the NRS rollout moves through the taxpayer bands.
   - Personalisation: role title, segment.
   - Proof: Duplo's SI and APP licences.
   - CTA: a question about how they're handling clearance.
   - Built inside W3 as cells 1 to 4, so no new build is needed.
2. **Large taxpayers past 31 July.**
   - Hook: the NRS said defaulters face "appropriate regulatory and enforcement actions."
   - Personalisation: company name, segment, ERP (if known).
   - Proof: licences; clearance from the existing ERP.
   - Differs from #3 because the deadline has already passed.
3. **Medium taxpayers.**
   - Same as #2, framed as before enforcement starts in January 2027.
   - Personalisation is limited because the turnover band can only be stated where it's public.
4. **Finance, AP and tax hiring.**
   - The approved W3 spec: 12 angle cells (3 role families x 4 segments).
5. **Outlet growth.**
   - Hook: new stores opened.
   - Personalisation: the outlet count cluster (10+).
   - Proof: `{{social_proof_s2}}`.
   - CTA: how approvals work across the new stores.
6. **E-invoicing advisers.**
   - Hook: their published e-invoicing guidance.
   - Offer: Duplo as the licensed SI and APP for their clients, plus payments.
   - CTA: a referral or partner conversation.
   - This is a partner motion, so expect a longer cycle.
7. **Recently funded.**
   - Hook: the new round.
   - Angle: "Invest in structure early" (PressOne).
   - Persona: founder or CEO.
8. **Competitor showcase accounts.**
   - E-invoicing only. Never knock the competitor.
   - Tiny list, so it may be one-to-one emails rather than a campaign.
9. **New finance leader.**
   - Hook: first months in the role.
   - Personalisation: start date and previous company.
   - Angle: quick wins in approvals and payouts, plus e-invoicing readiness.
10. **ERP partners.**
    - Hook: their clients' ERP has to reach the NRS through an SI or APP.
    - Offer: partnership.
11. **Cross-border payers.**
    - Hook: the overseas suppliers, clients or contractors named in their job post or website.
    - Proof: 80+ currencies, "T+0 settlement depending on the corridor".
12. **FMCG distributor collections.**
    - Hook: the size of their retailer and distributor network.
    - Proof: Duplo began with FMCG distributors.
    - CTA: how collections are reconciled today.
13. **Newly licensed fintechs.**
    - Hook: the new licence.
    - Offer: the Atlas API set.
    - Hold until S5 is reopened.
14. **Multi-location lookalike.**
    - Hook: a chain like theirs.
    - Proof: Eat'N'Go, once approved in facts.md. Without approval, use "a 200+ store QSR operator" only if facts.md allows it.
15. **Multi-bank treasury.**
    - Hook: the number of banks listed in their annual report.
    - Offer: TMS.
    - Verify the source on 5 reports first.
16. **Logistics lookalike.**
    - Same as #14 with S3 seeds.
    - Logos only, no result claims.
17. **Churches and security.**
    - Branch-count hook. Test small; there's no proof yet.
18. **Creative Ideas.**
    - Claygent reads their site and maps 3 uses to Duplo's pillars.
    - CTA: "Would any of these be useful?"
19. **SA Xero users.**
    - Park until South Africa is in scope.

---

## No-AI campaigns

**"PDF invoices are over"** (S1 to S3, ₦1bn+ where known, economic buyer)
- Why it works without AI: the regulation is the personalisation. Every VAT-registered business must move to NRS e-invoices, and Duplo's own FAQ answers "Can I continue using PDF invoices?" with "No." (`market.md`).
- Message: the rule change, one line on Duplo's SI and APP licences, then a question about how far along they are.

**"Pay 500 vendors in one run"** (S2 and S3, champion)
- Why it works without AI: one concrete capability ("Pay up to 500 recipients at a time") for a payout-heavy list.
- Message: the capability, one line on reconciliation, then a question about how payouts run today.

---

## Front-end offers

1. **E-invoicing readiness check.** A short review of how their invoices would get from the ERP to the NRS today, and what's missing. It builds on Duplo's existing countdown and FAQ (`nrs-e-invoicing` page). Best for ideas 1 to 3.
2. **The NRS e-book.** Duplo already offers "Download free e-book". It's a low-friction first yes for #2, #3 and #6.
3. **Free tier quick win.** "Sign up for free" ($0/month), with 100 free payouts on paid plans (`company.md`). They run one real bulk payout and see reconciliation. Best for ideas 4, 5 and 14.

---

## Recommended next step

| Path | Ideas | What to do |
|---|---|---|
| Build | #4 and #1 | Already specced as W3 (`output/campaigns/w3-finance-ap-tax-hiring/workflow-spec.md`). Run `/campaign-copy` once `context/messaging/facts.md` exists |
| One-off list | #2 | Pull it before January 2027 |
| Next spec | #14 | Run `/build-workflow-spec` on it |
