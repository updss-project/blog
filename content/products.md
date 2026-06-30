---
title: "Products Under Governance"
layout: "single"
url: "/products/"
summary: "Ten products under one harness. What they do, where they are."
ShowToc: true
cover:
  image: "/images/products.jpg"
---

There's a moment when something you've been building stops being a tool you're aware of using and becomes the way you work. That moment came for me a few weeks ago with UPDSS.

It now holds together nine real products. Three of them are under Tatwam Labs and six are under Omysha. UPDSS itself is the tenth, the harness that runs through all of them.

These aren't small things. Each one is a serious system with its own users and its own working life. What UPDSS does is sit underneath all of them, and let one person working with AI coordinate the building of nine real systems at the same time, without the work fragmenting into nine unrelated shops.

What follows is the current portfolio. Each card has two lines. The first says what the product is for, and who it's for. The second is the governance evidence. Gate status, release count, where the work actually sits today. The cards are proof, not theory.

---

## UPDSS

> Methodology and tooling for AI-agent-assisted product development. The harness the rest of this list sits inside.

> v0.14.9.2 shipped 2026-06-26. 23 tagged releases since April. Every release closed through its own G1, G2, G3.

The story since April has been about the foundation bearing its own weight. The v0.13 family was about hardening governance and giving the sandbox tooling room to grow. The v0.14 family was about dashboard credibility and routing work cleanly across multiple product vaults. v0.14.9 carried Theme I, the IPDC work, which is what made it possible to run all ten of these in parallel. There's a separate page for that: see [IPDC](/methodology/ipdc/).

