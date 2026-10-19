# Price-compliance audit service

**Period:** Jul 2026 – Oct 2026 · **Status:** Console in beta; independent review is a prototype

## Problem

In one of the business's marketplaces, a fixed-book-price law governs how certain books can be priced, and a regulator publishes a register of the titles it covers. The catalogue keeps its own fixed-price flags. Compliance and pricing work needed a systematic way to find where those flags disagree with the register, with enough evidence to act on.

## Approach

**The audit console.** It pulls the regulator's register through its official API and compares the catalogue against it. Its main job is to surface books that the catalogue flags as fixed-price but the register doesn't support. It keeps a full price history as evidence, and a scheduled export delivers the findings to pricing specialists. It runs on serverless AWS infrastructure defined in AWS CDK, with a Spark-based comparison step and a command-line tool.

An integration-test suite runs against the service.

**The independent review.** Later I rebuilt the fixed-price classification from scratch as a Spark review job, deliberately without reusing the earlier implementation, so that it could check the console rather than repeat it. Rows it can't classify with confidence stay visible, each with a reason. Standard prices are kept separate from promotional ones. The work also produced a data proposal to widen the register information available to the join.

## Key design decisions

- **Evidence first.** A compliance finding is only useful with its price history attached, so the console stores history, not just current state.
- **Independent re-implementation as verification.** A check that shares code with the thing it checks isn't independent.
- **Never hide uncertainty.** Ambiguous rows are reported with reasons instead of being forced into a category.
- **Separate price types.** Standard and promotional prices answer different compliance questions, so they never share a column.

## Tech

TypeScript, AWS CDK, AWS Lambda, AWS Fargate, Apache Spark (SQL), S3, a Python validation layer with tests, and a Java integration-test suite.

## My role

I designed and built the console, its infrastructure, the command-line tool and most of the integration tests. Later I designed and built the independent review job and its validation scripts.

## Status

- The audit console is in beta. Handing it over to downstream analytics and integrating it with the catalogue are separate pieces of work.
- The independent review is a prototype waiting for human verification. It has no production table yet.

## What I learned

When the question is "is this catalogue compliant?", a second implementation written without looking at the first is worth more than more tests on the first. Keeping uncertain rows visible, with their reasons, gives reviewers something to verify instead of a cleaner-looking result that hides edge cases. Separating standard from promotional prices from the start rules out a whole class of misleading comparisons.
