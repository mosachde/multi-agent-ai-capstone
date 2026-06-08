# Product Requirements Document (PRD)

## Document Info

| Field | Value |
|-------|-------|
| Product | Sales Enablement & Meeting Automation Crew |
| Use Case | Use Case 3 — Maven Agentic AI Architect Capstone (Lesson 4) |
| Version | 1.1 |
| Selected Runtime | `crewai` (`AAMAD_TARGET_RUNTIME=crewai`) |
| Input Artifacts | `mrd.md`, `MRD-v2.md` |
| Output Path | `project-context/1.define/prd-v1.md` |
| Primary Persona | Mid-market B2B SaaS Account Executive |

---

## 1. Executive Summary

### Problem Statement (Research-backed)

B2B account executives spend **60–70% of their time on non-selling tasks**—research, CRM logging, meeting prep, and follow-up—leaving only **30–35% for active selling** (Salesforce State of Sales 2025/2026; Everstage). Reps lose an estimated **~2.5 hrs/week on prospect research** and **~6–8 hrs/week on CRM admin** alone (SyncGTM 2026).

Existing tools solve fragments of this problem (Gong for conversation intelligence, Outreach for engagement, Clari for forecasting) but remain **disconnected**. Organizations use **3+ enablement tools** on average (SiftHub), and **90% are adopting or planning GTM AI** (Highspot 2025)—yet buyers still lack a unified **prep → meeting → follow-up orchestration layer**.

**Target market:** Sales enablement platform market **USD 3.25–5.23B (2024)**, growing **~14–17% CAGR** to **USD 9–13B by 2030** (Grand View Research, Next MSC). Initial beachhead: **mid-market B2B SaaS teams (5–50 reps)**, North America first.

### Solution Overview (Evidence-based)

The **Sales Enablement & Meeting Automation Crew** is a CrewAI multi-agent system that coordinates specialized agents—Prospect Research, Competitive Intelligence, Meeting Prep, Meeting Insights, and Follow-Up—under a **Sales Manager coordinator agent**.

**Unique value proposition:** Orchestrates the full meeting lifecycle in one crew with shared context, rather than competing as another point solution.

**Key differentiators vs. incumbents:**

| Incumbent | Strength | Gap this product fills |
|-----------|----------|------------------------|
| Gong | Conversation analytics | Weak cross-workflow prep orchestration |
| Outreach / Salesloft | Engagement sequences | Platform lock-in; less customizable agent roles |
| Salesforce Agentforce | CRM-native agents | Salesforce-ecosystem bound |
| Custom ChatGPT | Ad hoc assistance | No persistent deal context or structured handoffs |

**Expected outcomes — business KPIs per AE (post-pilot, research-backed targets):**

| KPI | Research benchmark | Product target (per AE, 12 mo) |
|-----|-------------------|-------------------------------|
| Revenue per AE | +41% ($1.75M vs $1.24M) — Optifai, N=938 | **+15–25%** (conservative vs. benchmark) |
| Deals closed per AE / quarter | +53% productivity → ~15→23 deals/mo — Optifai | **+10–20%** closed-won deals |
| Win rate | +25% (course); 42% more likely — Highspot | **+10–15%** absolute or relative improvement |
| Sales cycle / time to close | ↓28% cycle — Optifai; ↓32% — Salesforce | **↓15–25%** days to close |
| Quota attainment | Top performers 3.5× more proactive — Careertrainer | **+10%** reps at ≥100% quota |

**Expected outcomes — MVP leading indicators (capstone / 90-day pilot):**

- Prep time per meeting ↓ **80%+**
- Time-to-follow-up ↓ from **24–48 hrs → <1 hr**
- Meeting-to-next-step conversion ↑ **≥15%** (deals advancing within 7 days of meeting)
- CRM draft completeness ↑ **≥90%** required fields populated
- Rep-reported confidence before meetings ↑ (qualitative)

Leading indicators above are **proxies** for revenue and deal closure KPIs; pilot must measure both layers (see Section 7).

