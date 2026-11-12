# Duplo GTM Context OS

## Company
Duplo (tryduplo.com), a financial operating system for African businesses (source: GTM team brief). Full description to fill in after research, with sources.

## How this folder works
- `.claude/skills/`: reusable Claude skills for GTM tasks. Read when a task matches a skill.
- `context/icp/`: ideal customer profiles and personas. Read before any targeting, prospecting or copy.
- `context/positioning/`: positioning, category and competitive differentiation. Read before strategy or messaging work.
- `context/messaging/`: approved messaging, value props and proof points. Read before writing any copy.
- `context/brand-kit/`: voice, tone, visual and style rules. Read before producing any customer-facing asset.
- `context/docs/research/`: raw research and source material with citations. Read when you need to verify a fact.
- `knowledge-base/playbooks/`: repeatable how-to processes. Read before running a known motion.
- `knowledge-base/learnings/`: what worked, what didn't, and why. Read before planning new campaigns.
- `meetings/`: meeting notes and transcripts. Read when a task references a past conversation or decision.
- `output/campaigns/`: finished and in-progress campaign deliverables. Write here; read for prior examples.
- `tools/`: scripts and tool configs. Read when a task needs automation or an integration.
- `weekly-priorities/`: one file per ISO week (YYYY-Www.md). Read this week's file at the start of every session.
- `work-log/`: one file per ISO week, matching weekly-priorities. Append an entry for every change.

## Skills
Type `/` to run. Method docs live in `knowledge-base/playbooks/`.
- Strategy: `/signal-matrix` (ICP segments + buying signals), `/campaign-strategy` (15 to 25 ranked outbound ideas).
- Build: `/build-workflow-spec` (structure one play, no copy), then `/campaign-copy` (email + LinkedIn copy from verified facts only).
- Clay builds: `/campaign-contract` > `/design-clay-table-json` > `/import-clay-table` > `/save-clay-schema`; `/build-clay` and `/prompt-builder` for workbooks and AI column prompts.
- Clay ops: `/calculate-costs`, `/workspace-cost-audit` (spend), `/export-tables` (CSV), `/campaign-qa` (QA before launch).
- Research: `/crawl4ai` (crawl sites to Markdown), `/agent-browser` (drive a real browser).
- Deliverability and visuals: `/inbox-placement-test` (EmailBison + MailReach), `/miro-gtm-flowchart` (GTM map on Miro).

## Rules
- Cite a source for every company fact.
- Never invent numbers. If a number is unknown, say so.
- No em dashes in any copy.
- Ask before sending or publishing anything.
- Log every change in this week's work log (`work-log/YYYY-Www.md`).

## Tools
GTM stack:
- Clay: enrichment, list building and AI columns. Most Clay skills run through `tools/rex/`.
- HubSpot: CRM, source of truth for contacts, companies and deals.
- Instantly / PlusVibe: cold email sequencing. Check campaigns with `/campaign-qa` before launch.
- n8n: workflow automation between tools.

Local tools (`tools/`, read each README before use):
- `tools/rex/`: unofficial Clay SDK using your browser session. Start with `GETTING_STARTED.md`. Session files stay out of Git.
- `tools/crawl4ai/`, `tools/agent-browser/`: install guides for local crawling and browser control.
- Miro MCP: needed for `/miro-gtm-flowchart` (not connected yet).
