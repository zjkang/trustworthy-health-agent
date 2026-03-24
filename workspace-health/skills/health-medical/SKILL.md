---
name: health-medical
description: "Medical health monitoring: lab result interpretation (CBC, lipids, thyroid, hormones, 50+ biomarkers), blood pressure tracking and hypertension management, blood glucose monitoring and diabetes risk assessment, sleep analysis (efficiency, STOP-BANG), and longitudinal health trend analysis. Use when interpreting lab results, tracking blood pressure or blood glucose, analyzing sleep, or reviewing health trends."
metadata: {"nanobot": {"emoji": "🏥"}}
---

# Health Medical

Medical marker interpretation, blood pressure and blood glucose monitoring, sleep analysis, and health trend tracking.

## 1. Lab Result Interpretation

When user shares blood work, FeNO, urinalysis, or other medical data:

1. Reference `references/medical-markers.md` for normal ranges and clinical significance
2. Flag out-of-range values with severity (mild/moderate/concerning)
3. Explain what each marker means in plain language
4. Note trends if historical data exists in profile
5. **Always remind: this is informational, not a diagnosis. Consult their doctor.**

## 2. Blood Pressure Monitoring

When user reports blood pressure readings:

### Recording
1. Log to today's daily log (AM/PM) and `USER.md` Blood Pressure History
2. Record: date, time, systolic/diastolic, pulse, context (resting/post-exercise/stressed/post-medication)
3. Note arm used and posture if provided

### Classification (AHA/ESC Guidelines)
| Category | Systolic | | Diastolic |
|----------|----------|---|-----------|
| Normal | <120 | and | <80 |
| Elevated | 120-129 | and | <80 |
| Stage 1 Hypertension | 130-139 | or | 80-89 |
| Stage 2 Hypertension | >=140 | or | >=90 |
| Hypertensive Crisis | >180 | and/or | >120 |

### Trend Analysis
- Calculate 7-day average (separate AM and PM)
- Track morning vs evening patterns (morning surge is a risk factor)
- Identify white-coat hypertension (clinic high, home normal) vs masked hypertension (clinic normal, home high)
- Monitor response to lifestyle changes or medication adjustments

### Correlations
- Exercise effect: acute rise during, chronic reduction with regular activity
- Sodium intake: track dietary sodium from meal logs
- Sleep quality: poor sleep correlates with elevated morning BP
- Stress: context-tagged readings help identify stress-related spikes
- Medication timing: pre/post-dose readings for medication effectiveness

### Alerts — Flag for Doctor
- Sustained Stage 2 readings (>=140/90 on 3+ occasions)
- Any Hypertensive Crisis reading (>180/120) — seek immediate care
- Sudden change >20 mmHg from baseline
- Significant AM/PM asymmetry (>15 mmHg difference)
- Orthostatic drop (>20 systolic upon standing)

### Lifestyle Recommendations (by evidence strength)
- DASH diet: -11 mmHg systolic
- Sodium reduction (<1500mg/day): -5-6 mmHg
- Regular aerobic exercise (150 min/week): -5-8 mmHg
- Weight loss (per 1kg): -1 mmHg
- Limit alcohol: -4 mmHg
- Potassium increase (3500-5000mg/day): -4-5 mmHg

Reference `references/blood-pressure.md` for detailed clinical guidelines.

## 3. Blood Glucose Monitoring

When user reports blood glucose readings or CGM data:

### Recording
1. Log to today's daily log and `USER.md` Blood Glucose History
2. Record: date, time, reading (mmol/L or mg/dL), context (fasting/pre-meal/2h-post-meal/random/bedtime)
3. Convert units if needed: mmol/L x 18 = mg/dL

### Classification (ADA Guidelines)
| Context | Normal | Pre-diabetes | Diabetes |
|---------|--------|-------------|----------|
| Fasting | <5.6 mmol/L (<100 mg/dL) | 5.6-6.9 (100-125) | >=7.0 (>=126) |
| 2h Post-meal | <7.8 (140) | 7.8-11.0 (140-199) | >=11.1 (>=200) |
| HbA1c | <5.7% | 5.7-6.4% | >=6.5% |
| Random | - | - | >=11.1 (>=200) with symptoms |

### Trend Analysis
- Calculate fasting average over 7 and 30 days
- Track post-meal glucose response patterns
- Identify high-variability periods (coefficient of variation >36% = unstable)
- Monitor time-in-range for CGM users: target 70-180 mg/dL (3.9-10.0 mmol/L) for >70% of readings

### Glycemic Response Tracking
- Identify trigger foods: which meals cause the highest post-meal spikes?
- Meal sequencing effect: vegetables/protein before carbs can reduce spikes by 30-40%
- Exercise timing: 15-min walk post-meal can reduce peak glucose by 1-2 mmol/L

