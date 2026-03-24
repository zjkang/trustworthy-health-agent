---
name: health-nutrition
description: "Meal and nutrition analysis: photo/text meal analysis with calorie/macro breakdown, advanced micronutrient tracking, weight loss metabolism (BMR/TDEE/deficit), supplement and medication guidance (GLP-1, evidence-tiered supplements). Use when analyzing food, tracking diet, managing weight loss, or discussing supplements/medications."
metadata: {"nanobot": {"emoji": "🍽️"}}
---

# Health Nutrition

Comprehensive nutrition analysis, weight loss management, and supplement/medication guidance.

## 1. Meal Analysis (Photo or Text)

When user shares a meal photo or describes food:

1. Identify all food items, estimate portion sizes
2. Reference `references/nutrition.md` for caloric density, macro ratios
3. For Chinese brand products (bubble tea, convenience store items, packaged foods), reference `references/cn-brands.md` for accurate nutritional data
4. Calculate: calories, protein (g), carbs (g), fat (g), fiber (g)
5. Compare against user's daily targets from `health/goals.md`
6. Provide remaining budget for the day
7. Flag nutritional gaps or excesses

Output format: concise, no lecture. Numbers first, advice second.

## 2. Supplement Guidance

When user asks about supplements or reports what they take:

1. Reference `references/supplements.md`
2. Check for interactions with user's medications (from profile)
3. Advise timing (with meals, empty stomach, etc.)
4. Evidence-based recommendations only — no hype

## 3. Weight Loss Medication Guidance

When user asks about GLP-1, semaglutide, Ozempic, Wegovy, tirzepatide, or any weight loss medication:

1. Reference `references/medications.md` for mechanism, efficacy, side effects, contraindications
2. Cross-reference user's profile: BMI, comorbidities, current medications, medical history
3. Use the clinical decision framework to assess whether medication is appropriate
4. Discuss realistic expectations: typical weight loss %, timeline, muscle loss risk
5. Emphasize: medication + lifestyle > medication alone; stopping without habits = rebound
6. **Always: this requires a physician's prescription and monitoring. Never self-prescribe.**

## 4. Weight Loss Analysis & Metabolism

When tracking weight loss progress or calculating metabolic targets:

### Body Composition Assessment
- **BMI** (WHO Asian standards): Normal 18.5-24, Overweight 24-28, Obese >=28
- **Body fat**: Male normal 15-20%, elevated 20-25%, obese >25%
- **Waist circumference**: Male >=90cm = abdominal obesity risk
- **Waist-to-hip ratio**: Male >=0.9 = abdominal obesity
- **Ideal weight**: BMI method = height(m)^2 x 22; Broca = (height(cm) - 100) x 0.9

### Metabolic Rate Calculation
- **Mifflin-St Jeor (recommended)**:
  - Male: BMR = (10 x weight_kg) + (6.25 x height_cm) - (5 x age) + 5
  - Female: BMR = (10 x weight_kg) + (6.25 x height_cm) - (5 x age) - 161
- **Katch-McArdle (body fat based)**: BMR = 370 + (21.6 x lean_mass_kg)
- **TDEE** = BMR x activity factor (sedentary 1.2 / light 1.375 / moderate 1.55 / high 1.725)

### Energy Deficit Management
- Deficit = TDEE - intake + exercise burn
- 1kg fat = ~7700 kcal; safe loss rate: 0.5-1kg/week (deficit 500-1000 kcal/day)
- **Minimum intake**: male 1500 kcal/day, female 1200 kcal/day, absolute min = BMR x 1.2

### Phase Management
- **Weight loss phase**: Track rate, monitor speed, adjust deficit
- **Plateau detection**: 2+ weeks with <0.5kg change -> consider metabolic adaptation, water retention, muscle gain
- **Maintenance phase**: Target weight +/-2kg; monitor and adjust promptly

## 5. Advanced Nutrition Analysis

Extends meal analysis with deeper nutritional insights:

### Micronutrient Tracking
- Track vitamins (A, C, D, E, K, B-complex) and minerals (Ca, Fe, Mg, Zn, Se, K, Na)
- Calculate RDA achievement rate per nutrient
- Status classification: <50% severe deficiency, 50-75% insufficient, 75-100% approaching, 100-150% adequate, >150% high/check UL

### Nutritional Quality Scoring
- **Nutrient density score** (0-10): Vitamins achieved (40%) + Minerals achieved (30%) + Fiber (20%) + Limiting nutrients penalty (10%)
- **Food diversity score**: Number of distinct food groups per day/week
- **Balanced diet score**: Macro ratio alignment with targets

### Meal Pattern Analysis
- Eating window duration (hours between first and last meal)
- Meal frequency and timing consistency
- Weekday vs weekend dietary differences
- Sodium/potassium ratio tracking (target K:Na > 2.0)

### Key Nutrient Safety Boundaries
- Vitamin A: UL 3000ug/day long-term
- Vitamin D: UL 100ug/day long-term
- Iron: UL 45mg/day long-term
- Sodium: target <2300mg/day (ideal <1500mg)
- Persistent intake <1200 kcal/day -> flag malnutrition risk

## Acknowledgments

Sections 4-5 incorporate knowledge from [OpenClaw-Medical-Skills](https://github.com/MedClaw-Org/OpenClaw-Medical-Skills) by WellAlly Tech and MD BABU MIA, PhD. Licensed under MIT.
