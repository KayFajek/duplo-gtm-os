# Duplo outbound GTM system

Status: v1 design, 2026-11-12. Nothing here is built or live yet.
Built from: `context/icp/icp.md`, `context/icp/personas.md`, `context/icp/signal-matrix.md` (and the research behind them), the stack in `CLAUDE.md`, and the playbooks in `knowledge-base/playbooks/`.

Tool labels:
- **STACK** = already in Duplo's stack (`CLAUDE.md`): Clay, HubSpot, Instantly / PlusVibe, n8n, and the local tools rex, crawl4ai and agent-browser.
- **PLAYBOOK** = recommended by `knowledge-base/playbooks/`, but not yet in the stack.
- **TO DECIDE** = a choice nobody has made yet. All of these are listed at the end.
- **INFERRED** = not supported by the research.

No numbers are invented. Any threshold not set by the research or the playbooks is marked TO DECIDE.

## The flow at a glance

Visual flowchart on Miro: [Duplo GTM System](https://miro.com/app/board/uXjVEOG1MrE=/) (drawn 2026-11-13 with `/miro-gtm-flowchart`; spec in `diagrams/duplo-gtm-system.json`). The board has three rows: row 1 (1A to 1K) is steps 0 to 4; row 2 (2A to 2K) is steps 5 to 7 plus copy and launch approval from step 8; row 3 (3A to 3F) is sending (step 8) through the weekly loop (step 11).

```
0 Context OS ─► 1 Signals ─► 2 Find companies ─► 3 Enrich ─► 4 ICP fit check ─► 5 Route by segment
                                                                 │ fail                    │
                                                                 ▼                         ▼
                                                            Drop + log            6 Find people ─► 7 Find + verify emails
                                                                                                         │
       11 Weekly feedback loop ◄── 10 HubSpot CRM ◄── 9 Reply handling ◄── 8 Sequence (email + LinkedIn) ◄┘
              │
              └──────► back into 0 (Context OS files updated)
```

Every step writes back to one shared Clay workbook (Companies table, People table) and, from step 8 onward, to HubSpot.

---

## Step 0. Context OS

**What happens.** Every workflow starts by reading the context files. Nothing is targeted or written from memory.

**Tools.** The repo itself and Claude skills. The skill chain is `/signal-matrix`, then `/campaign-strategy`, then `/build-workflow-spec`, then `/campaign-copy`. Clay builds go through `/campaign-contract`, `/design-clay-table-json`, `/import-clay-table` and `/save-clay-schema` (STACK).

**Files read.**

| File | Used for |
|---|---|
| `icp.md` | Fit rules (step 4) |
| `personas.md` | Titles (step 6) and objections (step 9) |
| `signal-matrix.md` | Signals (step 1), routing (step 5) and the tracker |
| `context/docs/research/` | Facts allowed in copy |
| `weekly-priorities/<week>.md` | Which workflows run this week |

**Decisions.**

| Decision | Answer | Leads to |
|---|---|---|
| Does `context/messaging/facts.md` exist, with approved customer names and claims? | No (true today) | Create it before any copy. Copy iron law 4 allows only social proof listed there. Until then, step 8 is blocked. |
| | Yes | Copy can use the listed names (for example Eat'N'Go or Maersk) and claims. |
| Does `context/brand-kit/brand.md` exist? | No (true today) | Copy follows `context/docs/research/voice.md` and the copy iron laws. |
| | Yes | Copy follows brand.md. |
| Is the workflow marked "Planned" in signal-matrix Matrix 3 and listed in this week's priorities? | No | Don't build it. |
| | Yes | Go to step 1 with a workflow-spec from `/build-workflow-spec`. |

---

## Step 1. Buying signals (always on)

**What happens.** Scheduled jobs pull fresh signal events. Each workflow starts from its signal, then applies the ICP filter, never the other way round (signal-matrix framework guardrail).

| Workflow | Signal | Source and tool | How often |
|---|---|---|---|
| W1 | Turnover in the ₦1bn to ₦5bn band before e-invoicing enforcement in January 2027 | NGX filings via `/crawl4ai`, then a Clay Use AI extraction column (STACK) | One-time build, then quarterly (INFERRED: filings are annual) |
| W2 | Outlet count up | Google Places API via Clay HTTP (STACK, Clay) | Monthly |
| W3 | Finance, AP or tax hiring | Apify Jobberman and MyJobMag actors (PLAYBOOK source, verified to exist), into Clay through an n8n webhook (STACK) | Weekly |
| W4 | New finance leader in the last 90 days | Clay Find People, filtered by title and start date (STACK) | Monthly |
| W5 | Funding in the last 6 months (VC-backed part of S3 to S5 only) | Crunchbase via Clay Find Organization | Every two weeks |
| W6 | Several banks listed in the annual report | `/crawl4ai` on annual reports, then Use AI (INFERRED source) | Quarterly |
| W7 | New CBN licence | CBN licence list via Clay HTTP (INFERRED; verify first) | Monthly |

Frequencies follow the periodicity table in `workflow-anatomy.md`. n8n (STACK) owns the schedules.

**Decisions.**

| Decision | Answer | Leads to |
|---|---|---|
| Is the source reachable and returning rows this run? | No | Alert the owner (n8n), skip the run and log it. Don't backfill by hand: manual research is not a workflow. |
| | Yes | Go to step 2. |
| Is the event within its window (job post or leadership change under 90 days; funding under 6 months)? | No | Drop the event. |
| | Yes | Keep it, tagged with the event date and the workflow ID (W#). |
| Has W7's CBN source been verified? | No | W7 stays off. |
| | Yes | Turn W7 on. |

---

## Step 2. Finding companies

**What happens.** Each signal event becomes one company row in the Clay Companies table, keyed on domain. That row is then checked against HubSpot.

**Tools.** Clay (STACK): a Companies table built with `/build-clay`, plus Claygent for domain lookup. HubSpot (STACK) is the source of truth for existing relationships.

**Decisions.**

| Decision | Answer | Leads to |
|---|---|---|
| Does the event have a company domain? | Yes | Normalise the domain and dedupe on it. |
| | No | Claygent looks up the domain from the company name. Still none: drop and log. |
| Is the company already in the Companies table? | Yes | Add the new signal to the existing row (signals stack). Don't create a duplicate. |
| | No | Create the row. |
| HubSpot status? | Customer | Suppress. Notify the account owner if the signal suggests an upsell (for example W6 multi-bank, which fits Treasury). |
| | Open deal | Suppress from sequences and notify the deal owner with the signal. |
| | Contacted inside the suppression window | Suppress (window length TO DECIDE). |
| | Closed-lost | Re-enter only after the waiting period (TO DECIDE), with a new signal. |
| | Unknown | Go to step 3. |

---

## Step 3. Enrichment

**What happens.** Paid enrichment adds the attributes the fit check and routing need, and nothing else.

| Field | Source | Needed for |
|---|---|---|
| Industry, headcount, HQ country | Clay company enrichment | Steps 4 and 5 |
| Turnover (where public) | NGX filings or press via Claygent | ICP band (U1) |
| Outlet count | Google Places | S2 fit (10+ outlets) |
| Foreign-currency flows (site mentions overseas clients or suppliers, or an import/export business) | Claygent scan of the company website (INFERRED method) | Routing of the S3 and S4 angles |
| Number of banks | Annual report "Bankers" section (INFERRED) | Treasury (TMS) upsell flag |
| Looks like a named customer | Clay lookalike (U4) | Social-proof match only |

**Tools.** Clay (STACK). Cost checks use `/calculate-costs` and `/workspace-cost-audit`. `/import-clay-table` proves every paid column on one row that should qualify and one that shouldn't, before it runs at scale.

**Decisions.**

| Decision | Answer | Leads to |
|---|---|---|
| Have the cheap fields been filled first (industry, country)? | They fail the ICP country or industry rule | Stop. Don't run paid columns. Go to step 4 as a fail. |
| | They pass | Run the paid columns. |
| Is the projected cost per row within budget? | No | Pause the column and review it with `/calculate-costs` (budget TO DECIDE). |
| | Yes | Run it. |
| Does Clay or Apollo coverage of Nigerian companies hold up on the 20-company test (an open question in signal-matrix)? | No | Lean on Claygent and website scraping for firmographics. Expect lower volume. |
| | Yes | Use the standard enrichment path. |

---

## Step 4. ICP fit check

**What happens.** A formula column applies the rules in `icp.md` and outputs Pass, Fail (with a reason) and a priority.

**Tool.** A Clay formula column. Its rules are copied from `icp.md`, which stays the source of truth.

**Decisions.**

| Decision | Answer | Leads to |
|---|---|---|
| Is the company operating in Nigeria? | No | Fail: "geo". South Africa is excluded in v1. |
| | Yes | Next check. |
| Does it match a not-a-fit row (core need is cards or payroll; MSME that wants POS or bookkeeping)? | Yes | Fail: "not-fit". |
| | No | Next check. |
| Is it VAT-registered, with turnover of ₦1bn or more (known)? | Yes, plus a B pattern (multi-location, foreign currency, or several banks) | Priority 1. |
| | Yes, no B pattern | Priority 2. Lead with e-invoicing only. |
| | Turnover unknown, but a B pattern | Priority 3. Lead with payments and approvals. |
| | Turnover known to be under ₦1bn, domestic only | Fail: "too early". Re-check before the July 2027 go-live (`icp.md`). |
| | Turnover unknown, no B pattern | Fail: "no evidence". |
| Did this company come from W1 (turnover band)? | Its turnover is ₦5bn+ | Large taxpayer, past the 31 July 2026 deadline. Use the deadline-passed angle. |
| | ₦1bn to ₦5bn | Medium taxpayer. Use the enforcement-from-January-2027 angle. |

Every Fail is kept with its reason, so the weekly loop can see which rules cut the most companies.

---

## Step 5. Routing by segment

**What happens.** Each passing company gets exactly one segment, which sets the lead product and the angles for each persona (signal-matrix, Matrix 1).

**Tool.** A Clay Use AI classifier column built with `/prompt-builder`. Its allowed outputs are S1 to S5 or "unclear".

| Segment | Lead product | Upsell | Status |
|---|---|---|---|
| S1 FMCG and distribution | Payments and collections, plus e-invoicing | Treasury (TMS) | Active |
| S2 Multi-location QSR and retail | Expense management, approvals, bulk payouts | E-invoicing | Active |
| S3 Logistics, ports, manufacturing | Global payments, vendor payouts | E-invoicing | Active |
| S4 B2B services and SaaS | Expense management, approvals | Cross-border payments | Active |
| S5 Fintech and remittance | Atlas payments API | Treasury | Hold. Low priority; may be inbound or partner-led (open question) |

**Decisions.**

| Decision | Answer | Leads to |
|---|---|---|
| Is the classifier output one of S1 to S4? | Yes | Go to step 6 with the segment angles. |
| | S5 | Hold it in a view for the sales team. No outbound in v1. |
| | Unclear | Manual review queue, or drop if the queue is over capacity (capacity TO DECIDE). |
| Which signal opens the email? | The strongest signal on the row | The opener follows signal priority: U1 deadline, then N1 outlets, then U2 hiring, then U3 new leader, then the segment fallback. This matches the build-first order in signal-matrix. |

---

## Step 6. Finding people

**What happens.** For each routed company, find up to three people: one per persona.

**Tools.** Clay Find People (MixRank) for structured search, plus Claygent for public-web coverage. Both feed one shared People table, as the `/build-clay` method requires (STACK).

| Persona | Title keywords (`personas.md`) | Who to reach |
|---|---|---|
| Champion: finance ops lead | Operations Lead, Operations Manager, Regional Operations Manager, Treasury Manager, AP Officer, Treasury Analyst | Always |
| Economic buyer: CEO or finance head | CFO, Head of Finance, Financial Controller. CEO or Founder at smaller firms | Always |
| Influencer: tax and systems owner | Tax or VAT Officer/Manager; ERP, IT or Finance Systems lead | Only on the U1 e-invoicing route (INFERRED: that's where integration gates the deal) |

**Decisions.**

| Decision | Answer | Leads to |
|---|---|---|
| Is the company small (headcount band TO DECIDE)? | Yes | Target the CEO or Founder as the economic buyer and skip the influencer (the Prunet pattern: the CEO found Duplo). |
| | No | Target the CFO or finance head. |
| Was a champion found? | Yes | Sequence the champion first. |
| | No | Economic buyer only. |
| Was anyone found? | No | Mark the company "no contact" and retry at the next refresh. No guessed emails. |
| How many people per company? | Up to 3, one per persona (cap TO DECIDE) | Multi-thread with a different angle for each persona. The economic buyer starts after the champion (offset TO DECIDE). |

---

## Step 7. Finding and verifying emails

**What happens.** A waterfall finds a work email, then a validator decides whether that email may be sent to.

**Tools.** A Clay waterfall in the order `knowledge-base/playbooks/clay/expert.md` recommends: Prospeo, then Icypeas, then Leadmagic, then Findymail (PLAYBOOK). Validation with Bounceban (PLAYBOOK). The provider choice is TO DECIDE, because Nigerian coverage hasn't been tested.

**Decisions.**

| Decision | Answer | Leads to |
|---|---|---|
| Was an email found? | Yes | Validate it. |
| | No | LinkedIn-only track, if there's a profile URL. Otherwise drop. |
| Validation result? | Valid | Email plus LinkedIn sequence. |
| | Catch-all | Send through a separate catch-all campaign, or LinkedIn only (policy TO DECIDE). |
| | Invalid or unknown | LinkedIn only. Never email. |
| Is the email a personal one (gmail and similar)? | Yes | Don't email it. LinkedIn only (INFERRED policy). |
| | No | Continue. |

---

## Step 8. The sequence (email and LinkedIn)

**What happens.** Each person gets a short sequence whose opener matches their signal and whose angle matches their segment and persona. Nothing goes out without human approval (`CLAUDE.md`: "Ask before sending or publishing anything.").

**Tools.**
- Copy: `/build-workflow-spec` builds the structure and gets it approved, then `/campaign-copy` writes the copy.
- Email: Instantly or PlusVibe (STACK). Which one is TO DECIDE.
- LinkedIn: no tool in the stack. The playbook example is HeyReach (TO DECIDE).
- QA: `/campaign-qa` checks the table and campaign, and `/inbox-placement-test` (EmailBison plus MailReach) checks deliverability.

**Copy rules (copy iron laws plus `CLAUDE.md`).**
- Aim for 60 words, 120 at most.
- The call to action is a question.
- 3:1, prospect to Duplo.
- Social proof must be matched, and only from facts.md.
- No em dashes.
- Each follow-up adds something new.
- Guess their situation, never diagnose their gaps.

**Opener waterfall (line 1, by signal).**

| Route | Situation line is about | Allowed facts (with research source) |
|---|---|---|
| U1, medium taxpayer | NRS enforcement from January 2027 | Timeline and penalties (`market.md`) |
| U1, large taxpayer | The 31 July 2026 deadline has passed | Same |
| N1 outlet growth | Running finance across more stores | Eat'N'Go pattern (`customers.md`, once approved in facts.md) |
| U2 hiring | Building out the finance team | Job title only |
| U3 new leader | First months in the role | Start date only |
| Fallback | Segment pain in the segment's language (signal-matrix angles) | None |

**Sequence shape (proposal, cadence TO DECIDE).**

| Step | Channel | What it does |
|---|---|---|
| 1 | Email | Signal opener. One value line. A question CTA. |
| 2 | LinkedIn | Connection request with no pitch, only if a profile exists. |
| 3 | Email | New proof: a case study matched to the segment. |
| 4 | LinkedIn | DM (`linkedin-dm-frameworks.md`), if connected. |
| 5 | Email | Last touch. Referral or e-invoicing deadline urgency (U1 routes only). |

**Decisions.**

| Decision | Answer | Leads to |
|---|---|---|
| Is the workflow spec approved (STRUCTURE-APPROVED)? | No | `/campaign-copy` won't run. |
| | Yes | Write the copy. |
| Did `/campaign-qa` pass? | No | Fix the table or copy and re-run QA. |
| | Yes | Placement test. |
| Inbox placement result? | Poor (threshold TO DECIDE) | Fix the domains or copy, then retest. Don't launch. |
| | Good | Ask the user to approve the launch. |
| Has the user approved? | No | Hold. |
| | Yes | Launch, and record the campaign in signal-matrix Matrix 3. |
| Does the person have a valid email? | Yes | Email plus LinkedIn. |
| | No | LinkedIn steps only. |
| Has the person replied on either channel? | Yes | Stop all steps for that person. Go to step 9. |

---

## Step 9. Reply handling

**What happens.** Every reply is classified within the working day (SLA TO DECIDE), then acted on and logged in HubSpot.

**Tools.** A webhook from the sequencer or LinkedIn tool, into n8n (STACK). An AI classifier inside n8n tags each reply. Positive replies always go to a human before any response is sent.

| Category | Action | HubSpot |
|---|---|---|
| Positive or interested | A human replies the same day with a meeting ask (copy iron law 6: step 2 earns the meeting) | Contact to SQL. Create a deal. |
| Referral ("talk to X") | Add X to the People table on the same route, with "referred by" in the opener | Log the referral on both contacts. |
| Objection | Reply from an approved objection response (to be written; source objections from `personas.md`: pricing clarity, PDF invoices, vendor choice versus other SI/APPs, ERP integration, security) | Tag the objection type. |
| Not now / timing | Snooze and re-enter at the date they give, or at the next signal | Task with the date. |
| Not interested | Stop. Suppress for the window (TO DECIDE). | Mark disqualified, with a reason. |
| Unsubscribe | Stop on every channel, permanently | Opt out. |
| Out of office | Pause, then resume after the return date | None. |
| Bounce | Mark the email invalid and switch to LinkedIn only | Count toward the step 11 bounce metric. |

**Decisions.**

| Decision | Answer | Leads to |
|---|---|---|
| Is the classifier confidence high? | No | Human review. |
| | Yes | Auto-tag. Positive replies still go to a human. |
| Is it a new objection type not listed in personas.md? | Yes | Log it for step 11 so it gets added to `personas.md`. |
| | No | Use the existing response. |

---

## Step 10. CRM (HubSpot)

**What happens.** HubSpot holds every company, contact and deal the system touches, so attribution survives after the Clay tables are archived.

**Tools.** HubSpot (STACK). Clay exports company and contact rows on enrollment, and n8n syncs reply events.

**Properties to write.** All are new custom properties (INFERRED design):

| Object | Properties |
|---|---|
| Company | segment (S1 to S5), ICP priority (1 to 3), signal(s) with dates, workflow ID (W#), turnover band, outlet count |
| Contact | persona (champion, economic buyer or influencer), campaign name, sequence step reached, reply category, objection type |
| Deal | source workflow (W#), first-touch signal, lead product |

**Decisions.**

| Decision | Answer | Leads to |
|---|---|---|
| Is the company already in HubSpot? | Yes | Update it. Never create a duplicate (dedupe on domain). |
| | No | Create it on enrollment. |
| Was a meeting booked? | Yes | Create a deal and assign an owner (rule TO DECIDE: by segment or round robin). |
| | No | Contact stays a lead. |
| Did the deal close? | Won | Customer. Add it as social proof in facts.md only if the customer approves. |
| | Lost | Record the reason and feed it to step 11 and to the closed-lost re-entry rule in step 2. |

---

## Step 11. Weekly feedback loop

**What happens.** Once a week, results are reviewed per workflow, segment, signal, persona and copy variant, then written back into the Context OS so next week starts smarter.

**Tools.**
- HubSpot reports and sequencer analytics.
- `/calculate-costs` and `/workspace-cost-audit` for spend.
- `/inbox-placement-test` for deliverability.
- The output is written into repo files and committed.

**Metrics.**
- Positive reply rate. This is the deciding metric (iron law 11), not total reply rate.
- Meetings and deals per workflow.
- Bounce rate.
- Cost per qualified company and per meeting.
- Fail reasons from step 4.
- New objections from step 9.

**Decisions.**

| Decision | Answer | Leads to |
|---|---|---|
| Does a copy variant have 200+ sends (iron law 11)? | No | Keep testing. |
| | Yes, and it leads on positive reply rate | Make it the default, and write a new challenger. |
| Is a workflow producing meetings? | Yes | Keep it. Consider widening its segment or keywords. |
| | No, after enough volume (TO DECIDE) | Change one variable (signal, angle or persona) per iteration (`workflow-anatomy.md`). If it still fails, kill it and record why in signal-matrix. |
| Is the bounce rate over the limit (TO DECIDE)? | Yes | Pause the campaign and audit step 7. |
| | No | Continue. |
| Is a C-tier hypothesis (U3, the new finance leader) producing meetings? | Yes | Promote it to B in `icp.md`. |
| | No | Leave it, or kill it. |
| Is the subject line older than 4 to 6 weeks, or the body older than 8 to 12 weeks (iron law 11)? | Yes | Refresh it. |
| | No | Keep it. |

**What gets written back (step 0 inputs).**

| File | Update |
|---|---|
| `context/icp/signal-matrix.md` | Re-score signals, kill or promote them, update the Matrix 3 tracker |
| `context/icp/icp.md` | Fit rules, the not-a-fit list, answered open questions |
| `context/icp/personas.md` | Real pains in the buyer's own words (from replies and calls), new objections |
| `context/messaging/` | Winning lines, objection responses, approved proof |
| `knowledge-base/learnings/` | What worked, what didn't, and why |
| `weekly-priorities/<next week>.md` | Which workflows run next week |
| `work-log/<week>.md` | The change log (required by `CLAUDE.md`) |

---

## Four-pillar check

| Pillar | Where the design meets it | Gaps |
|---|---|---|
| **Clean data** | Domain is the single key (step 2). HubSpot dedupe before any enrichment. Cheap fields gate the paid ones (step 3). Email validation before sending (step 7). Fail reasons kept (step 4). One shared People table (step 6). | Nigerian coverage of Clay, Apollo and the email providers is untested. Turnover data is only public for listed or press-covered firms. HubSpot custom properties don't exist yet. |
| **Always-on signals** | W1 to W6 run on n8n schedules at the frequencies in `workflow-anatomy.md` (step 1). Events are dated and expire. Signals stack on one company row. | W1 is quarterly at best, because filings are annual. W7's source is unverified. Volume per signal is unknown until the first run. |
| **Composable workflows** | U2 and U3 are one Clay build each, reused across S1 to S4 with keyword swaps. The skill chain (spec, then contract, then table JSON, then import) produces repeatable builds. Approved tables are saved with `/save-clay-schema`. The routing waterfall is shared. | No Clay tables exist yet. The LinkedIn tool and the sequencer aren't chosen. |
| **Tight feedback loops** | Weekly review (step 11) writes back to the same files step 0 reads. Reply categories and objections flow into personas. Positive reply rate is the deciding metric. Kill rules are documented. | No baseline metrics yet. Thresholds (bounce, kill volume) are TO DECIDE. Only two buyer quotes exist, so step 11's first job is collecting real buyer language. |

---

## Decisions still open (owner: Duplo GTM lead)

1. Instantly or PlusVibe as the email sequencer.
2. The LinkedIn tool (the playbook example is HeyReach), or LinkedIn steps sent by hand.
3. Email waterfall providers and the validator, after testing Nigerian coverage on 20 known companies.
4. Suppression window, closed-lost re-entry period, and the reply-handling SLA.
5. Catch-all email policy.
6. Bounce limit, placement threshold, and the volume needed before a workflow is killed.
7. People cap per company, and the delay before the economic buyer is contacted.
8. Enrichment budget per row and per month.
9. Deal-owner assignment rule in HubSpot.
10. Who approves each launch (the `CLAUDE.md` approval rule).
11. Creating `context/messaging/facts.md` with approved customer names and claims. **This blocks step 8.**
