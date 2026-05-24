# Implementation Strategy — sap-x402-agent

> Status: **v0.1 — draft, pending decisions in §1**. Will harden once category and agent concept are locked.
> Created: 2026-05-24

---

## 0. Snapshot

| Item | Value |
|------|-------|
| Bounty deadline | ~2026-06-04 (10 days from today) |
| Winner announcement | 2026-06-11 |
| Recommended category | **Category 2 — Ace Data Cloud Usage** |
| Repo | `RECTOR-LABS/sap-x402-agent` |
| Working branch | `dev` |

---

## 1. Decisions Pending

These must be locked before implementation:

- [ ] **Category** — recommend Cat 2 (lower capital risk, free credits = free volume)
- [ ] **Agent concept** — pick from §3 or propose alternative
- [ ] **Public distribution channel** — Telegram bot, X bot, webapp, or CLI? (drives multi-tenancy potential)
- [ ] **Programming language / runtime** — TypeScript (matches SAP SDK ecosystem) vs Rust (closer to Solana mainnet)
- [ ] **Budget ceiling for paid volume top-ups** if free credits run out

Track answers in PR / commit description when locked.

---

## 2. Approach (assumes Cat 2 — Ace Data Cloud Usage)

### 2.1 Core philosophy

Build an agent that solves a **real problem for real users** so that the resulting on-chain volume is unambiguously legitimate. Bounty's anti-gaming clause penalizes synthetic loops; organic multi-user traffic is the safest defense.

### 2.2 Architectural shape

```
                                  ┌─────────────────────────┐
                                  │   User / scheduler /    │
                                  │       webhook trigger   │
                                  └────────────┬────────────┘
                                               │
                                               ▼
┌──────────────────────────────┐    ┌─────────────────────────┐
│  SAP mainnet registration    │◄───┤  Agent runtime (Node)   │
│  (agent identity + metadata) │    └────────────┬────────────┘
└──────────────────────────────┘                 │
                                                 │ 1. tool discovery
                                                 ▼
                                  ┌─────────────────────────┐
                                  │  Synapse Client SDK     │
                                  │  (SAP query + RPC)      │
                                  └────────────┬────────────┘
                                               │
                                               │ 2. select 3+ ADC services
                                               ▼
                                  ┌─────────────────────────┐
                                  │  Ace Data Cloud APIs    │
                                  │  (search, chat, image,  │
                                  │   video, etc.)          │
                                  └────────────┬────────────┘
                                               │
                                               │ 3. settle per call
                                               ▼
                                  ┌─────────────────────────┐
                                  │  x402 (ADC facilitator) │
                                  │  Synapse RPC submit     │
                                  └────────────┬────────────┘
                                               │
                                               ▼
                                  ┌─────────────────────────┐
                                  │  Output → user channel  │
                                  │  + on-chain log         │
                                  └─────────────────────────┘
```

---

## 3. Candidate Agent Concepts (pick one)

Each candidate naturally consumes 3+ distinct ADC services. Pick based on which has the most plausible user demand and matches RECTOR's interests.

### A) Solana DeFi Daily Digest Agent (recommended starting point)
- **Audience:** Solana DeFi participants (large, already in our network)
- **Trigger:** Cron (daily) + on-demand via Telegram command
- **ADC services used:**
  1. Web search — pull top news mentions
  2. Chat/LLM — summarize and categorize
  3. Image gen — hero image for digest
  4. (Optional) Translation — for multi-language digest
- **Volume driver:** Each digest run = N user-targeted runs × M services. Multi-tenant naturally scales.
- **Why it wins:** Real audience, real opens/clicks for sponsor verification, runs on a schedule = consistent volume curve.

### B) Bounty Hunter Brief Agent
- **Audience:** Superteam bounty hunters (meta-pitch — sponsor will smile)
- **Trigger:** New bounty posted on Superteam Earn (poll feed)
- **ADC services used:** Search, LLM analysis, image gen, optional translation
- **Volume driver:** ~5–10 new bounties/day × N subscribers
- **Risk:** Smaller audience than DeFi digest; novelty bonus but volume ceiling lower.

### C) Solana Wallet Analyzer Agent
- **Audience:** Solana traders / portfolio holders
- **Trigger:** User-supplied wallet address (Telegram or X mention)
- **ADC services used:** Search (token data), chat/LLM (analysis), image gen (allocation chart), optional video for tutorial
- **Volume driver:** Pull-based; volume depends on user adoption velocity
- **Risk:** Lower baseline volume unless heavily promoted

### D) Solana Project Pitch-Deck Generator
- **Audience:** Solana founders applying to hackathons / grants
- **Trigger:** User submits idea via web form
- **ADC services used:** Search (market research), chat/LLM (deck content), image gen (slide visuals), video gen (highlight reel)
- **Volume driver:** Per-deck = 4–10 ADC calls. Volume depends on conversion.

---

## 4. Day-by-Day Timeline (10 days)

