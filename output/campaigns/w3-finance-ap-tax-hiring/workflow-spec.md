# W3: Finance, AP and tax hiring (workflow spec)

**Status:** STRUCTURE-APPROVED 2026-11-12 · copy pending

This is workflow C from the 2026-11-12 proposal, built on signal U2 (tracker row W3 in `context/icp/signal-matrix.md`).

Inputs:
- `context/icp/icp.md`, `context/icp/personas.md`, `context/icp/signal-matrix.md`
- `context/docs/gtm-system.md`, `context/docs/research/`
- `knowledge-base/playbooks/workflow-anatomy.md`, `signal-matrix-framework.md`

Flags:
- `context/messaging/facts.md` does not exist yet. Every customer name or claim stays a `{{placeholder}}` until it does. `/campaign-copy` creates the file.
- There is no positioning file (`context/positioning/` is empty).

Labels: INFERRED = not supported by the research. TO DECIDE = an open choice, tracked in `gtm-system.md` "Decisions still open".

---

## 1. Mother intent (trigger) [ENGINE]

A Nigerian company in an active Duplo segment has posted a finance, accounts payable, treasury or tax/VAT job in the last 90 days.

Catalog match: "Hiring specific roles" (`workflow-anatomy.md`). Signal U2 (`signal-matrix.md`).

---

## 2. Inclusion / exclusion [ICP]

### Include

| Box | Rule | Source |
|---|---|---|
| Geo | Job location in Nigeria, and the company operates in Nigeria | `icp.md` (South Africa is out in v1) |
| Segment | S1 FMCG and distribution, S2 multi-location QSR and retail, S3 logistics, ports and manufacturing, S4 B2B services and SaaS | `signal-matrix.md` Matrix 1 |
| Fit evidence | At least one pattern from `icp.md` section 3B:<br>- multi-location: 10+ outlets, or the job post mentions branches, stores, depots or regions<br>- foreign-currency flows mentioned in the job post<br>Turnover of ₦1bn or more, where known, also counts | `icp.md` sections 3B and 5 |
| Size | Minimum headcount `{{min_headcount}}`: TO DECIDE. The research gives no floor | `icp.md` |
| Trigger | Job title matches a finance role family (signal 2), posted within 90 days | `gtm-system.md` step 1 |
| Recipient | **Champion:** finance operations lead (Operations Lead or Manager, Treasury Manager, AP lead).<br>**Economic buyer:** CFO, Head of Finance or Financial Controller; CEO or Founder at smaller companies.<br>Never the person in the vacant role. The influencer is out of v1 | `personas.md`; `gtm-system.md` step 6 |

### Exclude

| Who | Why |
|---|---|
| Banks, microfinance banks, insurers, fintechs (S5) | Banks are a Duplo channel and also compete with it (`competitors.md`). S5 is on hold in v1 |
| Recruiters, staffing agencies, "confidential client" posts | The real employer can't be identified |
| Duplo customers, open deals, companies contacted inside the suppression window (TO DECIDE) | HubSpot is the source of truth (`gtm-system.md` step 2) |
| Competitors: Bujeti, Flex Finance, Paystack, Moniepoint, accredited e-invoicing providers (SI/APP) | `competitors.md` |
| Government and public-sector employers | Not in the ICP (INFERRED) |
| Companies the segment classifier marks S5 or unclear | Routing needs one of S1 to S4 |
| No fit evidence: no B pattern and turnover unknown | `icp.md` priority rules: Fail "no evidence" |

---

## 3. Sources [ENGINE]

| Purpose | Source | Tool |
|---|---|---|
| Trigger | Jobberman, accounting, auditing and finance category | Apify Jobberman scraper, run weekly by n8n into a Clay webhook table |
| Trigger | MyJobMag (Nigeria, finance field) | Apify MyJobMag scraper, same pipe |
| Trigger (backup) | LinkedIn Jobs | Clay "Find Jobs" (Nigerian coverage untested) |
| Domain | Employer name, resolved to a domain | Claygent |
| Firmographics | Industry, headcount, country | Clay company enrichment |
| Segment | Hard-enum classifier | Clay Use AI column, built with `/prompt-builder` |
| Outlet count (S2 only) | Google Places API | Clay HTTP |
| Relationship check | Customer, open deal, contacted | HubSpot lookup |
| People | Champion and economic buyer | Clay Find People plus Claygent, into one shared People table |
| Email | Waterfall: Prospeo, Icypeas, Leadmagic, Findymail, then Bounceban validation | Clay waterfall (PLAYBOOK; providers TO DECIDE) |

