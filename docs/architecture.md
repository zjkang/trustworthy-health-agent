# Architecture — Trustworthy Health Guidance System (THGS)

This document is the technical companion to the [README](../README.md). It describes each of the three architectural layers in turn — what the layer does, which code files implement it today, what research question it is built to answer, and what is planned next.

THGS is a **research validation platform**, not a clinical product. The architecture is intentionally prompt-driven: structured clinical guidelines (AHA, ADA, STOP-BANG, lab interpretation tables) act as the knowledge bases that constrain LLM behavior at every layer. The point of the system is to study whether this design produces reliable, temporally consistent, and safety-bounded health guidance — and the layered structure below is also the experimental decomposition we use to study it.

---

## Foundation — nanobot Agent Runtime

Before describing the three health layers, the foundation they sit on:

| Component | Path | Responsibility |
|---|---|---|
| Agent loop | [nanobot/agent](../nanobot/agent) | Tool-using LLM loop, message routing |
| Providers | [nanobot/providers](../nanobot/providers) | Anthropic Claude integration with prompt-cache optimization |
| Channels | [nanobot/channels](../nanobot/channels) | Telegram bot + CLI gateway |
| Session & bus | [nanobot/session](../nanobot/session), [nanobot/bus](../nanobot/bus) | Conversation state, inter-component messaging |
| Security | [nanobot/security](../nanobot/security) | `allowFrom` whitelist, channel auth |
| Cron scheduler | [nanobot/cron](../nanobot/cron) | Time-based reminders and proactive check-ins |
| Heartbeat | [nanobot/heartbeat](../nanobot/heartbeat) | 30-min periodic task runner driven by [HEARTBEAT.md](../workspace-health/HEARTBEAT.md) |
| Skills loader | [nanobot/skills](../nanobot/skills) | On-demand loading of `SKILL.md` definitions |
| File / exec / web tools | [nanobot/agent](../nanobot/agent) | `read_file`, `write_file`, `edit_file`, `exec`, `web_search`, `web_fetch` |

Status: ✅ Implemented. The runtime is health-agnostic and stable; everything below depends on it.

---

## Layer 1 — Health Information Extraction & Classification

**Research Pillar: Reliable Perception & Information Extraction**

### What this layer does

Layer 1 turns heterogeneous, mostly unstructured input — free-text chat messages, photos of meals, user-reported vital readings, Apple Health XML — into normalized, classified health data points that downstream layers can reason over. It is the system's perception front-end.

Concretely, this layer is responsible for:
- multi-source ingestion (chat, structured vitals, images, XML)
- clinical entity extraction (symptoms, medications, lab values, biometrics)
- domain classification against structured clinical guidelines (AHA blood-pressure staging, ADA glucose categories, 50+ lab-marker interpretation rules)
- nutritional analysis (macros, calories, supplement tracking, Chinese-food brand resolution)

### Which existing code files implement it

The layer is implemented as **prompt-level extraction grounded in structured reference markdown**, loaded on demand by nanobot's skill loader:

| Concern | Path |
|---|---|
| Routing & always-on extraction | [skills/health-core/SKILL.md](../workspace-health/skills/health-core/SKILL.md) |
| Daily-log capture template | [skills/health-core/templates/daily-log.md](../workspace-health/skills/health-core/templates/daily-log.md) |
| Medical extraction & classification | [skills/health-medical/SKILL.md](../workspace-health/skills/health-medical/SKILL.md) |
| AHA blood-pressure KB | [skills/health-medical/references/blood-pressure.md](../workspace-health/skills/health-medical/references/blood-pressure.md) |
| ADA blood-glucose KB | [skills/health-medical/references/blood-glucose.md](../workspace-health/skills/health-medical/references/blood-glucose.md) |
| Lab marker KB (50+) | [skills/health-medical/references/medical-markers.md](../workspace-health/skills/health-medical/references/medical-markers.md) |
| Nutrition skill | [skills/health-nutrition/SKILL.md](../workspace-health/skills/health-nutrition/SKILL.md) |
| Macros / calories KB | [skills/health-nutrition/references/nutrition.md](../workspace-health/skills/health-nutrition/references/nutrition.md) |
| Supplements KB | [skills/health-nutrition/references/supplements.md](../workspace-health/skills/health-nutrition/references/supplements.md) |
| Medications KB | [skills/health-nutrition/references/medications.md](../workspace-health/skills/health-nutrition/references/medications.md) |
| Chinese food brands KB | [skills/health-nutrition/references/cn-brands.md](../workspace-health/skills/health-nutrition/references/cn-brands.md) |

Status: 🔄 **Partially implemented.** Reference KBs exist and are loaded into the agent context; extraction and classification themselves are LLM-mediated rather than backed by a dedicated parser/validator.

### Research question being validated

