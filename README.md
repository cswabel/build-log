# Build log

I build AI agents, data automation and developer tooling for a business team that doesn't do engineering: the people who plan promotions, recover product availability and maintain curated title lists for the international books business of a large e-commerce company. Most of my projects sit between a process that used to run by hand in spreadsheets and the systems that hold the data. The pattern is usually the same: an agent talks to the user, deterministic code does the maths, and a person approves before anything touches production.

Every entry here is written fresh for this log. It covers what I built, why I built it that way and what I learned. It contains no code, data, configuration or internal system names from my employer.

## Themes

- **Agents orchestrate, scripts compute.** The language model collects inputs, explains results and runs the conversation. Tested, deterministic code does the selection, pricing, forecasting and ranking.
- **Propose-and-approve.** Before any production write, the agent shows the exact scope and a person confirms it. Approval is per stage and per run.
- **Governed, cited data.** Read-only agents answer only from published, dated outputs, and every answer says which data it came from.
- **Fail closed, leave a trail.** Bad input stops the run with a named reason. Audit logs are append-only, and a step only counts as a success when there is positive evidence of it.
- **Hermetic tests.** Tests run with no credentials and no network.
- **Tooling for the agents themselves.** MCP servers, coordination across several harnesses, personal skills and a nightly self-improvement loop.
- **Standards that suit the team.** Review rules and templates that people who are not full-time engineers can follow safely.

## Entries

| Entry | Summary | Period | Status |
| --- | --- | --- | --- |
| [Agent platform and delivery](projects/agent-platform-and-delivery.md) | One installable package that ships the team's agents, procedures, skills and deterministic code | Jun 2026 – Oct 2026 | Live, in active development |
| [Deal-planning automation agent](projects/deal-planning-agent.md) | A conversational agent over a deterministic calculation engine for promotion sourcing, readiness checks and ordering | Jun 2026 – Oct 2026 | Live, still evolving |
| [Inventory and availability recovery agent](projects/availability-recovery-agent.md) | A weekly triage and recovery workflow for products customers can't buy, with each production stage approved separately | Sep 2026 – Oct 2026 | Live, used weekly |
| [Governed-data Q&A agent](projects/governed-data-qa-agent.md) | A read-only agent that answers questions from a governed dataset and cites the data behind every answer | Jul 2026 – Sep 2026 | Live |
| [Weekly list-processing data job](projects/weekly-list-processing-data-job.md) | Turns a hand-consolidated monthly title list into a governed weekly dataset | Jun 2026 – Sep 2026 | Live |
| [Reporting and dashboards](projects/dashboards-and-data-feeds.md) | Deal-performance and selection reporting: dashboards first, later reproducible Excel outputs fed by scheduled data | Jun 2026 – Oct 2026 | Live; first dashboard superseded |
| [Price-compliance audit service](projects/price-compliance-audit-service.md) | Checks catalogue pricing flags against a regulator's public register, plus an independent re-check | Jul 2026 – Oct 2026 | Beta / prototype |
| [Multi-harness agent mesh](projects/multi-harness-mesh.md) | Connects Claude Code, Codex and Kiro so sessions can message each other, delegate work and share tooling | Oct 2026 | In personal use |
| [MCP servers for agents](projects/mcp-servers-for-agents.md) | MCP servers that let agents read internal reporting tools as the signed-in user | Aug 2026 – Sep 2026 | Merged; verified |
| [Engineering standards for non-engineering teams](projects/engineering-standards-for-non-engineering-teams.md) | A risk-tiered code-review standard and a shared team knowledge base | Jun 2026 – Sep 2026 | Live |
| [Vendor-file intake and consolidation](projects/vendor-file-intake-and-consolidation.md) | Deterministic consolidation of vendor workbooks and reusable intake of emailed files into S3 | Jun 2026 – Jul 2026 | Delivered / built |
| [Chat-based agent bridges](projects/chat-based-agent-bridges.md) | A team Slack assistant, a personal Slack-to-Claude-Code bridge and a scheduled personal agent daemon | Jul 2026 – Oct 2026 | Beta; in personal use |
| [Claude Code skills and self-improvement loop](projects/claude-code-skills-and-self-improvement.md) | Personal skills for context recovery, cross-model review, handoffs, nightly learning and weekly write-ups | Jul 2026 – Oct 2026 | In personal use |
| [Meeting transcription tool](projects/meeting-transcription-tool.md) | A local transcription workflow that writes resumable, incremental Markdown | Aug 2026 | Source merged |

See [CHANGELOG.md](CHANGELOG.md) for when entries were added.
