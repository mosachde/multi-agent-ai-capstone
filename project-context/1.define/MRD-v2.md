# Market Research Document — Version 2 (JTBD + Priorities)

## Document Info

| Field | Value |
|-------|-------|
| Version | v2 |
| Use Case | Use Case 3: Sales Enablement & Meeting Automation Crew |
| Focus | Simplified jobs, priorities, and phases |
| Parent Artifacts | `mrd.md` (market data), `MRD-v1.md` (detailed JTBD) |
| Course Context | Maven Agentic AI Architect — Lesson 4 Capstone |

---

## Use Case (One Paragraph)

A **multi-agent sales crew** (CrewAI) that helps B2B reps prepare for meetings, run them better informed, and follow up fast—with research, competitive intel, and CRM updates handled by specialized agents coordinated by a Sales Manager agent.

---

## How to Read This Doc

**Priority** = when to build it  
**Phase** = which part of the sales motion it belongs to

---

## Priority Definitions

| Priority | Meaning |
|----------|---------|
| **P0** | Build first — required for capstone demo |
| **P1** | Build next — adds real value after MVP works |
| **P2** | Skip for capstone — post-MVP or different product surface |

---

## Phase Definitions

| Phase | Name | What it covers |
|-------|------|----------------|
| **A** | Meeting Automation | Before, during, and after the meeting itself |
| **B** | Sales Automation inputs | Research and content that feeds meeting workflows |
| **C** | Intelligence loop | Deal memory, risk alerts, prioritization |
| **D** | Outbound / extended | Top-of-funnel SDR motion (later) |

---

## P0 — Build These 7 First (Capstone MVP)

| # | Job | Phase | Agent |
|---|-----|-------|-------|
| 1 | Read calendar → know a meeting is coming | A | Calendar integration |
| 2 | Research the account & contact | B | Prospect Research |
| 3 | Pull competitive intel | B | Competitive Intelligence |
| 4 | Generate pre-meeting brief | A | Meeting Prep |
| 5 | Summarize what happened in the meeting | A | Meeting Insights |
| 6 | Draft follow-up email | A | Follow-Up |
| 7 | Draft CRM update | A | Follow-Up |

**One-line story:** Calendar → research → brief → meeting → summary → follow-up + CRM

### P0 Flow

```
Phase B              Phase A                         Phase A
(research)           (before meeting)                (after meeting)

  #2 Research ──┐
  #3 Compete  ──┼──► #4 Brief ──► MEETING ──► #5 Summary ──► #6 Email
                │                                              #7 CRM
  #1 Calendar ──┘
```

### P0 Job Details

| # | When… | I want to… | So I can… |
|---|-------|------------|-----------|
| 1 | A meeting is on my calendar | Detect it automatically | Start prep without manual lookup |
| 2 | I'm preparing for a call | See company + buyer context in one place | Show up informed in minutes, not hours |
| 3 | Competitors may be in the deal | Get positioning and talk tracks | Differentiate without guessing (human approves claims) |
| 4 | The meeting is about to start | Get a concise brief | Walk in confident with clear goals |
| 5 | The meeting just ended | Get notes and key decisions extracted | Act immediately without rewatching a recording |
| 6 | I need to follow up | Get a draft email | Send while interest is still high (human approves send) |
| 7 | CRM needs updating | Get a draft activity log + field updates | Keep pipeline accurate without manual entry (draft only) |

---

## P1 — Build These 4 Next

| # | Job | Phase | Agent |
|---|-----|-------|-------|
| 1 | Personalize follow-up content | B | Content Personalization |
| 2 | Remember context for the next meeting | C | Sales Manager + shared memory |
| 3 | Flag deal risks / missing stakeholders | C | Deal Progression |
| 4 | Score & prioritize which deals matter | C | Lead Scoring |

### P1 Job Details

| # | When… | I want to… | So I can… |
|---|-------|------------|-----------|
| 1 | I send follow-up | Tailor messaging to this account | Get higher response rates |
| 2 | I have another meeting on the same account | Reuse prior context automatically | Avoid starting research from scratch |
| 3 | A deal is moving (or stalling) | See risks and gaps | Advance or escalate before it's too late |
| 4 | I have too many opportunities | Know which deals to focus on today | Spend time on highest-impact revenue |

---

## P2 — Skip for Capstone

| # | Job | Phase | Agent |
|---|-----|-------|-------|
| 1 | Live coaching during the call | A | Meeting Insights (live) |
| 2 | Autonomous outbound / SDR emails | D | Outbound Agent |

| # | Why defer |
|---|-----------|
| 1 | Requires live recording/streaming + conversation intelligence integrations |
| 2 | Different product motion (top-of-funnel); scope creep for capstone |

---

## Agents (Simple Map)

| Agent | Owns |
|-------|------|
| **Sales Manager** | Coordinates all agents; shared memory (P1 #2) |
| **Prospect Research** | P0 #2 |
| **Competitive Intelligence** | P0 #3 |
| **Meeting Prep** | P0 #4 |
| **Meeting Insights** | P0 #5; P2 #1 (later) |
| **Follow-Up** | P0 #6, P0 #7 |
| **Content Personalization** | P1 #1 |
| **Deal Progression** | P1 #3 |
| **Lead Scoring** | P1 #4 |
| **Outbound** | P2 #2 (later) |

---

## Capstone Build Order

| Sprint | Build | Outcome |
|--------|-------|---------|
| 1 | P0 #4, #5, #6, #7 | Prep → follow-up loop (mock data OK) |
| 2 | P0 #2, #3 | Research feeds the brief |
| 3 | P0 #1 | Calendar triggers prep automatically |
| 4 | P1 #1 | Personalized follow-up |
| 5 | P1 #2, #3 | Memory loop + deal alerts |
| 6 | P1 #4 | Lead scoring |
| Later | P2 #1, #2 | Live coaching + outbound |

---

## Human-in-the-Loop (Required)

Rep must approve before:

- Customer-facing competitive claims (P0 #3)
- Sending follow-up email (P0 #6)
- Writing to CRM (P0 #7 — draft-only in MVP)

---

## Sources

1. `project-context/1.define/mrd.md` — market research
2. `project-context/1.define/MRD-v1.md` — detailed JTBD (superseded for prioritization by this doc)
3. Maven Capstone Use Case Brief — Use Case 3, Lesson 4
4. SyncGTM — sales task time benchmarks, 2026
5. Salesforce / Highspot — sales productivity and enablement statistics, 2025–2026

---

## Assumptions

1. Primary user is a mid-market B2B SaaS account executive.
2. P0 #1 can start as manual "prep this meeting" before calendar auto-trigger.
3. P0 #5 accepts transcript paste or mock recording (no Gong integration in MVP).
4. CRM writes remain draft-only until rep approval.
5. Runtime target: CrewAI (`AAMAD_TARGET_RUNTIME=crewai`).

---

## Open Questions

1. HubSpot or Salesforce for CRM integration?
2. Manual prep trigger OK for sprint 1, or calendar required from day one?
3. Merge Content Personalization into Follow-Up agent for MVP simplicity?

---

## Audit

| Field | Value |
|-------|-------|
| Timestamp | 2025-06-07T00:00:00Z |
| Persona | @product-mgr |
| Action | create-mrd-v2-simplified |
| Output Path | `project-context/1.define/MRD-v2.md` |
| Supersedes | `MRD-v1.md` for priority/phasing decisions |
| Prompt Trace | Omitted — simplification of MRD-v1 per user feedback |
