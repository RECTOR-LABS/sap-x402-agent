<!-- Satellite context file — extends the global hub (~/.claude/CLAUDE.md | ~/.pi/agent/AGENTS.md). Host-neutral; project-specific only. Do not duplicate hub standards here. -->

# sap-x402-agent

> Autonomous Solana agent built for the **OOBE × Ace Data Cloud** bounty. Discovers tools via Synapse Agent Protocol (SAP), executes AI tasks via Ace Data Cloud, and settles payments through x402 workflows end-to-end with no human in the loop.

**Status:** Work in progress — not yet implemented. See `STRATEGY.md` for the implementation plan.

## Bounty Context

- **Listing:** [Autonomous Agent Bounty: OOBE × Ace Data Cloud](https://superteam.fun/earn/listing/autonomous-agent-bounty-oobe-ace-data-cloud)
- **Sponsors:** [OOBE Protocol](https://www.oobeprotocol.ai/) × [Ace Data Cloud](https://platform.acedata.cloud)
- **Prize Pool:** 2,400 USDC across two categories
- **Deadline:** ~June 4, 2026 (winner announcement June 11, 2026)

## Structure

`src/` · `resources/` · `STRATEGY.md` (implementation plan) · `bounty-analysis.md` · `bounty-original.md` · `README.md`.

## Notes

- The flow: SAP tool discovery → Ace Data Cloud task execution → x402 payment settlement, fully autonomous.
- Pre-implementation — read `STRATEGY.md` before writing code.