### Correlations
- Sleep: poor sleep (< 6h) increases insulin resistance next day
- Exercise: acute glucose uptake during activity; improved insulin sensitivity 24-48h post
- Stress: cortisol elevation raises fasting glucose
- Fiber intake: higher fiber meals have lower glycemic response
- Medication timing: track glucose response relative to medication doses

### Alerts — Flag for Doctor
- Fasting glucose consistently >=7.0 mmol/L (>=126 mg/dL)
- Any reading >=13.9 mmol/L (>=250 mg/dL)
- Hypoglycemia: <3.9 mmol/L (<70 mg/dL) — eat fast-acting glucose immediately
- Severe hypoglycemia: <3.0 mmol/L (<54 mg/dL) — urgent medical attention
- HbA1c >=6.5% on lab results

Reference `references/blood-glucose.md` for detailed clinical guidelines.

## 4. Sleep Analysis

When analyzing sleep patterns or providing sleep improvement advice:

### Sleep Quality Assessment
- **Duration trend**: Track average sleep hours over time
- **Sleep efficiency**: Time asleep / time in bed (target >85%)
- **Sleep latency**: Time to fall asleep (>30min = concern)
- **Night awakenings**: Count and duration
- **Sleep consistency score**: Variability in bed/wake times (0-100)
- **Social jetlag**: Weekend vs weekday sleep difference

### Sleep Problem Identification
- **Insomnia types**: Onset difficulty, maintenance difficulty, early waking, mixed
- **Sleep apnea risk**: STOP-BANG screening (score >=3 = refer to doctor)
- **Sleep debt**: Ideal duration minus actual duration accumulated over time

### Sleep-Health Correlations
- **Sleep <-> Exercise**: Exercise days vs rest days sleep quality; exercise timing effects
- **Sleep <-> Diet**: Caffeine cutoff (2pm), alcohol impact, late meals
- **Sleep <-> Mood**: Bidirectional relationship, stress impact on latency
- **Sleep <-> Weight**: Poor sleep -> increased appetite hormones, weight gain risk
- **Sleep <-> Blood Pressure**: Poor sleep correlates with elevated morning BP
- **Sleep <-> Blood Glucose**: <6h sleep increases insulin resistance

### Improvement Recommendations (Priority Order)
1. Fix wake time consistency (including weekends)
2. Establish pre-sleep routine (devices off 30min before)
3. Optimize environment (18-22C, dark, quiet)
4. Lifestyle: move exercise earlier, caffeine before 2pm, no alcohol 3h before bed

## 5. Health Trend Analysis

For longitudinal health monitoring and multi-dimensional trend analysis:

### Multi-Dimension Tracking
- **Weight/BMI trend**: Direction, rate of change, goal trajectory
- **Blood pressure trend**: Weekly/monthly averages, classification changes
- **Blood glucose trend**: Fasting averages, post-meal patterns, HbA1c trajectory
- **Symptom patterns**: Frequency, severity, triggers, seasonal patterns
- **Medication adherence**: Compliance rate, missed dose patterns
- **Lab result trends**: Longitudinal biomarker tracking with reference ranges
- **Mood & sleep**: Bidirectional correlations

### Correlation Engine
- **Medication <-> Symptoms**: Did starting a new med correlate with symptom changes?
- **Lifestyle <-> Outcomes**: Diet/sleep/exercise impact on symptoms and mood
- **Treatment effectiveness**: Before/after comparison for interventions
- **BP <-> lifestyle factors**: Sodium, exercise, sleep, stress
- **Glucose <-> meal patterns**: Food types, timing, sequencing

### Change Detection & Alerts
- **Significant changes**: Rapid weight change (>1kg/week), new symptoms, medication changes
- **Deterioration patterns**: Early identification of health decline
- **Improvement recognition**: Highlight positive trends
- **Threshold alerts**: Approaching dangerous levels (BMI extremes, BP/glucose thresholds)

### Predictive Insights
- Risk assessment based on trend direction and velocity
- Plateau prediction for weight loss phases
- Preventive recommendations based on pattern recognition

## Disclaimer

This skill is for informational and educational purposes only. It does not provide medical diagnosis, treatment, or professional health advice. Always consult a qualified healthcare provider for medical concerns.

## Acknowledgments

Sleep analysis and health trend analysis sections incorporate knowledge from [OpenClaw-Medical-Skills](https://github.com/MedClaw-Org/OpenClaw-Medical-Skills) by WellAlly Tech and MD BABU MIA, PhD. Licensed under MIT.
