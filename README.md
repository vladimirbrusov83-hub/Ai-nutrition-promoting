# AI Nutrition Prompt Builder

A single-file, client-side tool that helps people build precise, personalized prompts for any AI assistant (ChatGPT, Claude, Gemini, Copilot) to generate a complete nutrition plan — from daily macros to a 7-day meal plan, recipes, and a grocery list.

**No AI runs on this page.** It is a guided form that assembles a well-structured prompt the user copies and pastes into their AI assistant of choice.

Built by [Vladimir Brusov](https://brusovcoach.org) — strength coach, St. Louis MO.

---

## Live demo

Served from `index.html` — open directly in any browser, no build step needed.

---

## What it does

The user fills out five sections and the prompt updates live in a side panel on the right. When done, they click **Copy prompt** and paste it into any AI assistant.

### Section 1 — Your Stats
- Imperial / Metric unit toggle (lbs/ft+in vs kg/cm)
- Age, sex, weight, height, body fat % (optional)
- Activity level (5 options: sedentary → athlete, with steps/day reference for each)
- Medical context: 8 condition checkboxes (Type 1/2 diabetes, PCOS, hypothyroidism, hypertension, high cholesterol, kidney disease, pregnancy/breastfeeding) + free-text field

### Section 2 — Your Goal
- Primary goal: fat loss, muscle gain, recomposition, maintain, performance, general health
- Calorie approach: let AI calculate TDEE, or choose a specific deficit/surplus level

### Section 3 — Diet & Preferences
- Diet type: omnivore, carnivore, pescatarian, vegetarian, vegan, halal, kosher, paleo, ketogenic
- Allergy/restriction checkboxes: gluten-free, dairy-free, tree nuts, peanuts, shellfish, egg-free, soy-free, low-FODMAP
- Foods to avoid (free text)
- Foods to include (free text)

### Section 4 — Meal Structure
- Meals per day (slider: 2–6)
- Eating schedule: classic 3 meals, IF 16:8 / 18:6 / 20:4, training-timed, custom
- Cooking time available: quick (<20 min), moderate (20–45 min), relaxed (up to 1 hr), meal-prep style
- Cooking skill level: beginner, intermediate, advanced
- Grocery budget: budget-friendly, moderate, flexible
- Free-text notes (training schedule, lifestyle context, family size, etc.)

### Section 5 — What to Generate
Six independently toggleable outputs:
| Output | Description |
|---|---|
| Daily calorie & macro targets | TDEE calculation + protein/carbs/fat in grams |
| Full 7-day meal plan | Every meal with ingredients, portions, and per-meal macros |
| Recipes with instructions | Step-by-step for 3+ meals from the plan |
| Weekly grocery list | Organized by category with quantities |
| Meal prep guide | Batch cooking tips and weekly prep schedule |
| Substitution list | 2 macro-equivalent alternatives for each main ingredient |

---

## Generated prompt structure

```
[Role instruction — expert nutrition specialist, no generic advice]

MY PROFILE
Age | Sex | Weight | Height | Body fat %
Activity level (with steps/day range)
Medical context + ⚠ adjustment instruction (if any conditions selected)

MY GOAL
Goal (full description)
Calorie approach

MY DIET & RESTRICTIONS
Diet type
Allergies / restrictions
Foods to avoid
Foods to include

MEAL STRUCTURE
Meals per day
Eating schedule
Cooking time / skill / budget
Additional context (free text)

PLEASE PROVIDE
[Numbered list of selected outputs — sequential regardless of which are checked]
— or fallback: "A complete, detailed nutrition plan..."

FORMAT REQUIREMENTS
• Clear headers and sections
• Culinarily coherent meals — no unusual flavor/texture combinations
• Specific gram/cup/oz amounts only — no vague terms
• Macros per meal + daily totals (when applicable)
• Avoid listed foods (when applicable)
• Realistic timeline — AI determines based on goal and stats
• State assumptions if profile is incomplete
```

---

## Technical details

| Property | Value |
|---|---|
| Stack | Vanilla HTML + CSS + JS — zero dependencies |
| Build step | None — open `index.html` directly |
| Frameworks | None |
| External resources | Google Fonts (Bebas Neue, Crimson Pro, JetBrains Mono) |
| AI on page | None |
| Persistence | localStorage — all fields auto-saved, restored on reload |
| Accessibility | Semantic HTML, labels on all inputs |

### Key JS functions

- **`setUnit(u)`** — switches imperial/metric, shows/hides height fields, updates labels and placeholders
- **`getData()`** — reads all form values and returns a flat data object including medical conditions
- **`buildPrompt()`** — assembles the prompt, updates the live preview, saves state to localStorage; fires on every `oninput` / `onchange`
- **`copyPrompt()`** — copies prompt to clipboard, shows "Copied!" state for 2.5s
- **`resetForm()`** — confirms with user, clears all fields and localStorage, restores defaults (macros + week plan checked, slider at 3, imperial units)
- **`saveState()` / `restoreState()`** — serialize/deserialize all form fields to localStorage under key `nutrition-prompt-state`

### Prompt logic notes

- Output items use `outputs.length + 1` before each `.push()` — sequential numbering regardless of which checkboxes are checked, no hardcoded "1."
- If all output checkboxes are unchecked, PLEASE PROVIDE falls back to a generic full-plan request
- Medical context injects a `⚠` warning line into the prompt only when at least one condition is selected
- The "show macros" format rule is only injected when a meal plan or macro output is selected
- The "avoid foods" format rule is only injected when the avoid field is non-empty

---

## UX features

- **Live prompt preview** — right panel updates on every keystroke
- **localStorage persistence** — refresh-safe; all 20+ fields auto-saved
- **Reset button** — top of form panel, requires confirmation before clearing
- **Mobile prompt button** — fixed red pill button on mobile ("View Your Prompt ↓") that auto-hides via IntersectionObserver when the prompt panel is in view

---

## Design system

Matches [brusovcoach.org](https://brusovcoach.org):

```css
--red:   #C8102E   /* primary action, section numbers, focus rings */
--gold:  #C9A84C   /* accent values, hints, AI chip borders on hover */
--black: #0A0A0A   /* page background */
--dark:  #111111
--dark2: #1A1A1A   /* card backgrounds */
--dark3: #242424   /* section headers, input backgrounds */
--gray:  #888888   /* muted labels */
--light: #D4CFC8   /* prompt text */
--white: #F5F0E8   /* primary text */
```

Fonts: Bebas Neue (display/headings), Crimson Pro (body), JetBrains Mono (labels, code, buttons)

### Mobile breakpoints
- `≤860px` — single column layout, prompt panel goes static, mobile prompt button appears
- `≤600px` — medical conditions grid drops to 1 column
- `≤480px` — field rows stack, allergy grid goes 1 column, tighter padding
- All inputs `font-size: 1rem` to prevent iOS Safari auto-zoom

---

## Usage

1. Open `index.html` in a browser
2. Fill in your stats, goal, diet preferences, and meal structure
3. Check the outputs you want in Section 5
4. Watch the prompt build live on the right panel
5. Click **Copy prompt**
6. Paste into ChatGPT, Claude, Gemini, Copilot, or any AI assistant

---

## License

MIT
