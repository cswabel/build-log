# Vendor-file intake and consolidation

**Period:** Jun 2026 – Jul 2026 · **Status:** Consolidation tool delivered; intake service built

## Problem

Several of the team's processes start with a vendor emailing a file. In one of them, vendors fill in a template workbook for upcoming new releases, and someone then merges the returned workbooks by hand into per-market files and a quarterly tracking table. Beyond that one process, the team had no reusable way to move an emailed file into storage where a data job could pick it up.

## Approach

**Consolidation.** I replaced the manual merge with a deterministic tool. It reads the template workbooks the vendors send back, consolidates them into one file per market and builds the quarterly tracking table. An outlier guard flags suspicious quantities for a person to review. It never changes them.

**Intake.** Separately, I built reusable inbound routing for any project that starts with an emailed file. Incoming mail is validated, failing closed on anything unexpected, and the accepted messages land in S3. The router never opens attachment contents. The infrastructure is defined in AWS CDK and has an integration test suite. The original design documents for this service were lost partway through, so I wrote a reconstructed requirements document from what had been built and why.

## Key design decisions

- **Deterministic consolidation.** The same vendor files always produce the same output, so the result can be checked and rerun.
- **Flag, don't fix.** The outlier guard never modifies data. It points a person at the suspicious values.
- **Fail closed at the front door.** Mail that doesn't match what the intake expects is rejected instead of passed along.
- **Never parse untrusted attachments in the router.** Leaving attachment bytes unopened keeps the routing layer small and limits what a malicious file can reach.
- **Reusable, not one-off.** The intake was built as a general entry point for vendor-file projects, not tied to the first one.
- **Recover lost context deliberately.** When the original documents disappeared, I rebuilt the requirements explicitly instead of carrying on from memory.

## Tech

Python, openpyxl, Excel, AWS CDK, S3, integration tests.

## My role

I designed and built both the consolidation tool and the intake service, and wrote the reconstructed requirements.

## Status

- The consolidation tool delivered a full quarter's workbooks in June 2026.
- The intake service is built and tested. This log makes no claim that it is in production use.

## What I learned

Small, boring rules carry most of the safety in file-intake work: validate early, reject what you don't recognise, and keep the part that handles untrusted content as small as possible. On the consolidation side, a guard that only flags leaves the decision with the people who own the vendor relationship, instead of silently "correcting" a vendor's numbers. And if design documents can be lost, it's worth writing down the reasoning behind a build while it's still fresh.
