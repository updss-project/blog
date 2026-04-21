---
title: "About UPDSS"
layout: "single"
url: "/about/"
summary: "What UPDSS is, why it exists, and what it means for AI-era software development."
ShowToc: false
cover:
  image: "/images/about-updss.jpg"
---

## The Problem Nobody Talks About

AI agents can write production code. Good code, often. Fast code, always. But here's what the demos don't show you: what happens after the code is written.

Who reviewed it? Was it scoped properly? Did the agent stay within bounds, or did it quietly rewrite a module nobody asked it to touch? Was there a budget, and did anyone track whether the agent blew past it? When three agents work on the same product simultaneously, who makes sure they aren't stepping on each other's commits?

These aren't hypothetical questions. They're the ones I hit every day running nine AI agents across six products from a terminal multiplexer.

## What UPDSS Actually Is

UPDSS (Universal Product Development Support System) is governance-as-code for AI-agent-driven software development. Think of it as the control plane that sits between your AI agents and your production environment.

It isn't a project management tool. It isn't a CI/CD pipeline. It isn't another AI coding assistant. It's the layer that ensures every line of agent-written code has traceability, passes through quality gates, and gets human approval before it ships.

The methodology combines Stage-Gate reviews (three gates, each requiring a human decision), Kanban flow (stories pulled when capacity exists, no sprints), and Shape Up appetite management (fixed budget, variable scope). Every story has a dollar cost. Every release has a budget. Every agent reports what it actually spent.

## Why It Works: The Recursive Property

Here's the part that matters most. UPDSS governs its own development.

The dashboard, the enforcement scripts, the gate tools, the pre-commit hooks: all built by AI agents operating under UPDSS governance. Every feature went through G1/G2/G3 gates. Every commit carries a story ID. Every release has a scope, a budget, and a retrospective.

This creates something you can't get any other way. When there's a bug in the governance layer, the agents building the governance hit it first. They report it, the fix ships in the next release, and the governance tightens. The vyavastha (the system's own structure) corrects itself through use.

## The Numbers

These are real, drawn from eight continuous weeks of operation:

- **50+ releases** shipped, from v0.1 through v0.11.9
- **~$500 total AI cost** across all agent sessions, all products
- **6 products** governed through a single platform (UPDSS itself, Omysha SignIn, Omysha SSP, Algo PropTrading, Marketing Tools, Process Modelling)
- **9 AI agents** directed by one human through a tmux terminal
- **Zero hard-rule violations.** No agent approved a gate, deployed without confirmation, or committed code outside story scope.

v0.11.8 budgeted $30. Spent $29.50. That kind of cost accuracy comes from mechanical enforcement, not good intentions.

## Open Source

UPDSS is being open-sourced. The methodology, the enforcement engine, and the governance architecture are available at [github.com/updss-project](https://github.com/updss-project).

If this resonates with problems you're facing, [read how the architecture works](/how-it-works/) or [support the project](/support/).