### Strategic Rationale

**Why multi-agent:** Sales workflows naturally decompose into specialist roles with handoffs (research → brief → summary → follow-up). A single monolithic agent cannot enforce least-privilege tools, parallel research, or human approval gates per step. CrewAI **hierarchical process**, **Flows**, and **shared memory** map directly to this domain (MRD-v2, `mrd.md` Dimension 2).

**Business case:** AI-augmented reps achieve **+41% revenue/rep** with **18% fewer activities** (Optifai, N=938); organizations using GTM AI report **+29% revenue growth** (Gong 2025). The product must ultimately move **revenue, deals closed, and win rate per AE**—not only efficiency metrics. MVP proves the workflow; pilot measures the revenue KPI cascade in Section 7.

**Market timing:** Buyer-side AI adoption (**94%** of B2B buyers use AI in buying — Forrester 2025) increases urgency for seller-side AI orchestration.

---

## 2. Market Context & User Analysis

### Target Market

| Segment | Characteristics | Size / Growth |
|---------|-----------------|---------------|
| Primary | Mid-market B2B SaaS AEs, 5–50 rep teams | Subset of USD 3.25–5.23B enablement TAM |
| Secondary | Sales managers, RevOps admins | Same org; buyer vs. user split |
| Geographic (MVP) | North America, English-only | Expand EU/APAC post-MVP (GDPR) |

### User Personas

**Persona 1: Alex — Account Executive (Primary)**
- Quota-carrying AE, 15–30 active opportunities
- Pain: prep inconsistency, delayed follow-ups, CRM backlog
- Goal: walk into meetings prepared; follow up same day
- Success: saves 5+ hrs/week on admin/research

**Persona 2: Jordan — Sales Manager**
- Manages 6–10 AEs
- Pain: poor CRM hygiene, late discovery of deal risks
- Goal: team consistency and forecast confidence
- Success: standardized briefs; visible next steps post-meeting

**Persona 3: Riley — RevOps Admin**
- Owns CRM config and tool stack
- Pain: tool sprawl, integration maintenance
- Goal: reliable data in CRM without rep burden
- Success: draft-then-approve CRM workflow; audit logs

### User Needs Analysis

**Critical pain points (MRD-v2 P0 jobs):**

| P0 # | Pain | Unmet need |
|------|------|------------|
| 1 | Manual meeting lookup | Auto-detect upcoming meetings |
| 2 | Scattered research | Single account/contact dossier |
| 3 | Stale battlecards | Fresh competitive talk tracks (verified) |
| 4 | Inconsistent prep | Structured pre-meeting brief |
| 5 | Delayed note-taking | Immediate post-meeting synthesis |
| 6 | Slow follow-up | Draft email within minutes |
| 7 | CRM neglect | Draft CRM update same session |

**User journey (MVP):**

```
Trigger (manual or calendar)
  → Crew kickoff (Sales Manager delegates)
  → Research + Competitive Intel (parallel)
  → Meeting Prep brief
  → [Human: meeting occurs]
  → Transcript/notes input
  → Meeting Insights summary
  → Follow-Up: email draft + CRM draft
  → Human approval gates
  → (Optional) send/save
```

**Adoption barriers:** Trust in AI outputs, CRM write anxiety, competitive claim accuracy.  
**Success factors:** Human-in-the-loop, citations on research, draft-only CRM in MVP, visible time saved.

### Competitive Landscape

| Competitor | Pricing benchmark | MVP positioning |
|------------|-------------------|-----------------|
| Gong | ~$1,300–3,000/user/yr | Complement, not replace |
| Outreach | Enterprise engagement | Avoid head-on; focus prep/follow-up loop |
| HubSpot/SF native AI | Bundled in CRM | Orchestration layer across tools |

**Pricing hypothesis (post-capstone):** USD 49–149/user/month orchestration tier vs. USD 250+/user/month full CI platforms.

---

## 3. Technical Requirements & Architecture

