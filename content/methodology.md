---
title: "Methodology"
layout: "single"
url: "/methodology/"
summary: "UPDSS combines Stage-Gate, Kanban, and Shape Up into a governance methodology designed for AI-agent-driven development."
mermaid: true
ShowToc: true
cover:
  image: "/images/methodology-gates.jpg"
---

## The UPDSS Methodology

UPDSS borrows its spine from three proven methodologies. Each contributes a specific structural element. None was adopted wholesale. The combination was driven by eight weeks of real operation, not by theory.

### Stage-Gate (Cooper)

Three gates, each requiring human approval:

- **G1, Scope Review**: Is this the right work, scoped correctly, with a realistic budget?
- **G2, Implementation Review**: Is the code sound, tested, and traceable?
- **G3, Release Review**: Is the release complete, with evidence, deployment verified?

Nothing ships without all three gates approved. Agents prepare findings and present them. Only humans approve. Gates can also reject outright, with `decision: rejected` and rework notes as a first-class state.

### Kanban (Flow-Based)

Stories flow through phases: `planned`, `g1_approved`, `in_progress`, `done_dev`, `done`, `released`. No sprints. No velocity targets. Work is pulled when capacity allows, not pushed on a schedule.

### Shape Up (Basecamp)

Fixed appetite, variable scope. Every release has a `budget_usd` and an `appetite` declaration. If the budget is exceeded, the agent must escalate, not silently continue. The scope adjusts to fit the appetite. UPDSS borrowed Shape Up's vocabulary (appetite, betting table, hill charts) without its fixed six-week cycle.

### The Release Lifecycle

A release moves through a defined sequence, with gate checkpoints where human decisions are required.

```mermaid
graph LR
    P["planned"] -->|"G1"| G1A["g1_approved"]
    G1A --> IP["in_progress"]
    IP --> DD["done_dev"]
    DD -->|"G2"| D["done"]
    D -->|"G3"| R["released"]

    style P fill:#533483,stroke:#e94560,color:#eee
    style G1A fill:#0f3460,stroke:#e94560,color:#eee
    style IP fill:#16213e,stroke:#e94560,color:#eee
    style DD fill:#16213e,stroke:#e94560,color:#eee
    style D fill:#0f3460,stroke:#e94560,color:#eee
    style R fill:#1a1a2e,stroke:#e94560,color:#eee
```

At G1, the human approves the scope and budget. At G2, the human reviews the implementation and test evidence. At G3, the human confirms the release is deployment-ready with rollback plans.

A release planned for a week can land in two days if agent throughput is high. v0.9 shipped 23 stories for $254. The release cycle is the unit of cadence, not a fixed time boundary.

### 4-Level Versioning

UPDSS uses semantic versioning with four levels: `a.x.y.z`

- **Version** (`a`): Fundamental platform change. Rare.
- **Major** (`a.x`): Significant capability milestone. New EPICs, architectural shifts.
- **Minor** (`a.x.y`): Planned features with stories and gates. The standard release unit.
- **Micro** (`a.x.y.z`): Small planned releases with 2 to 3 stories. Rapid patches.

In practice, most work happens at the minor and micro levels. The eight-week run went from v0.1 through v0.11.9, including four rapid micro-releases (v0.9.1.1 through v0.9.1.4) that each fixed a single bug in under 30 minutes.

### The Cost Model

Every story has a `cost_budget_usd`. Every release has a `budget_usd`. Every session writes its actual cost and model to the progress log. The dashboard shows cost forecasts and budget burn in real time.

The unit is tokens multiplied by model price. Measurable without self-report, fungible across models (Sonnet costs less per token than Opus, and the system accounts for it), and pre-committed before work begins.

v0.11.8 budgeted $30, spent $29.50. v0.11 budgeted $252, spent $252. That accuracy comes from two things: agents being transparent about their token usage, and JSONL session logs that verify it independently. The methodology enforces honest reporting. The logs make honest reporting auditable.

When a story's cost approaches its budget, the agent escalates. The human decides whether to increase the budget, reduce the scope, or stop. Agents don't silently continue past their budget. They can't (mechanically). This is the vyavastha of cost control: structure that holds because it's built into the system, not because someone remembers to check.

### One Deliberate Non-Adoption

UPDSS doesn't use sprints. A 1,003-line analysis (research R-088) mapped UPDSS against every major methodology and documented the rejection of sprints permanently. The reasoning: adding a time-boxed sprint layer inside a release would double the ceremony cost (two planning meetings, two retrospectives, two burn-down charts) without reducing risk. For a single-operator methodology with cost-bounded agent work, the release cycle is the right granularity.

---

*For the full architectural walkthrough, read [Governance-as-Code: How UPDSS Governs AI Agents Building Production Software](/posts/governance-as-code/). For a practical view of governance decisions, read [For Product Leaders](/for-product-leaders/).*
