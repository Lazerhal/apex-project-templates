# Project Overview — Governed System
**APEX ID:** APEX-GOV-YYYYMM-NNN | **Schema Version:** 1.0

---

## ⚠️ GOVERNANCE NOTICE
This is a governed system. All parameter changes to live configurations require
explicit owner approval via Telegram. The AI layer may recommend and test challengers
but may NEVER automatically promote to live without owner confirmation.
Emergency stop command: /stop_all

---

## 1-2. Identity and Summary

| Field | Value |
|---|---|
| Name | [Project Name] |
| Type | governed-system |
| Stage | [stage-0-architecture / stage-1-strategy / stage-2-paper / stage-3-small-live / stage-4-expansion] |
| Owner | Hunter Lazenby |
| Created | YYYY-MM-DD |

**Short Description:** [What the system does, what it controls, what the governance model is.]
**Goal:** [What success looks like at full operation.]
**Status Note:** [Current stage, what is being tested, last meaningful event.]

---

## 3-6. Repo, Stack, Run, Deploy

**Repo:** https://github.com/[USERNAME]/[repo] | **Branch:** main | **Private**

| Layer | Technology |
|---|---|
| Orchestration | [e.g. Python + Docker Compose] |
| AI Analysis | [e.g. Claude Sonnet (high-reasoning) + Gemini Flash (cheap repetitive)] |
| Exchange | [e.g. Coinbase Advanced API + WebSocket] |
| Data | [e.g. PostgreSQL + SQLite for fast local cache] |
| Control | [e.g. Telegram Bot API] |
| Reference Framework | [e.g. TradingAgents (TauricResearch) — multi-agent analysis layer] |

**Local Run:**
1. Configure `.env` (see Environment Variables — SANDBOX credentials only for local testing)
2. `docker compose up` — spins up all services in paper/sandbox mode
3. Telegram bot is the primary control interface

**NEVER run with live exchange credentials on a non-production machine.**

**Deploy:** Docker Compose on build node. No cloud hosting for live execution.

---

## 7. Environment Variables

| Variable | Purpose | Required | Env | Sensitive |
|---|---|---|---|---|
| COINBASE_API_KEY | Exchange access | Yes | sandbox + live (SEPARATE FILES) | ⚠️ Critical |
| COINBASE_API_SECRET | Exchange auth | Yes | sandbox + live (SEPARATE FILES) | ⚠️ Critical |
| ANTHROPIC_API_KEY | High-reasoning analysis | Yes | All | Yes |
| GOOGLE_API_KEY | Cheap repetitive analysis | Yes | All | Yes |
| TELEGRAM_BOT_TOKEN | Control interface | Yes | All | Yes |
| TELEGRAM_OWNER_CHAT_ID | Auth filter | Yes | All | Yes |
| DB_URL | PostgreSQL connection | Yes | All | Yes |

**CRITICAL:** Sandbox and live credentials are stored in SEPARATE .env files.
`.env.sandbox` — safe for testing. `.env.live` — restricted, never on non-production machine.

*No values recorded here — names only.*

---

## 8-9. Services and Costs

| Service | Purpose | Monthly Cost |
|---|---|---|
| [Exchange] | Trading execution | Variable (fees) |
| Anthropic API | High-reasoning analysis | Variable |
| Google AI | Cheap repetitive calls | Variable |
| Build node | Local execution | $0 (already owned) |

**Monthly Total Estimate:** $[X] | **Cost Efficiency:** [healthy/watch/inefficient]

---

## 10. Metrics and KPIs (Stage-Dependent)

| KPI | Value | Target | Source |
|---|---|---|---|
| Current stage | [stage] | stage-4-expansion | Internal |
| Paper trade win rate | [X]% | >55% | Internal logs |
| Paper trade Sharpe | [X] | >1.0 | Internal logs |
| Champion vs challenger delta | [X]% | >0 | Internal comparison |

