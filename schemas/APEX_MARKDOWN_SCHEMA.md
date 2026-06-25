# APEX Markdown Schema
**Version:** 1.0  
**Status:** Canonical  
**Purpose:** Defines the required structure, section order, field types, and validation rules for all APEX project documentation. This file is the authoritative spec that the APEX doc engine validates against.

---

## How This Schema Works

Every APEX-managed project must have documentation conforming to this schema. The doc engine reads this file to know what sections are required, what data types are expected, and what rules apply per project type and stage.

**Sync model:** The database is the live working layer. Markdown is generated from the database and serves as the handoff and portability layer. Markdown files are never the primary write target during normal dashboard operation — they are regenerated on save. Direct edits to markdown (e.g. from Claude Code or github.dev) are flagged as external edits and imported on next sync.

**File:** Every project repo contains `APEX.md` at root (system entry point) plus a `docs/` directory with individual section files.

---

## Section 1 — Project Identity (REQUIRED, ALL TYPES, ALL STAGES)

```yaml
section: project-identity
required: true
applies_to: all
fields:
  apex_id:
    type: string
    format: "APEX-{TYPE_CODE}-{YYYYMM}-{SEQ}"
    example: "APEX-SAAS-202606-001"
    description: Unique system identifier assigned at onboarding. Never changes.

  name:
    type: string
    max_length: 60
    description: Human-readable project name. Used in dashboard and Telegram.

  type:
    type: enum
    values: [product-tool, content-engine, saas, governed-system, infrastructure, idea]
    description: Project type. Determines which additional template sections are required.

  stage:
    type: enum
    values: [idea, planning, active-development, live, paused, retired]
    description: Current lifecycle stage.

  owner:
    type: string
    default: "Hunter Lazenby"

  created_date:
    type: date
    format: "YYYY-MM-DD"

  last_updated:
    type: date
    format: "YYYY-MM-DD"
    description: Last time any section of this doc was meaningfully updated.

  apex_version:
    type: string
    description: Version of the APEX schema this doc conforms to.
    default: "1.0"
```

---

## Section 2 — Project Summary (REQUIRED, ALL TYPES, ALL STAGES)

```yaml
section: project-summary
required: true
applies_to: all
fields:
  short_description:
    type: string
    max_length: 280
    description: One or two sentence description. What it is and who it's for.

  goal:
    type: string
    description: What success looks like. Specific and measurable where possible.

  status_note:
    type: string
    description: Free-text current status note. Updated frequently.

  live_url:
    type: url
    required_if: stage in [live]
    description: Primary public URL.

  internal_url:
    type: url
    required: false
    description: Internal dashboard or admin URL if applicable.
```

---

## Section 3 — Repository and Access (REQUIRED, ALL TYPES, STAGE >= planning)

```yaml
section: repo-access
required: true
applies_to: all
required_from_stage: planning
fields:
  github_repo:
    type: url
    format: "https://github.com/{owner}/{repo}"
    description: Primary GitHub repository URL.

  visibility:
    type: enum
    values: [public, private]

  primary_branch:
    type: string
    default: "main"

  additional_repos:
    type: list[url]
    required: false
    description: Any additional repos this project uses (e.g. separate frontend/backend).

  local_path:
    type: string
    required: false
    description: Path on build node if cloned locally.
    example: "~/repos/project-planner"

  remote_edit_method:
    type: string
    description: How to edit this repo without the build node.
    example: "github.dev for lightweight edits"
```

---

## Section 4 — Tech Stack (REQUIRED, ALL TYPES, STAGE >= planning)