> Assumes Cat 2 + Concept A. Adjust if other choices land.

### Day 0 (today, 2026-05-24)
- Lock decisions in §1 with RECTOR
- Reach out to sponsor (Telegram) for clarifications listed in `bounty-analysis.md` §2
- Create ADC account → collect free credits
- Read SAP Docs (`explorer.oobeprotocol.ai/docs`) cover to cover

### Day 1 (2026-05-25)
- Bootstrap Node/TS project structure in `src/`
- Install: `@oobe/synapse-client-sdk`, `@oobe/synapse-sap-sdk`, `@acedata/x402-client` (verify exact package names)
- Generate Solana keypair, fund with ~0.05 SOL for tx fees
- Register agent on SAP mainnet → record agent address in `STRATEGY.md`
- Verify presence on Synapse Explorer

### Day 2 (2026-05-26)
- Implement ADC client wrappers (3+ services: search, chat, image at minimum)
- Wire x402 payment flow for one service call end-to-end → see USDC settle on-chain
- Capture tx signatures for verification

### Day 3 (2026-05-27)
- Implement agent orchestration loop:
  - Tool discovery via SAP
  - Service selection logic
  - Sequenced execution
  - Per-call x402 settlement
- End-to-end run from CLI → first complete autonomous workflow

### Day 4 (2026-05-28)
- Add trigger surface:
  - Cron job runner (e.g., node-cron or a hosted scheduler)
  - Telegram bot interface (chosen channel for multi-tenancy)
- First scheduled run lands

### Day 5 (2026-05-29)
- Multi-tenancy: per-user state, per-user output
- Logging: tx signature, services called, timestamps → exposed via public dashboard endpoint
- Launch private beta with 5–10 testers (RECTOR's network)

### Day 6 (2026-05-30)
- Public soft launch: announce on RECTOR's X account, invite to Telegram
- Monitor: volume curve, errors, abuse attempts
- Fix bugs / hot patches

### Day 7 (2026-05-31)
- Continue running, drive sign-ups
- Add Synapse Sentinel call if pursuing Cat 1 as backup (skip if Cat 2 only)
- Refine logs for sponsor review readability

### Day 8 (2026-06-01)
- Record demo video walking through:
  1. Discovery via SAP
  2. ADC services consumed (and why each)
  3. End-to-end execution
  4. Payment settlement
  5. The autonomous aspect — no manual steps
- Edit + post unlisted on YouTube

### Day 9 (2026-06-02)
- Draft X post for submission
- Final code cleanup, repo README polish
- Pre-flight checks: agent running cleanly, dashboard accessible, video accessible
- **Submit via Superteam Earn (1 credit cost)**

### Day 10 (2026-06-03)
- Buffer day: handle last-minute issues, late volume push
- Final submission lockdown before deadline

### Days 11–17
- Agent continues running (volume accrues until sponsor cutoff)
- Sponsor review window (announcement 2026-06-11)

---

## 5. Deliverables Checklist

Submission requires:

- [ ] Agent registered and visible on SAP mainnet via Synapse Explorer
- [ ] ADC account active with consumption history visible
- [ ] At least 3 distinct ADC services called repeatedly
- [ ] Complete autonomous workflow demonstrable (trigger → execution → payment, no manual steps)
- [ ] Public GitHub repo: `RECTOR-LABS/sap-x402-agent`
- [ ] README explains the agent purpose, architecture, setup
- [ ] Demo video (≤5 min) covering all 5 walkthrough points
- [ ] Public X post tagging @OOBEonSol and @AceDataCloud, declaring **Category 2**, linking GitHub + video
- [ ] Submitted via [Superteam Earn listing](https://superteam.fun/earn/listing/autonomous-agent-bounty-oobe-ace-data-cloud) (1 credit)

---

## 6. Submission Plan

1. **Post on X** from `@rectorspace` (or chosen handle):
   ```
   Excited to ship @sap_x402_agent for the @OOBEonSol × @AceDataCloud autonomous agent bounty 🚀

   It's an autonomous on-chain agent on Solana that [one-liner].

   Category: Ace Data Cloud Usage (x402 Facilitator)

   📺 Demo: [YouTube link]
   💻 Code: github.com/RECTOR-LABS/sap-x402-agent
   🧠 Agent on SAP: explorer.oobeprotocol.ai/agents/[address]

   #Solana #x402 #OOBE
   ```
2. **Capture X post URL**
3. **Submit on Superteam Earn**: paste link, brief description, deliverables
4. **Confirm submission visible in listing** before deadline

---

## 7. Post-Submission

- Keep agent running through 2026-06-11 (winner announcement) — judges may continue measuring
- Document final volume numbers and tx counts in this file
- If we win → withdraw USDC, archive repo
- If we don't win → repo becomes foundation for SIPHER's autonomous-agent layer or future bounty submissions

---

## 8. Open Questions Log

Drop new questions here as they emerge during implementation.

- (none yet)