---

## 4. Signals table [ENGINE]

| # | Signal | Role | How it's read | Output |
|---|---|---|---|---|
| 1 | Finance-role job post in Nigeria, last 90 days | trigger | Boolean. All three must be true:<br>- the title matches any role-family regex (signal 2)<br>- the location contains Nigeria or a Nigerian state<br>- the post is 90 days old or less | true / false |
| 2 | Role family | router | Regex. The first match wins, in this order:<br>1. **Tax and e-invoicing:** title `tax\|VAT\|WHT\|compliance`, OR description `e-invoic\|NRS\|Merchant Buyer`<br>2. **Payables and treasury:** title `accounts payable\|payables\|AP \|treasury\|cash management\|disbursement\|reconciliation`<br>3. **Finance build-out:** title `accountant\|finance officer\|finance manager\|financial controller\|head of finance\|CFO\|finance director` | enum: `tax_einvoicing` / `payables_treasury` / `finance_buildout` |
| 3 | Segment | router | Use AI classifier on industry and company description. Hard enum. S5 and unclear are excluded | enum: S1 / S2 / S3 / S4 / S5 / unclear |
| 4 | Multi-location | context-clause (also an inclusion gate; never routes) | Count of outlets from Google Places (S2). Regex on the job description: `branch(es)?\|outlets?\|stores?\|depots?\|regions?\|locations` | Clusters: unknown / under 10 / 10 or more. Boolean `mentions_locations`. Copy mentions locations only if the cluster is "10 or more" or `mentions_locations` is true |
| 5 | Foreign-currency flows | context-clause (also an inclusion gate; never routes) | Regex on the job description: `USD\|dollar\|FX\|foreign currency\|import\|export\|international (suppliers\|clients\|payments)` | boolean `mentions_fx` |
| 6 | Finance hiring volume | context-clause (never routes) | Count of distinct finance-role posts from the same company within the window | Clusters: 1 / 2 or more, labelled `single_hire` / `team_growing`. Copy gets the label only, never the raw count |

Design notes:
- Two routers, so 3 x 4 = 12 cells.
- Tax and e-invoicing wins ties. Any post that touches e-invoicing goes down the deadline-aware route.

---

## 5. Routing matrix with angles [ENGINE]

### Axes

- Role family: 3 (tax and e-invoicing, payables and treasury, finance build-out).
- Segment: 4 (S1 to S4).
- **3 x 4 = 12 cells.** Each included company lands in exactly one.

### Data available by cell

| Availability | Fields |
|---|---|
| Every cell | `company_name`, `role_title`, `role_family`, `segment`, `job_posted_date`, the recipient's `first_name` and `persona` |
| Some cells only | `location_count` (S2 via Google Places), `mentions_locations`, `mentions_fx`, `hiring_volume_cluster` |

- No angle depends on conditional data unless the cell says so.
- Turnover band is unknown in this workflow, so no cell may claim which e-invoicing deadline applies to the company.
- Tax cells refer only to the phased NRS rollout, using the timeline in `market.md`.

### The 12 cells