```yaml
section: tech-stack
required: true
applies_to: all
required_from_stage: planning
fields:
  frontend:
    type: string
    required: false
    description: Frontend framework/library and version.
    example: "React 18 + Vite"

  backend:
    type: string
    required: false
    description: Backend runtime, framework, and version.
    example: "Node.js 24 + Express 4"

  database:
    type: string
    required: false
    description: Database(s) in use.
    example: "Supabase (PostgreSQL)"

  hosting:
    type: list[string]
    description: Where each component is hosted.
    example: ["Frontend: Render.com", "Backend: Render.com"]

  language_versions:
    type: dict
    description: Key language/runtime versions. Important for reproducibility.
    example: {python: "3.11", node: "24"}

  notable_dependencies:
    type: list[string]
    description: Key libraries or services that would be non-obvious to a new reader.
```

---

## Section 5 — Local Run Method (REQUIRED, ALL TYPES, STAGE >= active-development)

```yaml
section: local-run
required: true
applies_to: all
required_from_stage: active-development
fields:
  prerequisites:
    type: list[string]
    description: What must be installed/configured before running locally.

  run_steps:
    type: list[string]
    description: Step-by-step commands to run the project locally.
    example: ["cd server && npm install", "npm run dev"]

  environment_file:
    type: string
    description: Which .env file is needed and where it lives.
    example: "server/.env — see Environment Variables section"

  default_ports:
    type: dict
    required: false
    description: Local ports used by each service.
    example: {frontend: 5173, backend: 3001}

  known_local_quirks:
    type: string
    required: false
    description: Any known issues or non-obvious steps when running locally.
```

---

## Section 6 — Deploy Method (REQUIRED, ALL TYPES, STAGE >= live)

```yaml
section: deploy-method
required: true
applies_to: all
required_from_stage: live
fields:
  deploy_trigger:
    type: enum
    values: [git-push-auto, manual-command, github-actions, docker-compose, coolify-auto, other]

  deploy_steps:
    type: list[string]
    description: Step-by-step deploy process.

  rollback_method:
    type: string
    description: How to roll back a bad deploy.

  environment_promotion:
    type: string
    required: false
    description: If multiple environments exist, how code moves between them.
```

---

## Section 7 — Environment Variables (REQUIRED, ALL TYPES, STAGE >= active-development)

```yaml
section: environment-variables
required: true
applies_to: all
required_from_stage: active-development
rules:
  - "NEVER record secret values in this section — names only"
  - "Record where secrets are stored (e.g. Bitwarden, local .env, Railway env vars)"
  - "Mark each variable as required or optional"
fields:
  variables:
    type: list[env-var]
    schema:
      name:
        type: string
        example: "ANTHROPIC_API_KEY"
      purpose:
        type: string
        example: "Claude API authentication"
      required:
        type: bool
      stored_in:
        type: string
        example: "local .env, Railway environment"
      sensitive:
        type: bool
        default: true
```

---

## Section 8 — Services and Integrations (REQUIRED, ALL TYPES, STAGE >= planning)

```yaml
section: services-integrations
required: true
applies_to: all
required_from_stage: planning
fields:
  services:
    type: list[service]
    schema:
      name:
        type: string
        example: "Supabase"
      purpose:
        type: string
        example: "Authentication + PostgreSQL database"
      tier:
        type: string
        example: "Free"
      monthly_cost_usd:
        type: number
        default: 0
      dashboard_url:
        type: url
        required: false
      criticality:
        type: enum
        values: [critical, important, optional]
      notes:
        type: string
        required: false
```

---

## Section 9 — Costs (REQUIRED, ALL TYPES, STAGE >= live)

```yaml
section: costs
required: true
applies_to: all
required_from_stage: live
fields:
  monthly_fixed_usd:
    type: number
    description: Total predictable monthly cost.

  monthly_variable_usd:
    type: number
    description: Average variable cost (API calls, etc.). Estimate if unknown.

  monthly_total_estimate_usd:
    type: number
    description: fixed + variable estimate.

  annual_total_estimate_usd:
    type: number

  cost_items:
    type: list[cost-item]
    schema:
      name: {type: string}
      type: {type: enum, values: [fixed-monthly, fixed-annual, per-use, free]}
      amount_usd: {type: number}
      billing_date: {type: string, required: false}
      notes: {type: string, required: false}

  cost_efficiency_flag:
    type: enum
    values: [healthy, watch, inefficient]
    description: Set by recommendation engine. Manual override allowed.
```

