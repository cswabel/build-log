# Agent platform and delivery

**Period:** Jun 2026 – Oct 2026 · **Status:** Live, in active development

## Problem

The team's agents had started out as separately published packages. The people who use and maintain them are not full-time engineers, and they needed one thing to install, one place to look and one set of rules that every agent follows.

## Approach

I consolidated the agents into a single package. Each agent ships with its step-by-step operating procedures, its always-loaded context documents, its skills, and the deterministic Python that those skills call. In September 2026 this one package replaced the separate ones. Team members can run it in Kiro CLI or as a Claude Code plugin.

The package includes two procedures for keeping it healthy:

- **Bug reporting.** The agent interviews the reporter one question at a time and separates what was observed from what was inferred. It checks for duplicates and files the exact ticket it showed only after the reporter confirms.
- **Maintainer sync.** When the business team posts a change to a specification, a maintainer can turn it into a reviewed update to the package.

I also wrote a bootstrap configuration that sets up a fresh cloud development environment as the team's standard Claude Code installation. It contains no credentials.

## Key design decisions

- **The agent orchestrates and deterministic scripts compute.**
- **Every production write is propose-and-approve.** The agent shows the exact scope and a person confirms immediately before the write happens.
- **Hermetic tests.** They need no credentials or network.
- **Guardrails are not bugs.** The bug procedure checks whether a report describes a real defect or an expected guardrail firing. For a guardrail, it explains the rule instead of filing a ticket.

## Tech

Python (pandas, openpyxl, DuckDB, Playwright), pytest with coverage, Markdown procedures and context, agent specification files, Kiro CLI, Claude Code plugins, MCP, S3.

## My role

I designed and carried out the consolidation. I'm the author and maintainer of the package and of most of the agents and skills in it.

## Status

Live and installable by the team, and still in active development.

## What I learned

I kept an incident ledger while building this, and a few lessons from it held up:

- Assert the reason for an outcome, not only its status, because two different failures can return the same exit code.
- After fixing a defect, look for the same pattern elsewhere instead of patching one place.
- Define a list once and reuse it. Two guards that each keep their own copy of the same list can look like they confirm each other when they don't.
- Check a security posture against what is actually installed, not only against what was built.
