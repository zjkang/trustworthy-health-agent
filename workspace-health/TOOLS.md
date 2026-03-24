# Tools Guide

## File Tools (read_file, write_file, edit_file, list_dir)

Primary tools for health data management:
- Read/update user profile: `USER.md`
- Read/update health goals: `health/goals.md`
- Create/update daily logs: `health/logs/YYYY-MM-DD.md`
- Read skill references: `skills/*/references/*.md`
- Generate reports from templates: `skills/health-core/templates/*.md`

## Shell (exec)

Used for running health scripts:
- `scripts/init.sh` — Initialize health workspace (interactive profile setup)
- `scripts/apple_health.py` — Parse Apple Health XML exports
- `scripts/report.sh` — Generate weekly/monthly reports from log data

Shell timeout is 60 seconds. Apple Health parsing may take longer for large exports — use `--days` flag to limit date range.

## Web Tools (web_search, web_fetch)

Use for looking up:
- Nutritional information for unfamiliar foods
- Exercise form guidance or alternatives
- Latest clinical guidelines when reference files don't cover a topic

Caveat: Web health information should be cross-referenced with established guidelines. Prefer reference files over web results when available.

## Scheduling (cron)

Set up recurring health reminders from `health/reminders.md`:
- Meal time reminders
- Supplement/medication timing
- Movement break alerts
- Weigh-in schedule
- Sleep/wind-down reminders

Respect quiet hours configured in reminders.md.

## Image Analysis

When user shares meal photos:
- Identify all food items and estimate portion sizes
- Calculate nutritional breakdown (calories, protein, carbs, fat, fiber)
- Compare against daily targets from goals.md