---

## Section 10 — Metrics and KPIs (REQUIRED, STAGE >= live)

```yaml
section: metrics-kpis
required: true
applies_to: all
required_from_stage: live
fields:
  primary_kpis:
    type: list[kpi]
    schema:
      name: {type: string, example: "Monthly unique visitors"}
      current_value: {type: string, example: "~320/mo"}
      target_value: {type: string, required: false}
      data_source: {type: string, example: "Plausible Analytics"}
      last_updated: {type: date}

  revenue_monthly_usd:
    type: number
    required: false
    default: 0

  revenue_model:
    type: string
    description: How the project makes money (or plans to).

  analytics_links:
    type: list[url]
    required: false
    description: Direct links to dashboards (GA4, Plausible, Stripe, etc.)
```

---

## Section 11 — What Works Now (REQUIRED, STAGE >= active-development)

```yaml
section: what-works
required: true
applies_to: all
required_from_stage: active-development
fields:
  working_features:
    type: list[string]
    description: Bullet list of features/capabilities that are confirmed working.
    rules:
      - "Be specific. 'Auth works' is not useful. 'Supabase email+password auth with session persistence works' is."
```

---

## Section 12 — Known Issues (REQUIRED, STAGE >= active-development)

```yaml
section: known-issues
required: true
applies_to: all
required_from_stage: active-development
fields:
  issues:
    type: list[issue]
    schema:
      description: {type: string}
      severity: {type: enum, values: [critical, high, medium, low]}
      type: {type: enum, values: [bug, security, performance, ux, debt, missing-feature]}
      discovered_date: {type: date, required: false}
      workaround: {type: string, required: false}
      fix_status: {type: enum, values: [open, in-progress, wont-fix, resolved]}
```

---

## Section 13 — Task Backlog (REQUIRED, ALL TYPES, ALL STAGES)

```yaml
section: task-backlog
required: true
applies_to: all
fields:
  tasks:
    type: list[task]
    schema:
      id: {type: string, format: "T-{NNN}"}
      title: {type: string}
      description: {type: string, required: false}
      priority: {type: enum, values: [critical, high, medium, low, backlog]}
      type: {type: enum, values: [bug, feature, improvement, infrastructure, documentation, research]}
      estimated_effort: {type: enum, values: [trivial, small, medium, large, xl], required: false}
      assigned_to: {type: string, required: false, description: "Model or tool that should handle this"}
      blocked_by: {type: list[string], required: false}
      added_date: {type: date}
      completed_date: {type: date, required: false}
```

---

## Section 14 — Decision Log (REQUIRED, ALL TYPES, STAGE >= planning)

```yaml
section: decision-log
required: true
applies_to: all
required_from_stage: planning
fields:
  decisions:
    type: list[decision]
    schema:
      id: {type: string, format: "D-{NNN}"}
      date: {type: date}
      title: {type: string}
      context: {type: string, description: "Why was a decision needed?"}
      options_considered: {type: list[string], required: false}
      decision_made: {type: string}
      rationale: {type: string}
      made_by: {type: string, default: "Hunter Lazenby"}
      revisit_trigger: {type: string, required: false, description: "What would cause this decision to be reconsidered?"}
```

---

## Section 15 — Risk Log (REQUIRED, ALL TYPES, STAGE >= live)

```yaml
section: risk-log
required: true
applies_to: all
required_from_stage: live
fields:
  risks:
    type: list[risk]
    schema:
      id: {type: string, format: "R-{NNN}"}
      title: {type: string}
      description: {type: string}
      likelihood: {type: enum, values: [low, medium, high]}
      impact: {type: enum, values: [low, medium, high, critical]}
      mitigation: {type: string}
      status: {type: enum, values: [open, mitigated, accepted, resolved]}
```

