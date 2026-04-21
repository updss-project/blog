---
title: "For AI Agents"
layout: "single"
url: "/for-agents/"
summary: "What an AI agent sees when it boots into a UPDSS-governed session. The compliance rules, the hard constraints, and the communication patterns."
ShowToc: true
cover:
  image: "/images/for-agents.jpg"
---

This page describes what an AI agent experiences when it operates under UPDSS governance. Not what we tell people the agents do. What the agents actually see, check, and are constrained by.

Nobody else publishes this. That's deliberate on our part and, frankly, an oversight on everyone else's.

## The Readiness Check: Before Any Work Begins

When an agent starts a session, the first thing that runs is `readiness-check.py`. It verifies three categories of state before the agent writes a single line of code.

**Machine state.** Is Python available? Is Git configured? Is Docker running (if needed)? Are the required environment variables set, including `GITHUB_TOKEN`? If any of these fail, the session doesn't start.

**Product state.** Are the UPDSS pre-commit hooks installed? Does the vault pointer (`.updss/vault-pointer.yaml`) point to a valid vault repository? Is the code contract (`.updss/code-contract.yaml`) present? Is `core.hooksPath` correctly set to UPDSS's hooks directory?

**Session state.** Is there an active release? Is a story assigned to this agent? What phase is the story in? If the story is in `planned` but G1 hasn't been approved, the agent can't start implementation. If the story is already `released`, there's nothing to do.

Prerequisite failures are promoted to hard blockers (`[BLOCK]`). The agent can't work around them. It can't say "I'll fix the hooks later." The readiness check is a gate before the gate.

## The Hard Rules

Four constraints that never bend. In eight weeks of operation across six products and nine agents, zero violations. Not because agents are obedient. Because the constraints are mechanical.

### 1. Never Approve Gates

Agents prepare gate review materials. They gather evidence, run tests, draft checklists, write findings to `_OutBox/`. But they can't change a gate's `decision` field from null to `approved`. The YAML file defaults to `decision: ~` (null). Only the dashboard's approve endpoint, triggered by a human, can write the approval. Pre-commit hooks reject any commit that tries to set a gate decision.

This held even when the dashboard was broken and manual YAML editing was the only way to approve gates. Agents refused to write `decision: approved`. They waited for the fix. That's not willpower. That's a pre-commit hook.

### 2. Never Deploy

No agent can push to production without human confirmation. Deployment requires G3 approval, which requires a human decision, which requires the dashboard endpoint. The chain is unbreakable from the agent's side.

### 3. Never Write Code Outside Story Scope

Every story has a `scope` section and an `out_of_scope` section. The agent's work must stay within these boundaries. If the agent discovers something that needs fixing outside its story, it writes a report to `_OutBox/` recommending a new story. It doesn't fix it.

This is harder to enforce mechanically than the first two rules (scope is semantic, not syntactic). But the commit message format helps: every commit tagged with `[STORY-NNN-YY-ZZ][T-N]` links to a specific story and task. Code review at G2 can verify that commits don't drift.

### 4. No Code Without Traceability

Every commit must reference a story ID, a vault tag, or a GitHub Issue. The pre-commit hook validates the commit message format. Accepted prefixes: `[STORY-NNN-YY-ZZ][T-N]` for story work, `[VAULT]` for vault-only changes, `[FIX]` for quick fixes (linked to an issue), `[CURSOR]` for cursor updates, `[GATE]` for gate file changes. Anything else gets rejected.

This means you can trace any line of code back to the story that authorized it, the release that contained it, and the gate that approved it. Full audit trail from code to governance decision.

## Commit Discipline

Commit messages in UPDSS follow a strict taxonomy. Here's what the agent sees at commit time.

The format: `[STORY-NNN-YY-ZZ][T-N] description`

Breaking that down:
- `NNN` is the EPIC number
- `YY` is the feature number within that EPIC
- `ZZ` is the story number within that feature
- `T-N` is the task number within the story

So `[STORY-003-02-01][T-3] Add cost forecasting to release panel` means EPIC 003, Feature 02, Story 01, Task 3. You can read the full hierarchy from the commit message alone.

**Branch policy** is equally strict. Story work happens on release branches (`release/vX.Y`). The agent can't commit story work to `main`. Vault changes and certain administrative commits can go to `main`, but implementation code can't.

The pre-commit hook `check-methodology.py` enforces all of this. It runs on every commit in every product repo, because all product repos point their `core.hooksPath` to UPDSS's hooks directory. One set of rules, six products.

