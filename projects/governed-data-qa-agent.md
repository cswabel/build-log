# Governed-data Q&A agent

**Period:** Jul 2026 – Sep 2026 · **Status:** Live

## Problem

The business team has recurring questions about a curated monthly list of titles. Which titles are on it in a given marketplace? How are they doing week to week on availability, cost, list price, traffic, purchase-order progress and content quality? Each answer has to be traceable to a specific, dated run of governed data.

## Approach

I built a read-only agent that answers these questions only from the governed output of the [weekly list-processing data job](weekly-list-processing-data-job.md).

**Prep script.** Every session starts by running a prep script. It switches to read-only credentials, finds the latest good partition of the dataset, works out how stale that partition is and downloads it into a local cache keyed by run date. It then reports the columns the data actually contains. Its key-value output is binding on the agent.

**Querying.** The agent answers questions with DuckDB over the cached files, using the interpreter the prep script names.

**Ordering handoff.** On request it also hands over the latest per-marketplace ordering workbook, with flags for items ordered before and for foreign-language titles, so a person can review them before anything is ordered.

An earlier version of the package also included a broader agent for the same list. It covered metrics, list consolidation, lookups and vendor outreach, and produced an ordering handoff that came with a separate safety-review file. It was retired when the team's agents were consolidated (see [Agent platform and delivery](agent-platform-and-delivery.md)).

## Key design decisions

- **Read-only against the data.** The agent never writes to the shared data store and never queries the source warehouse directly. It reads only the published output.
- **Cite every answer.** Each answer names the marketplace, the membership month and the run date it came from.
- **Report staleness.** If the latest data is older than expected, the agent says so instead of hiding it.
- **Deduplicate at the right grain.** Results are reduced to one row per item per marketplace before counting.
- **Refuse what the data can't support.** For example, it declines profitability questions because the published data has no matching selling price in the reporting currency.
- **Minimal tool surface.** The agent has no MCP servers and no skills. Its grounding documents, a data dictionary and an output schema, are loaded as context. A larger reference is read only when needed.

## Tech

Agent specification files, Bash, Python, DuckDB, S3, Markdown grounding documents.

## My role

I designed and built the agent, its prep script, its grounding documents and its tests.

## Status

Live and used by the business team. It is now part of the team's consolidated agent package.

## What I learned

Testing has two layers. Hermetic suites cover the prep script and the logic that matches answers to expected results, and they run on every build. A golden evaluation set runs the installed agent against real data, so it runs by hand. Its expected answers are pinned to one fixed data snapshot, with a written recipe for re-pinning, so normal weekly refreshes don't break the evaluations. The cheap deterministic checks stay in the build, and the expensive model-driven ones stay out.
