# Hermes Paper Review Operation

Purpose: Hermes should no longer run broad hourly paper search as its main research job. Its main role is to read already selected papers, summarize them from multiple research perspectives, and have a separate verifier job check the summary.

Operating model:
- Reader job: 3 specialist perspectives in one LLM call.
- Verifier job: 2 verification perspectives in one LLM call.
- Curator job: no-agent script mirrors verified cron output into Obsidian.

Default cadence:
- Reader: 00:07, 08:07, 16:07 JST.
- Verifier: 00:37, 08:37, 16:37 JST.
- Curator: 00:55, 08:55, 16:55 JST.

Token policy:
- This is not five separate agents by default. The five roles are simulated across two calls to keep token use stable.
- If high-value papers accumulate, manually ask Codex to do deeper multi-agent reading.

Safety:
- Reader and verifier should not edit files or run shell commands.
- Curator is deterministic and no-agent; it only mirrors verifier outputs.
- Existing `approvals.cron_mode: deny` can remain unchanged.
