# Engineering standards for non-engineering teams

**Period:** Jun 2026 – Sep 2026 · **Status:** Live; adoption tracked per package

## Problem

The team's agent packages are maintained by people who are not full-time engineers. They needed a review standard proportionate to the risk of each change, one they could actually follow. The team also needed a single shared place for its operating knowledge.

## Approach

**A code-review standard for agent packages.** It sorts changes into green, amber and red tiers by risk, and each tier gets a review process to match. The standard includes:

- A code-review template, so every change explains itself the same way.
- Per-language rule files for the automated AI code reviewer.
- An adoption template that a package can copy to opt in.

**A team knowledge base.** I consolidated the team's operating knowledge into a single shared wiki and had teammates review it. It came with a plain-language explainer on how resource ownership and ticket routing work inside the company. I also took part in a company AI-fluency programme alongside this work.

## Key design decisions

- **Risk tiers instead of one rule for everything.** A wording change to an agent's instructions and a change to code that writes to production need different levels of scrutiny. Tiers make the difference explicit.
- **Separate what blocks from what advises.** Objective checks block a change; automated AI review comments are advisory. Mixing the two would either block good changes on a noisy comment or teach people to ignore the checks.
- **Adoption is opt-in and tracked.** Each package adopts the standard deliberately, using the template, and adoption is tracked package by package instead of being declared all at once.
- **Templates over prose.** People who aren't engineers follow a filled-in template more reliably than a policy document.
- **Write knowledge down once.** The wiki puts each operating fact in one place, with others pointing to it.

## Tech

Markdown, YAML rule files for an automated code reviewer, code-review templates, an internal wiki.

## My role

I wrote the review standard, its rule files and templates, and set up adoption tracking. I wrote and consolidated the team wiki and the explainer, with review from teammates.

## Status

The review standard is live and adoption is tracked per package. The team wiki was shared in June 2026.

## What I learned

I designed the standard on the view that people who don't do engineering full time follow standards that are proportionate and concrete: a few risk tiers, each with a template, instead of a general instruction to "review carefully". Keeping AI review advisory is meant to keep the blocking checks trustworthy. I also kept a principle from my own documentation habits: one fact, one place, referenced from everywhere else.
