---
name: health-fitness
description: "Exercise and fitness management: workout logging with HR zones, exercise programming with injury awareness, fitness analytics (volume trends, consistency scoring, MET-based calories), Apple Health integration for wearable data import. Use when logging workouts, planning exercise routines, analyzing fitness progress, or importing Apple Health data."
metadata: {"nanobot": {"emoji": "💪"}}
---

# Health Fitness

Exercise logging, programming, fitness analytics, and Apple Health integration.

## 1. Exercise Logging & Programming

When user shares workout data or asks for exercise advice:

1. Log workout to daily record: type, duration, calories, heart rate
2. Reference `references/exercise.md` for programming principles
3. Check user's injury history from profile before recommending exercises
4. Suggest modifications for known limitations
5. Track weekly volume and progressive overload

## 2. Apple Health Integration

When Apple Health data is available (via Shortcuts or export):

1. Parse activity, workout, body measurement, and sleep data using `scripts/apple_health.py`
2. Cross-reference with manual logs
3. Use for more accurate calorie expenditure estimates
4. Reference `references/apple-health.md` for data format and fields

Usage: `python scripts/apple_health.py export.xml --days 30`

Output: Markdown summary + JSON with extracted metrics (weight, body fat, workouts, steps, HR, HRV, sleep).

## 3. Fitness & Exercise Analysis

Extends workout logging with deeper exercise analytics:

### Exercise Trend Analysis
- **Volume trends**: Duration, distance, calories burned over time
- **Frequency trends**: Weekly exercise days, consistency score (0-100)
- **Intensity distribution**: Low/moderate/high intensity ratio
- **Type distribution**: Balance between cardio, strength, flexibility

### Progress Tracking
- **Running**: Pace improvement, distance progression, HR at same pace
- **Strength**: Weight increases, volume (sets x reps x weight), RPE trends
- **Endurance**: Duration extension, distance growth
- **Recovery**: Resting HR trend as fitness indicator

### Exercise Habit Analysis
- Preferred exercise times (morning/afternoon/evening)
- Consistency score: How regular is the exercise pattern?
- Rest day distribution and recovery adequacy
- Social jetlag equivalent for exercise (weekday vs weekend patterns)

### Exercise-Health Correlations
- **Exercise <-> Weight**: Calorie expenditure vs weight change
- **Exercise <-> Blood pressure**: Long-term BP reduction from regular activity
- **Exercise <-> Sleep**: Exercise timing and sleep quality impact
- **Exercise <-> Mood**: Exercise as mood regulation tool
- **Exercise <-> Blood glucose**: Post-exercise glucose uptake, improved insulin sensitivity

### MET-Based Calorie Calculation
- Walking (3-5 km/h): 3.5-5 MET
- Jogging (8 km/h): 8 MET
- Running (10 km/h): 10 MET
- Swimming: 6-10 MET
- Strength training: 5 MET
- Calories = MET x weight(kg) x hours

### Safety Signals
- Exercise HR > 95% max HR -> flag
- Resting HR > 100 bpm -> flag
- 7+ consecutive high-intensity days -> overtraining risk
- Weight loss > 1kg/week -> potentially unhealthy

## Acknowledgments

Fitness analysis section incorporates knowledge from [OpenClaw-Medical-Skills](https://github.com/MedClaw-Org/OpenClaw-Medical-Skills) by WellAlly Tech and MD BABU MIA, PhD. Licensed under MIT.
