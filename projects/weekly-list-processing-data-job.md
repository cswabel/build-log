# Weekly list-processing data job

**Period:** Jun 2026 – Sep 2026 · **Status:** Live since Aug 2026

## Problem

A curated monthly list of titles across several international marketplaces was consolidated by hand each month and wasn't connected to weekly performance metrics, so there was no governed weekly dataset for reporting or for agents to read.

## Approach

I started with a local consolidation tool that ran on a business user's Windows laptop, with no cloud services and no command line, and then built a scheduled weekly job:

1. **Input.** A business owner provides the monthly consolidated list, and a small converter turns the spreadsheet into a fixed input format.
2. **Weekly job.** A scheduled SQL job on Redshift validates the input, resolves book identifiers to catalogue products and joins the weekly metrics.
3. **Output.** Each run publishes a dated, governed dataset, including the rows that failed validation.

Its output feeds weekly reporting and the [governed-data Q&A agent](governed-data-qa-agent.md).

## Key design decisions

- **Keep it simple.** One job, with no extra stages unless they prove necessary.
- **Rejects are a first-class output.** A row that fails validation is kept and reported instead of being dropped.
- **Dated runs.** Consumers can tell which run they are reading and how old it is.
- **A written output contract.** A schema and a data dictionary define the metrics, and the agent's grounding documents follow them.

## Tech

SQL on Redshift, S3, Python (openpyxl) for the converter, golden evaluations.

## My role

I wrote the specification, the weekly job, the converter and the evaluations, and I built the earlier local tool.

## Status

Live since August 2026. A few metric definitions are still waiting for business sign-off.

## What I learned

Identifier resolution is where data quietly goes wrong: one book identifier can map to several catalogue products, and a join on the wrong key can drop records without any error. When a data source goes away, the honest move is to mark the affected field as unavailable until a replacement is agreed, not to fill it with something plausible.
