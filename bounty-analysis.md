# Bounty Analysis — OOBE × Ace Data Cloud

> Deep analysis for decision-making and execution planning. Not the bounty content itself — see [`bounty-original.md`](./bounty-original.md) for that.
> Last updated: 2026-05-24

---

## TL;DR

This is a **volume-pumping competition** with subjective anti-gaming review, not a "best demo wins" build-off. To win, the agent must generate **the highest legitimate on-chain payment volume** in one of two categories, while passing sponsor review for "real activity" patterns.

**Recommended category: Category 2 — Ace Data Cloud Usage**, because free signup credits convert directly to volume with no upfront capital outlay.

**Estimated effort:** 8–10 dev days for a polished submission. Deadline is ~10 days from listing fetch, so timeline is tight but feasible.

---

## 1. Economic Model

This is the most important section. The bounty is structured unlike most:

### What's being measured

| Category | Metric |
|----------|--------|
| Cat 1 (Payment Volume) | Total USDC volume of general AI service escrow payments routed through SAP |
| Cat 2 (Ace Data Cloud Usage) | Total volume of Ace Data Cloud services consumed via x402 |

### ROI math

For a single 1st-place prize ($700), the agent must generate the highest legitimate volume. The break-even depends on how the volume is sourced:

| Source of volume | Marginal cost per $ of volume | Net at $700 prize |
|------------------|-------------------------------|--------------------|
| Real users paying the agent | Negative (revenue) | Pure profit |
| Free signup credits (Cat 2 only) | ~0 | Near-pure profit |
| Own funds funding escrow | $1 of capital per $1 volume | Loss unless prize > total spend |
| Wash trading own funds | Same | **Disqualification risk** |

**Insight:** Cat 2's free credits on signup (Google/GitHub login) are the only economic lane that's risk-free. Cat 1 requires either (a) real customers, or (b) burning capital — and "wash trading" is explicitly grounds for disqualification.

### Submission distribution (unknown but estimable)

- 31 submissions as of fetch
- ~10 days remaining
- Unknown how many are placeholders vs serious builds
- A small number of well-funded teams could dominate Cat 1 with real customer volume; Cat 2 is more level-playing because of free credits

---

## 2. Technical Requirements Breakdown

### Common to both categories

- Agent registered on SAP mainnet
- Complete automated workflow (trigger → execution → payment, zero human input)
- Synapse RPC for execution

### Category 1 only

- Use **escrow** for payments
- At least 1 AI capability
- **Use Synapse Sentinel agent services at least once** — this is a hard requirement and pins us to a specific OOBE service

### Category 2 only

- ADC account at `platform.acedata.cloud`
- **x402** with AceDataCloud's facilitator
- **At least 3 distinct ADC services** — note: an unanswered comment from Luis Ploennig asks how "distinct" is defined (image/video/chat/search?)

### Open questions to clarify with sponsor (via Telegram)

1. Does "3 distinct ADC services" mean 3 different API endpoints, or 3 different service categories (e.g., image, video, chat, search)?
2. Do agents need to be open-sourced, or only registered on SAP?
3. Can a single agent compete in both categories with separate registrations, or must it be one agent / one category?
4. What's the minimum volume threshold to qualify for evaluation?
5. Does the demo video need to show a human face?

**Action item:** Reach out via the listing's Telegram link before committing significant build time.

---

## 3. Stack Reality Check

| Component | Maturity Signal | Risk |
|-----------|------------------|------|
| SAP SDK | OOBE Protocol's own SDK, claimed 15k+ downloads | Medium — early protocol, breaking changes possible |
| Synapse Client SDK | OOBE's unified stack | Medium — same |
| Ace Data Cloud APIs | 85M API calls Q1, $326K Q1 revenue | Low — mature, paying customers |
| Synapse Sentinel | Single agent address (`Ccr2yK...`) | Medium — opaque, must integrate (Cat 1 only) |
| x402 (OOBE server) | Open-source, OOBE-maintained | Medium |
| x402 (ADC client) | Open-source, ADC-maintained | Low–Medium |

**Live infrastructure concerns:**
- A commenter (Hickson Haziel) reported the **Synapse Explorer has broken pages**. Need to verify functionality before depending on it.
- Synapse RPC has a free tier; higher tier requires asking for a discount. Plan around the free tier limits.

---

## 4. Competition Analysis