### CrewAI Framework Specifications

**Process model:** `Process.hierarchical` with `manager_llm` (Sales Manager agent)

**Flow wrapper:** CrewAI Flows for deal-stage / meeting-type conditional routing (P1 enhancement)

**Memory:** Crew-level memory enabled for P1 #2; MVP may use in-session context only

**Orchestration pattern:**

```
Sales Manager (coordinator)
├── Prospect Research Agent    [P0 #2]
├── Competitive Intel Agent    [P0 #3]
├── Meeting Prep Agent         [P0 #4]
├── Meeting Insights Agent     [P0 #5]
└── Follow-Up Agent            [P0 #6, #7]
```

### Core Agent Definitions

#### sales_manager

- **role:** "Sales Operations Coordinator"
- **goal:** "Orchestrate specialist agents to deliver end-to-end meeting prep and follow-up workflows with accurate handoffs and human approval gates"
- **backstory:** "Senior sales ops leader who ensures every agent output is coherent, traceable, and ready for rep review before customer-facing use"
- **tools:** crew delegation only (no direct external writes)
- **memory:** true (P1)
- **delegation:** true — assigns tasks to specialist agents

#### prospect_research

- **role:** "Prospect & Account Research Analyst"
- **goal:** "Produce a consolidated account and contact dossier from CRM, web, and enrichment sources"
- **backstory:** "Former B2B research analyst skilled at synthesizing company news, org structure, and buyer context"
- **tools:** CRM read, web search, enrichment API (stub OK for MVP)
- **memory:** false (receives context from manager)
- **delegation:** false

#### competitive_intel

- **role:** "Competitive Intelligence Specialist"
- **goal:** "Identify likely competitors and produce positioning summaries with cited sources for rep approval"
- **backstory:** "Product marketing analyst focused on battlecards and win/loss patterns"
- **tools:** knowledge base RAG (battlecards), web search (limited)
- **memory:** false
- **delegation:** false

#### meeting_prep

- **role:** "Meeting Preparation Strategist"
- **goal:** "Generate a concise pre-meeting brief combining research, competitive context, CRM history, and recommended objectives"
- **backstory:** "Elite sales coach who prepares reps for high-stakes calls"
- **tools:** none (synthesizes upstream agent outputs)
- **memory:** false
- **delegation:** false

#### meeting_insights

- **role:** "Meeting Analysis Specialist"
- **goal:** "Extract decisions, action items, objections, and stakeholder mentions from meeting transcript or notes"
- **backstory:** "Conversation intelligence analyst trained on B2B sales calls"
- **tools:** transcript parser (text input MVP)
- **memory:** false
- **delegation:** false

#### follow_up

- **role:** "Follow-Up & CRM Documentation Agent"
- **goal:** "Draft follow-up email and CRM activity update for rep approval"
- **backstory:** "Sales assistant who ensures nothing falls through the cracks after meetings"
- **tools:** CRM read (draft write stub), email template generator
- **memory:** false
- **delegation:** false

### Integration Requirements

| System | MVP Scope | Priority |
|--------|-----------|----------|
| HubSpot CRM | Read contacts, deals, activities; draft note output | P0 (default; SF alternate) |
| Google Calendar / MS Graph | Read upcoming meetings, attendees | P0 #1 (Sprint 3) |
| Web search / enrichment | Account research | P0 #2 (stub acceptable Sprint 1) |
| Battlecard KB | Competitive intel RAG | P0 #3 |
| Gong / Chorus | Live CI | P2 — deferred |
| Email (SMTP/API) | Send after approval | Post-MVP |

**Authentication:** OAuth 2.0 for CRM/calendar; API keys via `.env` (see `.env.example`)

**Performance targets (MVP):**

