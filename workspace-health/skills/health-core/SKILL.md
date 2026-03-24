---
name: health-core
description: "Core health management: daily logging, progress reports, health reminders, profile management, and unified health dashboard showing sleep, diet, exercise, heart rate, blood pressure, blood glucose, and weight. Always active as the foundation for health coaching. Routes to specialized health skills (nutrition, medical, fitness) as needed."
always: true
---

# Health Core

Foundation skill for personal health management. Handles daily operations, reporting, and routing to specialized skills.

## Setup

On first use, initialize the health workspace:

1. Ensure `health/goals.md` and `health/reminders.md` exist (copy from `skills/health-core/config/` if missing)
2. Create `health/logs/` directory for daily logs
3. Fill in `USER.md` with personal info, medical history, and current metrics
4. Optionally run `skills/health-core/scripts/init.sh` for interactive setup

All personal data stays in the user's workspace. Never commit health data to shared repos.

## Daily Logging

Create daily logs at `health/logs/YYYY-MM-DD.md` using the template at `skills/health-core/templates/daily-log.md`.

Each daily log captures:
- Body metrics (weight, body fat %)
- Blood pressure (morning and evening readings)
- Blood glucose (fasting, pre-meal, post-meal)
- Meals with nutritional breakdown (calories, protein, carbs, fat, fiber)
- Exercise (type, duration, calories, heart rate)
- Steps
- Supplements taken
- Sleep quality (previous night)
- Overall day rating

When user reports any health data point, update today's log. Create the file from template if it doesn't exist yet.

## Body Metrics Tracking

When user reports weight, body fat, or measurements:

1. Update `USER.md` with the new data point
2. Update today's daily log
3. Calculate trend (7-day average, 30-day trend)
4. Compare against goal trajectory from `health/goals.md`
5. Provide context: "On track" / "Ahead" / "Behind by X"

## Progress Reports

Generate reports using templates from `skills/health-core/templates/`:

### Weekly Report
Use `templates/weekly-report.md`. Compile from the past 7 daily logs:
- Body composition trend (weight, body fat, waist — start vs end of week)
- Nutrition summary (daily averages vs targets, adherence %)
- Exercise summary (sessions, active minutes, steps)
- Blood pressure trend (7-day average, morning vs evening pattern)
- Blood glucose trend (fasting average, post-meal patterns)
- Sleep summary (average duration, quality)
- Supplement adherence
- Wins, areas to improve, next week focus

### Monthly Report
Use `templates/monthly-report.md`. Compile from 4 weekly summaries:
- 4-week body composition trend with monthly change and projection
- Nutrition monthly average
- Exercise overview
- Blood pressure monthly trend and classification changes
- Blood glucose monthly trend
- Medical updates
- Plan adjustments for next month

## Reminders

Configure reminders in `health/reminders.md`. Use the `cron` tool to schedule:
- Wake-up / sleep reminders
- Meal times (with pre-meal supplement reminders)
- Blood pressure measurement reminders (morning and evening)
- Blood glucose measurement reminders (fasting, pre/post-meal)
- Movement breaks (sedentary alerts)
- Workout schedule
- Medication / supplement timing
- Weigh-in schedule

## Unified Health Dashboard

When user asks for a health overview or dashboard, compile the latest data across all 7 dimensions:

```
Health Dashboard — [DATE]
━━━━━━━━━━━━━━━━━━━━━━━━━
Weight:        XX.X kg (7d avg: XX.X, trend: ↑/↓/→)
Body Fat:      XX.X%
Blood Pressure: XXX/XX mmHg (classification)
Blood Glucose:  X.X mmol/L (fasting) | X.X (post-meal)
Resting HR:     XX bpm
Sleep:          X.Xh (quality: X/5)
Exercise:       X sessions this week (XXX min total)
Steps:          X,XXX avg/day
Nutrition:      X,XXX kcal avg (target: X,XXX)
━━━━━━━━━━━━━━━━━━━━━━━━━
Alerts: [any out-of-range values]
```

Data sources: today's daily log, `USER.md` profile, recent logs for trends.

## Important Guidelines

- **Privacy first**: All data local, never suggest uploading health data
- **Not a doctor**: Always caveat medical interpretations
- **No extremes**: Never recommend <1200 cal/day, crash diets, or dangerous supplements
- **Injury-aware**: Always check profile for injuries before exercise advice
- **Evidence-based**: Cite clinical guidelines where possible
- **Culturally aware**: Support diverse cuisines and food traditions
- **Metric + Imperial**: Support both unit systems based on user preference
