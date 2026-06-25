# Project Overview — SaaS
**APEX ID:** APEX-SAAS-YYYYMM-NNN | **Schema Version:** 1.0

---

## 1-2. Identity and Summary

| Field | Value |
|---|---|
| Name | [Project Name] |
| Type | saas |
| Stage | [stage] |
| Live URL | [URL] |
| Owner | Hunter Lazenby |
| Created | YYYY-MM-DD |

**Short Description:** [What problem, who pays, how much.]
**Goal:** [MRR target, user target, or strategic purpose.]
**Status Note:** [Active users, MRR, last deploy, current priority.]

---

## 3-6. Repo, Stack, Run, Deploy

**Repo:** https://github.com/[USERNAME]/[repo] | **Branch:** main | **Private**

| Layer | Technology |
|---|---|
| Frontend | [e.g. React 18 + Vite] |
| Backend | [e.g. Node.js 24 + Express 4] |
| Database | [e.g. Supabase (PostgreSQL + Auth)] |
| Billing | [e.g. Stripe] |
| AI | [e.g. Claude Sonnet via Anthropic API] |
| Hosting | [e.g. Render.com (frontend + backend)] |
| Analytics | [e.g. Plausible + PostHog] |
| Errors | [e.g. Sentry] |

**Local Run:**
1. `cd server && npm install && cd ../client && npm install`
2. Configure `server/.env` (see Environment Variables)
3. `cd server && npm run dev` + `cd client && npm run dev`

**Ports:** Frontend: 5173 | Backend: 3001

**Deploy:** Git push to main → Render auto-deploys. Rollback via Render dashboard.

---

## 7. Environment Variables

| Variable | Purpose | Required | Stored In |
|---|---|---|---|
| ANTHROPIC_API_KEY | Plan generation | Yes | Render env |
| SUPABASE_URL | Database + Auth | Yes | Render env |
| SUPABASE_SERVICE_KEY | Server-side DB access | Yes | Render env |
| STRIPE_SECRET_KEY | Billing | Yes | Render env |
| STRIPE_WEBHOOK_SECRET | Webhook validation | Yes | Render env |
| STRIPE_PRICE_ID_PRO | Pro tier price ID | Yes | Render env |
| AMAZON_AFFILIATE_TAG | Affiliate links | Yes | Render env |
| SENTRY_DSN | Error tracking | Yes | Render env |
| POSTHOG_API_KEY | Product analytics | Yes | Render env |
| RESEND_API_KEY | Transactional email | Yes | Render env |

*No values recorded here — names only.*

---

## 8-9. Services and Costs

| Service | Purpose | Monthly Cost |
|---|---|---|
| Render.com | Frontend + backend hosting | $7 |
| Supabase | DB + Auth | Free |
| Stripe | Billing (0.5% per transaction) | Variable |
| Anthropic API | Plan generation | ~$0.005/plan |
| Namecheap | Domain | $1/mo amortised |
| Sentry | Error tracking | Free |
| PostHog | Product analytics | Free |
| Resend | Transactional email | Free |

**Monthly Fixed:** $[X] | **Monthly Variable:** $[X] | **Total Estimate:** $[X]
**Cost Efficiency:** [healthy/watch/inefficient]

---

## 10. Metrics and KPIs

| KPI | Value | Target | Source |
|---|---|---|---|
| Total registered users | [X] | [X] | Supabase |
| Active Pro subscribers | [X] | [X] | Stripe |
| MRR | $[X] | $[X] | Stripe |
| Plans generated MTD | [X] | [X] | Supabase |
| Free→Pro conversion rate | [X]% | [X]% | PostHog funnel |

**Revenue Model:** [e.g. Freemium — 2 free plans/lifetime, then $6/mo Pro. Amazon affiliate links in plans.]

---

## 11-12. What Works / Known Issues

**Working:**
- [Specific confirmed working feature]

**Known Issues:**

| ID | Description | Severity | Type | Status |
|---|---|---|---|---|
| KI-001 | Free tier enforced client-side via localStorage — bypassable | Critical | Security | Open |

---

## SaaS Extension

**Auth Flow:**  
[How signup, login, session, and OAuth work. Be specific about which Supabase methods are used.]

**Billing Flow:**  
[How a user goes from free → paid. Stripe checkout → webhook → Supabase field update → UI unlock.]

**Pricing Model:**

| Tier | Price | Billing | What's Included |
|---|---|---|---|
| Free | $0 | — | [X] plans lifetime |
| Pro | $[X]/mo | Monthly | Unlimited plans + [features] |

**Free Tier Limits:** [X plans lifetime]
**Free Tier Enforcement:** client-side (localStorage) — ⚠️ SECURITY RISK — server-side enforcement is T-001

**Conversion Funnel:**  
[Where users come from → sign up → hit paywall → upgrade or churn]

**Churn Risks:**
- [e.g. Users don't return after first plan]
- [e.g. No onboarding email sequence]

**Growth Levers:**
- [e.g. SEO pages for woodworking project types]
- [e.g. Shareable plan links]

**Affiliate Setup:**

| Network | Tag | Status | Integration |
|---|---|---|---|
| Amazon Associates | [tag] | Active | Inline in generated plans |
| Home Depot | — | Planned | Not yet set up |

---

## 13-17. Tasks, Decisions, Risks, Handoff

**Task Backlog:**

| ID | Title | Priority | Type | Effort |
|---|---|---|---|---|
| T-001 | Move free-tier enforcement to server-side | Critical | Security | Medium |
| T-002 | Add welcome email via Resend | High | Feature | Small |

**Decision Log:**

| ID | Date | Decision | Rationale |
|---|---|---|---|
| D-001 | YYYY-MM-DD | [Decision] | [Why] |

**Risk Log:**

| ID | Title | Likelihood | Impact | Mitigation | Status |
|---|---|---|---|---|---|
| R-001 | Free tier bypass | High | Medium | Server-side enforcement (T-001) | Open |

**Handoff:**
**Last Session:** YYYY-MM-DD via [tool]
**Done:** [What changed]
**Remains:** [What's next]
**Blocked:** [Nothing / blocker]
**Next should:** [Specific instruction]

---

## 18. Sync Status
**Status:** in_sync | **Last DB Write:** YYYY-MM-DD | **Schema Version:** 1.0
