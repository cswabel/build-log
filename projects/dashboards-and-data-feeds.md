# Reporting and dashboards

**Period:** Jun 2026 – Oct 2026 · **Status:** Deal-performance reporting live; regional workbook live; first deals dashboard superseded

## Problem

The business team needed repeatable reporting in two areas: how promotions performed, and how selection was doing across a regional group of marketplaces. For the deal numbers, readers also needed to see which figures had been checked.

## Approach

**Deal performance**, in three versions:

1. A QuickSight dashboard. The two-week and year-over-year design ran into the service's dataset and refresh limits, so I documented the limits and changed approach.
2. Scheduled SQL extracts merged into a multi-sheet Excel workbook.
3. A deterministic, values-only Excel workbook built by the [deal-planning agent](deal-planning-agent.md). The workbook has a data dictionary and a validation status for every column. A separate live-data view is in review.

**Regional selection.** I built a consolidated multi-tab QuickSight dashboard that was marked as interim until a cross-border data source arrived. After that I moved this reporting to Excel, where Power Query and lookup formulas assemble the workbook from scheduled extracts.

## Key design decisions

- **Values-only workbooks.** No live queries, so a given file always shows the same numbers.
- **Validation status per column.** Readers can see which numbers have been checked.
- **Document platform limits.** The next design choice gets made on facts.
- **Keep live data separate** from the official report.

## Tech

QuickSight, SQL on Redshift, S3, Python (openpyxl), Excel Power Query, SharePoint.

## My role

I designed and built the reporting: specifications, SQL, workbook generators and dashboards.

## Status

- Deal-performance reporting has been live since late September 2026, and the metric set was agreed in October 2026. Final acceptance is open.
- The regional workbook went live in July 2026.
- The first deals dashboard has been superseded.

## What I learned

The right surface for recurring numbers depends as much on refresh and dataset limits as on the charts. Both threads ended up as reproducible Excel outputs fed by scheduled data, with each column marked as validated or not while its definitions were still being agreed.