| Cell | Deterministic read | What it tells us (inferred situation) | The angle it earns |
|---|---|---|---|
| 1. Tax and e-invoicing, S1 FMCG and distribution | role_family = tax_einvoicing AND segment = S1 | They are adding tax capacity while NRS e-invoicing moves through the taxpayer bands. FMCG firms issue many invoices to distributors and retailers, and each one now needs clearance. | Clearing distributor and retailer invoices with the NRS, and matching each payment to its cleared invoice in the same system. Duplo holds both NRS licences (SI and APP) and runs collections [company]. |
| 2. Tax and e-invoicing, S2 multi-location QSR and retail | role_family = tax_einvoicing AND segment = S2 | They are adding tax capacity across a chain whose stores buy from many suppliers. As the buyer, the chain can only claim input VAT on supplier invoices that were cleared [market]. | Supplier invoices for every store arrive as cleared e-invoices and are approved and paid from one queue. Input-VAT claims then rest on compliant invoices. |
| 3. Tax and e-invoicing, S3 logistics, ports and manufacturing | role_family = tax_einvoicing AND segment = S3 | They are adding tax capacity in a business that usually runs an ERP and pays many vendors. Compliance means connecting that ERP to the NRS through an accredited SI or APP [market]. | NRS clearance straight from the ERP they already run (SAP, Oracle, Microsoft Dynamics, QuickBooks [company]) through a licensed provider, without replacing the ERP. Vendor payments run on the same platform. |
| 4. Tax and e-invoicing, S4 B2B services and SaaS | role_family = tax_einvoicing AND segment = S4 | They are adding tax capacity in a services firm that invoices corporate clients. Those clients need cleared invoices to claim input VAT [market], so invoice compliance now affects whether the firm gets paid. | Issuing NRS-compliant e-invoices to corporate clients and collecting payment in one place, without a heavy integration project ("No complex or heavy integrations to manage" [voice]). |
| 5. Payables and treasury, S1 FMCG and distribution | role_family = payables_treasury AND segment = S1 | They are staffing the money-out and cash side of a business that pays many suppliers and distributors and holds money in several banks. | Paying suppliers in bulk and seeing cash across every bank from one place ("Control cash across every bank, account and entity" [voice]), without replacing their banks or ERP [company]. |
| 6. Payables and treasury, S2 multi-location QSR and retail | role_family = payables_treasury AND segment = S2 | They are staffing vendor payments in a chain where every store generates its own vendor bills and approvals. | Vendor payments and approvals for every store run through one workflow with defined approval stages. Social proof: `{{social_proof_s2}}` (pending facts.md). |
| 7. Payables and treasury, S3 logistics, ports and manufacturing | role_family = payables_treasury AND segment = S3 | They are staffing payouts in a business that pays many contractors, hauliers and vendors, some in foreign currency when `mentions_fx` is true. | Paying hundreds of contractors and vendors in one bulk run (up to 500 recipients [company]), reconciled automatically. When `mentions_fx` is true, local and foreign-currency payouts go in the same run. |
| 8. Payables and treasury, S4 B2B services and SaaS | role_family = payables_treasury AND segment = S4 | They are bringing in someone to run payments in a services firm where a small team pays contractors, salaries and vendors, often approving them in chat. | Moving payment approvals into a defined workflow and paying contractors and salaries by bulk upload. When `mentions_fx` is true, overseas contractors are paid from the same place. Social proof: `{{social_proof_s4}}` (pending facts.md). |
| 9. Finance build-out, S1 FMCG and distribution | role_family = finance_buildout AND segment = S1 | They are growing the finance team in a business that collects from many retailers and distributors and has to reconcile those collections. | Retailer collections land in dedicated virtual accounts and reconcile automatically, so the new team starts on that rather than on manual matching [company]. |
| 10. Finance build-out, S2 multi-location QSR and retail | role_family = finance_buildout AND segment = S2 | They are growing the finance team in a chain where spending happens in every store. | Spend visibility across every store, with expense accounts and budgets per store or team, set up while the finance team is being built [company]. |
| 11. Finance build-out, S3 logistics, ports and manufacturing | role_family = finance_buildout AND segment = S3 | They are growing the finance team in a business with local and international payments and an ERP. | One platform for local and international payments that syncs with the ERP, so the growing team doesn't reconcile across separate bank portals [company]. |
| 12. Finance build-out, S4 B2B services and SaaS | role_family = finance_buildout AND segment = S4 | They are formalising finance in a growing services firm. A Duplo SaaS customer gave this stage's advice as "Invest in structure early." [customers] | Putting approvals, expense accounts and reconciliation in place while the finance function is being formalised. Social proof: `{{social_proof_s4}}` (pending facts.md). |

### Same cell, two recipients

The cell's angle stays the same for both. Only the framing changes, so no new axis is needed.

- **Champion:** the day-to-day work (approvals, payouts, invoice clearance).
- **Economic buyer:** the company-level view. That is compliance exposure in cells 1 to 4, and cost and visibility in cells 5 to 12.

### Niche-native check

| Cells | Result |
|---|---|
| 1 to 4 | Pass. They rest on Duplo's two NRS licences, which no spend tool in `competitors.md` is shown to hold. |
| 5 to 8 | Pass, on segment detail: distributor payments, per-store bills, haulier payouts, small-team contractor pay. |
| 9 to 12 | Weakest. Close to what Bujeti could claim. Kept in v1 because general finance posts are most of the volume. These are the first cells to cut or sharpen in the weekly review. |

