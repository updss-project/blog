---
title: "What's Shipped"
layout: "single"
url: "/whats-shipped/"
summary: "Three months of releases across ten products. Not a changelog. The arc of what's settled."
ShowToc: true
cover:
  image: "/images/whats-shipped.jpg"
---

The first six months of building UPDSS, "what shipped" was a question I answered for myself. Two products, a handful of features, an architecture that was still moving under the work it was holding up.

Three months later, the answer comes back as ten products and twenty-three tagged releases of the methodology itself, sitting alongside whatever each of those products has shipped in parallel. The shape of "what's shipped" has changed.

What I want to do on this page is not enumerate the changelog. The git tag list will always be more honest than a marketing summary. What I want to record is the arc, what's settled and what hasn't, across the methodology and the portfolio that sits inside it.

---

## UPDSS, the methodology itself

Twenty-three tagged releases since April, in two families.

### v0.13 family: governance hardening, sandbox tooling

Seven patches, from v0.13.0 in mid-April through v0.13.7 in mid-May. The story here is the methodology learning to enforce itself instead of asking the agents to remember to. Three things took shape in the v0.13 family: pre-commit CHKs, verifier pairs binding documented rules to executable checks, and sandbox tooling that made invasive refactors safe without breaking the live vault. A lot of the v0.13 work was about catching small consistency drifts before they accumulated into incidents.

### v0.14 family: enforcement architecture, dashboard credibility, clone-first completion

Sixteen patches and hotfixes, from v0.14.1 through v0.14.9.2. The v0.14 work picked up a different problem. The methodology had hardened, but the dashboard hadn't kept pace with what the methodology was actually doing. Stories that were partly done read as "0 of 9 done." Inconsistency banners fired where they shouldn't. v0.14.8 was specifically about catching all of that, and it pulled in three productive complaints from the Tatwam-AlgoTrading G2 review into the scope as load-bearing fixes.

v0.14.8.1 and v0.14.8.2 were the clone-first completion hotfixes. UPDSS now reads other products' vaults through a per-product local clone where possible, instead of round-tripping through the GitHub Contents API for every dashboard refresh. Page load latency dropped from minutes to seconds.

v0.14.9 was the largest single release of the methodology since onboarding multi-product. Six themes, with Theme I shipping IPDC (see the [page on that](/methodology/ipdc/) for protocol detail). The other themes were a verifier_chk pair convention so every doctrine pointer binds to an automated check, a doctrine sweep through the methodology, and a what's-blocking-cli that lets an agent ask "what would fire if I committed these staged paths" before actually committing.

v0.14.9.1 and v0.14.9.2 are the hotfixes that closed the residual gaps the release identified at G3. Three fixes shipped within forty-eight hours of being filed at the v0.14.9 retro: canonical-list parsing in the dashboard, evidence sub-key cross-validation, and gate-approve idempotency for same-user re-clicks.

For the full list, see [github.com/omyshafoundation/a4g_updss/tags](https://github.com/omyshafoundation/a4g_updss/tags). The arc is what matters here.

---

## Tatwam AlgoTrading

*v0.3 shipped 2026-06-30.*

The largest single release in AT's history. Thirty-four stories plus three follow-up sprints. v0.3 was the paper-trading promotion chain, the work that took strategies from "ran in backtest" to "runs in paper mode with an operator at the dashboard, ready for a trader-bound risk gate to flip live." The Nautilus TradingNode binding came forward from the v0.4 plan because the timing fit. v0.4 G1 prep is imminent at the time of writing.

Across the April-to-June window, AT moved from v0.2.5 through v0.3. Catalog and iron condor templates landed. The framework and Nikhil's monthly weekly strategies settled in. The visual strategy template iterated through several drafts. The structural reorganization split the runtime, the strategies, and the operator surface into surfaces that could iterate independently.

---

## Tatwam StrategyBuilder

*v0.1 in progress.*

A new product. SB was forked from AT at v0.2.6 on 2026-06-19. v0.1, "Export and Packaging Foundation," is the work to make strategy authoring a first-class surface. The first release is about exporting packages that AT can import for execution. The fork happened because authoring and execution had started paying for each other's release cadence, and the cleaner separation has already paid for itself in iteration speed on the authoring side.

---

## Tatwam TestDataManager

*v0.3.4 deployed.*

The test data plane for the Tatwam stack. v0.3.4 deployed the cross-vault HMAC endpoint, the dynamic NIFTY chain resolver, the WAL+S3 sink, and the always-on capture scope. 402,909 rows of yesterday's AT-pilot data backfilled into the canonical catalog, eleven NIFTY symbols, live. Most of the work here is quiet. That's the point. AT and SB depend on this being predictable.

---

## A4G Collab Hub

*v0.1 shipped 2026-06-29.*

The first non-Tatwam external product to come through the full UPDSS gate sequence. Built for the A4G Intelex 2026 essay contest. Participant registration, submission portal, three-tier jury pipeline, AI-detection and cross-similarity quality checks, real-time operations dashboard. Live at [hub.a4gcollab.org](https://hub.a4gcollab.org). G3 was approved by Sneha Chouksey on 2026-06-24, which made it the first gate signed by someone outside the Omysha / Tatwam orbit.

---

## Omysha Marketing Tools

*v0.1 in progress.*

v0.1 is the brand-agnostic marketing operations foundation. The Brand Integration socket from EPIC-006 is what makes "one platform, multiple brands" possible in practice, instead of one platform that has to be forked per brand. The Anunaad Reflections newsletter and the Substack pipeline are early consumers.

---

## Omysha SignIn

*Live.*

Identity is load-bearing for the rest of this list. The Authentik instance behind [signin.omysha.org](https://signin.omysha.org) is what signs every gate approval across every product vault. The JWT issued at login becomes the token_hash in the gate commit, and that's the chain that makes "Sneha approved A4G Hub G3" something you can verify, not just claim.

---

## Omysha SSP, Org Process Modeling, Synapse

*Planning phase.*

Three products under shape rather than under ship. SSP is the ERP-like operational backbone, scope under shape and not yet at G1. Org Process Modeling is in early collaboration with Mohammad Shahjad, BPMN-first then automation second. Synapse is a registered product with a vault scaffold and a vision pending. The reason these get listed at all is the same reason the registry exists. The portfolio is what it is. Hiding the shape of the work earlier than it's ready isn't honesty.

---

## What three months of shipping has actually settled

Twenty-three releases of one methodology in three months is a lot, but it's the wrong number to optimize for. What I'm watching is whether the methodology now bears its own weight. The actual test: does a CHK at pre-commit catch the class of mistake it was written for, and does a story passing G1 and G2 ship at G3 without needing a hotfix in the next forty-eight hours.

The answer is closer to yes than it was three months ago, and not as close as it'll be in three more.

Coherence across ten products is the harder problem now, not throughput on any one of them. The IPDC work mattered for that reason. The dashboard credibility work mattered for the same reason. The methodology has settled into a rhythm, and the portfolio has settled into a shape. What's left is the work of keeping both honest as the AI agents underneath them get faster.
