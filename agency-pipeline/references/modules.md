# Agency Tools — Module Reference

## Table of Contents
1. [Setup Wizard](#setup-wizard)
2. [Intake Server](#intake-server)
3. [Client Onboard](#client-onboard)
4. [Quote Generator](#quote-generator)
5. [Proposal Generator](#proposal-generator)
6. [Contracts & Invoices](#contracts--invoices)
7. [Recurring Invoices](#recurring-invoices)
8. [AI Campaign](#ai-campaign)
9. [Campaign Generator](#campaign-generator)
10. [Campaign Autopilot](#campaign-autopilot)
11. [Content Calendar](#content-calendar)
12. [Pipeline](#pipeline)
13. [Slideshow Generator](#slideshow-generator)
14. [Video Compiler](#video-compiler)
15. [Approve](#approve)
16. [Client Portal](#client-portal)
17. [Postiz Sync](#postiz-sync)
18. [A/B Test](#ab-test)
19. [Feedback Loop](#feedback-loop)
20. [Daily Run](#daily-run)
21. [Client Report](#client-report)
22. [Dashboard](#dashboard)
23. [Client Health](#client-health)
24. [Notifications](#notifications)
25. [Morning Brief](#morning-brief)
26. [Outreach](#outreach)
27. [Outreach Send](#outreach-send)
28. [Lead Magnet](#lead-magnet)
29. [Email Library](#email-library)

---

## Setup Wizard
**File:** `src/setup-wizard.js`

Interactive first-run configuration. Creates `.env`, seeds demo data, verifies integrations.

| Command | Description |
|---------|-------------|
| `npm run setup` | Run setup wizard |
| `npm run setup:init` | Interactive config (AI provider, Postiz, SMTP, agency details) |
| `npm run setup:demo` | Seed "FitFlow" demo client with full lifecycle data |
| `npm run setup:check` | Health check all integrations |

---

## Intake Server
**File:** `src/intake-server.js`

Self-service web form for prospects. Gradient UI, mobile-friendly. Collects business info, audience, brand voice, platforms, goals, budget, contact. Auto-creates client config on submit.

| Command | Description |
|---------|-------------|
| `npm run intake` | Start intake form server on port 3700 |

Token-based access control for sharing intake links.

---

## Client Onboard
**File:** `src/client-onboard.js`

CLI-based interactive client onboarding. Creates brand bible, platform config, goals.

| Command | Description |
|---------|-------------|
| `npm run onboard -- --client <slug>` | Onboard a client interactively |

---

## Quote Generator
**File:** `src/quote-generator.js`

Professional HTML quotes with 3-tier comparison. Auto-reads intake data. Smart discounts for bundles/commitments. Pricing: ~€2.5k/€4.5k/€7.2k per month.

| Command | Description |
|---------|-------------|
| `npm run quote -- --client <slug>` | Generate quote |
| `npm run quote:template` | Show quote template |
| `npm run quote:list` | List generated quotes |
| `npm run quote:tiers` | Show 3-tier comparison |

---

## Proposal Generator
**File:** `src/proposal-generator.js`

Generate polished client proposals.

| Command | Description |
|---------|-------------|
| `npm run proposal -- --client <slug>` | Generate proposal |

---

## Contracts & Invoices
**File:** `src/contracts.js`

SOW generator with 6-service catalog (social, content, ads, tiktok, strategy, brand). Invoice generator with sequential numbering (INV-2026-0001). Kleinunternehmer §19 UStG compliant.

| Command | Description |
|---------|-------------|
| `npm run contract -- --client <slug>` | Generate Statement of Work |
| `npm run contract:invoice -- --client <slug>` | Generate invoice |
| `npm run contract:list` | List all contracts |
| `npm run contract:template` | Show contract template |
| `npm run contract:paid -- --invoice <id>` | Mark invoice as paid |

---

## Recurring Invoices
**File:** `src/recurring-invoices.js`

Auto-generates monthly invoices from active contracts. Overdue detection and payment reminders.

| Command | Description |
|---------|-------------|
| `npm run invoice:generate` | Generate due invoices |
| `npm run invoice:preview` | Preview without creating |
| `npm run invoice:list` | List all invoices |
| `npm run invoice:overdue` | Show overdue invoices |
| `npm run invoice:remind` | Send payment reminders |

---

## AI Campaign
**File:** `src/ai-campaign.js`

AI content generation from briefs. Core content creation engine.

| Command | Description |
|---------|-------------|
| `npm run ai-campaign -- --client <slug> --brief "..."` | Generate content from brief |

---

## Campaign Generator
**File:** `src/campaign-generator.js`

Template-based campaign generation (non-AI).

| Command | Description |
|---------|-------------|
| `npm run campaign -- --client <slug>` | Generate from templates |

---

## Campaign Autopilot
**File:** `src/campaign-autopilot.js`

One topic → unified multi-platform campaign (TikTok, Instagram, LinkedIn, X, Email). Cross-platform narrative, not separate posts. Built-in slideshow generation. Optional Postiz auto-scheduling.

| Command | Description |
|---------|-------------|
| `npm run autopilot -- --topic "..." --audience "..."` | Generate campaign |
| `npm run autopilot -- --topic "..." --slides` | With slideshow generation |
| `npm run autopilot -- --topic "..." --schedule 2026-03-01` | With auto-scheduling |

---

## Content Calendar
**File:** `src/content-calendar.js`

Manage the content calendar per client.

| Command | Description |
|---------|-------------|
| `npm run calendar -- --client <slug>` | View/manage calendar |

---

## Pipeline
**File:** `src/pipeline.js`

Content pipeline management — track content through draft → review → approved → published states.

| Command | Description |
|---------|-------------|
| `npm run pipeline -- --client <slug>` | View/manage pipeline |

---

## Slideshow Generator
**File:** `src/slideshow-generator.js`

Generate branded slideshows (SVG → PNG) for visual platforms.

| Command | Description |
|---------|-------------|
| `npm run slideshow -- --client <slug>` | Generate slideshows |

---

## Video Compiler
**File:** `src/video-compiler.js`

Compile video content from assets.

| Command | Description |
|---------|-------------|
| `npm run video -- --client <slug>` | Compile video |

---

## Approve
**File:** `src/approve.js`

Content approval workflow.

| Command | Description |
|---------|-------------|
| `npm run approve -- --client <slug>` | Run approval flow |

---

## Client Portal
**File:** `src/client-portal.js`

Web portal for client content review.

| Command | Description |
|---------|-------------|
| `npm run portal -- --client <slug>` | Launch portal |
| `npm run portal:token -- --client <slug>` | Generate access token |
| `npm run portal:tokens` | List all tokens |

---

## Postiz Sync
**File:** `src/postiz-sync.js`

Bridge to Postiz scheduling platform. Syncs calendar, A/B tests, and pulls metrics.

| Command | Description |
|---------|-------------|
| `npm run postiz` | General Postiz operations |
| `npm run postiz:status` | Check Postiz connection |
| `npm run postiz:channels` | List connected channels |
| `npm run postiz:analytics -- --client <slug>` | Pull analytics |
| `npm run postiz:sync -- --client <slug>` | Sync calendar to Postiz |
| `npm run postiz:ab -- --client <slug>` | Sync A/B tests |
| `npm run postiz:metrics -- --client <slug>` | Pull post metrics |

---

## A/B Test
**File:** `src/ab-test.js`

A/B hook testing with statistical significance analysis.

| Command | Description |
|---------|-------------|
| `npm run ab:create -- --client <slug>` | Create A/B test |
| `npm run ab:list` | List all tests |
| `npm run ab:analyze -- --client <slug>` | Analyze results |
| `npm run ab:learnings` | View accumulated learnings |
| `npm run ab:suggest -- --client <slug>` | AI-suggested tests |

---

## Feedback Loop
**File:** `src/feedback-loop.js`

Performance analysis → optimization brief generation. Closes the loop between analytics and content strategy.

| Command | Description |
|---------|-------------|
| `npm run loop -- --client <slug>` | Full analyze + generate brief |
| `npm run loop:analyze -- --client <slug>` | Analyze only |
| `npm run loop:brief -- --client <slug>` | Generate optimization brief only |

---

## Daily Run
**File:** `src/daily-run.js`

Full autopilot orchestrator. Metrics → analyze → briefs → campaigns → reports. Emits health events for scores < 60.

| Command | Description |
|---------|-------------|
| `npm run daily -- --client <slug>` | Single client daily loop |
| `npm run daily:all` | All clients |
| `npm run daily:dry` | Dry run |

Flags: `--auto` (skip approval), `--report-only`, `--skip-report`

---

## Client Report
**File:** `src/client-report.js`

Polished HTML reports with KPI cards, top performers, upcoming campaigns. Auto-emails if SMTP configured.

| Command | Description |
|---------|-------------|
| `npm run report -- --client <slug>` | Generate report |

---

## Dashboard
**File:** `src/dashboard.js`

Agency-wide dashboard view.

| Command | Description |
|---------|-------------|
| `npm run dashboard` | Show dashboard |

---

## Client Health
**File:** `src/client-health.js`

5-dimension scoring (content, engagement, financial, contract, pipeline). Weighted overall score with status levels: 🟢 Healthy / 🟡 Needs Attention / 🟠 At Risk / 🔴 Critical.

| Command | Description |
|---------|-------------|
| `npm run health -- --client <slug>` | Full health report |
| `npm run health:alerts` | Only clients needing attention |
| `npm run health:json` | JSON output |
| `npm run health:history -- --client <slug>` | Score trend over time |
| `npm run health:telegram` | Telegram-formatted output |

---

## Notifications
**File:** `src/notifications.js`

Central event queue with priority (critical/warn/info). 24h dedup. 12 event types.

| Command | Description |
|---------|-------------|
| `npm run notify` | Process pending notifications |
| `npm run notify:digest` | Daily digest |
| `npm run notify:history` | View history (last 500) |
| `npm run notify:clear` | Clear queue |
| `npm run notify:test` | Send test notification |

---

## Morning Brief
**File:** `src/morning-brief.js`

8-section daily overview: health, pipeline, due emails, overdue invoices, pending approvals, calendar gaps, contract warnings, recent events. Auto-generates action items.

| Command | Description |
|---------|-------------|
| `npm run brief` | Full morning briefing |
| `npm run brief:telegram` | Telegram format |
| `npm run brief:json` | JSON output |
| `npm run brief:health` | Health section only |
| `npm run brief:pipeline` | Pipeline section only |
| `npm run brief:invoices` | Invoices section only |

---

## Outreach
**File:** `src/outreach.js`

Prospect research → personalized 5-email sequences → pipeline tracking.

| Command | Description |
|---------|-------------|
| `npm run outreach:find -- --industry "..." --location "..."` | Find prospects |
| `npm run outreach:research -- --url "..."` | Analyze prospect website |
| `npm run outreach:sequence -- --prospect <slug>` | Generate email sequence |
| `npm run outreach:preview -- --prospect <slug>` | Preview emails |
| `npm run outreach:send -- --prospect <slug>` | Mark as sent |
| `npm run outreach:list` | List all prospects |
| `npm run outreach:status` | Pipeline overview |
| `npm run outreach:report` | HTML pipeline report |

---

## Outreach Send
**File:** `src/outreach-send.js`

SMTP email sender for outreach sequences. Respects delays between steps. Send logging.

| Command | Description |
|---------|-------------|
| `npm run email:due` | Show emails due to send |
| `npm run email:send` | Send due emails |
| `npm run email:dry-run` | Preview without sending |
| `npm run email:status` | Delivery status |

---

## Lead Magnet
**File:** `src/lead-magnet.js`

4 types of AI-generated lead magnets → print-ready HTML. CTA boxes included.

| Command | Description |
|---------|-------------|
| `npm run lead-magnet -- --type checklist --topic "..." --audience "..."` | Checklist |
| `npm run lead-magnet:guide -- --topic "..." --audience "..."` | Multi-chapter guide |
| `npm run lead-magnet:toolkit -- --topic "..." --audience "..."` | Tool comparison |
| `npm run lead-magnet:audit -- --topic "..." --audience "..."` | Audit report |

---

## Email Library
**File:** `src/lib/email.js`

Zero-dependency SMTP sender (STARTTLS + AUTH PLAIN). Multipart HTML+text. Bulk send with rate limiting. Falls back to `data/outbox/` if no SMTP configured.

Not called directly — used by other modules (daily-run, outreach-send, invoice:remind).