---

## Section 16 — Recommendations (MANAGED BY APEX ENGINE)

```yaml
section: recommendations
required: false
managed_by: apex-recommendation-engine
fields:
  recommendations:
    type: list[recommendation]
    schema:
      id: {type: string, format: "REC-{NNN}"}
      generated_date: {type: date}
      type:
        type: enum
        values: [bug-fix, ux-improvement, performance, cost-reduction, traffic-opportunity,
                 monetisation, reuse-suggestion, doc-fix, experiment, kill-score, security]
      severity: {type: enum, values: [critical, high, medium, low]}
      title: {type: string}
      rationale: {type: string}
      supporting_signals: {type: list[string]}
      estimated_effort: {type: enum, values: [trivial, small, medium, large, xl]}
      expected_benefit: {type: string}
      suggested_action: {type: string}
      status: {type: enum, values: [new, reviewed, accepted, rejected, in-progress, done]}
```

---

## Section 17 — Handoff (MANAGED AFTER EVERY BUILD SESSION)

```yaml
section: handoff
required: true
applies_to: all
required_from_stage: active-development
rules:
  - "Must answer all four handoff questions every time it is updated"
  - "Should be written assuming the next reader has zero context from this session"
  - "Updated at end of every meaningful build session"
fields:
  last_session_date: {type: date}
  last_session_tool: {type: string, example: "Claude Code"}

  what_was_done:
    type: string
    description: Concrete summary of what changed in this session.

  what_remains:
    type: string
    description: What is next and approximately how much work it is.

  what_is_blocked:
    type: string
    description: Anything that cannot proceed without a missing piece.

  next_model_should:
    type: string
    description: Specific instruction for the next AI tool or human picking this up.

  context_pack_id:
    type: string
    required: false
    description: ID of the context pack generated by APEX for this handoff.
```

---

## Section 18 — Doc Sync Status (MANAGED BY APEX ENGINE)

```yaml
section: doc-sync-status
required: true
applies_to: all
managed_by: apex-doc-engine
fields:
  sync_status:
    type: enum
    values: [in_sync, markdown_ahead, db_ahead, conflict]
    description: Current sync state between this markdown file and the APEX database.

  last_db_write: {type: datetime}
  last_markdown_write: {type: datetime}
  last_sync_check: {type: datetime}
  conflict_details: {type: string, required: false}
  schema_version: {type: string, description: "APEX schema version this doc was generated against"}
```

---

## Section 19 — Experiments (OPTIONAL)

```yaml
section: experiments
required: false
fields:
  experiments:
    type: list[experiment]
    schema:
      id: {type: string, format: "EXP-{NNN}"}
      title: {type: string}
      hypothesis: {type: string}
      method: {type: string}
      start_date: {type: date}
      end_date: {type: date, required: false}
      result: {type: string, required: false}
      conclusion: {type: enum, values: [winner, loser, inconclusive, in-progress]}
```

---

## Project-Type Extension Sections

These sections are required IN ADDITION to the universal sections above, based on project type.

---

### Type: product-tool (e.g. VoltForge)

```yaml
section: product-tool-extension
required: true
applies_to: [product-tool]
fields:
  feature_inventory:
    type: list[feature]
    schema:
      name: {type: string}
      description: {type: string}
      status: {type: enum, values: [shipped, in-progress, planned, cut]}
      premium: {type: bool, default: false}

  monetisation_options:
    type: list[string]
    description: All viable monetisation paths identified.

  marketplace_listings:
    type: list[marketplace-listing]
    schema:
      platform: {type: string, example: "Gumroad"}
      url: {type: url, required: false}
      price_usd: {type: number}
      active: {type: bool}

  distribution_channels:
    type: list[string]
    description: Where the product is or will be available.

  premium_upgrade_roadmap:
    type: string
    required: false
    description: What a paid/premium version could include.

  maturity_level:
    type: enum
    values: [prototype, v1-mvp, polished, mature]
```