### Branch coverage

- Cells 7 and 8 mention foreign currency only when `mentions_fx` is true.
- Cells 2, 6 and 10 can always mention stores, because S2 inclusion requires multi-location evidence.
- No cell uses `location_count` or `hiring_volume_cluster` as its angle. Both are context clauses only.
- `{{social_proof_*}}` stays empty until facts.md approves names. If none is approved, the proof clause is dropped and the angle still stands.

---

## 6. Variables contract [ENGINE]

| Variable | Source field | Exists in branches |
|---|---|---|
| `{{first_name}}` | People table: person first name (Find People or Claygent) | All |
| `{{company_name}}` | Companies table: normalised company name | All |
| `{{role_title}}` | Trigger table: job title, cleaned (no reference codes, no location suffix) | All |
| `{{role_family}}` | Signal 2 enum | All. Routing only, never printed |
| `{{segment}}` | Signal 3 enum | All. Routing only, never printed |
| `{{recipient_persona}}` | People table: persona enum (champion / economic_buyer) from title match | All. Routing only, never printed |
| `{{job_posted_date}}` | Trigger table: posted date | All. Freshness gate only, never printed |
| `{{location_count_cluster}}` | Signal 4: label from the Google Places count (`10_plus`) | S2 only, when Places returns a count of 10 or more |
| `{{mentions_locations}}` | Signal 4 boolean | All (may be false) |
| `{{mentions_fx}}` | Signal 5 boolean | All (may be false). Used by cells 7 and 8 |
| `{{hiring_volume_cluster}}` | Signal 6 label: `single_hire` / `team_growing` | All |
| `{{social_proof_s2}}` | `context/messaging/facts.md` (approved customer name and claim for S2) | Cells 2, 6, 10. **Empty until facts.md exists** |
| `{{social_proof_s4}}` | `context/messaging/facts.md` (approved customer name and claim for S4) | Cells 4, 8, 12. **Empty until facts.md exists** |

Rules:
- If a field isn't delivered by the enrichment, copy can never say it.
- Raw counts, turnover figures and e-invoicing deadline dates specific to the company are not in this contract. Copy must not use them.

---

## 7. Cadence and channel [ENGINE]

**Periodicity:** always-on, weekly.
- n8n runs the Apify scrapers weekly. Job posts are high velocity (`workflow-anatomy.md` periodicity table).
- A post stays eligible for 90 days from its posted date.
- A company re-enters only with a new qualifying post, after the suppression window (TO DECIDE).

**Channel:** email and LinkedIn (`gtm-system.md` step 8).

| Item | Detail |
|---|---|
| Email | Instantly or PlusVibe (TO DECIDE). Valid, verified emails only. Catch-all policy TO DECIDE |
| LinkedIn | Tool TO DECIDE (playbook example: HeyReach). Used alone when there's no valid email |
| Sequence shape | 1. email<br>2. LinkedIn connect<br>3. email<br>4. LinkedIn DM<br>5. email<br>Day gaps TO DECIDE |
| Multi-threading | Champion first. The economic buyer starts after an offset (TO DECIDE). Up to one person per persona |
| Stop rule | Any reply on any channel stops every step for that person. It then goes to reply handling (`gtm-system.md` step 9) |

**Before launch:**
1. `/campaign-qa` passes.
2. `/inbox-placement-test` passes.
3. The user approves the launch (`CLAUDE.md`: ask before sending).

---

## Definition of done

- [x] Mother intent: one sentence, signal-led, matched to the catalog
- [x] Inclusion: ICP box, trigger definition, recipient [ICP]
- [x] Exclusion: who's out and why
- [x] Sources: trigger and every enrichment
- [x] Signals table: every read is deterministic, every role is declared
- [x] Count clusters declared (signals 4 and 6; neither routes)
- [x] Routing matrix with the angle column complete (12 cells)
- [x] Branch coverage checked
- [x] Variables contract: every variable mapped to its source field
- [x] Engine/ICP markers on every section
- [x] Cadence and channel declared

## Next

Copy: `/campaign-copy`, run on this file.
- Its gate 1 reads this status line and this checklist.
- `context/messaging/facts.md` must exist before any social proof is used.