- Full prep workflow (P0 #2–4): **< 3 minutes** end-to-end
- Post-meeting package (P0 #5–7): **< 2 minutes** after transcript input
- Concurrent users (capstone demo): **1–5**

### Infrastructure Specifications (Capstone MVP)

| Component | Specification |
|-----------|---------------|
| Cloud | Local dev + optional Docker; AWS/GCP defer to Phase 2 deliver |
| Compute | 1 backend instance; LLM via API |
| Storage | File-based session logs; SQLite or JSON for demo state |
| Logging | `project-context/2.build/logs` — redact secrets |
| Monitoring | Structured agent trace logs; token/cost counters |

---

## 4. Functional Requirements

### P0 — Core Features (Capstone MVP)

Traceability: MRD-v2 P0 #1–7

---

**FR-P0-01: Manual Meeting Workflow Trigger**

- **User story:** As an AE, I want to start a prep workflow by providing meeting context (account, attendees, datetime) so that I can get a brief without calendar integration on day one.
- **Acceptance criteria:**
  - Rep can submit meeting context via chat UI or API
  - System returns workflow run ID and status
  - Works without calendar connected (Sprint 1 default)
- **Dependencies:** Sales Manager agent, crew kickoff
- **MRD trace:** P0 #4 (Sprint 1); unblocks before P0 #1

---

**FR-P0-02: Prospect & Account Research**

- **User story:** As an AE, I want a consolidated account dossier so that I can understand the company and attendees before my meeting.
- **Acceptance criteria:**
  - Output includes: company summary, recent news, attendee roles, prior CRM interactions
  - Sources cited for external facts
  - Completes in < 90 seconds (mock enrichment OK)
- **Agent:** prospect_research
- **MRD trace:** P0 #2

---

**FR-P0-03: Competitive Intelligence Brief**

- **User story:** As an AE, I want competitive positioning and talk tracks so that I can differentiate confidently in the meeting.
- **Acceptance criteria:**
  - Identifies 1–3 likely competitors
  - Provides strengths/weaknesses and suggested responses
  - Flags unverified claims for rep review
  - Requires explicit rep approval before inclusion in customer-facing follow-up
- **Agent:** competitive_intel
- **MRD trace:** P0 #3

---

**FR-P0-04: Pre-Meeting Brief Generation**

- **User story:** As an AE, I want a single pre-meeting brief so that I know who I'm meeting, what's at stake, and what to accomplish.
- **Acceptance criteria:**
  - Brief answers: who, company context, deal stage, stakeholders, competitors, meeting objectives, suggested questions
  - Combines FR-P0-02 and FR-P0-03 outputs
  - Formatted markdown or structured JSON for UI rendering
- **Agent:** meeting_prep
- **MRD trace:** P0 #4

---

**FR-P0-05: Post-Meeting Summary**

- **User story:** As an AE, I want meeting notes synthesized from a transcript or my notes so that I can act immediately after the call.
- **Acceptance criteria:**
  - Accepts pasted transcript or bullet notes (no Gong required)
  - Extracts: summary, decisions, action items, objections, stakeholders mentioned
  - Output structured for downstream follow-up agent
- **Agent:** meeting_insights
- **MRD trace:** P0 #5

---

**FR-P0-06: Follow-Up Email Draft**

- **User story:** As an AE, I want a draft follow-up email so that I can send it quickly while the conversation is fresh.
- **Acceptance criteria:**
  - Email reflects meeting summary content accurately
  - Rep must approve before send (send deferred in MVP; copy/export OK)
  - Includes clear next steps and proposed follow-up date
- **Agent:** follow_up
- **MRD trace:** P0 #6

---

**FR-P0-07: CRM Update Draft**

- **User story:** As an AE, I want a draft CRM activity log and field updates so that I can keep pipeline data accurate without manual entry.
- **Acceptance criteria:**
  - Draft includes: activity note, suggested next step, suggested close date/stage (if applicable)
  - **No auto-write to CRM in MVP** — display draft for copy or future approve-and-commit
  - Maps to HubSpot contact/deal IDs when CRM connected
- **Agent:** follow_up
- **MRD trace:** P0 #7

---

**FR-P0-08: Calendar-Triggered Prep (Sprint 3)**

- **User story:** As an AE, I want prep to start automatically when a meeting is on my calendar so that I don't have to manually trigger workflows.
- **Acceptance criteria:**
  - Reads calendar events within configurable window (default T-60 min)
  - Auto-starts FR-P0-02 through FR-P0-04 pipeline
  - Rep notified when brief is ready
- **MRD trace:** P0 #1

---

### P1 — Enhanced Features

| ID | Feature | User story summary | MRD trace |
|----|---------|-------------------|-----------|
| FR-P1-01 | Personalized follow-up | Tailor email tone/content to account persona and prior interactions | P1 #1 |
| FR-P1-02 | Shared deal memory | Persist account context across meetings on same deal | P1 #2 |
| FR-P1-03 | Deal risk alerts | Flag missing stakeholders, stale deals, objection patterns | P1 #3 |
| FR-P1-04 | Lead/deal scoring | Rank opportunities for daily focus | P1 #4 |
| FR-P1-05 | Deal-stage Flow routing | Route agent tasks by discovery/demo/negotiation stage | Phase C |

### P2 — Future Features (Out of Scope — Capstone)

| ID | Feature | Reason deferred | MRD trace |
|----|---------|-----------------|-----------|
| FR-P2-01 | Live in-meeting coaching | Requires streaming CI integration | P2 #1 |
| FR-P2-02 | Autonomous outbound SDR | Different product surface | P2 #2 |

---

## 5. Non-Functional Requirements

### Performance

| Requirement | Target |
|-------------|--------|
| Prep workflow latency | < 3 min P95 |
| Post-meeting package latency | < 2 min P95 |
| API availability (demo) | 99% during demo windows |

### Security & Compliance

| Requirement | MVP implementation |
|-------------|-------------------|
| Secrets management | Environment variables only; `.env.example` documented |
| PII in logs | Redact emails, phone numbers in trace logs |
| CRM access | Read + draft; no silent writes |
| Competitive claims | Human approval gate |
| GDPR / SOC 2 | Document deferrals; synthetic demo data for capstone |

### Scalability & Reliability

| Requirement | MVP | Production target |
|-------------|-----|-------------------|
| Concurrent crews | 1–5 | 50+ (Phase 3) |
| Retry on LLM failure | 2 retries with backoff | Required |
| Idempotency | Same meeting ID → same workflow key | Required for calendar trigger |

---

## 6. User Experience Design

### Interface Requirements (MVP)

- **Chat-first UI** (AAMAD standard): single interface to trigger workflows and view agent outputs
- **Structured output cards:** Pre-meeting brief | Post-meeting summary | Email draft | CRM draft
- **Approval buttons:** Approve / Edit / Reject for competitive claims, email, CRM draft
- **Status indicator:** Workflow progress (researching → briefing → ready)
- **Mobile:** Not required for capstone MVP

### Agent Interaction Design

| Pattern | Implementation |
|---------|----------------|
| Transparency | Show which agent produced each section |
| Explainability | Citations on research and competitive claims |
| Error handling | Graceful fallback if enrichment API unavailable |
| Human-in-the-loop | Mandatory approval before customer-facing or CRM commit actions |

---

## 7. Success Metrics & KPIs

### Revenue & Deal Outcomes per AE (Primary Business KPIs)

These are the **north-star business metrics** the product must influence. Targets are per quota-carrying AE unless noted as team-level.

| KPI | Definition | Research benchmark | 90-day pilot target | 12-month target (per AE) |
|-----|------------|-------------------|---------------------|--------------------------|
| **Revenue per AE** | Closed-won revenue attributed to AE | +41% ($1.75M vs $1.24M) — Optifai, N=938 | **+5–10%** vs. pre-pilot baseline | **+15–25%** |
| **Deals closed per AE** | Count of closed-won opportunities per quarter | ~15→23 deals/mo with AI (+53% productivity) — Optifai | **+5–10%** deals closed | **+10–20%** deals closed |
| **Win rate** | Closed-won ÷ (closed-won + closed-lost) | +25% improvement (course); 42% more likely — Highspot | **+5%** absolute improvement | **+10–15%** relative improvement |
| **Sales cycle length** | Days from opportunity create → closed-won | ↓28% — Optifai; ↓32% — Salesforce; ↓45 days enterprise (course, segment-dependent) | **↓10%** median days to close | **↓15–25%** median days to close |
| **Deal velocity** | (Opportunities × avg deal size × win rate) ÷ cycle days | B2B SaaS median ~$8,200/day pipeline velocity — Optifai | **+10%** velocity | **+20–28%** velocity |
| **Average deal size** | Mean closed-won ACV | +50% avg deal ($72K vs $48K) — Optifai AI-augmented | Hold or **+5%** | **+10–15%** |
| **Quota attainment** | % of AE at ≥100% quota | Mature enablement 4:1 ROI — SiftHub | **+10%** of team at quota | **+15–20%** of team at quota |
| **Team revenue growth** | Org-level GTM revenue YoY | +29% vs peers — Gong 2025 | Directionally positive | **+10–20%** (team, with attribution controls) |

**Measurement requirements:**

- Establish **90-day pre-pilot baseline** per AE: revenue, deals closed, win rate, avg cycle days, pipeline velocity
- Use **control group or staggered rollout** where possible (5–10 AEs treatment vs. matched cohort)
- Attribute revenue to crew usage via CRM: meetings prepped/followed-up through system vs. not
- Report KPIs **monthly per AE** and rolled up at team level for CRO/RevOps review

### KPI Cascade (Leading → Lagging)

```
Efficiency (MVP)                    Deal motion (Pilot)              Revenue (12 mo)
─────────────────                   ───────────────────              ────────────────
Prep time ↓ 80%          ──►        Meeting→next step ↑ 15%  ──►   Deals closed/AE ↑ 10–20%
Follow-up < 1 hr         ──►        Win rate ↑ 5–10%       ──►   Win rate ↑ 10–15%
CRM completeness ↑ 90%   ──►        Cycle days ↓ 10–25%    ──►   Revenue/AE ↑ 15–25%
Selling time reinvested  ──►        Pipeline velocity ↑    ──►   Quota attainment ↑
```

### Business Metrics (Leading — MVP / 90-Day Pilot)

| Metric | Target | Measurement | Links to revenue KPI |
|--------|--------|-------------|----------------------|
| Prep time per meeting | ↓ 80% vs. manual baseline | Workflow timestamps + self-report | More meetings run well → win rate ↑ |
| Time to follow-up draft | < 15 min post-meeting | Workflow timestamps | Faster follow-up → deal velocity ↑ |
| Meeting-to-next-step conversion | ↑ ≥ 15% within 7 days | CRM stage change after crew-tagged meeting | Deals advance → closure rate ↑ |
| Brief completeness score | ≥ 90% required fields | Schema validation | Better prep → win rate ↑ |
| Rep approval rate of drafts | ≥ 70% approved with minor edits | Approval UI events | Adoption → KPI cascade activates |
| Selling time reinvested | ↑ ≥ 5 hrs/week per AE | Time study or self-report | Capacity for more deals → deals closed ↑ |

### Business Metrics (Lagging — Pilot & Production)

| Metric | Pilot target (90 days) | 12-month target | Benchmark source |
|--------|------------------------|-----------------|------------------|
| Revenue per AE | +5–10% | +15–25% | Optifai +41%; Gong +29% org growth |
| Deals closed per AE | +5–10% | +10–20% | Optifai 15→23 deals/mo |
| Win rate | +5% absolute | +10–15% relative | Highspot 42% more likely |
| Sales cycle reduction | ↓10% | ↓15–25% | Optifai 28%; Salesforce 32% |
| Deal velocity | +10% | +20–28% | Optifai pipeline velocity data |
| ROI on enablement spend | Break-even path documented | **4:1** enablement ROI | SiftHub mature programs |

**Honest attribution note (from `mrd.md`):** Only ~5% of enterprises reliably measure AI ROI today (IBM, 2026). This product must instrument the KPI cascade above from day one of pilot—not rely on efficiency metrics alone.

### Technical Metrics

| Metric | Target |
|--------|--------|
| Agent task success rate | ≥ 95% without unrecoverable error |
| Hallucination flag rate (competitive) | 100% unverified claims flagged |
| Cost per workflow run | < USD 2.00 (MVP budget) |

### User Experience Metrics

| Metric | Target |
|--------|--------|
| Task completion rate | ≥ 90% of started workflows complete |
| Rep trust score (survey) | ≥ 4/5 after 2 weeks |
| Time-to-first-brief (onboarding) | < 10 min including connect |

---

## 8. Implementation Strategy

### Development Phases (Aligned to MRD-v2 Sprints)

**Phase 1 — MVP Core (Sprints 1–3)**

| Sprint | Scope | FR IDs |
|--------|-------|--------|
| 1 | Prep → follow-up loop with mock data | FR-P0-01, 04, 05, 06, 07 |
| 2 | Live research + competitive intel | FR-P0-02, 03 |
| 3 | Calendar auto-trigger | FR-P0-08 |

Deliverables: CrewAI crew (`config/agents.yaml`, `config/tasks.yaml`, `crew.py`), chat API, basic UI shell

**Phase 2 — Enhanced (Sprints 4–6)**

| Sprint | Scope | FR IDs |
|--------|-------|--------|
| 4 | Personalized follow-up | FR-P1-01 |
| 5 | Memory + deal alerts | FR-P1-02, 03 |
| 6 | Lead scoring | FR-P1-04, 05 |

**Phase 3 — Scale (Post-capstone)**

- CRM approve-and-commit writes
- Gong integration
- Enterprise SSO, SOC 2 path
- FR-P2-* evaluation

### Resource Requirements

| Role | Capstone need |
|------|---------------|
| Backend engineer | CrewAI crew, API, integrations |
| Frontend engineer | Chat UI, approval flows |
| Integration engineer | CRM + calendar wiring |
| QA | End-to-end workflow validation |

**Budget (API costs):** USD 500–2,000 for capstone development and demos

### Risk Mitigation

| Risk | Mitigation |
|------|------------|
| Hallucinated competitive claims | RAG + approval gate + citations |
| CRM data quality | Validate required fields; enrichment fallback |
| Scope creep | Strict P0-only for capstone demo sign-off |
| LLM cost overrun | Token budgets per workflow; caching research |
| Low rep trust | Draft-only mode; transparent agent attribution |

---

## 9. Launch & Go-to-Market Strategy

### Beta Testing Plan (Post-capstone path)

- **Segment:** 3–5 mid-market SaaS teams with HubSpot + Google Calendar
- **Duration:** 90 days (minimum for revenue KPI measurement)
- **Success — leading:** ≥ 70% weekly active reps; prep time ↓ 80%; follow-up < 1 hr
- **Success — business (per AE):** Revenue +5–10%; deals closed +5–10%; win rate +5%; cycle ↓10%
- **Feedback:** Weekly rep interviews + monthly KPI dashboard for RevOps/CRO

### Market Launch Strategy

- **Beachhead:** B2B SaaS AEs already on HubSpot
- **Channel:** Product-led demo → RevOps buyer conversation
- **Pricing hypothesis:** USD 79/user/month orchestration tier
- **Message:** "Your sales crew prepares, summarizes, and follows up—so you sell."

### Launch Success Criteria

| Milestone | Criteria |
|-----------|----------|
| Capstone demo | Full P0 loop live in ≤ 6 weeks |
| Beta (90 days) | 3 design partners; per-AE KPI baseline + **+5% revenue or deals closed** measured |
| GA readiness | CRM write with approval; SOC 2 roadmap; **12-month KPI targets** instrumented in product analytics |

---

## Requirements Traceability Matrix

| MRD-v2 Ref | PRD FR | Agent |
|------------|--------|-------|
| P0 #1 | FR-P0-08 | Calendar + Sales Manager |
| P0 #2 | FR-P0-02 | prospect_research |
| P0 #3 | FR-P0-03 | competitive_intel |
| P0 #4 | FR-P0-04 | meeting_prep |
| P0 #5 | FR-P0-05 | meeting_insights |
| P0 #6 | FR-P0-06 | follow_up |
| P0 #7 | FR-P0-07 | follow_up |
| P1 #1 | FR-P1-01 | follow_up / content |
| P1 #2 | FR-P1-02 | sales_manager |
| P1 #3 | FR-P1-03 | deal_progression (new P1 agent) |
| P1 #4 | FR-P1-04 | lead_scoring (new P1 agent) |
| P2 #1 | FR-P2-01 | meeting_insights (live) |
| P2 #2 | FR-P2-02 | outbound (new P2 agent) |

---

## Quality Assurance Checklist

- [x] All requirements traceable to `MRD-v2.md` and `mrd.md`
- [x] Technical specifications feasible with CrewAI hierarchical + Flows
- [x] Success metrics aligned with business objectives (leading indicators for MVP)
- [x] Resource requirements scoped for capstone
- [x] Risk mitigation documented
- [x] Timeline aligned to MRD-v2 sprint plan (6 sprints)

---

## Sources

1. `project-context/1.define/MRD-v2.md` — JTBD priorities and phasing (primary input)
2. `project-context/1.define/mrd.md` — quantitative market research
3. Grand View Research, Next MSC — sales enablement market sizing
4. Gong — State of Revenue Growth 2025
5. Highspot — State of Sales Enablement 2025
6. Salesforce — State of Sales 2025/2026
7. SyncGTM — sales task time benchmarks, 2026
8. Forrester — B2B buyer AI adoption, 2025
9. Optifai — AI-augmented sales productivity benchmark, N=938
10. CrewAI documentation — hierarchical process, Flows, memory
11. Maven Capstone Use Case Brief — Use Case 3, Lesson 4

---

## Assumptions

1. **Runtime:** `AAMAD_TARGET_RUNTIME=crewai` for all implementation epics.
2. **CRM default:** HubSpot sandbox for MVP (Salesforce documented as alternate).
3. **Sprint 1 trigger:** Manual meeting input acceptable before calendar integration (FR-P0-01 before FR-P0-08).
4. **Transcript input:** Pasted text or mock transcript for FR-P0-05; no Gong in MVP.
5. **CRM writes:** Draft-only through capstone demo; approve-and-commit is post-MVP.
6. **Content Personalization (P1 #1):** Merged into Follow-Up agent unless scope allows separate agent.
7. **English only** for MVP UI and agent outputs.

---

## Open Questions

1. Confirm HubSpot vs. Salesforce as primary CRM for capstone build.
2. Confirm manual trigger sufficient for Sprint 1 demo acceptance.
3. Minimum HubSpot fields for CRM draft schema (deal stage, next step, close date — confirm with stakeholder).
4. Should capstone demo use synthetic account data exclusively, or connect to live sandbox?
5. LLM provider preference (OpenAI vs. Anthropic) for `manager_llm` and specialist agents?

---

## Audit

| Field | Value |
|-------|-------|
| Timestamp | 2025-06-07T00:00:00Z |
| Persona | @product-mgr |
| Action | create-prd → update-v1.1-revenue-kpis |
| Template | `.cursor/templates/prd-template.md` |
| Input | `project-context/1.define/MRD-v2.md`, `mrd.md` |
| Output Path | `project-context/1.define/prd-v1.md` |
| Runtime | crewai |
| Model | Claude (Cursor Agent) |
| Temperature | Low (deterministic artifact generation) |
| Traceability | MRD-v2 → PRD → pending SAD (@system.arch) |
| Prompt Trace | Omitted — PRD generated from approved define artifacts; no production runtime prompts |