**Revenue Model:** [e.g. Trading profit — target X% monthly on deployed capital]

---

## 11-12. What Works / Known Issues

**Working:**
- [Specific confirmed working component]

**Known Issues:**

| ID | Description | Severity | Type | Status |
|---|---|---|---|---|
| KI-001 | [Issue] | [severity] | [type] | open |

---

## Governed System Extension

**Current Stage:** [stage-0-architecture]

**Stage Gate Status:**

| Stage | Status | Completion Date |
|---|---|---|
| Stage 0 — Architecture | [complete/in-progress/not-started] | YYYY-MM-DD |
| Stage 1 — Strategy Formalization | not-started | — |
| Stage 2 — Paper Trading | not-started | — |
| Stage 3 — Small Live | not-started | — |
| Stage 4 — Expansion | not-started | — |

**Governance Policy:**
[The rules. What the AI can do autonomously vs what requires approval. Be explicit.]
- AI MAY: generate recommendations, launch paper challengers, log decisions
- AI MAY NOT: change live parameters, promote challengers to live, increase position sizes
- REQUIRES TELEGRAM APPROVAL: any live execution change, any real-money parameter change

**Risk Limits:**

| Limit | Value | Enforced in Code |
|---|---|---|
| Max position size | $[X] | No (Stage 0) |
| Max daily loss | $[X] | No (Stage 0) |
| Max open positions | [X] | No (Stage 0) |
| Stop trading threshold | -[X]% drawdown | No (Stage 0) |

**Champion Strategy:**  
[Not yet defined — Stage 0]

**Challenger Strategies:**  
[None — Stage 0]

**Promotion Criteria:**  
A challenger must outperform the champion on: fee-adjusted return, max drawdown, Sharpe-equivalent stability over 30+ paper trading days. Owner Telegram approval always required for live promotion.

**Emergency Controls:**
- `/stop_all` — halt all open positions and disable live trading
- `/pause [strategy]` — pause specific strategy without closing positions
- Kill switch: set `KILL_SWITCH=true` in `.env.live` — system refuses all new orders

**Safety Constraints (hard-coded, never overridden by AI):**
- Maximum position size cap regardless of signal confidence
- Mandatory session window (no trading outside defined hours)
- No leverage without explicit Stage 4 governance approval
- All live trades logged with full AI reasoning chain preserved

**Reference Frameworks:**
- TradingAgents (TauricResearch) — multi-agent LLM trading framework, basis for analyst agent architecture
- Craig Percoco intraday methodology — market structure, fair value gaps, NY session timing

---

## 13-17. Tasks, Decisions, Risks, Handoff

**Task Backlog:**

| ID | Title | Priority | Type | Effort |
|---|---|---|---|---|
| T-001 | Complete Stage 0 architecture document | Critical | Infrastructure | Large |
| T-002 | Study and document TradingAgents framework applicability | High | Research | Medium |

**Decision Log:**

| ID | Date | Decision | Rationale |
|---|---|---|---|
| D-001 | YYYY-MM-DD | Use TradingAgents as analysis layer foundation | Avoids rebuilding multi-agent framework from scratch; 87k+ stars, active maintenance, Claude support |

**Risk Log:**

| ID | Title | Likelihood | Impact | Mitigation | Status |
|---|---|---|---|---|---|
| R-001 | Live trading with unvalidated strategy | High | Critical | Stage gates enforce paper-first progression | Open |
| R-002 | AI hallucinating trading signals | Medium | High | Human approval required for all live changes | Open |

**Handoff:**
**Last Session:** YYYY-MM-DD via [tool]
**Done:** [What changed]
**Remains:** [What's next]
**Blocked:** [Nothing / blocker]
**Next should:** [Specific instruction]

---

## 18. Sync Status
**Status:** in_sync | **Last DB Write:** YYYY-MM-DD | **Schema Version:** 1.0
