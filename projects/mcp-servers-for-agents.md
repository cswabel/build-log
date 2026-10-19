# MCP servers for agents

**Period:** Aug 2026 – Sep 2026 · **Status:** Reporting server merged; sales server verified; team-kit contributions merged

## Problem

Agents working for the business team often needed a number from an internal reporting tool, such as a weekly business metric or a daily sales series. Otherwise the agent had to ask a person for an export. I wanted agents to check figures themselves, safely, and only with the access the person using them already has.

## Approach

I built MCP servers in front of two internal reporting tools and contributed to a third.

**Business-metrics reporting server.** This is a stdio MCP server in Go for the company's internal business-metrics reporting tool. It authenticates as the calling user through corporate sign-in and exposes many typed tools over an allowlisted set of endpoints. In September 2026 I widened its marketplace and product-category coverage and put bounds on response sizes.

**Daily sales server.** This MCP server reads the JSON endpoints behind an internal daily sales-reporting tool. I wrote it first in TypeScript, tested with vitest, then rewrote it in Go with no dependencies. It is pinned to a golden test fixture, which I reproduced against a spreadsheet export from the same tool. The Go version adds a breakdown by deal type, and it refuses to add up deal types that overlap.

**Team productivity kit.** I added extensions to an MCP server the team already maintained.

## Key design decisions

- **The caller's identity, not a service identity.** Each server sees only what the signed-in user could already see in the browser.
- **An allowlisted endpoint contract.** The server calls only a fixed, reviewed set of endpoints. It isn't a general proxy.
- **Bounded responses.** Large responses are capped so a single tool call can't flood an agent's context window.
- **Golden fixtures.** Server output is pinned to a reference result that was checked against the tool's own export.
- **Refuse invalid aggregations.** Where categories overlap, the server won't produce a misleading total.
- **No third-party dependencies in the Go rewrite.** There is less to install and less to audit.

## Tech

Go, TypeScript, Node.js, vitest, MCP (stdio).

## My role

I designed and built both servers and wrote the extensions to the team's existing server.

## Status

- The Go reporting server is merged and in use.
- The Go sales server is complete and verified against its fixture.

## What I learned

For agent tooling, refusing a question you can't answer correctly matters as much as answering the ones you can. Overlapping categories quietly double-count, and an agent will happily sum them unless the tool says no. Bounding response size matters for correctness too: an agent working from a truncated response doesn't know it.
