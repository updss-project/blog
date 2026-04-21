---
title: "For Product Leaders"
layout: "single"
url: "/for-product-leaders/"
summary: "What a CXO or VP Engineering sees when governing AI-driven development with UPDSS."
ShowToc: true
cover:
  image: "/images/for-product-leaders.jpg"
---

You don't write code. But you make the decisions that determine whether AI-written code should ship. UPDSS was designed for exactly that split: agents do the building, humans do the governing.

Here's what governing AI-agent-driven development actually looks like.

## Stories: The Unit of Work

Every piece of work in UPDSS starts as a story. A story isn't a Jira ticket with a title and an assignee. It's a structured contract with specific fields that matter for governance.

**Scope** defines exactly what the agent will build. Not vaguely ("improve the dashboard") but precisely ("add cost forecasting to the release detail panel, using JSONL session logs as the data source").

**Out-of-scope** is equally important. It tells the agent what NOT to touch. Without this, an agent tasked with fixing a button will quietly refactor the entire component tree. I've seen it happen. More than once.

**Budget** is a dollar amount. Not story points, not t-shirt sizes. Actual dollars. A typical story costs $3 to $15 in API costs. The agent knows its budget before it writes a single line of code.

**Appetite** comes from Shape Up methodology. It answers the question: how much time and money is this problem worth? A $5 story that's burning $12 at 70% completion triggers an escalation. The agent stops and asks. It doesn't silently continue.

## The Three Gates

Nothing ships without passing three human-approved gates. This is the Stage-Gate spine of UPDSS.

### G1: Scope Review

Before any agent writes code, a human reviews the scope. The questions at G1:

- Is this the right work to do right now?
- Is the scope tight enough that an agent can execute without ambiguity?
- Is the budget realistic for what's being asked?
- Are the out-of-scope boundaries clear?

G1 approval means "yes, build this." G1 rejection means "rewrite the scope" (and the agent does, with specific feedback from the rejection notes).

### G2: Implementation Review

After the agent finishes building, before the code merges to main. The questions at G2:

- Do the tests pass?
- Does the implementation match the scope?
- Is the code traceable? (Every commit linked to a story ID.)
- Is documentation updated?

G2 is where you catch scope creep, untested edge cases, and the occasional agent that decided to "improve" something outside its mandate.

### G3: Release Review

Before deployment to production. The questions at G3:

- Is CI green?
- Are release notes complete?
- Is there a rollback plan?
- Does the deployment evidence confirm it's actually running?

G3 approval means "ship it." And here's the critical constraint: agents can't approve any gate. The gate file's `decision` field defaults to null. Only a human, through the dashboard, can change it to `approved`. The system doesn't just suggest this rule. It enforces it mechanically. An agent literally can't write `decision: approved` to the file, because the pre-commit hooks will reject the commit.

## The Release Lifecycle

A release in UPDSS follows a clear path:

**Planned.** Stories are drafted, budgets estimated, scope defined. The release declares its appetite: "This is worth 3 to 4 days of agent work and $30 in API costs."

**G1 Approved.** Human reviews and approves the scope. Agents can begin.

**In Progress.** Agents pull stories and build. Each agent works on one story at a time, reports progress to `_OutBox/`, and tracks its cost in real time.

**Done (Dev).** All stories complete. Code is ready for implementation review.

**Done.** G2 approved. Ready for release review.

**Released.** G3 approved. Deployed to production with evidence.

The whole thing uses Kanban flow. No sprints. No two-week boundaries. Work gets pulled when capacity exists. A release that was planned for a week can land in two days if agent throughput is high. v0.9 shipped 23 stories for $254. Shape Up would call that a cycle-budgeting error. We call it Tuesday.

## The Cost Model

This is where UPDSS changes the conversation for product leaders.

Every story has a `cost_budget_usd`. Every release has a `budget_usd`. Every agent session writes its actual cost (tokens multiplied by model price) to the progress log. The dashboard shows budget burn in real time, with per-story breakdowns and forecasts.

Here's a real example. Release v0.11.8 was budgeted at $30. Final cost: $29.50. That's not estimation. That's measurement.

The unit is dollars, not story points. Dollars are fungible across models (switching from Opus to Sonnet changes the cost per token, and the system tracks it). Dollars are comparable across releases. And dollars are something every product leader already understands.

When you're reviewing a proposed release, you can ask: "Is this feature worth $30 in AI costs plus two days of my review time?" That's a concrete question with a concrete answer. Try asking that with story points.

## The Dashboard

The UPDSS dashboard is your primary interface for governance. It shows:

- All active releases across all products, with their current phase
- Budget vs. actual cost per release and per story
- Gate status (which gates are pending your review)
- Story flow (what's planned, what's in progress, what's done)
- Cost forecasts based on burn rate
- Decision history (every gate approval, rejection, and the notes attached)

You don't need to understand Git, YAML, or terminal commands to use it. The dashboard translates the vault's machine-readable state into human-readable governance decisions.

## When to Trust Agents, When to Intervene

After 50+ releases, here's the practical framework I use.

**Trust agents with:** implementation within a well-scoped story, test writing, documentation updates, progress reporting, cost tracking, preparing gate review materials.

**Intervene for:** scope decisions (what to build next), gate approvals (always human), budget overruns (agents escalate, you decide whether to continue or cut scope), architectural choices that affect multiple products, and anything where the agent says "I'm not sure."

**Watch for:** stories that keep expanding in scope (the agent finds "one more thing" to fix), cost burn that's tracking ahead of progress, and agents that stop communicating through `_OutBox/` (usually means they're stuck in a loop).

The pattern is straightforward. Agents are excellent executors within clear boundaries. They're poor judges of whether the boundaries are right. That judgment is your job. UPDSS gives you the structure and the data to make those judgments well.

## What This Means for Your Organization

If you're running AI agents on production code today, you already have a governance problem. You might not have noticed it yet because the volume is still low or because a senior engineer is manually reviewing everything. But as you scale to more agents, more products, and more concurrent work, the manual approach breaks.

UPDSS is one answer to that problem. Built from real operational experience (not theory), validated across six products and 50+ releases, costing roughly $500 total to prove out.

The model choice is secondary. The vyavastha around the model is primary. The system, the structure, the governance. That's what UPDSS provides.

*Want to see the technical architecture? Read [How It Works](/how-it-works/). Want to see the agent's perspective? Read [For AI Agents](/for-agents/).*