> **When grounded in structured clinical reference guidelines, can an LLM agent reliably extract and classify health information from heterogeneous, real-world inputs — and where does it fail silently?**

Specifically the layer is set up to study:
- Extraction accuracy against ground-truth labels in chat-style and image-style inputs.
- The marginal contribution of structured KBs vs. relying on the model's parametric knowledge alone.
- Failure modes that look fluent but are factually wrong (silent hallucination), which are the central reliability hazard for any health-adjacent LLM system.

This is the area connected most directly to prior work on robust query-based detection and structured information extraction (*LP-DETR*, ICIC 2025; *PaQ-DETR*, CVPR 2026).

### What's planned next

- **Schema-validated extraction.** Add a thin validator between the LLM and persistence so that `BP 135/85` becomes a typed record instead of free-text in markdown.
- **Image-input evaluation harness.** Standardize meal-photo benchmarking so we can measure Layer 1 perception independently of Layer 2/3.
- **Silent-failure probes.** Build a small adversarial set (ambiguous units, mixed-language inputs, partially redacted readings) and track how often the LLM extracts confidently-wrong values.
- **Migration of reference KBs to structured form.** Convert the markdown threshold tables into machine-checkable JSON/YAML so classification can be cross-verified.

---

## Layer 2 — Temporal Trend Analysis & Longitudinal Reasoning

**Research Pillar: Multimodal & Temporally Consistent Reasoning**

### What this layer does

Layer 2 takes the per-event records produced by Layer 1 and reasons over them as a **time series**. It is the system's longitudinal memory.

Concretely:
- ingest Apple Health XML and convert it into the same time-indexed format as user-reported entries
- aggregate daily logs into weekly and monthly trend reports
- model fitness signals over time (heart-rate zones, training load, recovery patterns)
- surface cross-variable correlations (diet × weight × activity × sleep)

### Which existing code files implement it

| Concern | Path |
|---|---|
| Apple Health XML parser | [skills/health-fitness/scripts/apple_health.py](../workspace-health/skills/health-fitness/scripts/apple_health.py) |
| Fitness skill definition | [skills/health-fitness/SKILL.md](../workspace-health/skills/health-fitness/SKILL.md) |
| HR-zone & wearables KBs | [skills/health-fitness/references/](../workspace-health/skills/health-fitness/references/) |
| Daily log time series (on disk) | [workspace-health/health/logs/](../workspace-health/health/logs/) |
| Daily-log template | [skills/health-core/templates/daily-log.md](../workspace-health/skills/health-core/templates/daily-log.md) |
| Weekly report template | [skills/health-core/templates/weekly-report.md](../workspace-health/skills/health-core/templates/weekly-report.md) |
| Monthly report template | [skills/health-core/templates/monthly-report.md](../workspace-health/skills/health-core/templates/monthly-report.md) |
| Goals & target trajectories | [workspace-health/health/goals.md](../workspace-health/health/goals.md) |

Status: 🔄 **Partially implemented.** `apple_health.py` is the one piece of real Python in the health layer and runs end-to-end; the templates exist; trend aggregation itself is performed by the LLM at report-generation time using the templates as scaffolding (no charting, no statistical validation, no automatic Apple Health sync).

### Research question being validated

> **Can an LLM agent maintain temporal consistency across longitudinal health records, and produce trend conclusions that match what a deterministic analytics pipeline would produce on the same data?**

Specifically:
- Do weekly/monthly summaries stay self-consistent across regenerations on the same source logs?
- Do they stay consistent across overlapping windows (e.g. does the last-week-of-March summary agree with the March monthly summary on shared days)?
- Where does LLM-mediated aggregation drift relative to a reference computed-statistics baseline?

This is the area connected to prior work on temporally consistent multimodal reasoning (*V-CORE*, ICME 2026) and collaborative visualization (*CoVis*, IEEE CSCWD 2025).

### What's planned next

- **Automatic Apple Health sync.** Replace manual `apple_health.py` invocation with a scheduled pipeline triggered through nanobot's cron layer.
- **Reference analytics baseline.** Implement a deterministic summary computer (Python, no LLM) over the same time-indexed data, so LLM reports can be diffed against ground truth.
- **Structured time-series store.** Migrate from `health/logs/YYYY-MM-DD.md` to a typed store (initially SQLite) while keeping markdown export for transparency.
- **Cross-window consistency tests.** Automate the overlap checks (week-vs-month, month-vs-quarter) as a regression suite.

---

## Layer 3 — Safety-Constrained Recommendation Generation

**Research Pillar: AI Safety & Output Controllability**

### What this layer does

Layer 3 sits between the agent's reasoning and any externally visible response. It enforces three guarantees:

