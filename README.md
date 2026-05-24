# sap-x402-agent

Autonomous Solana agent built for the **OOBE × Ace Data Cloud** bounty. Discovers tools via Synapse Agent Protocol (SAP), executes AI tasks via Ace Data Cloud, and settles payments through x402 workflows end-to-end with no human in the loop.

> **Status:** Work in progress — not yet implemented. See `STRATEGY.md` for the implementation plan.

## Bounty Context

- **Listing:** [Autonomous Agent Bounty: OOBE × Ace Data Cloud](https://superteam.fun/earn/listing/autonomous-agent-bounty-oobe-ace-data-cloud)
- **Sponsors:** [OOBE Protocol](https://www.oobeprotocol.ai/) × [Ace Data Cloud](https://platform.acedata.cloud)
- **Prize Pool:** 2,400 USDC across two categories
- **Deadline:** ~June 4, 2026 (winner announcement June 11, 2026)

Full bounty text preserved in [`bounty-original.md`](./bounty-original.md). Analysis and competitive read in [`bounty-analysis.md`](./bounty-analysis.md). Implementation plan in [`STRATEGY.md`](./STRATEGY.md).

## What the Agent Does

The agent runs a complete autonomous workflow:

1. **Discover** tools and AI services via SAP
2. **Execute** real tasks using at least 3 distinct Ace Data Cloud services
3. **Settle** payments via x402 (Ace Data Cloud facilitator) on Solana mainnet
4. **Loop** — triggered by schedule / event / user input, no manual intervention

## Stack

| Layer | Tech |
|-------|------|
| Agent registration & coordination | [SAP SDK](https://github.com/OOBE-PROTOCOL/synapse-sap-sdk) |
| Execution, RPC, tooling | [Synapse Client SDK](https://github.com/OOBE-PROTOCOL/synapse-client-sdk) |
| AI services | [Ace Data Cloud APIs](https://platform.acedata.cloud) |
| Payment settlement | [x402 Client SDK](https://github.com/AceDataCloud/X402Client) |
| Network | Solana mainnet |

## Repository Layout

```
.
├── README.md              # this file
├── bounty-original.md     # verbatim bounty content (reference)
├── bounty-analysis.md     # competitive analysis + risk assessment
├── STRATEGY.md            # implementation plan + timeline
├── resources/             # reference docs, links, notes
└── src/                   # implementation
```

## Getting Started

Setup instructions will be added once the implementation begins. Track progress on the `dev` branch.

## License

TBD — likely MIT once initial implementation lands.
