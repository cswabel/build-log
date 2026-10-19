# Meeting transcription tool

**Period:** Aug 2026 · **Status:** Source merged; runtime and adoption not yet verified

## Problem

Recordings are only useful once they're turned into text that people and agents can search and build on. Long recordings take a while to process, and a job that fails partway shouldn't lose what it has already transcribed. Some recordings also live on SharePoint, not on the machine doing the work.

## Approach

I packaged a local recording-transcription workflow. It accepts local recordings and recordings stored on SharePoint, runs speech transcription on the CPU and writes its output as Markdown. It writes incrementally, adding text as it goes instead of producing one file at the end, so an interrupted run can resume where it stopped. Alongside the readable transcript, it produces machine-readable contracts, so other tools and agents can rely on a known structure instead of parsing free text. It does not try to identify individual speakers. It replaced an earlier version of the workflow, which has been archived.

## Key design decisions

- **Local processing.** Transcription runs on the machine itself, on the CPU, with no need for specialised hardware.
- **Incremental and resumable output.** Writing as it goes means a long job never has to start again from nothing.
- **Markdown as the main output.** People can read it, and it diffs cleanly.
- **Machine-readable contracts alongside the text.** Downstream agents can rely on a stable structure instead of parsing free text.
- **Explicit scope.** Speaker identification is left out deliberately and stated as a limitation, not attempted badly.
- **Archive, don't delete, the predecessor.** The earlier version stays available for reference.

## Tech

CPU-based speech transcription, Markdown output with machine-readable contracts, SharePoint as an input source.

## My role

I designed and packaged the workflow and replaced its predecessor.

## Status

The source was merged in August 2026. I haven't yet confirmed how it runs in practice or how widely it's used, so I make no claim about either.

## What I learned

For long-running batch jobs that people run on their own machines, being able to resume matters more than raw speed. An interrupted run that picks up where it left off is a minor inconvenience, while one that starts over gets abandoned. Producing structured output alongside human-readable text costs little at design time and makes the tool useful to agents as well as people. Stating what the tool does not do, in this case identifying speakers, sets expectations better than a partial feature would.
