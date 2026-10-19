# Deal-planning automation agent

**Period:** Jun 2026 – Oct 2026 · **Status:** Live, still evolving

## Problem

Planning a promotion across several international marketplaces was a manual weekly routine run largely in spreadsheets. It covered choosing titles, pricing, economics, forecasting, ordering and upload files, then readiness checks before go-live.

## Approach

I built a conversational agent on top of a deterministic calculation engine.

**Inputs.** The agent collects what a deal event needs: dates, discount and profitability floors, a revenue goal, the funding source, the promotion name, and per-vendor budgets for vendor-funded events. It applies documented per-market defaults only for the discount inputs, tells the user which defaults it used and asks for everything else instead of guessing.

**Calculation engine.** The agent calls a fixed sequence of steps: selection, uplift, elasticity, pricing, profitability, forecasting, funding and ranking. Each step's formulas are written down in reference documents.

**Review.** The agent presents a ranked list and goes through an add/remove review with the user. It produces draft upload workbooks and an HTML report. Only after the user confirms does it write the final files and append the event to an append-only deals record in S3.

**After sourcing.** Readiness checks run from scheduling to go-live and cover selection, pricing rules, availability, profitability and the risk of listings being suppressed. When there is a demand gap, the agent produces ordering and forecast-override workbooks for the shortfall only., and a per-event ordering log prevents double orders. The agent also generates the daily deal-performance workbook (see [Reporting and dashboards](dashboards-and-data-feeds.md)).

Before this, I built a local "one query, one click" Python app for the same routine and worked on a prototype fork of an internal deal-planning web tool. Both stayed prototypes.

## Key design decisions

- **The model never does the deal maths.** It runs the conversation, and the engine does the calculations.
- **Funding source changes the logic.** Company-funded and vendor-funded events take different paths, and a vendor-funded run won't start without budgets.
- **Every commercial action is propose-and-approve.** The agent never changes a live price, removes a title, places an order or submits to a platform on its own.
- **Undefined rules fail loudly.** A market without defined suppression rules fails its readiness check. It doesn't borrow another market's rules.
- **No way around a legal price floor.** Each pricing proposal is tagged with whether it can be applied, and a legal floor can't be overridden.

## Tech

Python, pandas, openpyxl, pytest with coverage over synthetic fixtures, HTML reports, S3, SQL feeds from the data warehouse, Kiro and Claude Code.

## My role

I designed and built the agent and the engine and kept them aligned with the business team's specifications.

## Status

Live and in use. Reporting was added in September 2026 and readiness snapshots in October 2026. Final acceptance of the report is still open.

## What I learned

With the maths outside the model, the same inputs always give the same list, so people can check it and challenge it. Saying which defaults were applied lets users correct an assumption before it reaches a deal.
