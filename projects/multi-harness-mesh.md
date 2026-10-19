# Multi-harness agent mesh

**Period:** Oct 2026 · **Status:** In personal use; not released as a shared service

## Problem

I work with three coding-agent CLIs: Claude Code, Codex and Kiro. Each has its own MCP servers, skills, instructions and usage limits. Sessions in one couldn't see or talk to sessions in another, and I couldn't move work to whichever tool had room for it, or have one tool check another's output, without copying things between them by hand.

## Approach

I built a TypeScript/Node tool and an MCP server that connect the three harnesses:

- **Peers and messaging.** Any session can list its peers and send them messages.
- **Delegation.** A session can hand a task to another, and the work goes to whichever backend has the most headroom.
- **Shared tooling.** Each harness can read the others' instructions, skills and MCP configuration, so I don't have to keep three copies.
- **Scripted workflows.** An orchestrator splits a job across managers and workers, and a verifier checks the result. The verifier deliberately runs on a different harness from the workers.
- **Terminal UI.** A chat interface in the terminal shows what's happening, with a live session and a model picker.
- **Safety hooks.** Every harness on the mesh has hooks that block destructive commands.

## Key design decisions

- **Cross-model verification.** Running the verifier on a different harness from the workers gives an independent second opinion, not a model grading its own work.
- **Route by headroom.** Usage limits are a real constraint, so delegation goes to whichever backend has capacity, not to a fixed default.
- **Share configuration instead of duplicating it.** Exposing each harness's instructions and skills to the others keeps one source of truth.
- **Guardrails at the harness layer.** Destructive-command blocking is a hook every connected session gets, not something each prompt has to remember.
- **MCP as the interface.** Because the mesh is an MCP server, each harness uses it through the tool interface it already understands.

## Tech

TypeScript, Node.js, MCP, Claude Code, Codex CLI, Kiro CLI, Bedrock.

## My role

I designed and built it.

## Status

Installed in all three harnesses and in personal use. A code review for it is pending, and it isn't released as a shared service. My [personal agent daemon](chat-based-agent-bridges.md) uses it to run comparisons across harnesses.

## What I learned

Independent review by another model works best on small units, such as one module, one commit or one diff. Handing a reviewer a whole long session to review went badly. Sharing instructions, skills and server configuration across harnesses was as important as messaging: without it, each tool's setup drifts away from the others.
