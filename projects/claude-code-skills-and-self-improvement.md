# Claude Code skills and self-improvement loop

**Period:** Jul 2026 – Oct 2026 · **Status:** In personal use

## Problem

Long agent sessions lose context when they are compacted or restarted. A model reviewing its own work misses things, and lessons from one session are gone by the next. I wanted my coding agent to recover cleanly, get independent review and improve over time, with me approving what it learns.

## Approach

I wrote a set of personal Claude Code skills:

- **`catch-up`** checkpoints and recovers working context across compaction and new sessions, and keeps a per-project mistakes log. Hooks run it when a session starts and before compaction.
- **`codex`** runs the OpenAI Codex CLI headlessly to review Claude's latest change or write tests for it. Claude then records whether it agrees or disagrees with each finding.
- **`review-with-claude`** works the other way round: Codex asks the local Claude Code CLI for a second opinion and checks the findings.
- **`handoff`** writes a per-project handoff note, updates a dated pointer in my project index and saves only durable facts to memory.
- **`improve`** is a nightly loop run by launchd. It finds where the agent went wrong in recent sessions (corrections, redirects, repeated failures, recurring blockers) and scores those sessions against my rules. It then proposes learnings, such as memory entries, mistake-log entries or rule and skill edits, into an approval inbox. Once a learning is applied, the loop tracks whether the same miss happens again.
- **A weekly write-up skill** drafts the week's work for an internal profile page in that page's existing style. It runs every Friday on a schedule and publishes only after I approve.

I also experimented with an attributed internal fork of an open-source, simplicity-first coding skill set. It's packaged as an off-by-default plugin with review, audit and technical-debt-ledger skills and hooks.

## Key design decisions

- **I approve what the agent learns.** The loop proposes and I decide.
- **Measure whether a lesson worked.** I track whether a miss recurs after its fix.
- **Independent review comes from a different model.** Its findings are triaged, not accepted wholesale.
- **Only durable facts go into memory.** Details that will be stale tomorrow are left out.
- **Draft, then publish.** Anything under my name stays a draft until I approve it.

## Tech

Claude Code (skills, hooks, plugins), Codex CLI, launchd, cron, Markdown.

## My role

I designed and wrote these skills, and packaged the fork.

## Status

In personal use. The context-recovery skill was published as a package in July 2026. The fork is an experiment.

## What I learned

The most useful improvements came from treating the agent's mistakes like incidents: record them, find the pattern and check that the fix held. Cross-model review works best on small, focused diffs. Requiring my approval for every learned rule stops the loop from drifting toward rules that sound sensible but are wrong.
