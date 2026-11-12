# Duplo signal matrix

Status: WORK IN PROGRESS (v1, 2026-11-12). These are hypotheses to validate with Duplo sales and leadership, not decisions.
Inputs: `context/icp/icp.md`, `context/icp/personas.md`, `context/docs/research/*`. Method: `knowledge-base/playbooks/signal-matrix-framework.md`, `workflow-anatomy.md`.
Tags point to research files: [company] [customers] [competitors] [market] [signals] [voice]. INFERRED = not supported by the research. Web checks made today are cited inline with their URL.
Row axis chosen with the user: **industry** (mutually exclusive). The e-invoicing deadline is one universal signal reused across rows.

---

## Matrix 1: Segments x personas

Personas (from `personas.md`):
- **Champion:** the finance operations lead.
- **Economic buyer:** the CEO or finance head.
- **Influencer:** the tax and systems owner.

### Segment definitions

| # | Segment | Target definition (filterable) | Lead product / upsell (hypothesis) | Duplo proof | Example prospects (verified to exist, not qualified) | Priority |
|---|---|---|---|---|---|---|
| S1 | FMCG and distribution | Nigerian FMCG manufacturers and distributors, VAT-registered, turnover ₦1bn+ (where known), many retailers or distributors paying in | Payments and collections, plus NRS e-invoicing / Treasury (TMS) | First customers were FMCG distributors. Heineken is named. FMCG firms are "exploring Duplo TMS" [customers] | Nestlé Nigeria Plc, Dangote Sugar Refinery Plc, BUA Foods Plc ([Nairametrics, 2026-05-04](https://nairametrics.com/2026/05/04/10-most-indebted-fmcg-companies-by-total-borrowings-in-2025/6/)). Distributors: requires Clay research (Nigeria, industry = wholesale/FMCG distribution, 50+ employees) | High |
| S2 | Multi-location QSR and retail | Nigerian restaurant, supermarket and retail chains with 10+ outlets (outlet count from Google Places or the company site) | Expense management, approvals and bulk vendor payouts, plus NRS e-invoicing | Eat'N'Go (200+ stores, 70+ on Duplo). MDS (44 locations) [customers] | Chicken Republic, Kilimanjaro ([Intelpoint](https://intelpoint.co/insights/chicken-republic-has-more-than-double-the-outlets-of-kilimanjaro-its-closest-rival/)); Bokku Mart, Addide Supermarket (44 stores), Market Square (36 stores) ([Nairametrics, 2025-03-30](https://nairametrics.com/2025/03/30/top-10-supermarket-chains-in-nigeria-by-store-count-dominating-the-retail-sector)) | High |
| S3 | Logistics, ports, shipping and manufacturing | Nigerian logistics, freight, haulage, port and industrial firms with many vendor or contractor payouts, and foreign-currency flows | Global payments and vendor payouts, plus NRS e-invoicing | Maersk, DP World, Krones [customers] | Kobo360, GIG Logistics, TradeDepot, Topship, truQ ([TheCondia](https://thecondia.com/top-tech-logistics-companies-nigeria/); [Zawya](https://www.zawya.com/en/economy/africa/top-5-nigerian-tech-startups-innovating-the-logistics-sector-in-q2-yxc998th)) | Medium |
| S4 | B2B services and SaaS (agencies, professional services, SaaS) | Nigerian B2B SaaS, agencies and professional services firms with 20+ staff, billing clients or paying contractors, some abroad | Expense management and approvals, plus cross-border payments | PressOne Africa, Prunet Digital [customers] | Requires Clay research (Nigeria, industry = software / marketing services / professional services, 20 to 500 employees) | Medium |
| S5 | Fintech and remittance (API buyers) | Nigerian or Nigeria-corridor payment and remittance companies needing virtual accounts, payouts and FX by API | Atlas payments API / Treasury | Kredete, Riz Remit [customers] | Requires Clay research (industry = financial services/fintech, keywords "remittance", "payouts", Nigeria corridor) | Low (few personas fit, small volume, INFERRED) |

Priority reasoning:
- **S1 and S2** have the most named-customer proof and the clearest link to e-invoicing and payouts.
- **S3** has strong enterprise logos, but the research has no detail on what those customers use.
- **S4** has only small-customer proof.
- **S5** is a different buyer (engineering), and tax pain is weak (INFERRED).

### Angle per cell
Each angle is grounded in research where a tag is shown. Otherwise it is INFERRED.

| Segment | Champion: finance ops lead | Economic buyer: CEO / finance head | Influencer: tax and systems owner |
|---|---|---|---|
| S1 FMCG and distribution | Collecting from many retailers and distributors and reconciling it. "One distributor can cater to over 1,000 retailers" [voice] | E-invoicing enforcement for ₦1bn to ₦5bn firms from January 2027, plus cash across several banks (TMS) [market] [customers] | Invoices cleared through an SI or APP from the ERP (SAP, Dynamics) with no PDF fallback [market] [company] |
| S2 Multi-location QSR and retail | Approvals and vendor payments across many stores. "So many payments. So many tools. Too many approvals stuck in a queue." [customers] | Spend visibility across every store, as at Eat'N'Go and MDS [customers] | Store-level expenses and VAT handled consistently. Duplo shows "Tax Management" and "Calculate taxes easily" [company] [voice] |
| S3 Logistics, ports, manufacturing | Paying hundreds of contractors and vendors. "Pay hundreds of recipients in minutes, not hours." [voice] | Local and foreign-currency payments in one place. Claims 80+ currencies and 160+ countries [company] | Connecting Oracle or SAP to NRS clearance [company] [market] |
| S4 B2B services and SaaS | Approvals off WhatsApp. "Managing the money side of this was absolute chaos if I'm being honest." [customers] | Founder time back: "Duplo gave us our time back." [customers] | INFERRED: billing international clients and paying overseas contractors (Duplo blog topic) [voice] |
| S5 Fintech and remittance | INFERRED: payouts and virtual accounts at volume (Atlas). "48 hours" to activate [company] | INFERRED: one provider for collections, payouts and FX [company] | INFERRED: engineering lead evaluates the API set (Virtual Accounts, Payout, FX & Quote) [company] |

---

## Matrix 2: Signals

### The four tests (from `workflow-anatomy.md`)
- **Reason:** a clear buying reason now.
- **Volume:** enough companies to sustain a workflow.
- **Data:** we can reliably get the data.
- **Story:** a natural line in outreach.

Each test is scored 1 to 5. Volume estimates are not given because there is no data yet; the first run should measure them. Coverage of Apollo-based Clay actions in Nigeria is untested (INFERRED risk).

### Universal signals (build once, swap keywords per segment)

| ID | Signal | Mother intent | Data source | Tool / Clay action | Segments | Reason | Volume | Data | Story | Total | Status |
|---|---|---|---|---|---|---|---|---|---|---|---|
| U1 | Turnover in the ₦1bn to ₦5bn band, before e-invoicing enforcement in January 2027 (₦5bn+ firms are past the 31 July 2026 deadline) | Regulatory changes | Audited accounts of NGX-listed companies; revenue figures in press; credit-bureau data (INFERRED, untested) [signals] | Crawl filings with `/crawl4ai`, extract turnover with a Claygent / Use AI column, then band it | S1 S2 S3 S4 | 5 | 2 | 2 | 5 | 14 | VERIFIED (PARTIAL): only companies whose turnover is public |
| U2 | Hiring finance, AP, treasury or tax/VAT roles, or a job post that mentions "e-invoicing", "reconciliation" or "VAT" | Hiring specific roles | Jobberman and MyJobMag via Apify actors (exist: [Jobberman scraper](https://apify.com/blackfalcondata/jobberman-scraper), [MyJobMag scraper](https://apify.com/nocodeninja_ng/myjobmag-scraper)); LinkedIn Jobs | Apify actor, then Clay HTTP import; Clay "Find Jobs" as backup | S1 to S4 | 3 (INFERRED: no Duplo deal traced to it) | 4 | 4 | 4 | 15 | VERIFIED (source exists; reason untested) |
| U3 | New CFO, Financial Controller, Head of Finance or Treasury Manager in the last 90 days | New leadership | LinkedIn profiles via Clay "Find People" (title + start date) | Clay Find People | S1 to S4 | 3 (INFERRED) | 3 | 3 (INFERRED: Nigerian LinkedIn coverage untested) | 4 | 13 | VERIFIED (PARTIAL) |
| U4 | Lookalike of named customers (Eat'N'Go, MDS, Maersk, DP World, PressOne, Kredete) | Lookalike of customers | Customer list [customers] | Clay lookalike / company enrichment | All | 2 (static, no timing) | 4 | 4 | 4 | 14 | VERIFIED (COMPOUND): use only as a secondary enrichment |
| U5 | Raised funding in the last 6 months | Funding and growth | Crunchbase via Clay "Find Organization" | Clay Find Organization | S3 (tech logistics), S4, S5 | 3 (INFERRED) | 2 | 4 | 4 | 13 | VERIFIED: only for the VC-backed part of S3, S4 and S5 |
| U6 | Uses QuickBooks, Xero, Zoho Books or Sage | Using specific software | BuiltWith can't see backend accounting tools (framework rule) | n/a | n/a | n/a | n/a | n/a | n/a | n/a | KILLED as a mother intent. Detect through job-post keywords inside U2 instead |

### Niche-specific signals

| ID | Signal | Mother intent | Data source | Tool | Segment | Reason | Volume | Data | Story | Total | Status |
|---|---|---|---|---|---|---|---|---|---|---|---|
| N1 | Outlet count up (new stores or branches) since the last snapshot | Niche-specific play (physical world) | Google Places API, monthly snapshot per chain; company store-locator pages | Clay HTTP to Google Places, then delta compare | S2 (also S1 depots) | 4 (matches the Eat'N'Go and MDS pattern; timing link INFERRED) | 3 | 4 | 5 | 16 | VERIFIED (source is standard; Nigeria Places coverage untested) |
| N2 | Several banks listed in the annual report (multi-bank cash, TMS fit) | Niche-specific play | "Bankers" section of NGX-listed annual reports (INFERRED that most list it) | `/crawl4ai` plus a Use AI extraction column | S1 | 3 | 2 | 3 | 4 | 12 | VERIFIED (PARTIAL), INFERRED source. Check on 5 reports first |
| N3 | New CBN licence (PSSP, IMTO, PSP) | Licensing events | CBN list of licensed institutions (INFERRED: not checked this session) | Clay HTTP scrape, then a diff | S5 | 4 | 2 | 3 | 4 | 13 | INFERRED. Verify that the source exists before building |
| N4 | Supplier to large or medium taxpayers (the buyer loses input-VAT credit on invoices that weren't cleared) | Regulatory changes | No public supplier lists [signals] | n/a | S1 S3 | 5 | n/a | 1 | 5 | n/a | KILLED: no data source |
| N5 | Wema ALAT Business customer in the free window | Niche-specific play | Wema owns the list [signals] | n/a | All | 4 | n/a | 1 | 4 | n/a | KILLED for outbound. Inbound or partner channel only |
| N6 | Brass customer moving to Paystack MFB | Competitor customers | No list [signals] | n/a | S4 | 3 | n/a | 1 | 3 | n/a | KILLED: no data source |
| N7 | Cash-heavy firm hit by the CBN 5% fee on business withdrawals over ₦5m a week | Regulatory changes | Not observable per company [market] | n/a | S1 S2 | 3 | n/a | 1 | 4 | n/a | KILLED as a mother intent. Use as a copy line for S1 and S2 |

**Compounding:** 5 universal signals (U1 to U5) are worth building, and 3 niche signals (N1 to N3). U1 to U3 use one Clay build each across S1 to S4, so only keywords and angles change.

### Play routing by segment
Signal, then segment angle, then product.

| Signal | S1 FMCG | S2 QSR/retail | S3 Logistics/mfg | S4 B2B services/SaaS | S5 Fintech |
|---|---|---|---|---|---|
| U1 turnover band | E-invoicing plus collections | E-invoicing plus store payouts | E-invoicing plus vendor payouts | E-invoicing (where turnover is ₦1bn+) | n/a |
| U2 hiring | AP/treasury roles, then payments and TMS | Finance/AP roles, then expense and approvals | AP roles, then payouts | Finance roles, then expense | Engineering/payments roles, then Atlas (INFERRED) |
| U3 new finance leader | TMS / e-invoicing | Spend visibility | Global payments | Expense | n/a |
| N1 outlet growth | Depots (INFERRED) | Expense, approvals, payouts | n/a | n/a | n/a |
| N2 multi-bank | TMS | n/a | n/a | n/a | n/a |
| N3 CBN licence | n/a | n/a | n/a | n/a | Atlas API |

---

## Build first (recommended)

1. **U1: e-invoicing turnover band (S1 to S3).** It has the strongest buying reason (dated penalties) and is the only signal where Duplo's two NRS licences set it apart from spend tools [competitors]. The window is time-bound: enforcement for medium taxpayers starts January 2027 [market]. The weakness is data: only companies whose turnover is public qualify, so start with NGX-listed companies.
2. **N1: outlet growth (S2).** It closely matches the best case study (Eat'N'Go). It's programmable through Google Places and gives the most natural story. It's also a physical-world signal that competitors are unlikely to monitor (INFERRED).
3. **U2: finance, AP and tax hiring (S1 to S4).** This is the volume engine. The Nigerian job-board scrapers exist. It's one build with keyword swaps per segment. Pair it with U1 where the turnover is known.
4. **U3: new finance leader (S1 to S4).** It's cheap to add on the same Clay table as U2. It's a hypothesis, so measure replies before scaling (C-tier in `icp.md`).

Use U4 (lookalike) as a secondary enrichment on all four, not as its own workflow.

---

## Matrix 3: Workflow tracker

| W # | Status | Workflow name | Mother intent | Campaigns | Campaign status | Launch date | Campaign name | Persona | Industry | Company size | Channel | Total leads | Bounce |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| W1 | Planned | E-invoicing deadline: turnover band | Regulatory changes | | | | | | S1 to S3 | ₦1bn+ turnover | | | |
| W2 | Planned | Outlet growth: multi-location chains | Niche-specific (physical world) | | | | | | S2 | 10+ outlets | | | |
| W3 | Planned | Finance, AP and tax hiring | Hiring specific roles | | | | | | S1 to S4 | | | | |
| W4 | Planned | New finance leader (90 days) | New leadership | | | | | | S1 to S4 | | | | |
| W5 | Planned | Recently funded | Funding and growth | | | | | | S3 to S5 | | | | |
| W6 | Planned | Multi-bank groups (TMS) | Niche-specific | | | | | | S1 | | | | |
| W7 | Planned (verify source first) | New CBN licence | Licensing events | | | | | | S5 | | | | |

---

## Open questions
- Does Apollo or Clay have usable coverage of Nigerian companies and people? Test it on 20 known companies (Eat'N'Go, Chicken Republic, Nestlé Nigeria) before building U2 to U4.
- How many companies does U1 actually reach? List the NGX FMCG, consumer and industrial issuers and count how many publish turnover.
- Which segment does Duplo's sales team see winning today? Check this against HubSpot closed-won deals.
- Is S5 worth a workflow at all, or is it inbound and partner-led?
- South Africa is deliberately left out of v1 (thin evidence) [customers].
