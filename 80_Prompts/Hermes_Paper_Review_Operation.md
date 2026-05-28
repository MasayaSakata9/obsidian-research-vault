# Hermes Paper Review Operation

Purpose: Hermes should no longer run broad hourly paper search as its main research job. Its main role is to read already selected papers, summarize them from multiple research perspectives, and have a separate verifier job check the summary.

Operating model:
- Reader job: 3 specialist perspectives in one LLM call.
- Verifier job: 2 verification perspectives in one LLM call.
- Curator job: no-agent script mirrors verified cron output into Obsidian.

Default cadence:
- Reader: every 3 hours at 00:07, 03:07, 06:07, 09:07, 12:07, 15:07, 18:07, 21:07 JST.
- Verifier: one hour after each reader run at 01:37, 04:37, 07:37, 10:37, 13:37, 16:37, 19:37, 22:37 JST.
- Curator: two hours after each reader run at 02:55, 05:55, 08:55, 11:55, 14:55, 17:55, 20:55, 23:55 JST.
- This gives up to 8 paper-review cycles per day.

Token policy:
- This is not five separate agents by default. The five roles are simulated across two calls to keep token use stable.
- If high-value papers accumulate, manually ask Codex to do deeper multi-agent reading.

Safety:
- Reader and verifier should not edit files or run shell commands.
- Curator is deterministic and no-agent; it only mirrors verifier outputs.
- Existing `approvals.cron_mode: deny` can remain unchanged.