---

### Type: content-engine (e.g. GearGameDaily)

```yaml
section: content-engine-extension
required: true
applies_to: [content-engine]
fields:
  publishing_cadence:
    type: string
    example: "1 post/day at 13:00 UTC"

  content_pipeline:
    type: string
    description: How content is generated and published end-to-end.

  content_runway:
    type: string
    description: How much content is queued or pre-planned.
    example: "~731 species (~2 years at current cadence)"

  seo_status:
    type: string
    description: Current SEO state — indexed pages, Search Console status, ranking signals.

  analytics_status:
    type: string
    description: What analytics are active and what they show.

  monetisation_sources:
    type: list[monetisation-source]
    schema:
      source: {type: string, example: "Amazon Associates"}
      status: {type: enum, values: [active, pending, planned]}
      monthly_revenue_estimate_usd: {type: number, default: 0}

  promotion_channels:
    type: list[string]
    description: Active and planned promotion channels.

  image_sources:
    type: list[string]
    description: Where images come from and any rate limit/quality concerns.

  scheduler_health:
    type: enum
    values: [healthy, degraded, down, unknown]
```

---

### Type: saas (e.g. DIY Cut List)

```yaml
section: saas-extension
required: true
applies_to: [saas]
fields:
  auth_flow:
    type: string
    description: How authentication works end-to-end.

  billing_flow:
    type: string
    description: How billing and subscriptions work.

  pricing_model:
    type: list[pricing-tier]
    schema:
      name: {type: string, example: "Pro"}
      price_usd: {type: number}
      billing_period: {type: enum, values: [monthly, annual, one-time, free]}
      features: {type: list[string]}

  free_tier_limits:
    type: string
    description: What free users can do and how limits are enforced.

  free_tier_enforcement:
    type: enum
    values: [server-side, client-side, mixed]
    description: >
      CRITICAL FIELD. Client-side enforcement is a security risk.
      DIY Cut List currently uses client-side (localStorage) — flagged as HIGH PRIORITY fix.

  conversion_funnel:
    type: string
    description: How users move from signup to paid.

  churn_risks:
    type: list[string]

  growth_levers:
    type: list[string]

  affiliate_setup:
    type: list[affiliate]
    schema:
      network: {type: string}
      tag: {type: string}
      status: {type: enum, values: [active, pending, planned]}
      integration_method: {type: string}
```

---

### Type: governed-system (e.g. BATCH)

```yaml
section: governed-system-extension
required: true
applies_to: [governed-system]
fields:
  current_stage:
    type: enum
    values: [stage-0-architecture, stage-1-strategy, stage-2-paper, stage-3-small-live, stage-4-expansion]

  stage_gate_status:
    type: dict
    description: Completion status of each stage gate exit criterion.

  governance_policy:
    type: string
    description: The governing rules for this system — what requires approval, what is automatic.

  approval_tree:
    type: string
    description: Who approves what. For APEX single-owner systems, this is always Hunter Lazenby via Telegram.

  risk_limits:
    type: list[risk-limit]
    schema:
      name: {type: string, example: "Max position size"}
      value: {type: string, example: "$50 per trade"}
      enforced_in_code: {type: bool}

  champion_strategy:
    type: string
    required: false
    description: Current live/paper champion strategy description.

  challenger_strategies:
    type: list[string]
    required: false
    description: Active challenger strategies being tested.

  promotion_criteria:
    type: string
    description: What a challenger must demonstrate to replace the champion.

  emergency_controls:
    type: list[string]
    description: Emergency stop mechanisms and how to trigger them.

  safety_constraints:
    type: list[string]
    description: Hard-coded safety rules that can never be overridden by the AI layer.

  reference_frameworks:
    type: list[string]
    description: External frameworks or research this system is built on or adapted from.
    example: ["TradingAgents (TauricResearch) — multi-agent analysis layer"]
```

