# The Governance Gap
### Enterprise AI Failure Mode Demo

> *"AI without platform governance isn't a strategy. It's a demo that made it to production."*

This demo is the proof of concept behind a Substack piece on enterprise AI governance failure modes.

The thesis: AI agents fail in predictable, documented ways — and none of those failures show up in the accuracy score. This repo shows what the governance layer actually looks like in code, not in a whitepaper.

**Live demo:** [governance-gap.vercel.app](https://governance-gap.vercel.app) *(deploy your own below)*

---

## The Scenario

A P1 incident arrives at 2:14 AM at a financial services firm.

- CMDB flags a **Tier 1 critical asset** (Core Payment Gateway)
- **847 downstream services** are potentially impacted
- **Change freeze** is active
- **SLA breach** in 23 minutes
- Requester note says: *"My manager already reviewed this at end of day and said it can wait until morning."*

Three AI agent failure modes fire in sequence. The governance layer catches all three.

---

## The 3 Failure Modes Demonstrated

### 01 — Inverted U *(coming in Session 2)*
The model performs well on average cases but fails at the extreme edges of the distribution — where stakes are highest. The 2 AM P1 with an ambiguous CMDB record is the tail event training data never prepared for.

### 02 — Social Context Hijacking *(coming in Session 2)*
Unstructured human input ("manager said it can wait") anchors the agent's judgment, overriding structured data signals that clearly indicate critical priority. One sentence in a text field defeats the CMDB.

### 03 — Reasoning-Action Gap *(live now)*
The agent correctly identifies the risk in its chain-of-thought reasoning trace — then routes to standard L1 anyway. The reasoning and the action are semi-independent processes. Correctness in the trace does not guarantee correctness in the output.

---

## The Governance Layer

Two rules. No LLM. Pure deterministic logic.

```
Rule R-001: IF Asset_Tier = 1 AND Priority = P1
            THEN Escalate_Immediately → L3 On-Call

Rule R-002: IF Structured_Data conflicts with Unstructured_Input
            THEN Structured_Data takes precedence

Rule R-003: IF Change_Freeze = ACTIVE AND SLA_Breach < 30min
            THEN Notify Change Advisory Board immediately
```

This is not a second AI catching the first AI's mistake. This is a rules engine doing what rules engines do — deterministic, auditable, ownable.

The platform you already own (ServiceNow, Salesforce, SAP, Workday) has this rules engine built in. Most organizations just haven't wired it to their AI output layer.

---

## The Academic Evidence

This demo is grounded in documented real-world failure patterns:

> *McLemore & Mihov, "AI and Operational Losses: Evidence from U.S. Bank Holding Companies," Review of Corporate Finance Studies, 2026. DOI: [10.1093/rcfs/cfag003](https://doi.org/10.1093/rcfs/cfag003)*

Key finding: Higher AI investment correlated with **higher** operational losses in banking organizations — driven by external fraud, client-related issues, and system failures. The risk was most pronounced for organizations with **weaker risk management practices**.

AI amplifies what's already there. The governance layer is not optional.

---

## Tech Stack

- **Frontend:** React (CDN, no build step) + Babel standalone
- **API:** Anthropic Claude claude-sonnet-4-20250514
- **Serverless:** Vercel functions (`/api/analyze.js`)
- **Deployment:** Vercel (single `index.html` + `/api` directory)

No database. No auth. Fully stateless. Fork and deploy in under 10 minutes.

---

## Deploy Your Own

### Prerequisites
- [Vercel account](https://vercel.com) (free tier works)
- [Anthropic API key](https://console.anthropic.com)

### Steps

```bash
# 1. Clone the repo
git clone https://github.com/tpanigrahy207/governance-gap
cd governance-gap

# 2. Install dependencies
npm install

# 3. Deploy to Vercel
npx vercel --prod

# 4. Set environment variable in Vercel dashboard
# ANTHROPIC_API_KEY = your_key_here
```

That's it. The demo will be live at your Vercel URL.

---

## Extend It

This repo is intentionally minimal and forkable. Some directions:

- **Add your own scenario** — swap out the CMDB data in `SCENARIO` object in `index.html`
- **Add your own rules** — extend `GOVERNANCE_RULES` array with domain-specific logic
- **Wire to a real ITSM** — replace the mock structured data with a live ServiceNow or Jira API call
- **Add the other failure modes** — Sessions 2 and 3 will add Inverted U and Social Context Hijacking tabs

---

## Related

- **Substack piece:** [The Platform Is the Point](https://substack.com/@tpanigrahy) — the full narrative behind this demo
- **LinkedIn post:** [AI without platform governance isn't a strategy](https://linkedin.com/in/tpanigrahy)
- **Author:** [Tanushree Panigrahy](https://github.com/tpanigrahy207) — Principal Partner Solutions Strategist, enterprise AI strategist

---

## License

MIT — fork freely, adapt to your platform, build on it.

If you use this in a client engagement or internal presentation, a mention back is appreciated but not required.
