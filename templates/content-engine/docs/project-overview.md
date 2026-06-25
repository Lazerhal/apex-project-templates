# Project Overview — Content Engine
**APEX ID:** APEX-CONT-YYYYMM-NNN | **Schema Version:** 1.0

---

## 1-2. Identity and Summary

| Field | Value |
|---|---|
| Name | [Project Name] |
| Type | content-engine |
| Stage | [stage] |
| Live URL | [URL] |
| Owner | Hunter Lazenby |
| Created | YYYY-MM-DD |

**Short Description:** [What content, how often, who reads it, how it earns.]  
**Goal:** [Traffic target, revenue target, or strategic purpose.]  
**Status Note:** [Current state — scheduler health, recent traffic trend, last publish.]

---

## 3-6. Repo, Stack, Run, Deploy

**Repo:** https://github.com/[USERNAME]/[repo] | **Branch:** main | **Visibility:** [public/private]

| Layer | Technology |
|---|---|
| Backend | [e.g. Python 3.11 + Flask] |
| AI/Content | [e.g. Google Gemini Flash — content generation] |
| Persistence | [e.g. Google Drive API — post storage] |
| Hosting | [e.g. Railway] |
| Images | [e.g. Pexels API + Unsplash API] |
| Analytics | [e.g. GA4 + Google Search Console] |

**Local Run:**
1. `pip install -r requirements.txt`
2. Configure `.env` with all keys (see Environment Variables)
3. `python app.py`

**Deploy:** Git push to main → Railway auto-deploys. Rollback via Railway dashboard.

---

## 7. Environment Variables

| Variable | Purpose | Required | Stored In |
|---|---|---|---|
| GEMINI_API_KEY | AI content generation | Yes | Railway env |
| GOOGLE_DRIVE_CREDS | Drive persistence | Yes | Railway env (JSON) |
| PEXELS_API_KEY | Hero images | Yes | Railway env |
| UNSPLASH_ACCESS_KEY | Fallback images | Yes | Railway env |
| AMAZON_AFFILIATE_TAG | Affiliate links | Yes | Railway env |
| GA_MEASUREMENT_ID | Analytics | Yes | Railway env |

*No values recorded here — names only.*

---

## 8-9. Services and Costs

| Service | Purpose | Monthly Cost |
|---|---|---|
| [e.g. Railway] | Hosting | $5 |
| [e.g. Gemini API] | Content generation | Variable |
| [e.g. Pexels] | Images | Free |
| [e.g. Namecheap] | Domain | $1.25 (amortised) |

**Monthly Total Estimate:** $[X]  **Cost Efficiency:** [healthy/watch/inefficient]

---

## 10. Metrics and KPIs

| KPI | Value | Target | Source |
|---|---|---|---|
| Monthly sessions | [X] | [X] | Plausible / GA4 |
| Indexed pages | [X] | [X] | Search Console |
| Affiliate clicks MTD | [X] | [X] | Amazon Associates |
| Affiliate revenue MTD | $[X] | $[X] | Amazon Associates |
| Content runway | [X posts / X months] | 12+ months | Internal count |

**Revenue Model:** [e.g. Amazon Associates affiliate commissions]

---

## 11-12. What Works / Known Issues

**Working:**
- [Specific working feature]
- Daily scheduler publishing at [time]

**Known Issues:**

| ID | Description | Severity | Status |
|---|---|---|---|
| KI-001 | [Issue] | [severity] | open |

---

## Content Engine Extension

**Publishing Cadence:** [e.g. 1 post/day at 13:00 UTC]

**Content Pipeline:**  
[Step-by-step: how a post goes from trigger → AI generation → formatting → image selection → publish]

**Content Runway:** [e.g. ~731 topics queued (~2 years at current cadence)]

**SEO Status:** [Indexed pages, ranking, Search Console health]

**Monetisation Sources:**

| Source | Status | Monthly Est. |
|---|---|---|
| [Amazon Associates] | Active | $[X] |

**Promotion Channels:**
- [Active: e.g. Organic search only]
- [Planned: e.g. Reddit r/hunting, newsletter]

**Image Sources:**
- [e.g. Pexels API (primary) — free tier, 200 req/hr]
- [e.g. Unsplash API (fallback) — demo tier, 50 req/hr]

**Scheduler Health:** [healthy/degraded/down/unknown]

---

## 13-17. Tasks, Decisions, Risks, Handoff

**Task Backlog:**

| ID | Title | Priority | Type | Effort |
|---|---|---|---|---|
| T-001 | [Task] | [priority] | [type] | [effort] |

**Decision Log:**

| ID | Date | Decision | Rationale |
|---|---|---|---|
| D-001 | YYYY-MM-DD | [Decision] | [Why] |

**Handoff:**  
**Last Session:** YYYY-MM-DD via [tool]  
**Done:** [What changed]  
**Remains:** [What's next]  
**Blocked:** [Nothing / blocker]  
**Next should:** [Specific instruction]

---

## 18. Sync Status
**Status:** in_sync | **Last DB Write:** YYYY-MM-DD | **Schema Version:** 1.0