1. **Risk classification before output.** Vitals and screening scores are staged against AHA/ADA tables or STOP-BANG before any recommendation is composed.
2. **Escalation routing.** When a reading crosses a guideline-defined safety threshold (hypertensive crisis, severe hyper/hypoglycemia, high STOP-BANG score), the agent is instructed to route to *"please consult a healthcare professional"* rather than self-management guidance.
3. **Output bounding.** No diagnosis, no prescription, no override of professional advice; recommendations stay within evidence-based guidelines (AHA, ADA, WHO) and respect injuries and conditions recorded in the user profile.

It also owns access control (channel-level allowlist) and proactive safety nudges (cron + heartbeat).

### Which existing code files implement it

| Concern | Path |
|---|---|
| Agent persona, safety rules, "not a doctor" | [workspace-health/AGENTS.md](../workspace-health/AGENTS.md) |
| Identity, tone, boundaries | [workspace-health/SOUL.md](../workspace-health/SOUL.md) |
| Tool guidance | [workspace-health/TOOLS.md](../workspace-health/TOOLS.md) |
| AHA risk staging KB | [skills/health-medical/references/blood-pressure.md](../workspace-health/skills/health-medical/references/blood-pressure.md) |
| ADA risk staging KB | [skills/health-medical/references/blood-glucose.md](../workspace-health/skills/health-medical/references/blood-glucose.md) |
| STOP-BANG screening | encoded in [skills/health-medical/SKILL.md](../workspace-health/skills/health-medical/SKILL.md) |
| Access control (allowlist) | nanobot config + [nanobot/security](../nanobot/security) |
| Proactive check-ins | [workspace-health/HEARTBEAT.md](../workspace-health/HEARTBEAT.md), [workspace-health/health/reminders.md](../workspace-health/health/reminders.md) |

Status: 🔄 **Partially implemented.** Access control (`allowFrom`) and the cron/heartbeat infrastructure are production-grade; risk classification, escalation phrasing, and output bounding are enforced at the **prompt level** rather than as a deterministic gate. The end-to-end Layer 1 → Layer 2 → Layer 3 integration is in progress.

### Research question being validated

> **How often does an LLM agent breach its guideline-defined safety boundary under realistic and adversarial input, and which architectural choices most reduce those breaches?**

Specifically:
- Does the agent ever provide specific dosage advice, give a diagnosis, or contradict its "consult a professional" instruction?
- How does breach rate change between (a) prompt-only guardrails, (b) prompt + structured risk tables, and (c) a deterministic pre-output gate that blocks outputs above a guideline-defined risk threshold?
- How robust is the safety boundary to multilingual input, ambiguous symptoms, and intent-shifted prompts?

This is the area connected to prior work on intent-level controllability of LLM outputs (*IntentPrompt*, EMNLP Findings 2025).

### What's planned next

- **Deterministic pre-output gate.** Promote the AHA/ADA/STOP-BANG checks from prompt-level rules to an explicit pre-output classifier that can hard-block or rewrite responses.
- **Guardrail evaluation set.** Curate a structured red-team suite (crisis-level vitals, dosage requests, diagnosis requests, intent-shifted prompts) and measure breach rate per architecture variant.
- **Layer-to-layer contracts.** Make the hand-off from Layer 1 (extracted entity) to Layer 3 (risk classification) explicit in code, so a value cannot be acted on without first being staged.
- **Audit log.** Persist every Layer-3 decision (classification, escalation, allowed/denied) for offline review.

---

## End-to-End Data Flow

```
   Telegram / CLI message ──► nanobot agent loop
                                   │
                                   ▼
                    ┌─────── LAYER 1 ───────┐
                    │  Skills: health-core  │
                    │           health-medical
                    │           health-nutrition
                    │  KB: AHA / ADA / labs / nutrition
                    │  Output: extracted, classified entities
                    └───────────┬───────────┘
                                ▼
                    ┌─────── LAYER 2 ───────┐
                    │  apple_health.py      │
                    │  daily/weekly/monthly │
                    │  templates            │
                    │  Output: trend report │
                    └───────────┬───────────┘
                                ▼
                    ┌─────── LAYER 3 ───────┐
                    │  AHA/ADA risk staging │
                    │  STOP-BANG screening  │
                    │  Persona-bounded      │
                    │  output / referral    │
                    └───────────┬───────────┘
                                ▼
            Telegram / CLI response (safety-bounded)
```

Persistence at every layer is currently **markdown files on disk**:

- [workspace-health/USER.md](../workspace-health/USER.md.template) — profile + weight history
- [workspace-health/health/goals.md](../workspace-health/health/goals.md) — goals
- [workspace-health/health/reminders.md](../workspace-health/health/reminders.md) — reminder configuration
- `workspace-health/health/logs/YYYY-MM-DD.md` — daily logs (the time series)

Migration to a structured store is planned but not yet implemented.

---

## Disclaimer

> ⚠️ **Research Prototype — Not for Clinical Use.** This document describes a research architecture. The system does not provide medical advice, diagnosis, or treatment. Always consult a qualified healthcare professional for medical decisions.
