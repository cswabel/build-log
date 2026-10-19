# Chat-based agent bridges

**Period:** Jul 2026 – Oct 2026 · **Status:** Team assistant in beta; personal bridge and daemon in personal use

## Problem

The team works in Slack, so agents are most useful there. I built three Slack-connected systems: a shared team assistant and two personal tools.

## Approach

**Team Slack assistant.** Someone mentions the assistant in Slack, and the request is handed to a serverless AWS backend that runs Claude Code on Bedrock with no tools. Anything the assistant needs to read is fetched beforehand, and it stores nothing.

**Personal Slack-to-Claude-Code bridge.** This bridge, which only I can use, lets me drive Claude Code on my workstation from Slack on my phone. It keeps one persistent Claude Code session per Slack thread, streams output live, and has pause, resume and stop controls.

**Personal agent daemon.** This Python daemon uses only the standard library. It turns messages in my Slack self-DM into headless Claude Code sessions and runs scheduled jobs such as a morning note, draft replies that are sent only after I approve them, tracker and write-up drafts, and a self-improvement loop. Approval rules and an audit log are enforced in code.

## Key design decisions

- **No tools for the shared assistant.** Reads are fetched up front, so the model can't decide to fetch anything else.
- **Stateless.** The shared assistant keeps nothing after it replies.
- **Approval rules in code, not prompts.** In the daemon, for example, a draft reply can't be sent until I approve it.

## Tech

AWS CDK, AWS Lambda, SQS, AWS Fargate, Bedrock, Slack (including Socket Mode), Python, launchd, Claude Code, Codex CLI, Kiro CLI.

## My role

I built the team assistant and the bridge, and built the daemon by adapting and extending an existing personal-agent design.

## Status

The team assistant is in beta, with later capabilities gated. The bridge is a working personal tool. The daemon is in personal use and still in progress as of October 2026.

## What I learned

For a shared assistant, taking away the model's tools and pre-fetching what it needs keeps its behaviour and permissions simple. For personal automation, approval belongs in the program rather than in the prompt: code that won't send without approval can be relied on in a way an instruction can't.