Sample of currently-public competitor repos (from GitHub search 2026-05-24):

| Repo | Owner | Approach signal |
|------|-------|-----------------|
| `AtlasNexusTech/oobe-ace-agent` | Atlas Nexus | "x402 tool discovery, search, chat, image intelligence on SAP mainnet" — multi-service Cat 2 |
| `AtlasNexusTech/oobe-sap-agent` | Atlas Nexus | "Solana mainnet agent with x402 payments, ADC integration, Synapse Sentinel" — likely Cat 1 |
| `AtlasNexusTech/nexus-scout-sap` | Atlas Nexus | "3 x402 tools published (ADC Search, Chat, Images)" — submitter publishing tools, not just consuming |
| `huangzesen/oobe-ace-research-agent` | huangzesen | Research-focused workflow |
| `BoozeLee/synapse-ace-agent` | BoozeLee | "SAP, x402, Sentinel validation, multi-service workflows" — covers all bases |
| `BoozeLee/trendforge-agent` | BoozeLee | Trend research |
| `Saber1Y/AgentSAP` | Saber1Y | "discovers tools through SAP, uses ADC to analyze and generate bounty execution assets" — meta bounty hunter |
| `597226617/oobe-ace-agent` | 597226617 | Generic naming |
| `cool0123/oobe-ace-signal-agent` | cool0123 | "Solana signal agent" |
| `GilangDjati/solana-syndicate-agent` | GilangDjati | "Content Syndicate Agent powered by Synapse SAP and x402" |
| `asroryandesfar-art/synapsepay-autonomous-agent` | asroryandesfar | Hits all requirements per description |

**Notable patterns:**
- Atlas Nexus has **three repos**, suggesting they're competing aggressively in both categories with separate codebases
- Multiple teams are publishing tools to SAP (provider side), not just consuming — this is a different volume play
- "Research agent" / "trend agent" archetypes are common — saturated narrative
- Few competitors mention real users / multi-tenancy — that's a potential differentiation

**Edge:** Build something that solves a real problem a real audience pays attention to. Volume that comes from genuine usage will pass sponsor review easily and may have higher per-invocation value than a competitor running synthetic loops.

---

## 5. Risk Assessment

| Risk | Severity | Mitigation |
|------|----------|------------|
| Wash-trading disqualification | High | Build something with real user interaction or naturally-diverse calls |
| Volume floor too high for free credits | Medium | Recruit real users (Telegram bot, public webapp); plan top-up budget |
| Synapse Explorer / SDK bugs | Medium | Test integrations early in days 1–2 before architectural commitment |
| Ambiguous "3 distinct services" | Medium | Ask sponsor on Telegram, then conservatively use 4–5 services |
| Late entry (31 submissions in) | Low–Medium | Quality > novelty. Most submissions are likely WIP. |
| Subjective final scoring | Medium | Submit clean demo, transparent logs, public dashboard |
| Spreading too thin on both categories | Medium | Pick ONE category. Don't split focus. |
| Mainnet capital at risk | Low (Cat 2) / High (Cat 1) | Choose Cat 2 to avoid significant on-chain capital outlay |
| Time conflicts with other commitments | Unknown | Confirm Kami (Kamino) wrap-up is done before committing |

---

## 6. Fit Assessment

**Why this bounty fits RECTOR-LABS:**

- Solana DeFi focus — core competency
- Autonomous agent architecture aligns with the CIPHER / SIPHER multi-agent thesis
- SAP integration knowledge is reusable for future SIPHER work
- x402 payment workflow exposure carries forward to other agent bounties

**Why it might not fit:**

- 10-day window is tight if other commitments are active
- Volume race favors continuous-runtime operations — needs sustained execution attention
- Sponsor's subjective scoring adds qualitative uncertainty on top of quantitative effort

---

## 7. Decision Inputs Still Open

These determine implementation direction and need RECTOR's call before [`STRATEGY.md`](./STRATEGY.md) crystallizes:

1. **Category choice** — Recommend Cat 2 (Ace Data Cloud Usage). Confirm or override.
2. **Agent concept** — Several candidates listed in `STRATEGY.md`. Pick one or propose alternative.
3. **Budget for top-up volume** — Free credits + Synapse free tier should cover MVP. Any additional capital allocation?
4. **Time allocation** — Confirm 10-day commitment is realistic given Kami / other concurrent work.

Once these four are locked, implementation can start immediately.