[github.com/omyshafoundation/a4g_updss](https://github.com/omyshafoundation/a4g_updss)

---

## Tatwam AlgoTrading

> Algorithmic options trading on NSE Nifty. Iron Condor templates, bespoke author-mode strategies, paper and live execution with a Nautilus runtime.

> v0.3 closed at G3 on 2026-06-30 (34 stories plus three follow-up sprints, the largest single release in AT's history). v0.4 G1 prep is imminent.

v0.3 was about promotion. The chain from backtest to paper-mode finally settled into something an operator can run without holding it together by hand. The Nautilus TradingNode binding came forward from the v0.4 plan because the timing was right.

[github.com/Tatwam-Labs/Tatwam-AlgoTrading](https://github.com/Tatwam-Labs/Tatwam-AlgoTrading)

---

## Tatwam StrategyBuilder

> Strategy authoring and backtest workbench for Nifty options. Bespoke and templated strategies, run against TDM catalogs, export to AT for paper or live execution.

> v0.1 in progress. Forked from Tatwam AlgoTrading at v0.2.6 on 2026-06-19. Authoring is now its own product.

The fork happened because the authoring surface and the execution surface had started paying for each other's release cadence. AT executes. SB authors. They still share data and conventions through the UPDSS multi-product schema. The cleaner separation has already paid for itself in how fast SB can iterate.

[github.com/Tatwam-Labs/Tatwam-StrategyBuilder](https://github.com/Tatwam-Labs/Tatwam-StrategyBuilder)

---

## Tatwam TestDataManager

> Test data plane for the Tatwam stack. Live broker tick capture from Zerodha and Accelpix, historical option-chain catalogs, AT-pilot data continuity.

> v0.3.4 deployed and capturing. 402,909 rows backfilled into the canonical catalog from yesterday's AT-pilot run.

The thing that makes "yesterday's market data" actually retrievable, not approximated. AT and SB consume from here. v0.3.4 closed the cross-vault HMAC endpoint, the dynamic NIFTY chain resolver, the WAL+S3 sink, and switched scope to always-on. It's quiet infrastructure. That's the point.

[github.com/Tatwam-Labs/Tatwam-TestDataManager](https://github.com/Tatwam-Labs/Tatwam-TestDataManager)

---

## A4G Collab Hub

> Essay contest platform built for A4G Intelex 2026. Participant registration, submission portal, three-tier jury pipeline, AI-detection and cross-similarity quality checks, real-time operations dashboard.

> v0.1 shipped 2026-06-29. Live at [hub.a4gcollab.org](https://hub.a4gcollab.org). G3 approved 2026-06-24 by Sneha Chouksey.

The first product outside Tatwam to come through the full UPDSS gate sequence. A small platform by feature count, but a meaningful one. It proved that the multi-product schema we ratified back in v0.9 actually does what it said it would: a new product can be onboarded and shipped without bending the methodology to fit.

[github.com/omyshafoundation/hub_a4gcollab](https://github.com/omyshafoundation/hub_a4gcollab)

---

## Omysha SignIn

> Authentik-based centralised IAM for the Omysha portfolio. OIDC login, group-based authorization, the identity substrate the rest of these systems lean on.

> Live at [signin.omysha.org](https://signin.omysha.org). UPDSS dashboard signs every gate approval with a JWT issued from this instance.

Identity is load-bearing here. The dashboard's gate-approval signature, the token_hash in every G1/G2/G3 commit across every product, all of it comes back to a JWT from this Authentik. That's the thing that makes a gate approval verifiable rather than just claimed.

[github.com/omyshafoundation/omysha-signin](https://github.com/omyshafoundation/omysha-signin)

---

## Omysha Secretariat Support Platform

> ERP-like operational backbone for the Omysha Foundation. Planning surface, HR and OPM dashboards, timesheets, an agent interface for routine secretariat work.

> v0.1 scope under shape. Planning phase, not yet at G1.

It sits where "stuff humans do to run an organization" meets "stuff agents could do if the work were structured for it." The Org Process Modeling product (further down this list) feeds the upstream BPMN. SSP is where those models get operationalized.

[github.com/omyshafoundation/omysha-ssp](https://github.com/omyshafoundation/omysha-ssp)

---

## Omysha Marketing Tools

> Brand-agnostic marketing operations. Multi-channel publishing, campaign management, content pipeline, email, image generation, analytics. Brand content databases plug in through a defined integration contract.

> v0.1 active. The Brand Integration socket from EPIC-006 is what makes "one platform, multiple brands" possible.

This is the socket-and-plug pattern earning its keep. One marketing-ops platform. Multiple brands feed their own content and campaign databases through a clean contract. The Anunaad Reflections newsletter, this site's posts, the Substack pipeline. They all end up here eventually.

[github.com/omyshafoundation/Omysha-MarketingTools](https://github.com/omyshafoundation/Omysha-MarketingTools)

---

## Omysha Org Process Modeling

> BPMN process mapping for the Omysha product suite, with the eventual goal of agent-driven execution where the model is the spec the agent runs from.

> Planning phase. Early scope work in collaboration with Mohammad Shahjad.

The architecture call we're making is modeling first and automation second. BPMN-XML as the source of truth, dashboard tooling for editing and version control, agents that read the model and execute against it. Automating too early is how you end up with rigid processes nobody can adjust.

[github.com/omyshafoundation/Omysha-OrgProcessModeling](https://github.com/omyshafoundation/Omysha-OrgProcessModeling)

---

## Omysha Synapse

> Under shape. Vision pending; vault scaffold exists.

> Planning phase. Listed for transparency. When the vision lands, this card gets a paragraph.

The registry is the registry. A product that's just been registered shouldn't pretend to be more than that. Watch this space.

[github.com/omyshafoundation/Omysha-Synapse](https://github.com/omyshafoundation/Omysha-Synapse)

---

## What ten products under one harness actually looks like

This many can run in parallel under one human not because the human got faster. The substrate got better.

Each product has its own vault repo, its own G1/G2/G3 gates, its own release-scope.yaml and retrospectives. The methodology is the same. The artefacts are per-product. The [dashboard](https://updss.omysha.org) reads from every vault at once, so the operations surface stays unified even when the work is distributed across a dozen agent panes.

Cross-product coordination runs through IPDC. It's the inter-pane communication tool that lets an orchestrator route work to per-product release agents without anything getting lost in chat scrollback. There's a [page on that](/methodology/ipdc/) if you want the protocol detail.

One human still approves every gate. That part doesn't scale by automation. It scales by the agents preparing better packets, so the approval can be quick when it comes. The methodology has settled into a rhythm where shipping is no longer the bottleneck. What we're building for now is coherence.
