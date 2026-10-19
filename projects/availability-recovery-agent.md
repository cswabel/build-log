# Inventory and availability recovery agent

**Period:** Sep 2026 – Oct 2026 · **Status:** Live, used weekly

## Problem

Every week some catalogue items across several marketplaces can't be bought by customers. Recovering these items is a recurring workflow that touches several production systems, so every step has to be safe and auditable.

## Approach

I packaged the team's existing workflow into an agent and made it robust.

**Triage.** The agent reads the weekly extracts and sorts every item into one that can be acted on and one that needs escalating to a person.

**Production stages.** Four recovery stages follow, each proposed by the agent and approved separately by a person before it writes anything.

## Key design decisions

- **Fresh confirmation for every write.** Before each live stage, the agent shows exactly what it intends to change, and the approval covers only that stage in that run.
- **Success needs positive evidence.** An accepted request alone doesn't count as success.
- **No premature claims.** An item counts as recovered only when later data shows it.
- **Confirmed failures only.** Transient errors are retried before anything is recorded as a real failure.
- **Append-only audit.** Every attempt is logged, and long runs can resume safely.

## Tech

Python, Playwright, requests, pytest with branch coverage, Kiro CLI.

## My role

I packaged and hardened the workflow into this agent, including its triage and stage runners and its test suite.

## Status

Live in the team's shared agent package and used weekly.

## What I learned

The obvious diagnosis for a failing run was wrong, so I learned to separate transient errors from real failures before trusting either. For a production write, "the request was accepted" and "the change happened" are different claims, and the audit log should record only the second.
