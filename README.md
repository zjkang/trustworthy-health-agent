# Trustworthy Health Guidance System (THGS)

*A Research Validation Platform for Trustworthy AI in Healthcare*

## 1. Overview

This is an independent research prototype validating how three foundational AI capabilities — reliable perception, temporally consistent reasoning, and safety-controllable generation — can be integrated into a single, end-to-end health guidance pipeline. It is a **research validation platform, not a commercial product**: its purpose is to produce evidence about whether contemporary LLM agents, when scaffolded by structured clinical knowledge, can meet the trustworthiness bar required by health-adjacent applications.

The system is built on a general-purpose LLM agent runtime ([nanobot](https://github.com/imClumsyPanda/nanobot)) with a health-domain skill layer on top. It uses a **prompt-driven architecture** in which structured clinical reference guidelines (AHA blood pressure staging, ADA glycemic classification, standard lab interpretation tables, STOP-BANG screening, and similar evidence-based protocols) serve as the knowledge bases the agent reasons over. This design directly reflects the research intent: to evaluate whether LLM-based agents can reliably extract health information from heterogeneous inputs, maintain temporal consistency across longitudinal records, and operate inside well-defined safety boundaries — using clinical guidelines as the guardrails rather than relying on the model's parametric knowledge alone.

The system is deployed as a personal health guidance assistant accessible through Telegram and a CLI gateway, with cron-based proactive check-ins, heartbeat-driven periodic tasks, and Apple Health XML integration for longitudinal sensor data. All user health data stays local; the only external call is to the LLM provider for inference.

## 2. Three-Layer Architecture

```
┌──────────────────────────────────────────────────────────────────┐
│               Trustworthy Health Guidance System                  │
│          Research Prototype · Built on nanobot runtime            │
└──────────────────────────────────────────────────────────────────┘
  Input: user messages (Telegram / CLI) · Apple Health XML · daily logs
                              │
                              ▼
  ┌─────────────────────────────────────────────────────────────┐
  │  LAYER 1 │ Health Information Extraction & Classification   │
  │  ───────────────────────────────────────────────────────   │
  │  Research Pillar: Reliable Perception & Information         │
  │                   Extraction                                │
  │                                                             │
  │  • Multi-source ingestion: free-text logs, structured       │
  │    vitals, nutritional input, Apple Health XML              │
  │  • Clinical entity extraction: symptoms, medications,       │
  │    lab values, biometrics                                   │
  │  • Domain classification via structured reference           │
  │    guidelines: AHA blood pressure staging, ADA glucose      │
  │    classification, 50+ lab marker interpretation            │
  │  • Nutritional analysis: macronutrients, calories,          │
  │    supplement tracking                                      │
  │                                                             │
  │  Related research: LP-DETR (ICIC 2025),                    │
  │                    PaQ-DETR (CVPR 2026)                    │
  │  Status: partially implemented — prompt-driven extraction   │
  │  with clinical reference knowledge bases                    │
  └─────────────────────────────────────────────────────────────┘
                              │
                              ▼
  ┌─────────────────────────────────────────────────────────────┐
  │  LAYER 2 │ Temporal Trend Analysis & Longitudinal Reasoning │
  │  ───────────────────────────────────────────────────────   │
  │  Research Pillar: Multimodal & Temporally Consistent        │
  │                   Reasoning                                 │
  │                                                             │
  │  • Apple Health XML parsing and ingestion (apple_health.py) │
  │  • Daily health log aggregation (markdown time series)      │
  │  • Weekly and monthly trend report generation               │
  │  • Fitness temporal modeling: heart rate zones, training    │
  │    load, recovery patterns                                  │
  │  • Cross-variable correlation: diet × weight × activity    │
  │    × sleep                                                  │
  │                                                             │
  │  Related research: V-CORE (ICME 2026),                     │
  │                    CoVis (IEEE CSCWD 2025)                  │
  │  Status: partially implemented — Apple Health XML parser    │
  │  and report generation exist; trend reasoning is            │
  │  LLM-mediated via structured templates                      │
  └─────────────────────────────────────────────────────────────┘
                              │
                              ▼
  ┌─────────────────────────────────────────────────────────────┐
  │  LAYER 3 │ Safety-Constrained Recommendation Generation    │
  │  ───────────────────────────────────────────────────────   │
  │  Research Pillar: AI Safety & Output Controllability        │
  │                                                             │
  │  • Risk-level classification before any output:            │
  │    AHA/ADA staging, STOP-BANG sleep apnea screening        │
  │  • High-risk flag routing: escalation to professional       │
  │    referral when safety boundary is exceeded               │
  │  • Access control: allowlist-based session security         │
  │  • Cron-based proactive safety check-ins and reminders     │
  │  • Output bounded by clinical guidelines — no diagnosis,   │
  │    no prescription, no override of professional advice     │
  │                                                             │
  │  Related research: IntentPrompt (EMNLP Findings 2025)      │
  │  Status: partially implemented — risk classification and    │
  │  routing logic exist; guardrail enforcement is             │
  │  guideline-constrained                                     │
  └─────────────────────────────────────────────────────────────┘
                              │
                              ▼
  Output: structured health summary · trend assessment ·
          safety-bounded guidance · or professional referral
  ════════════════════════════════════════════════════════════════
  Foundation: nanobot agent runtime
  LLM providers (Anthropic Claude) · Telegram/CLI channels ·
  session management · cron scheduler · skill loader
  ════════════════════════════════════════════════════════════════
```

A deeper, file-level walkthrough of each layer is available in [docs/architecture.md](docs/architecture.md).

## 3. Implementation Status

| Component | Status | Notes |
|---|---|---|
| nanobot runtime (LLM loop, providers, channels) | ✅ Implemented | Anthropic provider with prompt cache optimization |
| Telegram + CLI channels | ✅ Implemented | Production-ready gateways |
| Session management & access control | ✅ Implemented | `allowFrom` whitelist security |
| Cron scheduler & heartbeat | ✅ Implemented | Proactive reminders and task monitoring |
| Layer 1: Clinical entity extraction | 🔄 Partial | AHA/ADA/lab guidelines implemented as reference knowledge bases |
| Layer 1: Nutritional analysis | 🔄 Partial | Macro/calorie tracking, CN food library |
| Layer 2: Apple Health XML parsing | 🔄 Partial | `apple_health.py` exists; manual invocation |
| Layer 2: Trend report generation | 🔄 Partial | Weekly/monthly templates implemented |
| Layer 2: Automatic Health sync | 🚧 Planned | Automatic sync pipeline in progress |
| Layer 3: Risk classification & routing | 🔄 Partial | AHA/ADA staging, STOP-BANG screening |
| Layer 3: Full guardrail pipeline | 🚧 In progress | Integration of three layers underway |
| Structured data store | 🚧 Planned | Currently markdown-based; schema migration planned |

Legend: ✅ Implemented · 🔄 Partially implemented · 🚧 Planned / In progress

## 4. Research Context

This prototype operationalizes a research agenda on trustworthy AI for health-adjacent applications, drawing on a body of prior work that maps directly onto the three architectural layers above:

- **Layer 1 — Reliable Perception & Information Extraction** builds on prior work in robust query-based detection and structured information extraction, including *LP-DETR* (ICIC 2025) and *PaQ-DETR* (CVPR 2026), which inform the design choices around extracting clinical entities and vitals from heterogeneous input streams.
- **Layer 2 — Multimodal & Temporally Consistent Reasoning** is informed by *V-CORE* (ICME 2026) on temporally consistent multimodal reasoning and *CoVis* (IEEE CSCWD 2025) on collaborative visualization, which together motivate the longitudinal report and trend-analysis design.
- **Layer 3 — AI Safety & Output Controllability** is grounded in *IntentPrompt* (EMNLP Findings 2025), which studies intent-level controllability of LLM outputs — directly relevant to keeping health guidance bounded by clinical guidelines.

The platform is intended to support collaborative research efforts with academic partners (NYU, Boise State University, UIUC) on evaluating LLM-agent reliability, temporal consistency, and safety-constrained generation in real, longitudinal health-data settings. *(Specific collaboration scopes and named investigators are maintained separately and can be substituted into this section by the project owner.)*

## 5. Quick Start

### Prerequisites
- Python ≥ 3.11
- [Anthropic API key](https://console.anthropic.com/)
- Telegram account (for mobile access)

### Installation

```bash
git clone https://github.com/YOUR_USERNAME/trustworthy-health-agent.git
cd trustworthy-health-agent

python3.12 -m venv .venv
source .venv/bin/activate

pip install -e .
```

### Configuration

```bash
nanobot onboard --workspace ./workspace-health
```

Or manually create `~/.nanobot/config.json`:

```json
{
  "agents": {
    "defaults": {
      "model": "anthropic/claude-haiku-4-5-20251001",
      "provider": "anthropic",
      "workspace": "/path/to/workspace-health"
    }
  },
  "providers": {
    "anthropic": { "apiKey": "your-api-key" }
  },
  "channels": {
    "telegram": {
      "enabled": true,
      "token": "your-bot-token",
      "allowFrom": ["your-user-id"]
    }
  }
}
```

### Set up the health profile

```bash
cp workspace-health/USER.md.template workspace-health/USER.md
# Edit with personal info, or let the agent collect it through conversation
```

### Run

```bash
# Telegram gateway
source .venv/bin/activate && nanobot gateway

# Interactive CLI
nanobot agent
```

## 6. Usage Examples (via Telegram)

- Send a meal photo → nutritional breakdown (Layer 1 extraction)
- "Blood pressure 135/85" → auto-log and AHA classification (Layer 1 → Layer 3)
- "Weekly report" → multi-dimension health summary (Layer 2)
- "Weight 75.3kg" → profile update and trend assessment (Layer 1 → Layer 2)

## 7. Privacy

All health data stays **100% local** on the user's machine. Nothing is uploaded to external services. The only external call is to the LLM API for inference.

## 8. Built On

- [nanobot](https://github.com/imClumsyPanda/nanobot) — ultra-lightweight AI agent framework
- [OpenClaw-Medical-Skills](https://github.com/MedClaw-Org/OpenClaw-Medical-Skills) — health knowledge bases (MIT)

## 9. Disclaimer

> ⚠️ **Research Prototype — Not for Clinical Use**
>
> This system is a research validation platform designed to explore
> trustworthy AI architectures for health guidance. It does not provide
> medical advice, diagnosis, or treatment. Always consult a qualified
> healthcare professional for medical decisions.

## 10. License

MIT
