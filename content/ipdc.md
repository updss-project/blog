---
title: "IPDC: Inter-Pane Direct Communication"
layout: "single"
url: "/methodology/ipdc/"
summary: "How the agents working on ten different products talk to each other without a human in the middle. The protocol, the tripwire, the audit log, and why it mattered."
ShowToc: true
cover:
  image: "/images/ipdc.jpg"
---

A few weeks ago I had a small problem with how I work.

I had eight AI assistants running on my computer at the same time, each one helping me with a different project. Some of those projects talked to each other, which meant the assistants needed to talk to each other too. They couldn't. When one assistant needed to tell another something, I had to walk the message across myself. Click on the first assistant. Read what it wanted to say. Click on the second one. Type the message in. Wait for the reply. Walk it back.

By the next evening, this was solved.

In one day, my AI assistants and I built a tiny program called pane-msg. It lets them send messages to each other directly, without me in the middle. By the end of the day, nine windows on my screen were talking to each other. I was free to do other things.

What surprised me more than the program was the absorption. The pane-msg tool took one day to build. Then a different AI assistant added it into the rest of how I work, and that second step took six minutes and thirty-five seconds. From "here is a new thing" to "the new thing is part of how we work," in under seven minutes. The methodology already had clear rules for how new things get added. The new tool just fit into them.

Good governance is what makes fast change possible. That's the larger thing this page is about. The smaller thing is the protocol itself, which is what the rest of this page documents.

---

## What pane-msg is

Pane-msg is a one-file bash + embedded python program with two real dependencies, `python3` and `tmux`. It sends messages between tmux panes that have been labeled with `tmux select-pane -T <label>`. The aliases are `pane-message`, `ipdc`, and `iptc`. They all point at the same script.

The CLI shape is small enough to fit on one screen.

```
pane-msg <target-label> "<message>"           # single-line message, wraps in envelope
pane-msg <target-label> -                     # multi-line message from stdin
pane-msg <target-label> --ack "<status>"      # ACK back (no envelope)
pane-msg <target-label> --done "<summary>"    # DONE back (no envelope)
pane-msg --list                               # list reachable pane labels
```

Target resolution is fuzzy. Five tiers, from exact match through normalized match, substring match, Jaccard similarity, and finally SequenceMatcher. Callers can use approximate names, which matters when label conventions drift mid-session.

## The protocol, baked into the envelope

Every outbound message wraps the body in an envelope that tells the receiver exactly how to respond. The envelope is verbose on purpose. It's there to make the protocol unmissable even for an agent that's never seen pane-msg before.

```
=== PANE-MSG INCOMING ===
From:  <sender-label>
To:    <target-label>
Time:  <timestamp IST>
Topic: <subject>

PROTOCOL — receiver, please do these in order:

  1. ACK IMMEDIATELY (within seconds), INCLUDING your current status:
       pane-msg <sender> --ack "<status>"
     Status examples (be specific):
       "received, idle, starting now"
       "received, busy: <what you are doing> (~Nm), will queue this"
       "received, blocked on <thing>, will handle when unblocked"

  2. DO NOT interrupt your current work.

  3. When you reach this work, act on the message body below.

  4. WHEN DONE, send one line:
       pane-msg <sender> --done "<one-sentence summary of what you did>"
```

The status field in the ACK is what makes pane-msg safe for fire-and-forget. The sender learns immediately whether to expect action now or to wait. An ACK that says "received, busy: drafting v0.3 release notes (~15m), will queue this" is more useful than an ACK that just says "received," because the sender can plan the next message accordingly.

ACK and DONE responses intentionally skip the envelope-wrapping. If they wrapped, the receiver would have to ACK the ACK, and you'd get an infinite handshake loop.

## The anti-hallucination tripwire

On the second day of pane-msg's existence, one of the agent panes hallucinated a send. It claimed in its own log that it had pinged another pane and routed a verdict. The audit log proved zero invocations. Nothing had been sent.

The fix was small. Every real send now prints a single line to stderr in the form `pane-msg: <mode> -> <target> (%N)`. If the agent doesn't see that line in its tool output, the message didn't go out. Period.

The discipline this enforces is simple. Before any agent claims it sent a pane-msg, it must verify the stderr tripwire appeared in its own tool output. The tripwire is the load-bearing proof that the bash tool was invoked. Without it, the claim of a send can't be verified.

```
$ pane-msg AT-V0.3-code "test"
pane-msg: msg -> AT-V0.3-code (%4)
```

If you see the tripwire, the message went. If you don't, it didn't, regardless of what the agent's transcript says.

## The audit log

Append-only JSONL at `~/.local/share/pane-msg/log.jsonl`. Every send, every ACK, every DONE gets one line. Each line carries `ts`, `mode`, `sender`, `target_label`, `pane_id`, and the message body.

This is where the tripwire derives its authority. The log is the source of truth for "did this message go." If the agent's claim and the log disagree, the log wins.

Pretty-printing is one command.

```
pane-msg-watch                                # tail -F + pretty print
```

The log lives user-local, not in source control. Each operator's machine carries its own audit trail.

## pane-msg versus vault `_Inbox/`

These are deliberately separate concerns, not a primary-and-fallback pair.

|  | pane-msg | vault `_Inbox/` memo |
|---|---|---|
| Time horizon | seconds to minutes | hours to days |
| Audit trail | JSONL log (user-local) | git history (durable) |
| Body length | short, one envelope | long-form prose |
| Receiver state | live tmux pane | next time the agent reads inbox |
| Typical use | "ack: I'm busy, will queue" | "absorbed: filed as F-V0149-XYZ" |

The choice between them is owner-directed per matter, not auto-routed. A real-time coordination ping goes through pane-msg. A long-form methodology change request goes through the vault inbox.

## What this enabled: the 2026-06-22 cascade

The empirical case for pane-msg landed the day after it was built. On 2026-06-22, four separate incident classes were closed in a single day. The work was distributed across multiple agent panes, each working on a different product, coordinated through pane-msg with no human routing in the middle.

That's the thing the harness is for. Without pane-msg, that same coordination would have routed through me, walking each message between panes by hand. It would have taken a week.

A later refinement (STORY-005-04-95) made the delivery itself more reliable. Long envelopes were occasionally sitting in the receiver's input buffer unsubmitted because the post-paste sleep was too short for the receiving terminal to ingest before the Enter keystroke arrived. The fix was payload-adaptive sleep plus a double-Enter. Quiet infrastructure, the kind that doesn't get noticed once it works.

## What this is part of

Pane-msg sits inside a methodology that's already there, the one this whole site documents. The reason a one-day tool could be absorbed in seven minutes is the methodology had a place ready for it. Stories, gates, retrospectives, registry. A new tool that fits the pattern doesn't need governance scaffolding built around it. The scaffolding is already there.

The version of this argument I keep coming back to is the simpler one. When the rules are clear, a new tool fits into them in minutes. When the rules are missing, even a small change takes weeks. Pane-msg is the small example. The harness is the larger point.

For background on how it shipped, see PR #449 in [omyshafoundation/a4g_updss](https://github.com/omyshafoundation/a4g_updss/pull/449) (STORY-005-04-93 in the v0.14.9 release). For protocol reference, see the README at `product/code/tools/pane-msg/README.md` in the same repo.