## The _OutBox/_InBox Pattern

Agents don't talk to each other directly. They communicate asynchronously through file-based handoffs.

When an agent needs to report something (a completed task, a discovered bug, a gate review finding, a budget overrun), it writes a Markdown file to its vault's `_OutBox/` directory. The file includes YAML frontmatter:

```yaml
from: agent-signin-v0.2
to: orchestrator
re: Cross-repo contamination in readiness check
status: needs-attention
date: 2026-04-15
```

The orchestrator reads `_OutBox/` files across all product vaults, routes instructions, and writes responses to the relevant `_InBox/` directory.

Why files instead of direct messages? Because long messages sent via `tmux send-keys` trigger Claude Code's paste-mode trap, which gets stuck on "Pasting text..." indefinitely. We discovered this the hard way. The `_InBox/_OutBox` pattern replaced direct message passing for anything longer than a short command.

But there's a deeper reason too. File-based communication creates an audit trail. Every instruction, every report, every escalation is a file in Git with a timestamp and a commit hash. Conversations are ephemeral (and get compacted when context windows fill up). Files persist.

## What Happens on Failure

Agents fail. Models hallucinate. API calls time out. Code doesn't compile. UPDSS has explicit protocols for each failure mode.

### Transient vs. Fault Diagnosis

When something goes wrong, the agent's first job is classification. Is this transient (an API timeout, a flaky test, a network blip) or a fault (a logic error, a missing dependency, a scope misunderstanding)?

Transient failures get retried (up to a limit). Faults get reported to `_OutBox/` with diagnosis notes. The agent doesn't keep trying the same broken approach hoping it'll work on the fifth attempt.

### Budget Overrun Protocol

If a story's cost approaches its budget, the agent must escalate. Not continue working. Not cut corners. Escalate.

The escalation goes to `_OutBox/` with the current spend, the estimated remaining cost, and a recommendation (increase budget, reduce scope, or abandon the story). The human decides. The agent waits.

This is Shape Up's "fixed appetite, variable scope" principle made mechanical. The appetite is declared in dollars. The enforcement is a budget check. The escape valve is human judgment, not agent optimism.

### Stuck Detection

If an agent hasn't made progress (no commits, no `_OutBox/` reports) for an extended period, the orchestrator checks. Usually this means the agent is in a loop: trying the same fix repeatedly, waiting for a resource that isn't available, or confused about its current task.

The fix is usually simple. Read the agent's `cursor.yaml` to see what it thinks it's doing. Check `progress.md` for the last session entry. Send a short message via `tmux send-keys` to reorient it. If needed, restart the session (the readiness check will re-establish context from the vault state).

## The Compliance Rules

Here's the full set of constraints that shape every agent action in a UPDSS session. These aren't guidelines. They're enforced through hooks, readiness checks, and gate tool defaults.

1. **Every commit must be traceable** to a story, vault change, or issue
2. **Every story must have G1 approval** before implementation begins
3. **Every release must pass G1, G2, and G3** before deployment
4. **Agents can't approve gates** (mechanical enforcement via null default + dashboard-only approval)
5. **Agents can't deploy** without human confirmation through G3
6. **Budget overruns require escalation**, not silent continuation
7. **Out-of-scope work gets reported**, not performed
8. **Session state must be verified** before work begins (readiness check)
9. **Communication uses _OutBox/_InBox**, creating an audit trail
10. **Cursor state must match reality** (triad consistency check at commit time)
11. **Methodology doc versions must match version pins** (CI enforcement)
12. **Branch policy must be followed** (story work on release branches only)

## Why This Matters

Most organizations using AI agents for code generation treat the agent like a faster developer. Give it a prompt, get code back, review it, ship it. That works fine for isolated tasks.

It breaks completely at scale. When you have multiple agents, multiple products, concurrent releases, and real money flowing through API costs, you need governance that the agents can't circumvent. Not governance they're asked to follow. Governance that constrains their execution environment so violations are structurally impossible.

That's what UPDSS provides from the agent's perspective. Clear boundaries. Mechanical enforcement. An async communication channel that creates audit trails. And explicit protocols for every failure mode.

The vault is the agent's memory (because the agent's own context window is ephemeral). The hooks are the agent's constraints (because the agent can't be trained, only constrained). The gates are the human checkpoints (because some decisions should never be delegated to a model).

*For the human's perspective, read [For Product Leaders](/for-product-leaders/). For the technical architecture, read [How It Works](/how-it-works/).*
