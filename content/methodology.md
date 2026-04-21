---
title: "Methodology"
layout: "single"
url: "/methodology/"
summary: "UPDSS combines Stage-Gate, Kanban, and Shape Up into a governance methodology designed for AI-agent-driven development."
---

## The UPDSS Methodology

UPDSS isn't agile. It isn't waterfall. It's a hybrid of three proven methodologies, each contributing a specific structural element:

### Stage-Gate (Cooper)

Three gates, each requiring human approval:

- **G1 — Scope Review**: is this the right work, scoped correctly, with a realistic budget?
- **G2 — Implementation Review**: is the code sound, tested, and traceable?
- **G3 — Release Review**: is the release complete, with evidence, deployment verified?

Nothing ships without all three gates approved. Agents prepare findings and present them. Only humans approve.

### Kanban (Flow-Based)

Stories flow through phases: `planned → g1_approved → in_progress → done_dev → done → released`. No sprints. No velocity targets. Work is pulled when capacity allows, not pushed on a schedule.

### Shape Up (Basecamp)

Fixed appetite, variable scope. Every release has a `budget_usd` and `appetite` declaration. If the budget is exceeded, the agent must escalate, not silently continue. The scope adjusts to fit the appetite, not the other way around.

### The Release Model

UPDSS uses 4-level semantic versioning: `version.major.minor.micro`

- **Version** (a): fundamental platform change
- **Major** (a.x): significant capability milestone
- **Minor** (a.x.y): planned features with stories and gates
- **Micro** (a.x.y.z): small planned releases with 2-3 stories

### The Cost Model

Every story has a `cost_budget_usd`. Every release has a `cost_actual_usd` tracked from session logs. The dashboard shows cost forecasts and budget burn in real time.

v0.11.8 budgeted $30, spent $29.50. The accuracy comes from agents being transparent about their token usage and the methodology enforcing honest reporting.

---

*For the full architectural walkthrough, read [Governance-as-Code: How UPDSS Governs AI Agents Building Production Software](/posts/governance-as-code/).*