---

## Validation Rules

```yaml
validation:
  doc_engine_enforcement:
    - rule: "All required sections must be present for the project's current stage"
      action: reject_handoff_generation
      severity: error

    - rule: "No secret values may appear in any section"
      action: reject_sync_to_repo
      severity: critical
      pattern: >
        Regex scan for: API keys, tokens, passwords, connection strings,
        private keys. Matches on patterns like sk-..., Bearer ..., postgres://user:pass@

    - rule: "Handoff section must be updated if last_session_date is more than 7 days old and stage is active-development or live"
      action: create_recommendation
      severity: warning

    - rule: "free_tier_enforcement must not be 'client-side' for live SaaS projects without a critical security issue logged"
      action: create_recommendation
      severity: critical

    - rule: "governed-system projects must have all risk_limits marked enforced_in_code=true before stage >= stage-3-small-live"
      action: block_stage_progression
      severity: critical

  completeness_scoring:
    method: >
      Score = (present_required_fields / total_required_fields_for_stage) * 100
      Weighted: handoff=2x, known-issues=1.5x, task-backlog=1.5x, environment-variables=2x
    thresholds:
      build_ready: 85
      handoff_ready: 90
      live_ready: 95
```

---

## File Structure Per Project Repo

```text
project-root/
├── APEX.md                        ← Entry point. Contains: identity, summary, sync status, handoff.
│                                    Doc engine reads this first. Keep it concise — links to docs/.
├── .claude/
│   └── rules/
│       ├── coding.md              ← Coding standards for this project
│       ├── deployment.md          ← Deploy rules and cautions
│       ├── testing.md             ← Test requirements
│       ├── product.md             ← Product decisions and constraints
│       └── integrations.md        ← Integration-specific rules (API quirks, rate limits, etc.)
└── docs/
    ├── project-overview.md        ← Sections 1-2: identity + summary
    ├── repo-access.md             ← Section 3
    ├── tech-stack.md              ← Sections 4-6: stack + run + deploy
    ├── environment-variables.md   ← Section 7 (names only, never values)
    ├── integrations.md            ← Section 8: services and integrations
    ├── costs.md                   ← Section 9
    ← Section 10 (managed by APEX engine)
    ├── what-works.md              ← Section 11
    ├── known-issues.md            ← Section 12
    ├── tasks.md                   ← Section 13: task backlog
    ├── decisions.md               ← Section 14: decision log
    ├── risks.md                   ← Section 15
    ├── recommendations.md         ← Section 16 (managed by APEX engine)
    ├── handoff.md                 ← Section 17: updated every session
    ├── build-history.md           ← Running log of significant build events
    ├── experiments.md             ← Section 19 (optional)
    └── [type-extension].md        ← Type-specific extension section
```

---

## APEX.md Root File Format

The root `APEX.md` file is the first file any AI tool or human reads. It must be concise and always current. The doc engine regenerates it on every sync.

```markdown
# [Project Name]
**APEX ID:** [APEX-TYPE-YYYYMM-NNN]  
**Type:** [type]  **Stage:** [stage]  **Sync:** [sync_status]  
**Last Updated:** [date]  **Last Session:** [date] via [tool]

## What This Is
[short_description — one paragraph max]

## Current Status
[status_note — what is happening right now]

## Immediate Next Steps
[next_model_should from handoff section — copied here for fast access]

## What's Blocked
[what_is_blocked from handoff section]

## Key Links
- Repo: [github_repo]
- Live URL: [live_url if applicable]
- Docs: ./docs/
- Master Plan: [link to APEX master plan]

## Read Next
For full context load: docs/handoff.md → docs/tasks.md → docs/known-issues.md
For architecture: docs/tech-stack.md → docs/integrations.md
For costs: docs/costs.md
```
