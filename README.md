# Trustworthy Health Agent

An AI-powered personal health management agent built on the [nanobot](https://github.com/imClumsyPanda/nanobot) framework.

## What It Does

Monitors and manages your health across **7 key dimensions** through a Telegram bot:

- **Weight & Body Composition** — tracking, BMI, trend analysis
- **Nutrition** — meal photo analysis, calorie/macro breakdown, Chinese food database
- **Exercise** — workout logging, HR zones, fitness analytics, Apple Health integration
- **Sleep** — duration, efficiency, STOP-BANG screening
- **Blood Pressure** — AHA classification, trend analysis, lifestyle recommendations
- **Blood Glucose** — ADA classification, CGM support, glycemic response tracking
- **Lab Results** — 50+ biomarkers interpretation, trend tracking

## Architecture

Built as a nanobot workspace with 4 modular skills:

| Skill | Always Loaded | Description |
|-------|:---:|-------------|
| `health-core` | Yes | Daily logging, reports, reminders, health dashboard, skill routing |
| `health-nutrition` | No | Meal analysis, weight loss, supplements, medications |
| `health-medical` | No | Labs, blood pressure, blood glucose, sleep, health trends |
| `health-fitness` | No | Exercise, fitness analytics, Apple Health |

## Quick Start

### Prerequisites
- Python >= 3.11
- [Anthropic API key](https://console.anthropic.com/)
- Telegram account (for mobile access)

### Installation

```bash
# Clone
git clone https://github.com/YOUR_USERNAME/trustworthy-health-agent.git
cd trustworthy-health-agent

# Create virtual environment
python3.12 -m venv .venv
source .venv/bin/activate

# Install
pip install -e .
```

### Configuration

```bash
# Initialize
nanobot onboard --workspace ./workspace-health

# Or manually create ~/.nanobot/config.json:
```

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

### Setup your health profile

```bash
# Copy the template
cp workspace-health/USER.md.template workspace-health/USER.md
# Edit with your info, or let the agent collect it through conversation
```

### Run

```bash
# Start Telegram bot
source .venv/bin/activate && nanobot gateway

# Or interactive CLI
nanobot agent
```

## Usage Examples (via Telegram)

- Send a meal photo → get nutritional breakdown
- "Blood pressure 135/85" → auto-log and classify
- "Weekly report" → get health summary across all dimensions
- "Weight 75.3kg" → update profile and show trend

## Privacy

All health data stays **100% local** on your machine. Nothing is uploaded to external services. The only external call is to the LLM API for generating responses.

## Cost

Using Claude Haiku 4.5: approximately **$1-2/month** with normal daily usage.

## Built On

- [nanobot](https://github.com/imClumsyPanda/nanobot) — Ultra-lightweight AI assistant framework
- [OpenClaw-Medical-Skills](https://github.com/MedClaw-Org/OpenClaw-Medical-Skills) — Health knowledge bases (MIT license)

## Disclaimer

This agent is for informational and educational purposes only. It does not provide medical diagnosis, treatment, or professional health advice. Always consult a qualified healthcare provider for medical concerns.

## License

MIT
